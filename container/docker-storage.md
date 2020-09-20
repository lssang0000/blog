# Docker Storage
하나의 도커 이미지를 이용해 하나 이상의 도커 컨테이너를 만들 수 있다. 이 경우 이미지는 변경되면 안되며, 각 컨테이너에서 일어나는 변경을 관리하기 위해 컨테이너마다 데이터 파일을 별도로 관리해주어야 한다.

```
Host OS 파일시스템 |                | Container 파일시스템
                  ┌|─[Container 01]─|─[Data file01]
[Docker Image A] ─┼|─[Container 02]─|─[Data file02]
                  └|─[container 03]─|─[Data file03]
```
- Docker Image 파일은 Host OS의 파일시스템에서 관리됨
- Container가 사용하는 파일은 Host OS 위에 새롭게 구축된 파일시스템에서 관리됨

## Union File Mount System
도커는 유니온 파일 마운트 시스템을 이용하여 이러한 구조를 가능하게 하며 해준다. 유니온 마운트란, 하나의 디렉토리 지점에서 여러개의 디렉토리를 마운트 함으로써, 마치 하나의 통합된 디렉토리처럼 보이게 하는 개념이다.

```
[UnionFS] |  [A] [B''] [C'] [D']
----------|-----------------------
[layer 3] |     [B''] [C'] [D']
----------|-----------------------
[layer 2] |     [B']       [D]
----------|-----------------------
[layer 1] | [A] [B]        [C]
```

도커는 다양한 종류의 유니온 파일 시스템을 제공하기 위해 스토리지 드라이버를 지원한다. 도커가 지원하는 스토리지 드라이버와 몇 가지 유니온 파일 시스템에 살펴본다.

## Storage Driver 
내가 사용하는 Docker의 스토리지 드라이버 정보는 다음 명령어를 통해 확인할 수 있다.
```
$ docker info
```

다음은 커널 버전 5.3.0을 사용하는 Ubuntu 18.04 LTS에 실행한 결과이다. 다음 항목을 통해 설치된 도커의 스토리지 드라이버 정보를 확인한다.

- Storage Driver : Docker가 사용하는 스토리지 드라이버 타입
- Docker Root Dir : Container 파일 시스템을 관리하는 root 경로

```
Client:
 Debug Mode: false

Server:
 Containers: 2
  Running: 2
  Paused: 0
  Stopped: 0
 Images: 6
 Server Version: 19.03.5
 Storage Driver: overlay2
  Backing Filesystem: extfs
  Supports d_type: true
  Native Overlay Diff: true
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: b34a5c8af56e510852c35414db4c1f4fa6172339
 runc version: 3e425f80a8c931f88e6d94a8c831b9d5aa481657
 init version: fec3683
 Security Options:
  apparmor
  seccomp
   Profile: default
 Kernel Version: 5.3.0-59-generic
 Operating System: Ubuntu 18.04.3 LTS
 OSType: linux
 Architecture: x86_64
 CPUs: 6
 Total Memory: 15.31GiB
 Name: aiden-desktop
 ID: OLOJ:FN6K:NRCC:HXXI:J6F3:UBRO:37OO:GCMV:MITM:OD2B:VYS3:HOBH
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false
```

## AUFS

> AUFS(Advanced mulit-layered unification filesystem)는 2006년에 Junjiro Okajima에 의해 개발되었다. 신뢰성과 성능 개선을 목표로 하였으나 writable branch balancing, 및 기타 개선사항과 같은 일부 새로운 개념들도 도입하였으며, 이 중 일부는 현재 UnionFS 2.x 브랜치에 구현되어 있다. <br/><br/>
> [출처] Wikipedia

AUFS는 유니온 파일 시스템의 하나로 여러 개의 디렉토리를 일련의 스택으로 묶어서 하나의 단일 뷰로 보여준다. 스택을 구성하는 각 layer를 branch라고 부른다. AUFS를 사용하는 환경에서 도커 이미지는 여러개의 읽기 전용 branch로 구성된 Image Layer에 포함된다. 

컨테이너가 생성된 후 변경분은 Container Layer에서 관리되는 branch에 쓰여진다. 이 때 Image Layer와 동일한 부분은 그대로 적용하고 달라진 부분만 Container layer에 쓰여지기 때문에 디스크를 효율적으로 사용할 수 있다.

결과적으로 컨테이너가 실행된 후 Union mount point에서 Image Layer와 Container Layer를 마운트 하여 단일 뷰로 디렉토리를 사용할 수 있게된다.

```
AUFS Branch Uni | ~~~~~~~~~~~~~~~~  | Union mount point
------------------------------------|--------------------
AUFS Branch Con | ~~~~~~~~~~~~~~~~  | Container Layer
------------------------------------|--------------------
AUFS Branch N  | ~~~~~~~~~~~~~~~~   |
...                                 |
AUFS Branch 03 | ~~~~~~~~~~~~~~~~   | Image Layers
AUFS Branch 02 | ~~~~~~~~~~~~~~~~   |
AUFS Branch 01 | ~~~~~~~~~~~~~~~~   |
```

AUFS의 특징을 정리하자면 다음과 같다.
- 기본 파일 시스템으로  기본 extfs를 사용
- AUFS는 커널에 포함된 파일 시스템이 아니라 지원하지 않는 배포판도 있음
- 유니온 파일 시스템중 하나로 여러 디렉토리를 일련의 스택으로 묶어서 하나의 단일 뷰로 제공
- AUFS 스택은 여러 개의 파일 시스템을 단일 뷰로 만들고 이를 마운트 포인트로 제공
- 스택을 이루는 디렉토리들은 하나의 마운트 포인터로 묶일 수 있어야 하므로 반드시 같은 리눅스 호스트에 존재해야 함

## OverlayFS

> AUFS는 꽤 오래 전 (2006년) 에 개발되었기 때문에 레퍼런스가 많은 편이며 쉽게 사용할 수 있다는 장점이 있지만, 리눅스 커널의 Mainline에 포함되어 있지 않다는 단점이 있다. 따라서 CentOS와 같은 보수적인 레드햇 계열 리눅스는 AUFS 모듈을 커널에 내장시키지 않았으며, AUFS 대신에 Devicemapper를 사용하기도 했다 (물론 AUFS를 설치하면 CentOS에서도 사용할 수는 있다).<br/><br/>
> 그런데 Devicemapper는 안정성이 낮고 성능 또한 좋지 않아, 이를 위한 대안으로써 보편적으로 사용할 수 있는 OverlayFS가 많이 사용되는 추세에 있다. OverlayFS (오버레이 파일 시스템) 은 2010년에 개발된 유니온 마운트 파일 시스템으로, 2014년에 리눅스 커널 Mainline에 통합되었다. 따라서 CentOS에서도 OverlayFS를 사용할 수 있다. 리눅스 커널 4.x 버전에 이르러서는 OverlayFS의 향상된 버전인 Overlay2를 사용할 수 있으며, inode의 Utilization 향상 등의 기능이 추가되었다. <br/><br/>
> 참고로, AUFS는 소스코드가 읽기 힘들고 주석이 없다는 이유로 리눅스 커널에 통합되지 않았다고 한다. <br/><br/>
> [출처] [alice_k106님의 블로그](https://blog.naver.com/alice_k106/221530340759)

OverlayFS는 AUFS와 유사하지만 여러 개의 레이어가 스택을 이루는 AUFS와는 달리 단지 두개의 레이어만 가지고 있다. 따라서 AUFS와 같은 멀티레이어 이미지를 구성할 수 없다.
AUFS보다 좀더 빠른것이 특징이며, 리눅스 커널 3.18부터 정식 지원된다.



```
docker 구조     |                   | overlayfs 구조 
Container Mount | [A] [B'] [C] [D]  | merged
----------------|-------------------|--------------------
Container Layer |     [B']     [D]  | upperdir
----------------|-------------------|--------------------
Image Layers    | [A] [B]  [C]      | lowerdir
```

overlayFS는 Docker 구조에 대응되는 디렉토리를 lowerdir, upperdir, merged 라는 이름으로 관리한다. lowerdir은 읽기 전용인 이미지 레이어에 해당한다. Container Layer는 lowerdir의 파일들로부터 만들어지며 이는 upperdir에서 관리된다. 최종적으로 컨테이너가 실행될 때 merged 디렉토리가 생성되어 사용하게되는 구조이다.

lowerdir는 읽기 전용으로 변경을 허락하지 않아야한다. lowerdir 파일을 변경하게 되면 `lower_file` 과 같은 이름으로 upperdir에 그 변경 내용이 반영된다고 한다. 또한  lowerdir을 삭제하게 되면 upperdir에 `whiteout` 이라는 크기가 0인 파일이 생성되어 lower_file이 삭제되었음을 표시한다. 이 경우 merged에는 upperdir만 보이게 된다.

OverlayFS의 특징을 정리하자면 다음과 같다.

- AUFS와 비슷한 유니온 파일 시스템
- AUFS보다 단순한 디자인을 가지고있음
- 리눅스 커널 3.18부터 정식 지원
- 좀더 빠르다

## OverlayFS2

OverLayFS를 설명한 [도커 공식 문서](https://docs.docker.com/storage/storagedriver/overlayfs-driver/)에 따르면 대표적인 특징은 다음과 같다.

- 자체적으로 128개의 lower 레이어를 지원
- 더나은 성능, overlay보다 적은 inode 사용

더욱 상세한 설명은 더 공부 후 기술할 예정이다.

## Devicemapper - loop-lvm

(작성중)

## Device mapper - direct-lvm

(작성중)


## zfs

(작성중)


## btrfs

(작성중)


## Reference
- https://docs.docker.com/storage/storagedriver/select-storage-driver/
- https://docs.docker.com/storage/storagedriver/overlayfs-driver/
- https://ssup2.github.io/theory_analysis/Union_Mount_AUFS_Docker_Image_Layer/
- https://www.joinc.co.kr/w/man/12/docker/aufs
- https://www.joinc.co.kr/w/man/12/docker/storage
- https://blog.naver.com/alice_k106/221530340759

## [**Back to Blog Home**](../README.md)