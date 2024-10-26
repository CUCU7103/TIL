# Reader와 Writer

## 문자 기반 스트림

문자데이터를 다루는 데 사용된다는 것을 제외하고는 바이트기반 스트림과 문자기반 스트림의 사용방법은 거의 동일합니다.



## Reader

- 문자 기반 입력 스트림의 최상위 클래스로 추상 클래스입니다.  모든 문자 기반 입력 스트림은 이 클래스를 상속합니다.



주요 메서드

| 정의 된 메서드                                  | 설명                                                         |
| ----------------------------------------------- | ------------------------------------------------------------ |
| `abstract void close()`                         | - inputStream 을 닫음 - `스트림을 닫음으로써 사용하고 있던 자원을 반환` |
| void mark(int readlimit)                        | - inputStream 에서 현재 위치 표시                            |
| int read()                                      | - 입력소스로부터 하나의 문자를 읽어서 0~65535 범위의 정수값을 반환, 더 이상 읽어 올 데이터가 없으면 -1을 반환 |
| `int read(char[] c)`                            | - `입력소스로부터 매개변수로 주어진 배열 c의 크기만큼 읽어서 배열 c에 저장, 읽어 온 데이터의 개수 또는 -1을 반환` |
| `abstract int read(char[] c, int off, int len)` | - `입력소스로부터 최대 len개의 byte를 읽어서, char[] 배열 c의 지정된 위치(off)부터 저장하고 읽은 데이터 수 또는 -1 반환` |
| boolean ready()                                 | 입력소스로부터 데이터를 읽을 준비가 되어 있는 알려줌         |
| void reset()                                    | - mark() 마지막으로 호출한 위치로 이동                       |
| long skip(long n)                               | - 현재위치에서 문자 수(n) 만큼 스킵                          |



### Reader를 확장하고 주로 사용되어지는 클래스

- #### BufferedReader, CharArrayReader



## Writer

- 문자 기반 출력 스트림의 최상위 클래스로 추상 클래스입니다.  모든 문자 기반 입력 스트림은 이 클래스를 상속합니다.



주요 메서드

| 정의 된 메서드                           | 설명                                                         |
| ---------------------------------------- | ------------------------------------------------------------ |
| `void close()`                           | - OutputStream 을 닫음, 사용하고 있던 자원 반환              |
| `void flush()`                           | - `스트림버퍼에 남은 모든 내용을 출력소스에 쓴다.`           |
| void write(int i)                        | - 주어진 값을 출력소스에 쓴다.                               |
| `void write(char[] c)`                   | - `주어진 char[] 배열의 모든 내용을 출력소스에 쓴다.`        |
| `void write(char[] c, int off, int len)` | - `주어진 char c에 저장된 내용 중에서 off번째부터 len개 만큼만을 읽어서 출력소스에 쓴다.` |
| append(char c)                           | - 매개 변수로 넘어온 char를 추가한다.                        |
| append(chare)                            | - 매개 변수로 넘어온 charSequence를 추가한다.                |



- append()
  - StringBuilder나 StringBuffer로 만든 문자열을 처리할때 사용하는 것을 권장한다.



사용예시

```JAVA
public class MangeTextFile {

    public static void main(String[] args) {
        MangeTextFile manager = new MangeTextFile();
        int numberCount = 10;
        String fullPath = separator + "test-project" + separator +"text"
                + separator + "text.txt";
        manager.writeFile(fullPath,numberCount);
    }
    public void writeFile(String fileName, int numberCount) {
        FileWriter fileWriter = null;
        BufferedWriter bufferedWriter = null;
        try {
            fileWriter = new FileWriter(fileName);
            bufferedWriter = new BufferedWriter(fileWriter);
            for (int loop = 0; loop <= numberCount; loop++) {
                bufferedWriter.write(Integer.toString(loop)); // 기존 파일에 내용작성
                bufferedWriter.newLine(); // 작성 후 줄 바꿈
            }
            System.out.println("Write success !!");
        } catch (IOException e) {
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (bufferedWriter != null) {
                try {
                    bufferedWriter.close();
                } catch (IOException ioe) {
                    ioe.printStackTrace();
                }
            }
            if (fileWriter != null) {
                try {
                    fileWriter.close();
                } catch (IOException i) {
                    i.printStackTrace();
                }
            }
        }
    }
}
```

- Filewriter나 BufferedWriter 변수를 try 문 안에서 선언하면 finally에서 close() 메서드를 사용할 수 없으니 주의하자
- 그리고 close()를 통해서 닫아줄때는 먼저 객체를 생성한 역순으로 close()를 해주어야 한다.



작성한 파일을 읽는 작업까지 추가

```java
package io;

import static java.io.File.createTempFile;
import static java.io.File.separator;

import java.io.BufferedInputStream;
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import javax.sound.midi.Soundbank;

public class MangeTextFile {

    public static void main(String[] args) {
        MangeTextFile manager = new MangeTextFile();
        int numberCount = 10;
        String fullPath = separator + "test-project" + separator +"text"
                + separator + "text.txt";
        manager.writeFile(fullPath,numberCount);
        manager.readFile(fullPath);
    }
    public void writeFile(String fileName, int numberCount) {
        FileWriter fileWriter = null;
        BufferedWriter bufferedWriter = null;
        try {
            fileWriter = new FileWriter(fileName);
            bufferedWriter = new BufferedWriter(fileWriter);
            for (int loop = 0; loop <= numberCount; loop++) {
                bufferedWriter.write(Integer.toString(loop));
                bufferedWriter.newLine();
            }
            System.out.println("Write success !!");
        } catch (IOException e) {
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (bufferedWriter != null) {
                try {
                    bufferedWriter.close();
                } catch (IOException ioe) {
                    ioe.printStackTrace();
                }
            }
            if (fileWriter != null) {
                try {
                    fileWriter.close();
                } catch (IOException i) {
                    i.printStackTrace();
                }
            }
        }
    }

    public void readFile(String fileName){
        FileReader fileReader = null;
        BufferedReader bufferedReader = null;
        try{
            fileReader = new FileReader(fileName);
            bufferedReader = new BufferedReader(fileReader);
            String data;
            while((data=bufferedReader.readLine()) != null){
                System.out.println(data);
            }
            System.out.println("Read Success");
        }catch(IOException e){
            e.printStackTrace();
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            if(bufferedReader!= null){
                try{
                    bufferedReader.close();
                }catch (IOException ioe){
                    ioe.printStackTrace();
                }
            }
            if(fileReader != null){
                try{
                    fileReader.close();
                }catch(IOException ioe){
                    ioe.printStackTrace();
                }
            }
        }
    }
}

// Write success !!
0
1
2
3
4
5
6
7
8
9
10

Read Success

```



파일을 읽는데 너무 많은 코드를 작성하는 문제점이 있습니다.

java 7 이상에서는 Files 클래스를 사용하여 좀 더 간편하게 처리할 수 있습니다.

결과는 동일하게 나옵니다..

```java
public void readAndUseFiles(String fileName) {
    FileReader fileReader = null;
    BufferedReader bufferedReader = null;
    try{
        String data = new String(Files.readAllBytes(Paths.get(fileName)));
        System.out.println(data);

    }catch(IOException e){
        e.printStackTrace();
    }
}

// Write success !!
0
1
2
3
4
5
6
7
8
9
10

Read Success
```





