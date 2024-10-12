# Dockerfile

- 도커 이미지를 생성하기 위한 템플릿이다.





## Dockerfile의 구조

> [!NOTE]
>
> #### **하나의 Docker 이미지는 base 이미지부터 시작해서 기존 이미지위에 새로운 이미지를 중첩해서 여러 단계의 이미지 층(layer)을 쌓아가며 만들어집니다.**

#### 

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

위 명령어를 추가함으로써 컨테이너가 바로 종료되는 걸 막을 수 있다. 그런 뒤에 `docker exec -it`를 활용해 컨테이너 내부에 직접 들어가서 디버깅을 하면 된다.



