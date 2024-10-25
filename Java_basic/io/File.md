# 자바 I/O - File

> [!NOTE]
>
> - 데이터를 읽고 쓰는 행위를 말합니다.
> -  input output으로 프로그램 내에서 입력과 출력을 의미합니다.
>   - 사용자로부터 입출력, 파일로부터의 입출력, 네트워크로 부터의 입출력 등 여러 상황에서 발생할 수 있습니다.

기본적으로 자바의 java.io 패키지에서는

- 바이트 기반의 데이터를 처리하기 위해서 여러 종류의 스트림(stream)이라는 클래스를 제공합니다.
  - 읽는 작업은 InputStream, 쓰는 작업은 outputStream을 통해서 작업을 진행합니다

- char 기반의 문자열로만 되어있는 파일은 Reader와 Writer라는 클래스로 처리합니다.
  - 읽는 작업은 Reader, 쓰는 작업은 Writer를 사용합니다.



java 7 부터는 NIO2가 등장하면서 files라는 클래스가 등장했습니다. 이 클래스는 file 클래스에 있는 메서드들을 대체하여 제공합니다.

- File 클래스는 객체를 생성하여 데이터를 처리하는 반면에 Files 클래스는 모든 메서드가 static으로 선언되어져 있기 때문에 별도의 객체를 생성할 필요가 없다는 차이가 있습니다.

  

## File

**File** 클래스를 통하여 파일시스템의 파일이나 디렉터리를 조작(삭제, 파일명변경 등)을 할 수 있습니다.

**File 객체를 생성했다고 해서 파일이나 디렉토리가 생성되는 것은 아닙니다..**

**그리고 경로에 실제 파일이나 디렉토리가 없더라도 예외가 발생하지 않는다.**

**File 클래스로부터 File 객체를 생성하는 방법은 아래와 같다.**

```java
File file = new File("경로");
```

**경로 구분자는 OS마다 조금씩 다르다.**

**윈도우에서는 \\ 또는 /를 둘 다 사용할 수 있고, 맥 OS 및 리눅스에서는 /를 사용한다.**

 

**아래는 윈도우에서 File 객체를 생성하는 코드이다.**

```java
File temp = new File("C:/Temp/file.txt");
File temp = new File("C:\\Temp\\file.txt");
```



**파일이나 디렉토리가 실제 있는지 확인하고 싶다면 File 객체를 생성하고 나서 exists() 메소드를 호출해 보면 된다.**

```java
boolean isExist = file.exists(); //파일이나 폴더가 존재한다면 true를 리턴
```

- 예시

```java
public class FileSample {

    public static void main(String[] args) {
        FileSample sample = new FileSample();
        String pathName = "C:\\test-project\\";
        sample.checkPath(pathName);

    }

    private void checkPath(String pathName) {

        File file = new File(pathName);
        System.out.println(pathName + " is exists? = " + file.exists());
        
    }

}

```

- 실제 디렉터리를 생성하려면 mkdir()을 사용하면 됩니다.

![[Java] File과 Files 클래스 - File 클래스](https://blog.kakaocdn.net/dn/c5fw8R/btsplbxudWB/H8HWPrHjULWpkRKWtLqKB0/img.png)

```java
  public void makeDir(String pathName){
        File file = new File(pathName);
        System.out.println("Make " + pathName +" result = " + file.mkdir());
    }
```

**exists() 메소드의 리턴값이 true라면 아래 메소드를 사용할 수 있습니다.**

![[Java] File과 Files 클래스 - File 클래스](https://blog.kakaocdn.net/dn/pZkeS/btspE5hA8Q0/lNtNddNBj75TUEL85cy5K1/img.png)

![[Java] File과 Files 클래스 - File 클래스](https://blog.kakaocdn.net/dn/bd8Xcx/btspmKMYIFF/tEZnPX9FAqFYkXfa0J9OC0/img.png)

####  파일 및 디랙토리 생성

- `createNewFile()` : 새로운 파일을 생성
- `mkdir()` : 새로운 디렉토리를 생성
- `mkdirs()` : 경로상에 없는 모든 디렉토리를 생성

```java
public class FileExample {
	public static void main(String[] args) throws Exception {
		//File 객체 생성
		File dir = new File("C:/Temp/images");
		File file1 = new File("C:/Temp/file1.txt");
		File file2 = new File("C:/Temp/file2.txt");
		File file3 = new File("C:/Temp/file3.txt");

		//존재하지 않으면 디렉토리 또는 파일 생성
		if(dir.exists() == false) { dir.mkdirs(); }
		if(file1.exists() == false) { file1.createNewFile(); }
		if(file2.exists() == false) { file2.createNewFile(); }
		if(file3.exists() == false) { file3.createNewFile(); }

		//Temp 폴더의 내용을 출력
		File temp = new File("C:/Temp");
		File[] contents = temp.listFiles();

		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd a HH:mm");
		for(File file : contents) {
			System.out.printf("%-25s", sdf.format(new Date(file.lastModified())));
			if(file.isDirectory()) {
				System.out.printf("%-10s%-20s", "<DIR>", file.getName());
			} else {
				System.out.printf("%-10s%-20s", file.length(), file.getName());
			}
			System.out.println();
		}
	}
```



생성한 디렉토리에서 파일 생성하기

- 디렉터리가 아닌 파일을 처리하는 메서드

```java
public class FileManageClass {

    public static void main(String[] args) {
        FileManageClass sample = new FileManageClass();
        String pathName = File.separator+"test-project" +File.separator+"text";
        String fileName= "test.txt";

        sample.checkFile(pathName,fileName);
    }

    private void checkFile(String pathName, String fileName) {

        File file = new File(pathName,fileName);
        try{
            System.out.println("Create result = " + file.createNewFile());
        } catch (Exception e) {
            e.printStackTrace();
        }

    }
}
```





