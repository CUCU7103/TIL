# InputStream과 OutputStream



> [!NOTE]
>
> ### Stream이란?
>
> - **실제 입력이나 출력이 표현된 데이터의 흐름을 의미합니다.**
>
> - **자바는 I/O는 기본적으로 InputStream과 OutputStream이라는 abstract 클래스를 통해서 제공되어집니다**



## InputStream

- 바이트 데이터가 들어오는 통로의 역할이 기술된 추상 클래스입니다.

### InputStream의 주요한 메서드

- `public int available()`: 현재 입력 스트림에서 읽을 수 있는 바이트 수를 반환합니다.
- `public void close()`: 현재 inputStream과 관련된 자원을 반납하고 닫습니다.
- `public void mark(int readlimit)`: inputStream의 현재 위치를 표시합니다.
- `public void reset()`: mark() 메서드가 마지막으로 호출된 시점으로 이동합니다.
- `public boolean markSupported()`: inputStream이 mark() 및 reset() 메서드를 지원하는지 확인합니다.
- `public abstract int read()`: inputStream에서 한 바이트의 데이터를 일어와 int 타입으로 반환합니다.
- `public int read(byte[] b)`: inputStream에서 인자로 주어진 바이트 배열의 크기만큼 데이터를 읽어서 b에 저장하고 읽은 바이트 수를 반환합니다.
- `public int read(byte[] b, int off, int len)`: inputStream에서 최대 len만큼 읽어서 byte[] b의 off위치에 저장하고 읽은 바이트 수를 반환합니다.
- `public long skip(long n)`: inputStream에서 인자로 주어진 n바이트의 데이터를 건너뛰고 건너뛴 바이트 수를 반환합니다



**<u>반드시 기억해야 될 메서드는 read() 와 close()이다.</u>**

데이터를 읽을 때에는 read() 메서드를 사용하면 되고 해당 리소스를 닫을 때에는 close() 메서드를 호출해야 됩니다.



close()를 사용하는 이유?

- 닫히지 않는 스트림은 시스템 리소스를 계속 가지고 있어서 gc가 처리하지 못할 수 있기에 메모리 누수가 발생할 수 있습니다.
- 사용한 파일을 닫지 못하면 다른 응용프로그램이 해당 파일에 엑세스 할 수 없는 문제가 발생할 수 있습니다.
- close()를 사용하면 프로그램이 해당 파일에서 데이터를 읽어오는 작업을 마무리 하였음을 보장할 수 있습니다.



### InputStream을 확장한 주요 클래스

- **FileInputStream** : 파일을 읽는데 사용되어집니다

- **FilterInputStream**: 다른 입력 스트림을 포괄하면 단순히 InputStream 클래스가 포괄되어져 있습니다.

- **ObjectInputStream** : ObjectOutputStream으로 저장한 데이터를 읽는 데 사용되어집니다.



## OutputStream

- 바이트 데이터가 나가는 통로의 역할이 기술된 추상 클래스 입니다.



### OutputStream의 주요한 메서드

- `public void close()`: 현재 outputStream의 자원을 반납하고 닫습니다.
- `public void flush()`: 버퍼가 존재하는 경우, 해당 버퍼에 남아있는 데이터를 모두 방출한다.
- `public abstract void write(int b)`: 인자로 주어진 b의 1바이트 만큼만 outputStream에 씁니다.
- `public void write(byte[] b)`: 인자로 주어진 바이트 배열의 크기만큼 outputStream에 씁니다.
- `public void write(byte[] b, int off, int len)`: 인자로 주어진 바이트 배열의 off위치부터 len 크기 만큼의 바이트를 outputStream에 씁니다.



마찬가지로 데이터를 읽고 (write) 난 뒤에는 반드시 close() 해주어야 하는것은 inputstream과 동일하다.



flush()란?

- 파일 출력을 수행할 때, 프로그램은 데이터를 버퍼에 우선 채운 다음 버퍼가 다 차면 파일에 작성하게 되어 있습니다.
- flush() 버퍼가 다 차지 않더라도 파일에 데이터의 작성을 강제합니다. 
  - 데이터가 작성되지 않고 파일 작성이 종료되는 경우를 방지하기 위해 작성하는 메서드입니다.
- 하지만 close()를 호출하면 내부에서 버퍼에 남은 데이터를 flush 한 다음에 스트림을 닫게 되기에 close()를 잘 사용하면 큰 문제는 발생하지 않습니다.













