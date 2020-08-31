# SVN vs Git

<br/> 

## SVN
CVCS(Centralized version control system, 중앙 집중식 버전 관리 시스템)의 일종이다. 그림과 같이 SVN은 각 버전이 변경된 목록만 관리하는 증분 방식으로 관리된다. 

![](images/svn-changlist.png "그림 출처 : https://git-scm.com/book/en/v2")
[그림 1] SVN - Chang List

<br/> 

SVN 사용 시 작업 영역은 다음과 같다.  
- Central Repository
- Working Copy

모든 작업자는 하나의 Central Repository로부터 같은 리소스를 다운(Working Copy)받는다. 그리고 Working Copy를 수정하여 Commit하는 순간 Central Repository에 변경 목록이 반영되어 모든 작업자에게 공유된다.

## Git
DVCS(Distributed version control system, 분산 버전 관리 시스템)의 일종이다. 그림과 같이 Git은 각 버전마다 스냅샷이 관리되며, 스냅샷을 구성하는 각 파일은 이전 버전과 비교하여 변경된 파일은 파일 자체로, 동일한 파일은 링크로 관리된다. 

![](images/git-shapshot.png "그림 출처 : https://git-scm.com/book/en/v2")
[그림 2] Git - Shapshot

Git 사용 시 작업 영역은 다음과 같다.  
- Working Directory
- Staging Area
- Local Repository
- Remote Repoistry

모든 작업자는 Remote Repoistry로부터 리소스를 다운받으면 작업자 Local-PC에 생성되어있는 Local Repository에 리소스를 저장한다. 소스의 변경은 Working Directory에서 수행하고 반영할 파일은 Staging Area로 이동시켜 업로드를 수행한다.

<br/> 

![](images/centralized-vs-distributed.png "https://www.git-tower.com/learn/git/ebook/en/desktop-gui/appendix/from-subversion-to-git")
[그림 3] Git vs SVN

## Git vs SVN

지금까지 Git과 SVN의 특징을 살펴보았다. 이러한 특징에서 비롯된 두 버전관리 시스템의 장단점은 다음과 같다.

#### SVN의 장점
- 구조가 간단하고 직관적이다. 다수의 개발자들이 참여한 대규모 프로젝트에는 많은 주니어 개발자와 시니어 개발자로 구성된다. 또한 이들의 실력 또한 제각각일 것이다. 이러한 상황에는 소스관리 정책을 간소화하고 직관적인 SVN을 사용하는 것이 유리하다.

#### SVN의 단점
- 작은 실수가 전체에 영향을 미친다. Commit을 수행하면 바로 Central Repository에 반영되기 때문에 개인의 잘못된 코드가 전체에 영향을 미칠 수 있다.

- Branch 개념이 있어 이런 저런 전략을 수립할 수 있다. 하지만 각 Commit Point에서 전체 소스를 조립하기 위해 이전 revision을 조회하고 조립하는 과정이 필요하기 때문에 비교적 느리고, 작은 코드가 branch 전체에 영향을 줄 수 있다는 점에서 부적합하다.

#### Git의 장점
- 개별 작업이 가능하다. Commit을 수행한 후 Local Repository에 있는 모든 버전은 각각의 스냅샷을 가지기 때문에, 실행, 테스트, 디버깅 등의 모든 과정을 로컬 PC에서 비교적 간편하게 수행할 수 있다.

- 다양한 전략이 빠른 속도로 가능하다. Git은 Branch의 각 commit point가 스냅샷을 가지기 때문에 개발 소스와 테스트 소스, 릴리즈 소스의 분리 등 다양한 소스 관리 전략을 수립할 경우 각 버전에 대해 빠른 작업이 가능하다. 

- GitHub, GitLab 등 git 기반의 웹 호스팅 서비스, Devops 서비스 등의 생태계가 구축되어있어 다양한 연관 작업이 가능하다.

#### Git의 단점
- learning curve가 비교적 심히다. Commit, Checkout, Update 등 명령어가 비교적 간단한 SVN에 비해 다양한 Commit/Merge 전략을 위한 많은 명령어들이 제공되기 때문에 많이 써보지 않으면 사용하기 어렵다. 

- 잘못된 사용을 초래할 수 있다. 기능의 조합이 너무 유연해서 잘못된 branch 전략을 수립할 경우 프로젝트 관리가 매우 어려워 질 수 있다. 따라서, 전략을 명확히 공유할 수 있도록 프로젝트 팀 맴버가 한 자리 수로 구성되어있으며, 팀원 개개인이 역량을 충분히 가지고 있는 고급 프로젝트 관리에 적합하다.

## Reference
[1] [git-scm](https://git-scm.com/book/en/v2)
[2] [Distributes vs. Centralized](https://www.git-tower.com/learn/git/ebook/en/desktop-gui/appendix/from-subversion-to-git)

## [**Back to Blog Home**](../README.md)
