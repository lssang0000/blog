# NBase
- 분산 저장 플랫폼 브랜드

# NBase-T
- IEEE가 승인한 이더넷 표준 IEEE P802.3bz
- 2.5GBase-T와 5GBase-T를 정의함
- Cat-5e 이상의 이더넷 케이블을 통해 2.5G 또는 5G 속도를 구현해주는 기술
- 2016년 당시 케이블링 인프라를 교체하지 않고도 최고 속도를 5배 높일 수 있음

http://www.itworld.co.kr/tags/69253/NBase-T/101330
https://m.blog.naver.com/PostView.nhn?blogId=curbow&logNo=220897545054&proxyReferer=https:%2F%2Fwww.google.com%2F

- mysql 기반의 분산 DB
https://m.blog.naver.com/loleego/221612902715
https://m.blog.naver.com/loleego/221612883092
- 분산 db 특장단점 정리
- 데이터베이스 이중화
https://gywn.net/2017/06/mysql-slave-addition-effect/
https://getto215.tistory.com/12


# NBase-ARC(redis base)
- Autonomous Redis Cluster
- Autonomous : 사람이 직접 운영할 필요 없이 SW에서 자동으로 클러스터를 진단하고 형상을 변경할 수 있음
- Redis : 데이터 저장소
- Cluster : 전체 데이터가 분산되어 저장되고 처리됨

- 구성
  - configuration master : 전체 클러스터를 관리
  - 복수의 클러스터
  - 클러스터
    - 복수의 게이트웨이
    - 복수의 복제그룹
  - 레디스
    - 복제그룹의 단위 저장소

 하나의 클러스터는 용량과 처리 속도에 제약이 없는 안전한 Redis DB입니다. 기존 Redis API를 그대로 사용할 수 있으며, 어떤 게이트웨이에 접속해도 일관성이 보장됩니다.

- 레디스
  - key-data structure
  - hash, set, list, strings, sorted set 등의 데이터 구조를 지원
  - 빠른 처리속도와 검증된 소프트웨어 안정성
  - 모든 데이터를 메모리에 상주시켜 처리
  - 이벤트 기반의 네트워크 비동기 입출력 처리
  - 

https://d2.naver.com/helloworld/614607

# Arcus(memcached base)
- memcached zookeeper 기반의 메모리 캐시 클라우드
- memcached
  - key-value 저장소
  - 영구적이진 않음
  - 메모리에 저장하므로 서버 장애나 오류 발생 시 데이터 손실됨

- zookeeper
  - 분산처리 환경에서 사용 가능한 데이터 저장소
  - 
https://d2.naver.com/helloworld/294797
https://prev.github.io/posts/%EB%84%A4%EC%9D%B4%EB%B2%84-%EC%98%A4%ED%94%88%EC%86%8C%EC%8A%A4%EB%A1%9C-%ED%99%95%EC%9E%A5%EC%84%B1-%EC%9E%88%EB%8A%94-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98%EB%A5%BC-2%ED%8E%B8/


# memcached vs redis
https://medium.com/@chrisjune_13837/redis-vs-memcached-10e796ddd717

# Kafka
- 메시징 큐의 일종
- 대용량 메시지 처리 성능

# kafka vs redis

redis, memcached 기반의 분산 메모리 저장소 NBase-ARC. Arcus
kafka