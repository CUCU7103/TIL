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

![image](https://github.com/user-attachments/assets/226a6da4-ef46-4e0b-aa64-ed54425d7fb9)


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

2차원 배열 

- 일반적으로 int 변수명[][]; 의 형식으로 선언한다.
- 2차원 배열의 1번째 대괄호는 1차원 , 2번째 대괄호는 2차원이라고 한다.

```java
2차원 배열에서 (예시) int arr[][]
array[0] = int 배열이다.
array[0][0] = int 값이다.
```

```java

public class ArrayTwoDimension{
  
    public static void main(String[] args)
    {
        ArrayTwoDimension array = new ArrayTwoDimension();
        array.twoDimensionArray();
    }

    public void twoDimensionArray(){
        int [][] twoDim;
        twoDim = new int[2][3];
    }

}
```

그림을 통해 2차원 배열을 알기 쉽게 보자면 아래와 같다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/cVimSC/btrJrgdOZw4/X0PRc6jvU2SWrEhobTocT1/img.png)

위와 같이 배열을 생성하면 2차원 배열의 크기를 동일하게 만 설정할 수있다

2차원 배열의 크기를 별도로 지정하고자 하면 아래와 같이 하면된다.

```java
int twoDim[][];
twoDim = new int[2][]; // 1차원 배열의 크기를 지정한다.
// 그 다음 반드시 2차원 배열의 크기를 지정해 줘야한다.
twoDim[0] = new int[3];
twoDim[1] = new int[2];
```

2차원 배열을 한번에 선언하고 초기화 할 수 있다.

```java
int [][] array = {{1,2,3},{1,2,3}};
```

배열의 길이를 아는 방법

- length를 사용하여 값을 구할 수있다.
- 2차원 배열의 경우 배열명.length를 사용하면 몇개의 1차원 배열이 있는지 알 수있고
- 배열명[].length를 사용하면 해당 1차원 배열의 길이를 구할 수 있다.

```java
public class Arraylength
{
    
    public static void main(String[] args)
    {
        Arraylength array = new Arraylength();
        array.printArray();
        array.printArray2();
    }

    public void printArray2(){
        int [] arr = {1,2,3,4,5};
        System.out.println("array length = " + arr.length);
    }

    public void printArray(){
        int [][]twoDim = {{1,2,3},{1,2,3}};
        System.out.println("twoDim.length = " + twoDim.length);
        System.out.println("twoDim[0].length = " + twoDim[0].length);
    
    
    }

}

```

배열의 요소를 반복문으로 출력해보기

```java

public class Arraylength
{
    
    public static void main(String[] args)
    {
        Arraylength array = new Arraylength();
        array.printArray();
        
    }
    public void printArray(){
        int [][]twoDim = {{1,2,3},{1,2,3}};
        System.out.println("twoDim.length = " + twoDim.length);
        System.out.println("twoDim[0].length = " + twoDim[0].length);

		/*	
	  // 배열의 크기를 직접 하드코딩하는것은 좋지 않다.	 
				for(int i = 0; i < 2; i++){
            for(int j = 0; j < 3; j++){
              System.out.println("twoDim["+i+"]["+j+"]=" 
                + twoDim[i][j] );
          }
      */  }   

		
		// 아래와 같이 length를 활용하기 가변적으로 값을 받을 수 있다.
        for(int i = 0; i < twoDim.length; i++){
            for(int j = 0; j < twoDim[i[.length; j++){
                System.out.println("twoDim["+i+"]["+j+"]=" 
                + twoDim[i][j] );
            }
        }    

    }

   

}
// 결과
 twoDim[0][0]=1
twoDim[0][1]=2
twoDim[0][2]=3
twoDim[1][0]=1
twoDim[1][1]=2
twoDim[1][2]=3
```

### 배열을 위한  for문 (for-each)

```java
for(타입이름 임시변수명 :  반복대상 객체){

}
```

- 반복대상 객체 (여기서는 배열)의 값이 0번째 위치부터 순서대로 임시변수에 for 문을 돌면서 들어간다.
- 

해당 for문을 사용해서 좀 더 깔끔한 코드를 작성할 수 있다.

```java

public class ArrayNewFor{
    
    public static void main(String[] args) {
        ArrayNewFor arr = new ArrayNewFor();
        arr.newFor();
    }

    public void newFor(){
        int [] oneDim = {1,2,3,4,5};
        for(int data : oneDim){
            System.out.println(data);
        }
    }

}
// 결과
1
2
3
4
5

```

2차원 배열을 변경한다면?

```java
// 기존 반복문
    for(int i = 0; i < twoDim.length; i++){
            for(int j = 0; j < twoDim[i[.length; j++){
                System.out.println("twoDim["+i+"]["+j+"]=" 
                + twoDim[i][j] );
            }
        }  
        
        
 =============================================
 
 for-each를 사용한 코드
 
 for(int []arr1 : twoDim){
		for(int data : arr1){
			System.out.println(data);
		}
		
	}      
```
