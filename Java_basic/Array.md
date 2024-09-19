
## Array - 배열

- 하나의 변수안에 여러개의 값을 담을 수 있다.
- 배열은 일반적인 자료 구조중 하나이다.
    - 자료구조란? 데이터를 저장하기 위한 구조
- 모든 배열은 참조 자료형입니다.

```java
배열의 초기화

int[] scores = new int [5]; 

int[] scores
scores = new int[5];  
```

## 배열의 생성 - 예시

```java
public class Weeks
{

    public static void main(String[] args)
    {
        Weeks sample = new Weeks();
        sample.initWeek();
    }

    public void initWeek(){
        String[] week = new String[7];
        week[0] = "월";
        week[1] = "화";
        week[2] = "수";
        week[3] = "목";
        week[4] = "금";
        week[5] = "토";
        week[6] = "일";
        System.out.println(week[1]);
    }

}
// 출력값 화
```

배열의 인덱스의 시작은 0이다.

```java
public class main
{
    
    public static void main(String[] args)
    {
        Main sample = new Main();
        sample.init();
    }

    public void init(){
        int lottoNumbers = new int[7]; // 크기가 7인 배열 생성
        lottoNumbers [0] = 5;
        ...
        lottoNumbers [6] = 12;
        lottoNubmers [7] = 24;
    }
}

// 결과
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: Index 7 out of bounds for length 
```

크기가 7인 배열의 인덱스는 

0부터 시작하기 때문에  인덱스의 값은 6이 마지막이다.  7을 입력하면 생성한 배열의 크기 이상의 인덱스에 값을 넣기 때문에 

아래와 같은 오류가 발생한다.

```java
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: Index 7 out of bounds for length  
```

기본 자료형  배열 생성시 아무값도 입력하지 않고 생성하면, 각 자료형의 기본값과 동일하다

- 각 자료형의 기본값

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/4d733e24-dba1-495a-87c4-243568c1610e/ecd64ade-2af4-486c-acca-629931f69ba7/029a9e67-7178-412c-9674-0017e2b6eebf.png)

그렇다면 참조 자료형 배열 생성시 기본값은 어떻게 될까?

```java
public class Array1
{
    // tip: arguments are passed via the field below this editor
    public static void main(String[] args)
    {
        Array1 sample = new Array1();
        sample.referenceType();
    }

    public void referenceType(){
        String [] strings = new String[2];
        System.out.println(strings[1]);
        
    }

}
// 결과 -> null;
```

- 모든 참조 자료형은 초기화를 하지 않으면 null 이다.

인덱스를 통해 값을 출력하는 것이 아닌 배열을 그냥 출력하면 어떻게 나올까

- 배열을 그냥 출력하면 배열이 어떤 타입인지만 확인할 수있다.

```java

public class ArrayPrint
{
   
    public static void main(String[] args)
    {
        ArrayPrint array = new ArrayPrint();
        array.printString();
    }

    public void printString(){
        System.out.println("strings = " + new String[0]);
        System.out.println("arrays = " + new ArrayPrint[0]);
    }

}

// 값
strings = [Ljava.lang.String;@251a69d7
arrays = [LArrayPrint;@6b95977
```

### 배열을 선언하는 다른 방법

- 배열의 선언과 함께 초기화가 가능하다.
- 중괄호 안에 각 위치에 해당하는 값들을 콤마로 구분하여 나열해야한다.
- 보통 배열에 절대 변경되지 않을 값을 지정하는데 사용되어지는 방식이다.

```java
public class ArrayInitialize
{
   
    public static void main(String[] args)
    {
        ArrayInitialize array = new ArrayInitialize();
        array.otherInit();
    }   

    public void otherInit(){
   
        int [] lottoNumbers = {5,12,23,25,38,41,2};
    }

}
```

예를 들면  “달”이 있을 수 있다.

```java

public class MonthArray
{
    
    public static void main(String[] args)
    {
       MonthArray array= new MonthArray();
       System.out.println(array.getMonth(4));
    }

    public String getMonth(int monthInt){
        String [] month = {"1월","2월","3월","4월","5월",
        "6월","7월","8월","9월","10월","11월","12월"};
        return month[monthInt-1];
    }

}
```

배열의 길이를 아는 방법

- .length를 사용하면 됩니다.

```java
public class Exam {

    public static void main(String[] args) {
    
        String word = "Hello World";
        
        int[] array = {1,2,3,4,5};
        
        System.out.println(word.length());
        
        System.out.println(array.length);
    
    }
    
}

```
