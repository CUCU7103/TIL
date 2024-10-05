# 네이버 클라우드 서버 생성 및 배포 연습

참고 링크 

https://docs.3rdeyesys.com/docs/compute/vpc/server-create/

네이버 클라우드 서버에 스프링 프로젝트 띄우기

1. 네이버 클라우드 서버 설정하기
2. putty로 서버 접속 후 도커 셋팅 후 도커 허브에 로그인 하기
3. 스프링 프로젝트를  도커 이미지로 빌드한뒤 도커 허브로 푸쉬
4. 도커 허브에 푸쉬한 이미지를 서버에서 pull 한다
5. pull 한 이미지를 docker run 명령어를 통해서 실행시킨다
6. 설정한 공인 ip  주소를 통해서 접속한다.

서버에 도커 설치
```
패키지 업데이트 진행
sudo apt update

필수 패키지 설치
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y

도커 gpg 키 추가
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

도커 저장소 추가
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

도커 설치
sudo apt install docker-ce -y

도커 시작
sudo systemctl start docker

도커 허브 로그인 
docker login

```


이슈 :
 - 포트번호를 잘못 지정해서 접근하지 못햇던 오류 발생함.

스프링 부트 프로젝트에 도커 파일 설정함.

```docker
FROM openjdk:21-jdk-slim
ARG JAR_FILE=build/libs/*.jar
COPY ${JAR_FILE} app.jar
ENTRYPOINT ["java","-jar","/app.jar"]
```

application.properties

- 컨테이너 포트 지정함

```docker
server.port=28080
```

```docker

=== 로컬

도커 이미지 빌드
docker build -t test-app .

도커 허브에 이미지를 올리기 위해 태그 및 이미지명 수정
docker tag test-app:test starlightpizza/test-app:test

도커 허브에 이미지 업로드
docker push starlightpizza/test-app:test

===== 서버

이미지 확인후 컨테이너 실행 (포트포워딩 진행)
docker run -d -p 8080:28080 --name test-app starlightpizza/test-app:test

docker logs -f 이미지 id 로 동작하는거 확인

http://223.130.158.233:8080/test에 접속되는것 확인
```
