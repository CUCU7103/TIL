# SOLID 원칙 - SRP, OCP

- ### SOLID 원칙 

  - SRP : 단일 책임원칙
  - OCP:  개방 폐쇄원칙
  - LSP:  리스코프 치환원칙
  - ISP: 인터페이스 분리 원칙
  - DIP : 의존 역전 원칙



- 결합도와 응집도

  - 좋은 소프트웨어 설계를 위해서는 결합도는 낮추고 응집도는 높이는 것이 바람직합니다.
  - 결합도
    - **<u>모듈간의  상호 의존 정도로써 결합도가 낮으면 모듈 간의 상호 의존성이 줄어들어 객체의 재사용이나 수정,유지보수가 용이</u>**하다.
  - 응집도
    - <u>**하나의 모듈 내부에 존재하는 구성 요소들의 기능적 관련성으로 응집도가 높은 모듈은 하나의 책임에 집중하고 독립성이 높아셔 재사용이나 기능의 수정 유지보수가 용**이</u>하다.

  

  ## SRP - 단일 책임 원칙

  - 어떤 클래스를 변경해야 하는 이유는 오직 하나뿐이여야 한다.

    ![image-20250101214400694](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250101214400694.png)

  - 남자라고 하는 클래스와 남자 클래스에 의존하는 다양한 클래스가 있다고 가정하자
  - 위 그림을 보면 하나의 클래스에 여러 책임과 역할이 들어가 있다.

  ![image-20250101215119016](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250101215119016.png)

  - 그래서 위와 같이 역할을 분리하여 단일 책임을 가지게 하는 것을 단일 책임 원칙이라고 합니다.

    - 남자라는 하나의 클래스가 역할과 책임에 따라 네 개의 클래스로 쪼개진 것을 볼 수 있다.
    - 그리고 역할과 클래스명이 딱 떨어지니 이해하기도 쉽습니다.
    - 이러한 단일 책임 원칙은 속성 , 메서드, 패키지 , 모듈 , 컴포넌트 프레임워크 등에도 적용할 수 있는 개념입니다.

    

### 메서드가 단일 책임원칙을 지키지 않은 경우

``` java
Class 강아지{
    
    final static Boolean 수컷 = true;
    final static Boolean 암컷 = false;
    
    Boolean 성별;
    
    void 소변보다(){
        if(this.성별 == 수컷 ){
            // 한쪽 다리를 든다
        }else{
            // 앉는 자세로 본다.
        }
       
    }
    
    
}
```

- 메서드에서 강아지가 수컷이냐 암컷이냐에 따라 분기 처리가 진행되는것을 알 수 있습니다.
- 강아지 클래스의 메서드가 수컷 , 암컷강아지의 행위를 모두 구현하려고 하기에 메서드가 단일 책임 원칙을 위반한 것입니다.
- 메서드가 단일 책임 원칙을 지키지 않을 경우 나타나는 대표적인 냄새가 분기 처리를 위한 if문입니다.

- 아래코드와 같이 리펙터링 할 수있습니다.

``` java
abstarct class 강아지{
    abstarct void 소변보다()
}


class 수컷강아지 extends 강아지{	
	void 소변보다(){
        //  한쪽 다리를 들고본다.
    }
}

class 암컷강아지 extends 강아지{
    void 소변보다(){
        // 앉아서 본다.
    }
}
```



- 단일 책임 원칙과 객체 지향 4대 특성은 어떻게 결부되 있을까?
  - **단일 책임과 가장 관계가 깊은 것은 바로 모델링 과정을 담당하는 추상화이다**.
  - **애플리케이션의 경계를 정하고 추상화를 통해 클래스들을 선별하고 속성과 메서드를 설계할 때 반드시 단일 책임 원칙을 고려하는 습관을 들이자**.



## OCP - 개방 폐쇄 원칙

- 소프트웨어 엔티티(클래스, 모듈 , 함수 등)은 확장에 대해서는 열려 있어야 하지만 변경에 대해서는 닫혀 있어야 한다.

- 즉 자신의 확장에는 열려있고, 주변의 변화에 대해서는 닫혀 있어야 한다.

![image-20250101225304980](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250101225304980.png)

- 상위 클래스 또는 인터페이스를 중간에 둠으로써 다양한 자동차가 생긴다고 해도 객체지햐의 운전자는 운전습관에 영향을 받지 않는다.
- 다양한 자동차가 생긴다고 하는 것은 자동차 입장에서는 자신의 확장에는 개방돼 있는 것이고, 운전자 입장에서는 주변의 변화에 폐쇄되 잇는 것이다.

- OCP의 가장 좋은 예는  JDBC입니다.
- ![image-20250101225614185](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250101225614185.png)

- jdbc를 사용하는 클라이언트는 데이터베이스가 오라클에서 mysql로 Connection을 설정하는 부분 외에는 따로 수정할 필요가 없다.
- Connection 설정 부분을 별도의 설정 파일로 분리해두면 클라이언트 코드는 단 한 줄도 변경할 필요가 없다.
- 위 그림에서 SQL server를 오라클로 교체하거나 해도 자바 애플리케이션은 jdbc 인터페이스라고 하는 완충 장치로 인해 변화에 영향을 받지 않는다.
  - 즉 자바 애플리케이션은 데이터베이스라고 하는 주변의 변화에 닫혀 있는 것이다.
  - 데이터베이스를 교체한다는 것은 데이터베이스가 자신의 확장에는 열려 있다는 것입니다.



추가적으로 한 가지 예를 더 들어보자면, 현실 세계에서의 개방 폐쇄 원칙의 사례를 보자

![image-20250101230624294](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20250101230624294.png)

- 편의점에서는 일일 삼교대로 직원이 교대한다. 
- 하지만 직원이 바뀐다고 해서 손님이 구매라는 행위를 하는덴 영향이 없습니다.
  - 편의점 직원이 근본적으로 판매라고 하는 행위, 즉 손님과의 인터페이스가 바뀌지 않는 한 손님의 구매라는 행위는 직원이 세부적으로 변경되어도 전혀 영향을 받지 않는다.
  - 직원 교대라고 하는 주변의 변화에 손님의 구매 행위는 영향을 받지 않는 것이고, 직원은 교대라는 확장 행위에는 열려 있는것입니다.

- 개방 폐쇄 원칙을 따르지 않는다고 해서 객체 지향 프로그램을 구현하는 것이 불가능한 것은 아닙니다.
- 하지만 위 원칙을 무시하고 프로그램을 작성하면 객체 지향 프로그래밍의 가장 큰 장점이 유연성, 재사용성, 유지보수성 등을 얻을 수 없습니다.
