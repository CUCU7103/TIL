# Docker Volume(도커 볼륨)

### ✅ 컨테이너가 가진 문제점

Docker를 활용하면 특정 프로그램을 컨테이너로 띄울 수 있다.<br>
이 프로그램에 기능이 추가되면 새로운 이미지를 만들어서 컨테이너를 실행시켜야 한다.<br> 
이 때, Docker는 기존 컨테이너에서 변경된 부분을 수정하지 않고, 새로운 컨테이너를 만들어서 통째로 갈아끼우는 방식으로 교체를 한다.<br> 
이게 효율적이라고 생각했던 것이다.<br>

이런 특징 때문에 기존 컨테이너를 새로운 컨테이너로 교체하면, 기존 컨테이너 내부에 있던 데이터도 같이 삭제된다. 
만약 이 컨테이너가 MySQL을 실행시키는 컨테이너였다면 MySQL에 저장된 데이터도 같이 삭제 돼버린다. 

따라서 컨테이너 내부에 저장된 데이터가 삭제되면 안 되는 경우에는 **볼륨(Volume)**이라는 개념을 활용해야 한다. 

### ✅ Docker Volume(도커 볼륨)이란?

**도커의 볼륨(Volume)** 이란 **도커 컨테이너에서 데이터를 영속적으로 저장하기 위한 방법**이다. <br>
볼륨(Volume)은 컨테이너 자체의 저장 공간을 사용하지 않고, 호스트 자체의 저장 공간을 공유해서 사용하는 형태이다. 

![image](https://github.com/user-attachments/assets/13b01994-2d6c-439c-afff-cc897d0a6a25)


### ✅ 볼륨(Volume)을 사용하는 명령어

```
$ docker run -v [호스트의 디렉토리 절대경로]:[컨테이너의 디렉토리 절대경로]** [이미지명]:[태그명]
```

- [호스트의 디렉토리 절대 경로]에 디렉토리가 이미 존재할 경우, 호스트의 디렉터리가 컨테이너의 디렉터리를 덮어씌운다.
    
   ![image](https://github.com/user-attachments/assets/09d93134-29bd-4435-a426-35767e262bb8)

    
- [호스트의 디렉토리 절대 경로]에 디렉토리가 존재하지 않을 경우, 호스트의 디렉터리 절대 경로에 디렉터리를 새로 만들고 컨테이너의 디렉터리에 있는 파일들을 호스트의 디렉터리로 복사해온다.

  ![image](https://github.com/user-attachments/assets/afa41ffe-f26f-4d67-afe0-5e0dc424bc0a)