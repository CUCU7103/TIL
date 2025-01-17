## **TCP/IP 4계층 모델과 HTTP의 역할**

![image](https://github.com/user-attachments/assets/60c283d1-d219-4283-b99c-554c1bca8361)


#### **1. 애플리케이션 계층**

- **역할**: 

  - 애플리케이션 간 데이터 교환을 담당합니다.

- HTTP

  - 애플리케이션 계층에서 동작하며, **클라이언트와 서버 간의 데이터 요청 및 응답을 주고받는 프로토콜**입니다.

  - 브라우저(클라이언트)가 웹 서버에 요청(예: HTTP GET, POST)을 보내고 서버는 HTML, JSON 등의 데이터를 응답합니다.
  - 프로토콜 예시: HTTP(S), FTP, SMTP, DNS 등.

- 연결 구조

  - HTTP 요청은 **TCP**를 사용하여 전송됩니다.
  - 예: 브라우저에서 `www.example.com` 요청 시 HTTP가 데이터를 전송.

------

#### **2. 전송 계층**

- **역할**: 
  - **애플리케이션 계층에서 받은 데이터를 패킷으로 분할하고, 신뢰성 있는 데이터 전송(TCP) 또는 빠른 데이터 전송(UDP)을 보장**합니다.
- TCP
  - HTTP는 기본적으로 **TCP(Transmission Control Protocol)**를 사용합니다.
  - **TCP는 연결 지향 프로토콜로 3-way handshake를 통해 안정적으로 데이터를 전송**합니다.
  - HTTP/1.1의 Keep-Alive를 통해 TCP 연결을 유지하며 여러 요청을 처리할 수 있습니다.
  - 주요 기능
    1. 데이터 분할: 큰 데이터를 작은 패킷으로 분할.
    2. 순서 제어: 패킷이 순서대로 도착하도록 보장.
    3. 신뢰성: 손실된 패킷 재전송.
    4. 흐름 제어: 네트워크 혼잡을 방지.

------

#### **3. 인터넷 계층**

- **역할**: 
  - 전송 계층에서 받은 **패킷을 목적지 주소(IP 주소)로 라우팅하고 전달**합니다.
- IP (Internet Protocol)
  - HTTP 데이터가 담긴 TCP 세그먼트를 **IP 패킷**으로 캡슐화하여 전송합니다.
  - **IPv4** 또는 **IPv6**를 사용하여 송신자와 수신자의 IP 주소를 기반으로 데이터를 전송합니다.
  - 패킷이 여러 네트워크를 거치며 최적의 경로를 통해 목적지에 도달합니다.
- 주요 기능
  - 라우팅: 패킷이 네트워크를 통해 올바른 경로로 전달되도록 관리.
  - 주소 지정: 송신자와 수신자의 IP 주소를 정의.

------

#### **4. 네트워크 인터페이스 계층 (링크 계층)**

- **역할**: 
  - 물리적 네트워크 장치(Ethernet, Wi-Fi 등)에서 데이터를 전송합니다.
- HTTP와의 관계
  - HTTP 데이터는 네트워크 인터페이스 계층에서 실제 하드웨어(라우터, 스위치 등)를 통해 전달됩니다.
  - 전기 신호, 무선 신호 또는 광신호로 변환되어 전송됩니다.
- 주요 기능
  - 데이터 프레임 생성 및 전송.
  - MAC 주소를 사용하여 네트워크 내 장치 간 통신.



---

## **HTTP 요청과 TCP/IP 4계층의 흐름** 

1. **사용자 요청**:
   - 사용자가 브라우저에 `http://www.example.com`을 입력.
   - HTTP 요청 생성(예: GET 요청).
2. **애플리케이션 계층**:
   - HTTP 요청이 애플리케이션 계층에서 생성됩니다.
   - HTTP 요청 메시지가 전송 계층으로 전달됩니다.
3. **전송 계층**:
   - TCP가 HTTP 요청을 **세그먼트**로 분할.
   - TCP 3-way handshake로 서버와 연결 설정.
4. **인터넷 계층**:
   - TCP 세그먼트가 IP 패킷으로 캡슐화.
   - IP 주소를 기반으로 라우팅 및 데이터 전송.
5. **네트워크 인터페이스 계층**:
   - IP 패킷이 데이터 프레임으로 캡슐화되어 실제 네트워크(예: Ethernet, Wi-Fi)로 전송.
6. **서버에서 처리**:
   - 서버가 계층 구조를 거꾸로 처리하여 HTTP 요청을 확인.
   - 요청에 대한 응답 데이터를 생성 후 클라이언트로 다시 전송.

---



> [!TIP]
>
> ### 웹 호스팅
>
> - 웹 사이트나 온라인 서비스를 인터넷 상에서 운영하고 유지하기 위한 기반 인프라를 제공하는 역할
> - 즉 서버를 제공하여 사용자는 서비스에만 집중할 수있도록 합니다.
>
> ### 가상 호스팅
>
> - 하나의 서버에서 여러 웹사이트를 독립적으로 운영할 수 있습니다.
> - 단일 물리적 서버나, IP 주소를 사용하여 여러 개의 도메인이나 웹 사이트를 호스팅할 수 있게 해주는 기술
>   - 하나의 물리적 서버나, ip로  여러 이름의 도메인으로 각각 운용할 수있다.



## HTTP 1.0  vs HTTP 1.1

![image](https://github.com/user-attachments/assets/8579c659-2441-4e9e-988e-17a883b64d0b)




### **지속적인 연결 (Persistent Connections)** 

- **HTTP 1.0:** 	

  - 기본적으로 각 요청마다 새로운 TCP 연결을 설정하고 응답을 받은 후 연결을 닫습니다. 
  - 이를 "비연결성"이라 부르며, 여러 리소스를 요청할 때마다 **연결을 반복적으로 설정해야 하므로 성능에 비효율적**입니다.

- **HTTP 1.1:** 

  - 기본적으로 **지속적인 연결을 지원하여 동일한 TCP 연결을 통해 여러 요청과 응답을 주고받을 수 있습**니다. 

  - 이를 통해 연결 설정과 종료에 소요되는 시간을 줄이고 네트워크 효율성을 높입니다. 	

  - **`Connection: keep-alive` 헤더를 통해 지속적인 연결을 명시적으로 요청할 수도 있습니다**.

    

### **호스트 헤더 (Host Header)**

- **HTTP 1.0:** 

  - **하나의 서버 IP 주소에 하나의 호스트(도메인)만 연결할 수 있습니다**.
  - HTTP/1.0에서는 하나의 서버 IP에서 여러 도메인을 호스팅하기 위해 주로 **IP 기반 가상 호스팅(IP-based Virtual Hosting)** 방식을 사용하였습니다.

  

- **HTTP 1.1:** 

  - **`Host` 헤더를 필수로 포함하여 하나의 서버 IP 주소에 여러 도메인을 호스팅할 수 있게 합니다**. 
  - 가상 호스팅이 가능합니다.
    - 주로 사용되는 방식은 **"이름기반의 가상 호스트 방식**''입니다.

  

###  **파이프라이닝 (Pipelining)**

![image](https://github.com/user-attachments/assets/1692cb44-4ea3-4803-acf2-dc7ac04ead80)

- **HTTP 1.0:** 
  - 한 번에 하나의 요청만 처리할 수 있어, 여러 요청을 순차적으로 보내야 합니다.
- **HTTP 1.1:** 
  - 파이프라이닝을 지원하여 여러 요청을 동시에 전송하고 응답을 순서대로 받을 수 있습니다.
  -  이를 통해 대기 시간을 줄이고 성능을 향상시킵니다.
