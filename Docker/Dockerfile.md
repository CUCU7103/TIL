![image](https://github.com/user-attachments/assets/57d10017-8270-43a1-ac31-c22b541f9f61)# Dockerfile

- 도커 이미지를 생성하기 위한 템플릿이다.



## Dockerfile의 구조

> [!NOTE]
>
> #### **하나의 Docker 이미지는 base 이미지부터 시작해서 기존 이미지위에 새로운 이미지를 중첩해서 여러 단계의 이미지 층(layer)을 쌓아가며 만들어집니다.**
>
> #### docker build [옵션] <경로>
>
> - <경로>는 도커파일이 위치한 디렉토리의 경로를 나타내고, 옵션은 이미지 빌드 과정을 조정하고 세부 사항을 지정하는 데 사용됩니다.



### FROM

> [!IMPORTANT]
>
> - #### **FROM은 베이스 이미지를 생성하는 역할을 한다**.
>
> - Docker 컨테이너를 베이스 이미지를 기반으로 추가적인 셋팅을 할 수 있다. 
>
> - #### **Dockerfile 내부에 가장 먼저 등장해야되는 명령어이다.**
>
> - **base 이미지는 일반적으로 Docker Hub와 같은 Docker repository에 올려놓은 잘 알려진 공개 이미지인 경우가 많습니다.**



- 컨테이너를 새로 띄워서 환경을 구축 할때 기본적인 프로그램이 어떤게 깔려있으면 좋겠는지 선택하는 옵션
  - spring boot 프로젝트를 실행하려면 jdk 가 깔려있어야되고
  - javascrpt 관련된 것을 처리하기 위해선  Node.js가 있어야 한다.

이러한 기본적인 설정을 할 수있다.



``` 
# 문법
FROM [이미지명]
FROM [이미지명]:[태그명]

# 예시 
FROM ubuntu:18.04

```

#### 실습

- Dockerfile로 이미지(Image) 생성하는 문법

```dockerfile
# JDK 17
FROM openjdk:17-jdk
```

```
# Dockerfile로 이미지(Image) 생성하는 문법
docker build -t my-jdk17-server .

# 이미지를 기반으로 컨테이너 띄우기
docker run -d my-jdk17-server
```

Docker를 사용하면 대부분의 코드가 컨테이너 내부에서 작동한다. 

그러다보니 어떤 과정으로 처리됐는 지, 잘 처리는 됐는 지를 직접적으로 눈에 보이지 않는다. 이 때문에 Docker 학습에 어려움을 겪는다.

이를 해결하기 위해 우리는 2가지 방법을 이미 익혔다.

- `docker logs`를 활용해 컨테이너 로그 확인하기

- `docker exec -it`를 활용해 컨테이너 내부에 직접 들어가보기

  

그렇다면 종료된 컨테이너에는 어떻게 접근해야 될까

**Dockerfile**

```bash
FROM openjdk:17-jdk

...

**ENTRYPOINT ["/bin/bash", "-c", "sleep 500"]** # 500초 동안 시스템을 일시정지 시키는 명령어
```

위 명령어를 추가함으로써 컨테이너가 바로 종료되는 걸 막을 수 있다. 

그런 뒤에 `docker exec -it`를 활용해 컨테이너 내부에 직접 들어가서 디버깅을 하면 된다.





## Copy

COPY는 호스트 컴퓨터에 있는 파일을 복사해서 컨테이너로 전달합니다.

``` dockerfile
# 문법
COPY [호스트 컴퓨터에 있는 복사할 파일의 경로] [컨테이너에서 파일이 위치할 경로]

# 예시
COPY app.txt /app.txt
```

#### 파일 복사해보기

``` dockerfile
FROM ubuntu

COPY app.txt /app.txt

ENTRYPOINT ["/bin/bash", "-c", "sleep 500"] # 디버깅용 코드	
```

실행과정

``` dockerfile
docker build [옵션] <경로>

docker build -t my-server
docker run -d my-server
docker exec -it [Container ID] bash

=====================================================
bash-4.4# ls
app.txt  boot  etc   lib    media  opt   root  sbin  sys  usr
bin      dev   home  lib64  mnt    proc  run   srv   tmp  var
bash-4.4# cat app.txt 
Hello worldbash-4.4#
```

----

#### 폴더 안에 있는 모든 파일들 복사하기

- my-app 디렉터리 만들고, my-app 디렉터리 안에 파일 생성하기

#### Dockerfile

``` dockerfile
FROM ubuntu

COPY my-app /my-app/

ENTRYPOINT ["/bin/bash", "-c", "sleep 500"] # 디버깅용 코드
```

실행과정

``` dockerfile
$ docker build -t my-server .
$ docker run -d my-server
$ docker exec -it [Container ID] bash


==================================================
bash-4.4# ls
bin   dev  home  lib64  mnt     opt   root  sbin  sys  usr
boot  etc  lib   media  my-app  proc  run   srv   tmp  var
bash-4.4# cd my-app
bash-4.4# ls
test.txt
bash-4.4# cat test.txt 
testbash-4.4#

```

----

#### 와일드 카드 사용해보기

- app.txt, readme.txt 파일 2개 만들기

Dockerfile

```dockerfile
FROM ubuntu

COPY *.txt /text-files/

ENTRYPOINT ["/bin/bash", "-c", "sleep 500"] # 디버깅용 코드
```

- 주의) /text-files라고 적으면 안 되고 /text-files/라고 적어야 text-files라는 디렉토리 안에 파일들이 정상적으로 복사된다. 

----

####  .dockerignore 사용해보기

특정 파일 또는 폴더만 COPY를 하고 싶지 않을 수 있다. 그럴 때 .dockerignore를 활용한다. 

![image](https://github.com/user-attachments/assets/310fd136-cc6b-4aa2-8d9c-6652df836567)


Dockerfile

``` dockerfile
FROM ubuntu

COPY ./ /

ENTRYPOINT ["/bin/bash", "-c", "sleep 500"] # 디버깅용 코드
```



``` 
$ docker build -t my-server .
$ docker run -d my-server
$ docker exec -it [Container ID] bash

$ ls
```


## EntryPoint

- 컨테이너가 생성되고 최초로 실행할때 수행되는 명령어를 말합니다.

- docker run 명령을 통해 컨테이너를 실행할 때 **추가 인자로 덮어쓸 수 없습니다.**

````dockerfile
# 문법
ENTRYPOINT [명령문...]

# 예시
ENTRYPOINT ["node", "dist/main.js"]

````



로그를 통해서 실행되어진 것을 확인할 수 있다.

````dockerfile
FROM ubuntu

ENTRYPOINT ["/bin/bash", "-c", "echo hello"]

$ docker build -t my-server .
$ docker run -d my-server
$ docker ps -a
$ docker logs [Container ID]
````

![image](https://github.com/user-attachments/assets/7de4f031-a17e-4e7b-bd0d-b8ebe695e2f4)


간단한 스프링 부트 프로젝트 띄워보기

![image](https://github.com/user-attachments/assets/540ac56d-f8a0-4316-ad96-9e76bff51cf4)

- 프로젝트 생성 후 Dockerfile 생성하기

```
FROM openjdk:17-jdk

COPY build/libs/*SNAPSHOT.jar app.jar

ENTRYPOINT ["java", "-jar", "/app.jar"]
```

- 프로젝트 빌드하기

```
./gradlew clean build
```



> [!NOTE]
>
> Dockerfile을 바탕으로 이미지 빌드하기
>
> $ docker build -t hello-server .
>
> 
>
> 이미지가 잘 생성됐는 지 확인하기
>
> $ docker image ls
>
> 
>
> 생성한 이미지를 컨테이너로 실행시켜보기
>
> $ docker run -d -p 8080:8080 hello-server
>
> 
>
> 컨테이너 잘 실행되고 있는 지 확인하기
>
> $ docker ps



- docker log 명령을 사용해서 실행여부 확인하기

![image](https://github.com/user-attachments/assets/ed30c718-ffdf-4f6a-9f4b-1acbe39548eb)


<p></p>

## RUN

#### - 이미지를 생성하는 과정에서 사용할 명령문 실행



- 사용법

```
# 문법
RUN [명령문]

# 예시
RUN npm install
```



### ✅ `RUN` vs `ENTRYPOINT`

`RUN` 명령어와 `ENTRYPOINT` 명령어가 헷갈릴 때가 있다. 

둘 다 같이 명령어를 실행시키기 때문이다.

하지만 엄연히 둘의 사용 용도는 다르다.

- `RUN`은 ‘**이미지 생성 과정**’에서 필요한 명령어를 실행시킬 때 사용합니다
  - 또한 환경 셋팅할 때 주로 사용합니다.

- `ENTRYPOINT`는 생성된 이미지를 기반으로 **컨테이너를 생성한 직후에** 명령어를 실행시킬 때 사용한다.





미니 컴퓨터 환경이 ubuntu로 구성되었으면 좋겠고 git이 깔려있으면 좋겠다고 가정하자. 이런 환경을 구성하기 위해 Dockerfile을 활용해 ubuntu, git이 깔려있는 이미지를 만들면 된다. 

Dockerfile 작성하기

```Dockerfile
FROM ubuntu 

RUN apt update && apt install -y git 

ENTRYPOINT ["/bin/bash", "-c", "sleep 500"]
```



이미지 빌드 및 컨테이너 실행

```bash
$ docker build -t my-server . 
$ docker run -d my-server 
$ docker exec -it [Container ID] bash $ git -v # 컨테이너 내에 git이 잘 설치됐는 지 확인
```

## WORKDIR : 작업 디렉토리를 지정

#### WORKDIR으로 작업 디렉터리를 전환하면 **그 이후에 등장하는 모든 RUN, CMD, ENTRYPOINT, COPY, ADD 명**령문은 해당 디렉터리를 기준으로 실행된다.

#### 작업 디렉터리를 굳이 지정해주는 이유는 **컨테이너 내부의 폴더를 깔끔하게 관리하기 위해서이다.** 

컨테이너도 미니 컴퓨터와 같기 때문에 Dockerfile을 통해 생성되는 파일들을 특정 폴더에 정리해두는 것이 추후에 관리가 쉽다. 

만약 WORKDIR을 쓰지 않으면 컨테이너 내부에 존재하는 기존 파일들과 뒤섞여버린다. 

#### ![✅](data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==) 사용법

```dockerfile
# 문법 
WORKDIR [작업 디렉토리로 사용할 절대 경로] 
# 예시 
WORKDIR /usr/src/app
```



```dockerfile
# 문법
WORKDIR [작업 디렉토리로 사용할 절대 경로] 
# 예시 
WORKDIR /usr/src/app
```







#### ![🎯](data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==) 예제

app.txt, src, config.json 파일 만들기

Dockerfile 만들어서 이미지 생성 및 컨테이너 실행

> WORKDIR을 안 썼을 때 파일이 어떻게 구성되는 지 먼저 확인해보자.

Dockerfile

```dockerfile
FROM ubuntu 
COPY ./ ./ 
ENTRYPOINT ["/bin/bash", "-c", "sleep 500"] # 디버깅용 코드
```

```
$ docker build -t my-server . 
$ docker run -d my-server 
$ docker exec -it [Container ID] bash $ ls
```



> WORKDIR을 썼을 때 파일이 어떻게 구성되는 지 확인해보자. 

```Dockerfile
FROM ubuntu
WORKDIR /my-dir COPY ./ ./ 
ENTRYPOINT ["/bin/bash", "-c", "sleep 500"]
```

```
$ docker build -t my-server . 
$ docker run -d my-server 
$ docker exec -it [Container ID] bash $ ls
```

WORKDIR을 사용 안 할시

![image](https://github.com/user-attachments/assets/6b58d0f0-ae33-42bf-a16b-6d93f667e260)



WORKDIR을 사용 할 시

![image](https://github.com/user-attachments/assets/1ea49d45-05f4-41b7-8521-862b09c0971d)


WORKDIR을 사용하면 초기경로부터 잡아주고 해당 경로안에 파일이 생성된 것을 확인할 수 있다.














