# main() 메서드의 동작과정

![image-20241216001213903](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241216001213903.png)

- main 메서드는 프로그램이 실행되는 시작점입니다.



``` java
public class Start{
  public static void main(String[] args){
      System.out.println('Hello OOP!!');
  }
}

```



main() 메서드의 실행과정

1. JRE가 프로그램 안에 main() 메서드가 있는지 확인한다.

2. JRE는 Start 클래스에서 main() 메서드를 찾는다.

3. main() 메서드의 존재가 확인되면 JRE는 프로그램 실행을 위한 사전 준비에 착수한다.

4. JVM을 부팅시킨다.

   - 부팅된 jvm은 전처리를 진행한다.
   - 모든 자바 프로그램이 반드시 포함해야 되는 패키지가 있는데  java.lang 패키지이다.
   - jvm 먼저 java.lang 패키지를 메모리의 스태틱 영역에 가져다 놓는다.
   - 다음으로 jvm은 개발자가 작성한 모든 클래스와 임포트 패키지 역시 스태틱 영역에 가져다 놓는다.

5. main 메서드를 실행하기 위해 스택 프레임이 스택 영역에 할당되어진다.

   - 조금 정확히 보자면  중괄호를 만날 때 마다 스택 프레임이 생긴다 (클래스를 여는 중괄호 제외하고)

6. 메서드의 인자의 변수 공간을 할당한다

7. main 메서드 안의 첫 명령문을 실행하게 된다.

8. main 메서드 안의 코드를 전부 실행한뒤 main 메서드는 종료되어지는데 이때 JRE가 JVM을 종료하고 JRE 자체도 운영체제 상의 메모리에서 사라진다.

   

   jvm은 먼저 java.lang 패키지를 메모리의 스태틱 영역에 가져다 놓는다.

   ![image-20241216003113695](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241216003113695.png)

   main 메서드를 실행하기 위해 스택 프레임이 스택 영역에 할당되어진다.

   ![image-20241216003532759](https://raw.githubusercontent.com/CUCU7103/save-image-repo/main/image/image-20241216003532759.png)

