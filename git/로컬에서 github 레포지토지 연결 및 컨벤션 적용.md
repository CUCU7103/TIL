#  로컬에서 github 레포지토지 연결 및 컨벤션 적용



#### 1. 로컬 프로젝트 경로로 이동하기

``` bash
// 깃헙 주소 초기화
git init 
```

#### 2. 프로젝트와 github 리포지토리 연결하기

``` bash
git remote add origin 깃 헙주소				
```

> 완료메시지:
>
> - origin https://github.com/내깃허브주소/project.git (fetch)
> - origin https://github.com/내깃허브주소/project.git (push)

> :rocket: 원격 저장소의 목록을 확인하기 
>
> ``` bash
> git remote -v
> ```
>
> :rocket: 원격 저장소를 잘 못 연결했을 경우 연결 해제하기
>
> ``` bash
> git remote remove <repository name>
> ```
>
> 



#### 3. 연결한 repository에서 pull 받아서 로컬과 origin 동기화하기

``` bash
// develop 브랜치가 default이기에 아래와 같이 연결함.
git pull origin develop
```

> [!TIP]
>
> :rocket: 깃헙에서 default 브랜치를 develop 과 같은 신규 브랜치로 변경하였는데 연결 후 main만 보이는경우
>
> ``` bash
> // git checkout -b 브랜치명 origin/브랜치명
> git checkout -b develop origin/develop
> -> `develop` 브랜치로 전환되고 원격 `develop` 브랜치와 연결
> ```
>
> - 원격 브랜치를 로컬로 가져와서 전환할 수 있습니다:
>
>   



#### 4. 커밋 메시지 컨벤션 설정 진행

- 먼저 프로젝트 경로에 ` .gitmessage.txt`파일을 생성한다

    ![image](https://github.com/user-attachments/assets/3d88d393-1862-41b2-a679-5e53fd1f282e)

  

- ``` bash
  // 파일 내용은 아래와 같이 지정함.
  # --- 제목 Type 설명---
  # 최대 50자 이하
  # 제목 첫 글자를 대문자로
  # 제목은 명령문으로
  # 제목 끝에 마침표(.) 금지
  # 제목과 본문을 한 줄 띄워 분리하기
  #
  # Feat: | 새로운 기능 추가 |
  # Fix: | 버그 수정 |
  # Docs | 문서 수정, Readme 등 |
  # Test: | 테스트 코드 작성 및 추가 |
  # Refactor: | 코드 리펙토링 |
  # Init: | 프로젝트 초기 생성 |
  # chore: | 기능 추가, 버그 수정과 같은 주요 변경사항이 아닌 잡다한 작업을 처리|
  # -------------------
  #
  # body: 작업내역에 대해서 상세하게 기술. 20 줄 이내로 작성.
  #
  # footer: # 이슈번호
  ```

``` bash
// 깃 커밋 메시지 컨벤션 설정
// git config --global commit.template <gitmessage.txt 파일 경로>
git config --global commit.template C:/F-lab_project/helloWorld/helloWorld/.gitmessage.txt
```

#### 5.  로컬에 있는 프로젝트 리포지토리로 push 하기

- git이 원격저장소에 푸쉬하는 과정

![image](https://github.com/user-attachments/assets/9268b708-db12-4281-8afd-6943b7fd983e)


``` bash
// 모든 변경사항 스테이지에 올리기
git add . 
// 커밋 진행 이때 위에서 적용한 컨벤션 내용이 담긴 커밋 메시지 작성 창이 뜹니다. 결과는 아래 그림과 같습니다.
git commit

```

![image](https://github.com/user-attachments/assets/6d8bbfa0-da24-4da0-ad02-02e8a43dfaa1)


그런데 커밋에서 부터 아래의 오류 발생함.

> [!WARNING]
>
> ###  The requested URL returned error: 403 
>
> - 인증 오류 (Credential 문제)가 발생하였습니다.
> -  해당 레포지토리 주소에 접근 권한이 없다는 문제였습니다.

해결방법

1. url 주소를 변경하기

   1. git remote set-url origin https://내이름@github.com/내이름/repository이름.git 으로 지정

2. github 토큰 재발급받기

   1. Personal access tokens (classic) 생성 

      `https://countryxide.tistory.com/188` 해당 블로그를 참고하였다.

 ``` bash
// 생성한 토큰으로 인증 진행
git remote set-url origin https://<YOUR_TOKEN>@github.com/f-lab-edu/academy-platform.git
 ```

- 토큰 인증 후 push 진행

``` bash
// develop 브랜치로 푸쉬
git push origin develop
```

![image](https://github.com/user-attachments/assets/93f06dae-9c2c-4a8b-b396-ff8a439beac0)


- 정상적으로 push 된 것을 확인할 수 있다.

![image](https://github.com/user-attachments/assets/224b3152-6161-45c4-bd1d-43406dd51eff)








