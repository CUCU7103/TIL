# NIO2

- New Input Output2의 약자로 자바 1.4에 등장한 NIO를 개선

- javaj.io 패키지의 File 클래스에 미흡한 부분을 보완하는 내용이 다수 포함되어졌습니다.

  

#### NIO2에서 File 클래스를 대체하는 클래스

| **클래스**  | **설명**                                                     |
| ----------- | ------------------------------------------------------------ |
| Paths       | · 클래스에서 제공하는 static한 get() 메서드를 사용하면 Path 인터페이스 객체를 얻을 수 있음                                                                                                                  · Path 인터페이스: 파일과 경로에 대한 정보를 가짐 |
| Files       | · 기존 File 클래스에서 제공되던 클래스의 단점을 보완한 클래스                                      · 매우 많은 메소드를 제공                                                                                                            · Path 객체를 사용하여 파일을 통제하는데 사용 |
| FileSystems | · 현재 사용중인 파일 시스템에 대한 정보를 처리하는 데 필요한 메서드를 제공                   · 클래스에서 제공되는 static한 getDefault() 메서드를 사용하여 현재 사용중인 기본 파일 시스템에 대한 정보를 갖고 있는 FileSystem 인터페이스 객체를 얻을 수 있음 |
| FileStore   | · 파일을 저장하는 디바이스, 파티션, 볼륨 등에 대한 정보를 확인하는 데 필요한 메서드 제공 |



## Path

- 클래스에서 제공하는 static한 get() 메서드를 사용하면 Path 인터페이스 객체를 얻을 수 있습니다.                                                                                                                
- 파일과 경로에 대한 정보를 가집니다.

#### Path 인터페이스에 정의된 메서드

| **리턴 타입**  | **메서드(매개변수)**  | **설명**                                                     |
| -------------- | --------------------- | ------------------------------------------------------------ |
| int            | compareTo(Path other) | 파일 경로가 동일하면 0을 리턴,  상위 경로면 음수,  하위 경로면 양수 리턴,  음스와 양수 값의 차이나는 문자열 수 |
| Path           | getFileName()         | 부모 경로를 제외한 파일 또는 디렉토리 이름만 가진  Path 리턴 |
| FileSystem     | getFileSystem()       | FileSystem 객체 리턴                                         |
| Path           | getName(int index)    | C:\Temp\dir\file.txt 일 경우  index가 0이면 "Temp"의 Path 객체 리턴  index가 1이면 "dir"의 Path 객체 리턴  index가 2이면 "file.txt"의 Path 객체 리턴 |
| int            | getNameCount()        | 중첩 경로 수, C:\Temp\dir\file.txt일 경우 3을 리턴           |
| Path           | getParent()           | 바로 위 부모  폴더의 Path 리턴                               |
| Path           | getRoot()             | 루트 디렉토리의 Path 리턴                                    |
| Path           | relativeze()          | 매개 변수로 넘긴 Path와 현재 Path의 상대 경로 리턴           |
| Path           | toAbsolutePath()      | 상대 경로를 절대 경로로 견경                                 |
| Path           | resolve()             | 매개 변수로 넘어온 문자열을 하나의 경로로 생각하고, 현재 Path의 가장 마지막 path로 추가 |
| Iterator<Path> | iterator()            | 경로에 있는 모든 디렉토리와 파일을 Path 객체로   생성 후 반복자 리턴 |
| Path           | normalize()           | 상대 경로로 표기할 때 불필요한 요소 제거                     |
| WatchKey       | register(...)         | WatchService를 등록                                          |
| File           | toFile()              | java.io.file 객체로 리턴                                     |
| String         | toString()            | 파일 경로를 문자열로 리턴                                    |
| URI            | tiUri()               | 파일 경로를 URI 객체로 리턴                                  |

- 사용예시

```java
package nio2;

import java.nio.file.Path;
import java.nio.file.Paths;

public class PathAndFiles {

	public static void main(String[] args) {
		PathAndFiles sample = new PathAndFiles();
		String dir = "C:\\godojava\\nio\\nino";
		sample.checkPath(dir);


	}
	public static void checkPath(String dir) {
		Path path = Paths.get(dir);
		System.out.println(path.toString());
		System.out.println("getFileName " + path.getFileName());
		System.out.println("getNameCount " + path.getNameCount());
		System.out.println("getParent " + path.getParent());
		System.out.println("getRoot " + path.getRoot()); 
	}

}


// 실행결과
C:\godojava\nio\nino // 파일 경로 출력
getFileNamenino // 파일명 출력
getNameCount 3 // 중첩 경로 갯수 출력
getParentC:\godojava\nio // 부모 디렉터리 출력
getRootC:\ // 루트 디렉터리 출력함.
```



```java
public class PathAndFiles {

	public static void main(String[] args) {
		PathAndFiles sample = new PathAndFiles();
//		String dir = "C:\\godojava\\nio\\nino";
		String dir = "C:\\godojava\\nio\\nio2";
//		sample.checkPath(dir);
		String dir2 = "C:\\WINDOWS";
		sample.checkPath2(dir,dir2);

	}

	private  void checkPath2(String dir, String dir2) {
		Path path = Paths.get(dir);
		Path path2 = Paths.get(dir2);

		Path relatived = path.relativize(path2);
		System.out.println("relatived path " + relatived);
		Path absolute = relatived.toAbsolutePath();
		System.out.println("toAbsolutePath path = " + absolute);
		Path nomalized = absolute.normalize();
		System.out.println("normalized path = " + nomalized);

		Path resolved = path.resolve("godofjava");
		System.out.println("resolved path = " + resolved);

	}
    
//
relatived path ..\..\..\WINDOWS 
toAbsolutePath path = C:\project\pract-1\..\..\..\WINDOWS
normalized path = C:\WINDOWS
resolved path = C:\godojava\nio\nio2\godofjava
```



- relativzed() : 매개변수로 넘긴 Path와 현재 Path와의 상대 경로를 리턴한다.
  - 즉 현재 Path에서 매개 변수로 넘긴 경로로 이동하려고 할때 어떻게 이동해야 하는지 알려준다.

- toAbosolutePath(): 상대 경로로 되어 있는 것을 절대 경로로 변경시켜준다.

- normalize() : 경로상에 있는 ". " 이나 ".." 을 제거하는 작업을 한다.

- resolve() 메서드는 매개 변수로 넘어온 문자열을 하나의 경로로 생각하고 현재 Path의 가장 마지막 path로   

  추가한다.



## Files

- 기존 File 클래스에서 제공되던 클래스의 단점을 보완한 클래스
- 매우 많은 메소드를 제공
- Path 객체를 사용하여 파일을 통제하는데 사용



| **기능**                             | **관련 메서드**                                              |
| ------------------------------------ | ------------------------------------------------------------ |
| 복사 및 이동                         | copy(), move()                                               |
| 파일, 디렉터리 등 생성               | createDirectories(), createDirectory(), createFile(), createLink(), createSymbolicLink(), createTempDirectory(), createTempFile() |
| 삭제                                 | delete(), deleteIfExists()                                   |
| 읽기와 쓰기                          | readAllBytes(), readAllLines(), readAttributes(), readSymbolicLink(), write() |
| Stream 및 객체 생성                  | newBufferedReader(), newBufferedWriter(),newByteChannel(),new(),new(),new() |
| 특정 경로의 하위 파일 및 폴더를 탐색 | walk()                                                       |
| 각종 확인                            | get으로 시작하는 메서드와 is로 시작하는 메서드들로 파일의 상태 확인 (매우 많음) |



- 사용예시

```java
public class FilesManager {
	public List<String> getContents(){
		List<String> contents = new ArrayList<>();
		contents.add("첫번째줄");
		contents.add("두번째줄");
		contents.add("세번째줄");
		contents.add("Current Date = " + new Date());
		return contents;
	}

    // 
	public Path writeFile(Path path) throws Exception {
		Charset charset = Charset.forName("EUC-KR"); // 파일의 문자열 charset 객체를 저장하는 것
		List<String> contents = getContents();
		StandardOpenOption openOption = StandardOpenOption.CREATE; // 파일을 열때 조건을 설정함.
		return Files.write(path,contents,charset,openOption);
	}
    
    // 파일을 읽어서 콘솔에 그 결과를 출력하는 역할을함.
    public void readFile(Path path) throws Exception{
		Charset charset = Charset.forName("EUC-KR");
		System.out.println("Path = " + path);
		List<String> fileContents = Files.readAllLines(path,charset);
		for(String tempContents : fileContents){
			System.out.println(tempContents);
		}
		System.out.println();
	}
    
    	public void copyMoveDelete(Path fromPath,String fileName){
		try{
			Path toPath = fromPath.toAbsolutePath().getParent(); //.
			// -> 파일의 경로를 받고 상대경로로 변경하고 거기서 상위 경로를 추출
			Path copyPath = toPath.resolve("copied"); // 추출한 상위경로에다가 새새로운 경로를 생성하였다.
			if(!Files.exists(copyPath)){
				Files.createDirectories(copyPath);
			}
			Path copiedFilePath = copyPath.resolve(fileName); // 복사 작업을 위한 파일경로 지정
			StandardCopyOption copyOption = StandardCopyOption.REPLACE_EXISTING; // 파일 복사 옵션 설정
			Files.copy(fromPath,copiedFilePath,copyOption); // 파일 복사작업 실행

			System.out.println("*** copied file contents ****");
			readFile(copiedFilePath);
			
			// 파일의 이동과 파일이름의 변경 진행
			Path movedFilePath = Files.move(copiedFilePath,
					copyPath.resolve("moved.txt"),copyOption);
			
			// 파일과 경로 삭제 진행
			Files.delete(movedFilePath);
			Files.delete(copyPath);

		}catch (Exception e){
			e.printStackTrace();
		}
	}
    
}

// 결과
Created file contents
Path = AboutThisBook.txt
첫번째줄
두번째줄
세번째줄
Current Date = Sun Nov 03 16:47:11 KST 2024

*** copied file contents ****
Path = C:\project\pract-1\copied\AboutThisBook.txt
첫번째줄
두번째줄
세번째줄
Current Date = Sun Nov 03 16:47:11 KST 2024

```



- Charset 객체: 저장되는 파일의 문자열 캐릭터 셋 지정

- StandardOpenOption: 파일을 열 때의 조건, Enum 클래스

- write(): 메소드 시그니처는 두 가지



#### StandardOpenOption에 선언된 항목

| **항목**          | **내용**                                                     |
| ----------------- | ------------------------------------------------------------ |
| APPEND            | 쓰기 권한으로 파일을 열고, 기존에 존재하는 데이터가 있으면 가장 끝부붙부터 데이터를 저장 |
| CREATE            | 파일이 존재하지 않으면 새로 생성할 때 사용                   |
| CREATE_NEW        | 파일을 생성하며, 기존 파일이 있으면 실패로 간주              |
| DELETE_ON_CLOSE   | 파일을 닫을 떄 삭제                                          |
| DSYNC             | 파일을 수정하는 모든 작업이 동기적(순차적)으로 파일 저장소에서 처리되도록 함 |
| READ              | 읽기 권한으로 파일을 열 때 사용                              |
| SPARSE            | Sparse file. 파일을 Sparse 할 때 사용 (https://meetup.toast.com/posts/37) |
| SYNC              | 파일의 내용 및 메타 데이터에 대한 모든 업데이트는 순차적으로 파일 저장소에서 처리되도록 할 때 사용 |
| TRUNCATE_EXISTING | 이미 존재하는 파일이 있을 때 쓰기 권한으로 파일을 열고 해당 파일에 있는 모든 내용을 지울 때 사용 |
| WRITE             | 쓰기 권한으로 파일을 열 때 사용                              |



#### StandardCopyOption에 선언된 항목

| **항목**         | **내용**                                                     |
| ---------------- | ------------------------------------------------------------ |
| ATOMIC_MOVE      | · 시스템 처리를 통하여 단일 파일을 이동  · 복사할 때에는 사용 불가 |
| COPY_ATTRIBUTES  | · 새로운 파일에 속성 정보 복사                               |
| REPLACE_EXISTING | · 기존 파일이 있으면 새 파일로 변경                          |





### WatchService 인터페이스

- 파일이 변경되었는지 확인하는 인터페이스 입니다.

```java
// WatcherSample 클래스는 디렉토리를 감시하여 파일의 생성, 삭제, 수정 이벤트를 처리하는 스레드입니다.
public class WatcherSample extends Thread {
	// 감시할 디렉토리의 이름을 저장하는 변수
	String dirName;

	// main 메서드: 프로그램의 시작점
	public static void main(String args[]) throws InterruptedException {
		// 감시할 디렉토리 경로를 지정
		String dirName = "C:\\godofJava";
		// 생성 및 삭제할 파일 이름의 기본 이름
		String fileName = "file.txt";
		// WatcherSample 인스턴스를 생성하여 지정된 디렉토리를 감시
		WatcherSample sample = new WatcherSample(dirName);
		// 스레드를 데몬 스레드로 설정 (메인 스레드가 종료되면 데몬 스레드도 종료됨)
		sample.setDaemon(true);
		// 스레드를 시작
		sample.start();
		// 메인 스레드를 1초 동안 일시 정지
		Thread.sleep(1000);
		// 10번 반복하여 파일을 생성하고 삭제
		for(int loop = 0; loop < 10; loop++) {
			sample.fileWriteDelete(dirName, fileName + loop);
		}
	}

	// 생성자: 감시할 디렉토리 이름을 초기화
	public WatcherSample(String dirName) {
		this.dirName = dirName;
	}

	// 스레드의 실행 메서드
	public void run() {
		// 스레드 시작 메시지 출력
		System.out.println("### Watcher thread is started ### ");
		// 감시할 디렉토리 경로 출력
		System.out.format("Dir=%s\n", dirName);
		// 디렉토리 감시를 시작
		addWatcher();
	}

	// 디렉토리를 감시하고 이벤트를 처리하는 메서드
	public void addWatcher() {
		try {
			// 감시할 디렉토리의 Path 객체 생성
			Path dir = Paths.get(dirName); // 어떤 클래스를 감시할지 지정한다.
			// 기본 파일 시스템의 WatchService 생성
			WatchService watcher = FileSystems.getDefault().newWatchService();
			// 디렉토리를 감시할 이벤트 타입(생성, 삭제, 수정)을 등록
			WatchKey key = dir.register(watcher, ENTRY_CREATE, ENTRY_DELETE, ENTRY_MODIFY);
			// 무한 루프로 이벤트를 지속적으로 감시
			while (true) {
				// 다음 이벤트 키가 발생할 때까지 대기
				key = watcher.take();
				// 발생한 이벤트 리스트를 가져옴
				List<WatchEvent<?>> eventList = key.pollEvents();
				// 각 이벤트에 대해 처리
				for (WatchEvent<?> event : eventList) {
					// 이벤트의 컨텍스트(파일 이름)를 Path로 변환
					Path name = (Path) event.context();
					// 이벤트 종류에 따라 메시지 출력
					if (event.kind() == ENTRY_CREATE)
						System.out.format("%s created%n", name);
					else if (event.kind() == ENTRY_DELETE)
						System.out.format("%s deleted%n", name);
					else if (event.kind() == ENTRY_MODIFY)
						System.out.format("%s modified%n", name);
				}
				// 키를 재설정하여 다음 이벤트를 계속 감시
				key.reset();
			}
		} catch (IOException | InterruptedException e) {
			// 예외 발생 시 스택 트레이스 출력
			e.printStackTrace();
		}
	}

	// 파일을 생성하고 삭제하는 메서드
	private void fileWriteDelete(String dirName, String fileName) {
		// 파일의 전체 경로를 Path 객체로 생성
		Path path = Paths.get(dirName, fileName);
		// 파일에 쓸 내용
		String contents = "Watcher sample";
		// 파일 생성 옵션 설정
		StandardOpenOption openOption = StandardOpenOption.CREATE;
		try {
			// 파일 생성 메시지 출력
			System.out.println("Write " + fileName);
			// 파일에 내용 쓰기 (파일이 없으면 생성)
			Files.write(path, contents.getBytes(), openOption);
			// 파일 삭제
			Files.delete(path);
			// 잠시 대기하여 이벤트가 처리될 시간을 확보
			Thread.sleep(100);
		} catch (IOException | InterruptedException e) {
			// 예외 발생 시 스택 트레이스 출력
			e.printStackTrace();
		}
	}
}
```



