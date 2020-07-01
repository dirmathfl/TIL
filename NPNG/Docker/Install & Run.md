# Install & Run

> 우분투 18.04에서 도커를 설치를 다루고자 한다.  간단히 명령어 몇줄로 도커를 설치할 수 있으며, 도커 컨테이너를 실행하는 과정 또한 간단하다.:smile:



# 도커 설치

```shell
sudo apt install curl
curl -fsSL get.docker.com -o get-docker.sh
sh get-docker.sh
docker -v
```

 curl이 설치 되어 있지 않다면 첫 번째 줄과 같이 curl을 설치하는 과정을 진행 후에 아래의 명령어를 순차적으로 실행하면 도커를 편리하게 설치 할 수 있다. 마지막 줄인 docker -v에서 버전 정보가 출력된다면 정상적으로 도커가 설치 된 것이다.

  

# 도커 그룹에 유저 추가

```shell
sudo usermod -aG docker $USER
sudo usermod -aG docker other-user
```

 도커를 사용하기 위해서는 root 권한이 필요하여 sudo를 명령어를 붙여주어야 한다. 하지만 위와 같이 도커 그룹에 유저를 추가하면, sudo 없이 도커를 사용할 수 있다. 추가 후에 로그아웃 후에 다시 로그인을 하면 sudo 없이 사용할 수 있다.  

*이후의 설명들은 도커 그룹에 유저를 추가한 환경을 기준으로 하므로 docker 관련 명령어 실행 시 sudo를 붙이지 않는다.*

  

# 도커 사용 시작하기

> 도커를 사용을 시작한다는 것은 크게 2가지로 구분 된다. 첫 번째는 이미지를 빌드하는 것이고 두 번째는 컨테이너를 실행하는 것이다. 도커 컨테이너를 실행하기 위해서는 작업에 따라 적절한 이미지를 빌드하여, 이를 기반으로 컨테이너를 실행하여야 한다.  따라서 도커 컨테이너 실행을 다루기 전, 이미지를 다루기 위한 방법들을 먼저 알아보고자 한다. 아직 도커 컨테이너를 실행하기 전이므로 컨테이너를 이미지화 하는 것이 아닌 `Dockerfile`을 이미지 빌드로 사용하거나 도커 허브를 사용하는 방법을 소개하고자 한다.



## 1. 이미지 다루기

### 가. 이미지 빌드

 `Dockerfile`은 이미지를 어떻게 빌드 할지 기술되어 있으며 이전 글에서 다루었다. 간단히 다시 상기하자면, 어떤식으로 이미지를 만들지에 대한 설계도와 같다고 생각하면 쉽다. 

  

```dockerfile
FROM ubuntu:latest
```

​    

 현재 디렉토리에 위와 같은 `Dockerfile`이 있을 경우 `docker image build -t ubuntu:test .`와 같이 이미지를 생성 할 수 있다. 이미지를 생성하는 명령어는 `docker image build -t 이미지이름:태그 디렉토리 경로`이다.  



 이미지를 빌드 하는데 있어 **자주 사용되는 옵션 3가지**가 있다. 

* **-t** : 이미지명과 태그명을 붙일 수 있는 명령어로, 실제 사용에 필수적이다. 
  * 빌드하는 이미지에 따라 명칭과 태그(버전)으로 구분하기 위해 필요하다.
* **-f** : 해당 옵션은 이미지를 빌드 할때 `Dockerfile`을 참조하지만 이외의 다른 이름으로 생성된 파일을 지정하고자 할때 사용되는 옵션이다.
  * ex) `docker image build -f my-docker-file -t ubuntu:test .`

* **--pull** : 최초에 이미지를 빌드하기 위해서는 FROM에 기재된 베이스 이미지를 내려받는 작업을 진행하며, 이는 호스트에 저장된다. 호스트에서 해당 이미지를 삭제하지 않는 한 베이스 이미지를 새로 내려받지 않는데, 해당 명령어를 통해 강제로 베이스 이지미를 새로 받도록 지정할 수 있다. 
  * 이는 최신 이미지를 반영하고 할 때 유용하게 사용된다.

  

### 나. 이미지 검색

 도커 허브는 깃 허브와 비슷한 개념으로 도커 이미지를 repository에 올려 사용자나 조직 이름으로 도커 이미지를 관리할 수 있다. 또한 공개된 이미지라면 이미지 검색을 통해 필요한 이미지를 직접 만드는 대신 도커 허브에 존재하는 이미지들을 가져와 사용할 수도 있다.  



```shell
docker search --filter is-official=true ubuntu
```

 

 개별적으로 생성된 이미지를 사용하는 것이 아니라면 `--filter` 옵션을 통해 공식적으로 해당 이미지를 업로드 즉, 우분투면 우분투에서 생성한 이미지를 받아 올 수 있다.



```shell
docker search --filter is-automated=false ubuntu 
```

 만약 우분투에서 ngnix나 java의 환경을 갖춘 이미지가 소프트웨어 버전에 따라 업데이트 될 경우 자동으로 이미지 빌드가 되는 이미지를 가져오고 싶다면, 위와 같은 옵션을 통해 검색할 수 있다.

  

### 다. 이미지 받기

 사용할 이미지를 검색 한 후에 다운 받기 위해서는 `docker image pull` 명령어를 사용하면 된다.



```shell
docker image pull python:latest

latest: Pulling from library/python
e9afc4f90ab0: Pull complete 
989e6b19a265: Pull complete 
af14b6c2f878: Pull complete 
5573c4b30949: Pull complete 
11a88e764313: Pull complete 
ee776f0e36af: Pull complete 
513c90a1afc3: Pull complete 
df9b9e95bdb9: Pull complete 
86c9edb54464: Pull complete 
Digest: sha256:dd6cd8191ccbced2a6af5d0ddb51e6057c1444df14e14bcfd5c7b3ef78738050
Status: Downloaded newer image for python:latest
docker.io/library/python:latest
```

 

 이미지 받기를 실행하게 되면 해당 이름과 태그에 맞는 이미지를 호스트에 다운 받게 된다. 이 이미지는 컨테이너를 생성하는데 그대로 활용할 수 있다.



### 라. 이미지 목록

```shell
docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              test                330ae480cb85        2 weeks ago         125MB
ubuntu              16.04               330ae480cb85        2 weeks ago         125MB
```



 `Dockerfile`을 통해 빌드 된 이미지나 `docker image pull`을 통해 받은 이미지는 `docker image ls`라는 명령어를 통해 확인할 수 있다.  위의 경우 각각의 태그는 다르지만 동일한 `IMAGE ID`가 할당된 것을 보아 동일한 이미지를 사용하고 있음을 알 수 있다.  만약 태그를 변경하고 싶다면, `docker image tag ubuntu:test ubuntu:real`과 같이 기존의 이름:태그와 이름:변경할 태그를 입력하면 된다.



### 마. 이미지 공개

 앞서 설명하였듯이, 도커 허브를 통해 다른 소유자의 공개된 이미지를 받아 올 수도 있으며, 반대로 나의 이미지를 공개할 수도 있다. 도커 허브의 계정 생성 후 현재 호스트에 도커 허브 계정이 로그인된 상태라면, `docker image push`를 통해 나의 이미지를 도커 허브에 업로드 할 수 있다.

 즉, 이미지라는 개체만 다를 뿐이지 깃 허브를 사용하는 것과 비슷한 맥락이라고 생각하면 이해하기 쉽다.  

  



## 2. 컨테이너 다루기

### 가. 컨테이너 실행

 이미지 빌드와 도커 허브를 활용하는 방법을 통해, 이미지에 대한 이해가 어느 정도 되었다면 해당 이미지를 통해 도커 컨테이너를 실행하여 작업을 진행 할 수 있는 환경을 만들어야 한다. 이는 `docker container run`이라는 명령어를 통해 가능하다.



```shell
docker container run ubuntu:practice ls
bin
boot
dev
etc
home
lib
lib64
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
```

  

  예를 들어 ubuntu 이미지로 부터 컨테이너를 생성하고 ls 명령어를 실행하는 컨테이너는 위와 같이 동작한다. 해당 명령어는 작업을 반복하거나 프로세스가 길지 않으므로, 명령 수행 후 컨테이너는 종료 된다. 이는 처음 도커를 접할 때 당황스럽게 만드는 부분 중 하나다. VMware와 같이 컨테이너를 실행하면 실행 환경이 지속될 것으로 예상하지만 앞서 말하였듯이 반복되는 작업이나, 처리할 작업이 없는 경우 컨테이너는 종료 된다.



### 나. 컨테이너 목록

  현재 실행 중인 컨테이너나 중지 된 컨테이너의 목록을 보기 위해서는 `docker container ls`라는 명령어를 사용하면 된다. 



 `docker container ls`의 자주 사용 되는 옵션은 다음과 같다.

* **-q** : 컨테이너 ID만 보여 준다.
* **--filter** : 특정  조건의 컨테이너를 검색하기 위해 사용한다.
* **-a** : 종료된 컨테이너의 목록을 보여준다.



### 다. 컨테이너 정지 및 재시작

 실행 중인 컨테이너를 정지 하기 위해서는 `docker container stop` 명령어 뒤에 컨테이너 ID, 컨테이너 명을 인자로 주어 해당 컨테이너를 정지 상태로 만들 수 있다.  정지가 아닌 재시작을 위해서는 `docker container restart`를 사용하면 된다.

 단순한 명령어로 컨테이너 내부의 프로그램을 재실행할 수 있다면 재시작하는 방법 이외에 `docker container exec`을 통해 실행 중인 컨테이너로 명령어를 전달하는 방법도 있다.



### 라. 컨테이너 삭제

 정지 된 컨테이너 중 더 이상 사용할 필요가 없는 컨테이너의 경우 `docker container rm` 명령어 뒤에 컨테이너ID, 컨테이너 명을 인자로 주어 해당 컨테이너를 삭제 할 수 있다. 만약 실행 중인 컨테이너를 삭제하고자 한다면 뒤에 `-f `옵션을 추가로 지정하여야 한다. 컨테이너 정지 시 해당 컨테이너를 삭제하고자 한다면 실행할 때 `docker container run --rm`을 통해 정지가 될 시, 즉시 삭제하도록 할 수도 있다.



### 마. 파일 복사

 `docker container cp` 명령어는 호스트 ↔ 컨테니어 간의 파일 복사나, 컨테이너 ↔ 컨테이너 간의 파일 복사를 가능하게 한다. 컨테이너 내부에 실행되는 프로그램의 디버깅을 위해 컨테이너 내부의 로그를 호스트로 복사하는 작업에 주로 사용된다.



