## HTTP(Hyper Text Transfer Protocol)

**`HTTP`란 Hyper Text 데이터를 빠르게 전송하기 위한 프로토콜의 일종으로 서버-클라이언트 간의 데이터 전송 방식에 대한 규약**이다. HTML 문서를 비롯하여 이미지, 동영상, XML 문서 등 다양한 형태의 데이터 전송에 사용된다.

HTTP는 애플리케이션 레벨의 프로토콜로 TCP/IP 위에서 작동하며 Request/Response(요청/응답) 구조를 갖는다. Request와 Response는 모두 Start Line, Headers, Body 세 부분으로 구성되어 있다. 또한 HTTP는 일반적으로 80 port를 사용한다.

### HTTP Request와 HTTP Response

#### HTTP Request

기본적인 HTTP Request 구조는 Start Line, Headers, Empty Line(공백 라인; CRLF), Message Body로 이루어져있다.

```
Get /search?q=hi&hl=ko HTTP/1.1     //Start Line
Host: www.google.com                //Headers
                                    //Empty Line(CRLF)
                                    //Message Body
```

##### Start Line

말 그대로 HTTP Request의 첫 번째 라인으로, HTTP Method, Request Target, HTTP Version을 담고 있다.

- HTTP Method
  - Request가 의도하는 action을 정의하는 부분이다.
  - GET, POST, PUT, PATCH, DELETE 등이 있으며, 주로 사용되는 것은 GET과 POST이다.
- Request Target
  - Request가 전송되는 목표 URI (e.g. /user/login)
- HTTP Version
  - 말 그대로 HTTP 버전을 의미하며, HTTP/1.0, HTTP/1.1, HTTP/2, HTTP/3 등이 사용된다.

##### Headers

- Request에 대한 추가 정보(additional information)를 담고 있는 부분
- Key:Value 형식의 데이터를 담고 있다.
  - e.g. Accept: application/json

다음과 같은 Header 정보가 주로 사용된다.

- **Accept**
  - Request를 보낸 클라이언트가 받을 수 있는 Response 파일 형식.
  - \*/\* 은 모든 파일 형식을 지원한다는 의미
  - e.g. Accept: application/json
- **User-Agent**
  - 요청한 클라이언트에 대한 정보로서, S/W의 이름과 버전 등을 포함한다.
  - e.g. User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36
- **Host**
  - 요청한 서버의 Host
  - e.g. Host: localhost
- **Content-Type**
  - Request Message Body의 타입.
  - e.g. Content-Type: text/html; charset=utf-8
- **Content-Length**
  - Request Message Body의 길이
- **Refer**
  - 특정 페이지에서 링크를 클릭하여 요청하였을 때 사용되는 필드로, 링크를 제공한 페이지를 나타낸다.
  - e.g. Refer: https://github.com
- **Accept-Language**
  - 클라이언트가 인식할 수 있는 언어로 우선 순위 지정이 가능하다.
  - e.g. Accept-Language: ko, en-US;q=0.9, en;q=0.8, ko-KR;q=0.7
- **Accept-Encoding**
  - 클라이언트가 인식할 수 있는 인코딩 방식.
  - e.g. Accept-Encoding: gzip, deflate, br

##### Body

HTTP Request가 전송하는 데이터를 담고 있는 부분으로, 전송하는 데이터가 없을 경우 Body 는 비어있는 상태가 된다. 일반적으로 POST 요청일 때 HTML form 데이터가 포함되어 있다.

<br>

#### HTTP Response

HTTP Request와 마찬가지로 HTTP Response도 Start Line, Headers, Body 세 부분으로 구성된다.

```
HTTP/1.1 200 OK                             //Start Line
Content-Type: text/html;charset=UTF-8       //Headers
Content-Length: 2356
                                            //Empty Line(CRLF)
<html>
  <body> ... </body>                        //Message Body
</html>
```

##### Start Line

Response의 Start Line은 HTTP Version, Status Code, Reason Phrase를 담고 있다.

- Status Code
  - Response의 상태를 나타내는 코드로, 각각의 상태에 따라 다른 코드를 갖는다.
  - 정상 응답: 200, 존재하지 않는 페이지에 대한 요청: 404, 서버 오류: 500 등
- Reason Phrase
  - Status Code에 대해 사람이 이해할 수 있도록 적은 짧은 설명

##### Headers

- Response에 대한 추가 정보를 담고 있는 부분
- Key:Value 형식의 데이터를 담고 있다.
  - e.g. Content-Type: application/json

##### Body

HTTP Request와 마찬가지로 Response가 전송하는 데이터를 담고 있는 부분이다. 전송할 데이터가 없는 경우 Body는 비어있는 상태가 된다.

<br>

### HTTP 통신의 특성

HTTP는 비연결성(Connectionless)과 무상태(Stateless) 특성을 가진다.

- **비연결성(Connectionless)**: 클라이언트의 요청에 대해 서버가 적절한 응답을 하고 나면 연결을 끊어버리는 특성을 `비연결성(Connectionless)`이라고 한다.
  <br>

- **무상태(Stateless)**: Connectionless 특성으로 서버와 클라이언트 간의 연결이 끊어짐과 동시에 이전 요청에 대한 결과를 서버에서 유지하지 않는 성질을 `무상태(Stateless)`라고 한다.

이러한 특징 덕분에 멀티 쓰레드 환경에서 웹 애플리케이션이 보다 많은 요청을 처리할 수 있고, 상태를 유지하는 데 필요한 부담을 덜 수 있다. 즉, **비연결성으로 서버의 효율을 높이면서, 무상태 성질로 인해 단순한 구현이 가능**하다.

### HTTP의 통신 방식

HTTP는 버전에 따라 통신 방식이 다르다. HTTP/1.0 ~ HTTP/2까지는 `TCP` 연결을 사용하며, HTTP/3는 `UDP` 연결을 사용한다.

<br>
<hr>

## TCP(Transmission Control Protocol)

TCP는 직역하면 전송을 제어하는 프로토콜이란 의미로, **인터넷 상에서 데이터를 메시지 형태로 보내기 위해 IP와 함께 사용하는 프로토콜**이라고 할 수 있다. 일반적으로 TCP와 IP를 함께 사용하는데, IP는 데이터의 배달을 처리하고 TCP는 패킷을 추적하고 관리한다.

<img src="https://github.com/elegantstar/Network-Summary/blob/main/images/TCP.png?raw=true">

### TCP 특징

- 연결형 서비스로 가상 회선 방식을 제공한다.
  - 가상 회선 방식: 회선 방식과 비슷하게 논리적 경로를 설정한 후에 사용자 데이터를 전송하는 방식
  - 즉, 패킷이 지나갈 길을 먼저 정해놓는 것!
- 3-way handshaking 방식으로 연결을 설정하고, 4-way handshaking 방식으로 연결을 해제한다.
- 흐름 제어(Flow Control) 및 혼잡 제어(Congestion Control)가 가능하다.
- 높은 신뢰성을 보장한다.
- UDP보다 속도가 느리다.
- Full Duplex, Point-To-Point 방식
  - Full Duplex: 두 대의 단말기가 데이터를 동시에 송수신하기 위해 각각 독립된 회선을 사용하는 통신 방식
  - Point-To-Point: 네트워크에서 두 통신 노드 간의 직접적인 연결을 위해 일반적으로 사용되는 데이터 링크 프로토콜. (물리적 중개 장치를 통과하지 않음)

### 정리

TCP는 발신지와 수신지를 연결하여 패킷을 전송하기 위한 논리적 경로를 미리 배정(가상 회선)한다. 3-way handshaking 방식은 목적지와 수신지를 확실히 확인하여 정확한 전송을 보장하기 위한 세션 수립 과정을 의미한다. 그 이유는 TCP는 연결형 서비스로 신뢰성을 보장하기 때문이다. 또한, 데이터의 흐름 제어, 혼잡 제어도 가능하다.

그러나 이러한 기능 때문에 UDP보다 속도가 느리다는 특징을 갖는다. (CPU를 사용하기 때문) 그러므로 TCP는 연속성보다 신뢰성이 더 중요한 데이터의 전송에 적합한 프로토콜이다. (e.g. HTTP 통신, 이메일, 파일 전송 등)

<br>

### Handshake

`TCP`는 연결(Connection Establishment) 과정으로 `3-Way Handshake` 방식을 사용하고, 연결 해제(Connection Termination) 과정으로 `4-Way Handshake` 방식을 사용한다.

- **포트(Port) 상태 정보**

  - CLOSED: 포트가 닫힌 상태
  - LISTEN: 포트가 열린 상태로 연결 요청 대기 중
  - SYN_RCV: SYNC 요청을 받고 상대방 응답 대기 중
  - ESTABLISHED: 포트 연결 상태

- **플래그 정보**
  - TCP Header에는 CONTROL BIT(플래그 비트, 6bit)가 존재하며, 각각의 bit는 "URG-ACK-PSH-RST-SYN-FIN"의 의미를 갖는다. 즉, 특정 위치의 bit를 1로 설정하여 패킷이 갖는 의미를 전달할 수 있다.
  - SYN(Synchronize Sequence Number) / 000010
    - 연결 설정. Sequence Number를 랜덤으로 설정하여 세션을 연결하는 데에 사용하며, 초기에 Sequence Number를 전송한다.
  - ACK(Acknowledgement) / 010000
    - 응답 확인. 패킷을 받았음을 의미.
    - Acknowledgement Number 필드가 유효한지를 나타낸다.
    - 양단 프로세스가 쉬지 않고 데이터를 전송한다고 가정하면, 최초 연결 과정에서 전송되는 첫 번째 세그먼트를 제외한 모든 세그먼트의 ACK 비트는 1로 지정된다.
  - FIN(Finish) / 000001
    - 연결 해제. 세션 연결을 종료시킬 때 사용되며, 더이상 전송할 데이터가 없음을 의미.

#### 3-Way Handshake

<img src="https://github.com/elegantstar/Network-Summary/blob/main/images/tcp%20handshake.png?raw=true">

[이미지 출처](https://velog.io/@k-moovie/TCP-3-way-handshake%EB%A5%BC-%ED%95%99%EC%8A%B5%ED%95%B4-%EB%B3%B4%EC%9E%90)

TCP 통신을 이용하여 데이터를 전송하기 위해 네트워크 연결 설정(Connection Establishment)을 하는 과정. 아래는 Client에서 Server에 먼저 연결 요청을 하는 상황에 대한 예시이다. 물론, Server에서 먼저 연결을 요청하는 것도 가능하다.

- **Step 1. Client -> Server: SYN**
  **Client는 Server와 연결하기 위해 SYN(seq: x) 패킷을 보낸다.**

  - Client가 최초로 데이터를 전송할 때 Sequence Number를 임의의 랜덤 숫자로 지정하고, SYN 플래그 비트를 1로 설정한 세그먼트를 전송한다.
  - PORT 상태
    - Client: `CLOSED` -> `SYN_SENT `
    - Server: `LISTEN`

- **Step 2. Server -> Client: SYN + ACK**
  **Server가 Syn(x)를 받고, 패킷을 받았다는 의미의 ACK(x+1)와 SYN(y) 패킷을 보낸다.**

  - Server가 연결 요청을 수락했으며, Client의 요청 프로세스의 포트도 열어달라는 메시지를 전송(SYN-ACK signal bits set)
  - ACK Number 필드를 Sequence Number + 1로 지정하고, SYN과 ACK 플래그 비트를 1로 설정한 세그먼트를 전송한다.
  - PORT 상태
    - Client: `CLOSED`
    - Server: `SYN_RCV(SYN_RECEIVED)`

- **Step 3. Client -> Server: ACK**
  **Client가 Server의 응답 패킷을 받고, ACK(y+1) 패킷을 Server로 보낸다.**
  - Client 역시 연결 요청을 수락했음을 의미하는 ACK 패킷을 보내 연결 설정을 완료한다.
  - Client에서 전송할 데이터가 있으면 이 단계에서 데이터를 전송할 수 있다.
  - PORT 상태
    - Client: `ESTABLISHED`
    - Server: `SYN_RCV` -> `ESTABLISHED`

**Full-Duplex 통신 구성**

- Step 1, 2에서는 Client -> Server 방향에 대한 연결 파라미터(Sequence Number)를 설정하고 이를 승인한다.
- Step 2, 3에서는 Server -> Client 방향에 대한 연결 파라미터(Sequence Number)를 설정하고 이를 승인한다.

이런 과정을 통해서 Full-Duplex 통신이 구축된다.

<br>

#### 4-Way Handshake

<img src="https://github.com/elegantstar/Network-Summary/blob/main/images/4-way-handshaking.png?raw=true">

TCP 통신의 연결을 해제(Connection Termination)하기 위한 과정. 여기서는 FIN 플래그를 이용한다.

- **Step 1. Client -> Server: FIN(+ACK)**

  - Server와 Client가 연결된 상태에서 Client가 close()를 호출하여 접속을 끊고자 한다.
  - Client는 Server에게 연결을 종료한다는 `FIN` 플래그를 보내고 `FIN_WAIT1` 상태에 들어간다. (이때 FIN 패킷에는 실질적으로 ACK도 포함되어 있다)

- **Step 2. Server -> Client: ACK**

  - Server는 FIN을 받고, 이를 확인했다는 `ACK`를 클라이언트에게 보내며 자신의 통신이 끝날 때까지 기다린다. (이 상태를 TIME_WAIT 상태라고 한다.)
    - Server는 ACK Number 필드를 (Sequence Number + 1)로 지정하고, ACK 플래그 비트를 1로 설정한 세그먼트를 전송한다.
  - Server는 Client에게 응답을 보내고 `CLOSE_WAIT` 상태에 들어간다. 아직 전송할 데이터가 남아있다면 마저 전송을 마친 후에 close()를 호출한다.
  - Client에서는 Server로부터 ACK를 받은 후, 서버가 남은 데이터의 처리를 끝내고 FIN 패킷을 보낼 때까지 기다린다. (`FIN_WAIT_2`)

- **Step 3. Server -> Client: FIN**

  - 데이터를 모두 보낸 서버는 연결 종료에 합의한다는 의미로 `FIN` 패킷을 클라이언트에게 전송하고, 승인 번호를 보내줄 때까지 기다리는 `LAST_ACK` 상태로 들어간다.

- **Step 4. Client -> Server: ACK**

  - Client는 FIN을 받고, 확인했다는 의미의 `ACK`을 Server에 전송한다.
  - 아직 Server로부터 받지 못한 데이터가 있을 수 있으므로 `TIME_WAIT`을 통해 기다린다. (실질적 종료 과정 `CLOSED`에 들어가게 된다.)

    - `TIME_WAIT` 상태는 의도치 않은 에러로 인해 연결이 데드락으로 빠지는 것을 방지.
    - 만약 에러로 인해 종료가 지연되다가 타임이 초과되면 `CLOSED` 상태로 들어간다.

  - Server는 ACK를 받은 이후 소켓을 닫는다. (CLOSED)
  - TIME_WAIT 시간이 지나면 Client도 소켓을 닫는다. (CLOSED)

<br>

#### TCP Flow Control

TCP의 `흐름 제어(Flow Control)`은 송신 속도와 수신 속도의 균형을 맞추는 것으로, 수신 측의 Overflow를 방지하기 위해 송신 측의 Flow를 제어하는 기법을 말한다.

- STOP AND WAIT: 송신 측에서는 하나의 패킷을 보내고 수신 측으로부터 ACK가 올 때까지 메시지를 전송하지 않고 기다리는 방식(비효율적)
- SLIDING WINDOW: 수신 측에서 설정한 크기 만큼(window size) 패킷을 계속해서 전송하고, 수신 측으로부터 응답(ACK)이 오면 윈도우를 sliding 하여 다음 패킷을 전송하는 방식. 송/수신 window size는 수신 버퍼의 상태에 의해 결정된다.

`SLIDING WINDOW` 알고리즘은 크게 두 가지가 있다.

- GBN(Go-Back-N) 알고리즘: 데이터 패킷을 window size만큼 전송하되, **손실된 패킷이 존재할 경우 손실 패킷부터 재전송**하는 알고리즘.
  - 송신측 window size와 수신측 window 사이즈가 같지 않다.
  - 수신측에서는 순서대로 패킷을 수신하며, 현재 순서가 아닌 패킷을 수신할 경우 손실된 패킷 번호를 ACK로 전송한다.
  - window size만큼 패킷을 수신하면, 그 다음 패킷 번호를 ACK로 전송하여 송신 측에서 다음 데이터 전송하도록 요청한다.
- SR(Selective Repeat) 알고리즘: 데이터 패킷을 window size만큼 전송하되, **패킷 손실이 발생할 경우 손실된 패킷만 재전송**하는 알고리즘.
  - 송신측 window size와 수신측 window size가 항상 같다.
  - 수신측에서는 패킷 순서가 일치하지 않더라도 패킷 저장소에 저장한다.
  - 수신측에서 확인한 패킷의 순서 번호를 ACK로 전송하여 송신측에 수신한 패킷 정보를 알린다.
  - 송신측에서는 ACK Time out이 발생하면 수신 확인되지 않은 패킷을 누락된 패킷으로 간주하고 재전송한다.

현대 TCP 통신에서는 Go-Back-N 알고리즘과 Select Repeat 알고리즘을 혼합한 방식을 사용한다고 한다.

<br>

#### TCP Congestion Control

<img src="https://raw.githubusercontent.com/elegantstar/Network-Summary/main/images/Congestion.bmp">

TCP의 `혼잡 제어(Congestion Control)`은 네트워크가 혼잡할 때 라우터의 Overflow로 인해 패킷이 손실되는 상황을 방지하기 위해 데이터 송신 속도를 제어하는 기법을 말한다. 즉, 송신측의 데이터 전송 속도와 네트워크의 데이터 처리 속도 간 균형을 맞추는 것으로 볼 수 있다.

- AIMD(Additive Increase / Multiplicative Decrease)

  - 초기 window size를 1로 설정하여 패킷을 전송하고, 문제 없이 잘 도착하면 window size를 1씩 증가시키며 전송하는 방법
  - 패킷 전송에 실패하거나 Timeout(일정 시간 동안 ACK를 받지 못함)이 발생하면 window size를 절반으로 줄인다.
  - 여러 호스트가 한 네트워크를 공유하고 있으면 나중에 진입하는 쪽이 불리하지만, 시간이 지나면 평형 상태로 수렴하는 특징이 있어 공평한 방식이다.
  - 문제는 초기에 높은 네트워크 대역폭을 사용할 수 없기 때문에 시간이 오래 걸리고, 네트워크가 혼잡해지는 상황을 미리 감지할 수 없다. 즉, 네트워크가 혼잡해지고 나서야 대역폭을 줄이는 방식이다.

- Slow Start

  - 초기 window size를 1로 설정하고, 패킷이 문제 없이 잘 도착하면 window size를 2배씩 늘려가며 전송하는 방법으로, 초기 전송 속도를 증가시키는 데에 오랜 시간이 걸리는 AIMD 방식의 단점을 보완하였다.
  - 전송속도는 AIMD에 반해 지수 함수 꼴로 증가한다. 대신에 혼잡 현상이 발생하면 window size를 1로 떨어뜨리게 된다.
  - 처음에는 네트워크의 수용량을 예상할 수 있는 정보가 없지만, 한번 혼잡 현상이 발생하고 나면 네트워크의 수용량을 어느 정도 예상할 수 있다.
  - 그러므로 혼잡 현상이 발생하였던 window size의 절반까지는 이전처럼 지수 함수 꼴로 창 크기를 증가시키고 그 이후부터는 완만하게 1씩 증가시킨다.

- Fast Retransmit (빠른 재전송)

  - 빠른 재전송은 TCP의 혼잡 조절에 추가된 정책이다.
  - 패킷을 받는 쪽에서 먼저 도착해야할 패킷이 도착하지 않고 다음 패킷이 도착한 경우에도 ACK 패킷을 보내게 된다.
  - 단, 순서대로 잘 도착한 마지막 패킷의 다음 패킷의 순번을 ACK 패킷에 실어서 보내게 되므로, 중간에 하나가 손실되게 되면 송신 측에서는 순번이 중복된 ACK 패킷을 받게 된다. 이것을 감지하는 순간 문제가 되는 순번의 패킷을 재전송 해줄 수 있다.
  - 중복된 순번의 패킷을 3개 받으면 재전송을 하게 된다. 약간 혼잡한 상황이 일어난 것이므로 혼잡을 감지하고 window size를 줄이게 된다.

- Fast Recovery (빠른 회복)
  - 혼잡한 상태가 되면 window size를 1로 줄이지 않고 반으로 줄이고 선형증가시키는 방법이다. 이 정책까지 적용하면 혼잡 상황을 한번 겪고 나서부터는 순수한 AIMD 방식으로 동작하게 된다.

<br>
<hr>

## UDP(User Datagram Protocol)

UDP는 데이터를 데이터그램 단위로 처리하는 프로토콜을 의미하며, TCP와는 달리 비연결형 프로토콜이다. 여기서 `Datagram`이란 발신지와 수신지 컴퓨터 사이에서 데이터 교환 없이 독립적으로 전송되는 데이터를 말한다.

<img src="https://raw.githubusercontent.com/elegantstar/Network-Summary/main/images/UDP.bmp">

### UDP 특징

- 비연결형 프로토콜(Connectionless Protocol)

  - 논리적인 연결 설정 과정이 없기 때문에 데이터그램 전송 시마다 주소 정보를 설정하여 전송한다.
  - 데이터의 **순차적 전송을 보장하지 않는다.**
  - **데이터그램 기반의 전송 방식**을 사용한다. 즉, 데이터를 정해진 크기로 전송하는 방식을 사용한다.

- 신뢰할 수 없는 프로토콜(Unreliable Protocol)

  - 신뢰성 있는 TCP와는 달리 흐름 제어(Flow Control), 오류 제어(Error Control), 혼잡 제어(Congestion Control) 등의 기능을 수행하지 않는다.
  - 실질적으로 **IP 기반의 포트 정보를 이용하여 상위 송수신 어플리케이션을 식별**해 주는 역할 정도만 수행한다.

- 단순하고 가벼운 프로토콜로 **전송 속도가 빠르다.**
- 비신뢰적 특성으로 대량 데이터의 송수신은 부적절하며, 주로 **한 번의 패킷 송수신으로 완료되는 서비스에 많이 사용** (e.g. DNS, 동영상 스트리밍 등)

<br>
<hr>

## TCP/IP Model

`TCP/IP(Transfer Control Protocol/Internet Protocol)`는 미국 국방부(DoD)에서 정의한 네트워크 통신 표준 모델이다. OSI 모형이 아니라 TCP/IP가 산업 표준인 이유는 TCP/IP가 OSI보다 더 먼저 사용되었기 때문이다. TCP/IP와 OSI는 네트워크 통신 모델의 표준이라는 공통점이 있지만, TCP/IP는 OSI의 상위 레이어와 하위 레이어를 통합해서 더 간단하게 표현했다는 차이가 있다.

<img src="https://velog.velcdn.com/images%2Fjwkim%2Fpost%2F1a15f96e-6644-4527-a97d-50aa7be8b1ac%2Fimage.png">

위 이미지에서 중앙의 TCP/IP 모델은 최초로 정의되었던 TCP/IP 모델로서, 근래에는 우측 모델과 같이 업데이트 되었다. 초기 TCP/IP 모델은 OSI 7계층이 간소화 된 형태로 4개의 계층을 갖는다.

- `Application` + `Presentation` + `Session` -> `Application`
- `Network` -> `Internet`
- `Data-Link` + `Physical` -> `Link`

그러나 인터넷 개발 이후 해당 모델이 꾸준히 갱신되며 하위 레이어가 세분화 되었고, 우측의 5계층 모델인 `TCP/IP Updated Model`이 탄생하게 되었다. TCP/IP Updated 5계층 모델은 기존의 `Link`를 다시 두 레이어로 세분화 되었는데 이는 OSI 7계층의 1, 2 계층과 동일하다. 또한, `Internet` 레이어의 명칭을 OSI 7계층과 같은 `Network`로 변경함으로써 최종적으로 OSI 7계층에서 5~7 레이어를 하나의 레이어로 간소화한 형태가 되었다. `TCP/IP Updated` 모델은 현재 전 세계 표준으로 적용되고 있다.

### TCP/IP 5 Layers

| Layer Number | Layer Name  | Addressing  | Protocol Data Unit |            Protocol            |
| :----------: | :---------: | :---------: | :----------------: | :----------------------------: |
|      L5      | Application |      -      |      Message       |   HTTP, SSH, FTP, SMTP, POP    |
|      L4      |  Transport  | Port Number | Segment, Datagram  |            TCP, UDP            |
|      L3      |   Network   | IP Address  |    IP Datagram     |               IP               |
|      L2      |  Data-Link  | MAC Address |       Frame        | Ethernet, IEEE 802, Wi-Fi, LTE |
|      L1      |  Physical   |      -      |    Bit, Signal     |               -                |

<br>

#### L5 | Application Layer

- 프로그램 구현체와 사용자 인터페이스를 의미한다.
- OS에서 제공하는 L4 API를 사용하여 통신 프로그램이 구현된다.
- URL이 주어지면 DNS(Domain Name System)를 통해 IP 주소로 변환된다.
- Application Layer에서 만들어진 Message는 Transport Layer로 전달되면서 여러 개의 Packet으로 분할된다.
- HTTP, FTP, SMTP, POP 등 다양한 프로토콜이 활용된다.

<br>

#### L4 | Transport Layer

**Process-To-Process Delivery**

- Port 번호를 사용하여 최종 도착지인 프로세스까지 데이터를 전달한다.
- OS 커널에 S/W적으로 구현되어 있다.
- 패킷 전송 프로토콜로 TCP와 UDP가 있다.

|           | TCP                                          | UDP                                                 |
| :-------: | -------------------------------------------- | --------------------------------------------------- |
|   의미    | Transmission Control Protocol                | User Datagram Protocol                              |
| 연결 방식 | Connection-Oriented Byte Stream              | Connection-less Message Stream                      |
|   장점    | Reliable, Flow & Congestion Control, Ordered | Faster                                              |
|   단점    | Slower                                       | Unreliable, No Control, Unordered                   |
| Data Unit | Segment                                      | Datagram                                            |
|   활용    | 대부분의 애플리케이션                        | 게임, 스트리밍 등 실시간 속도가 중요한 애플리케이션 |

**Port Number**

- 컴퓨터는 `2-byte` 길이의 port 번호를 가진다.
- Well-known port (1~1000)는 OS와 주요 프로토콜에서 사용하기 때문에 사용자 애플리케이션은 그 외의 port 번호를 사용한다.

<br>

#### L3 | Network Layer

**Host-To-Host Delivery**

- Routing & Forwarding을 수행하여 목적지 IP 주소까지 패킷을 전달한다.
  - Routing: 출발지에서 목적지까지의 경로를 결정하는 것
  - Forwarding: 라우터의 입력 포트에서 출력 포트로 패킷을 이동시키는 것(라우팅 테이블이 존재함)
- OS 커널에 S/W적으로 구현되어 있다.
- 패킷이 도착하면, IP 주소의 광대역에 따라 Routing Table에 지정된 경로로 패킷을 Forwarding한다.

**IP Address**

- Internet Protocol Address
- Host의 논리적 주소로, 전 세계의 네트워크 상에서 유일하다.
- `IPv4`는 `4-byte`, `IPv6`는 `8-byte`의 주소를 갖는다.

<br>

#### L2 | Data-Link Layer

**Hop-To-Hop Delivery**

- Routing & Forwarding을 수행하여 목적지 MAC 주소까지 프레임(Frame)을 전달한다.
- Reliable delivery b/w adjacent nodes. 직접 연결된 서로 다른 2개의 네트워킹 장비 간의 데이터 전송이 이루어 진다.
- 상위 계층(Network Layer)에서 지정한 네트워크 장비와 통신을 담당한다.
- Ethernet Card(LAN Card)에 구현되어 있다.
- Frame: Ethernet Header와 Trailer가 추가된 데이터로 Data-Link Layer의 Data Unit.
  - Ethernet Header: 목적지 MAC Address(6-bytes) + 출발지 MAC Address(6-bytes) + Ethernet 유형
    - Ethernet 유형: 이터넷으로 전성되는 상위 계층 프로토콜의 종류를 식별하는 번호
  - Trailer: 데이터 뒤에 붙는 요소로써 FCS(Frame Check Sequence)라고도 한다. 데이터 전송 중 오류가 발생하지는 않았는지 확인하는 데에 사용된다.

**MAC Address**

- Media Access Control Address
- Ethernet Card의 물리적 주소로 로컬 네트워크 안에서만 유일하다.
- Gateway(Router)는 Ethernet Card를 2개 가지고 있어서 LAN과 WAN을 연결한다.
- `48-bit` 주소를 갖는다.
  - 앞의 24-bit는 LAN Card 제조사 번호이며, 뒤의 24-bit는 제조사에서 부여한 일련번호이다.
    - 실제로 제조사에서 부여하는 것은 아니고, 제조사가 IEEE로부터 MAC Address를 구입한다.
    - MAC Address 역시 이상적으로는 중복이 없어야 하지만, 실질적으로 3byte만으로 중복을 피하기는 어렵다. 그러나 MAC Address는 Local Network 상에서만 고유하면 문제가 없고, Local Network 상에서 동일한 MAC Address가 사용될 확률은 극도로 낮다.

**ARP**

- Address Resolution Protocol
- LAN 내부의 ARP Table을 참조하여 IP 주소를 MAC 주소로 변환한다.

<br>

#### L1 | Pysical Layer

- Encoding: 0과 1의 나열을 아날로그 신호로 변환해서 전송한다.
- Decoding: 아날로그 신호를 받으면 0과 1로 해석한다.
- 물리적, 기계적, 전기적 기능으로 H/W로 구현되어 있다. (PHY Chip)

<br>
<hr>

## OSI Model (Open Systems Interconnection Reference Model)

다양한 컴퓨터 시스템이 **표준 프로토콜**을 사용하여 통신할 수 있도록 국제 표준화 기구(ISO)에서 만든 개념 모델이다. 네트워크 프로토콜 디자인과 통신을 **7계층**으로 나누어 정의하였다.

OSI 표준 모형은 7계층으로 이루어져 있으며, 계층 별로 **역할을 분리**하여 각 계층이 **독립적으로 기능을 수행**하고, **계층 간 통신**을 통해 전체 통신 프로세스를 가능케 한다. 이를 통해 **네트워크 통신 과정을 단계 별로 파악**할 수 있으며, 문제가 발생했을 때에도 **문제 계층을 빠르게 파악**할 수 있다. 또한, 네트워크 통신에 필요한 **HW와 SW를 표준화**함으로써 서비스나 기기 간 호환을 가능하게 한다.

그러나, 현대 산업 표준 네트워크 통신 모델은 `TCP/IP 모델`이 차지하게 되었다. OSI 7 Layer는 장비 개발과 통신 자체를 어떻게 표준으로 잡을지 사용되는 반면에 실 질적인 통신 자체는 TCP/IP 프로토콜을 사용하는 것이다.

### OSI 7 Layers

TCP/IP 5 Layers에 존재하지 않는 두 계층인 Session Layer와 Presentation Layer에 대해서만 다룬다.

#### OSI L5 | Session Layer

- Data unit은 message이며, protocol로는 socket이 있다.
- 응용 프로그램 사이에서 Session이라고 하는 연결을 확립하고 유지하며 동기화하는 기능을 제공한다.
- Session Layer는 Presentation Layer로부터 받은 데이터를 효율적인 세션 관리를 위해 짧은 데이터 단위로 나누어 Transport Layer로 내려보낸다.

#### OSI L6 | Presentation Layer

- Data unit은 message이며, protocol로는 JEPG, MPEG 등이 있다.
- 송/수신자가 모두 이해할 수 있도록 데이터 표현 방식을 변환하는 기능을 담당한다.
- 송신측 Presentation Layer는 Application Layer로부터 받은 데이터의 보안과 효율적인 전송을 위해 암호화 및 압축 작업을 수행하여 Session Layer로 내려보낸다.

<br>
<hr>

## HTTPS(Hyper Text Transfer Protocol Secure)

**`HTTPS`는 HTTP에 데이터 암호화가 추가된 프로토콜**로서, HTTPS의 S는 Secure를 의미한다. HTTPS는 80번 포트를 사용하던 HTTP와 다르게 443번 포트를 사용하며, 네트워크 상에서 제 3자가 데이터를 임의로 확인할 수 없도록 암호화를 지원한다는 것이 특징이다.

### HTTP와 HTTPS

HTTP는 암호화를 제공하지 않기 때문에 보안에 취약하지만, HTTPS는 안전하게 데이터를 주고 받을 수 있다. 이전에는 암/복호화 과정 때문에 HTTPS가 HTTP에 느리다는 것을 단점으로 꼽았지만, 오늘날에는 그 차이를 느끼지 못 할 정도이다. HTTPS는 인증서를 발급하고 유지하기 위한 추가 비용이 발생한다는 점은 단점이 될 수 있겠다.

일반적으로 개인정보와 같은 민감한 데이터를 주고 받는 경우에는 HTTPS를 사용하고, 그렇지 않은 경우에는 HTTP를 사용한다. 모든 사이트에 HTTPS를 사용하지 않는 이유는 모든 데이터에 암/복호화를 하게 되면 리소스 낭비가 심하기 때문이다.

> 추가적으로 2016년 3월 22일을 시점으로 정보통신망법이 개정되어 회원가입, 로그인 등 회원에 대한 개인 정보를 취급하는 모든 웹사이트는 SSL 인증서를 구축해야 한다는 일명 'HTTPS 의무화'가 진행되었다. 따라서 이에 해당하는 경우에는 HTTPS를 필수로 사용해야 한다.

<br>

### HTTPS와 TLS(SSL)

HTTPS는 TLS(Transport Layer Security) 프로토콜 위에서 동작하는 프로토콜이라고 할 수 있다. 기존의 HTTP 프로토콜에 대한 Socket이 SSL(Secure Socket Layer) 또는 TLS라는 새로운 프로토콜로 대체된 것으로 이해할 수 있다. 기존 HTTP가 TCP와 직접 통신을 하였다면, HTTPS는 TLS와 통신하고, TLS가 TCP와 통신하게 된다.

HTTPS의 TLS는 대칭키 암호화 방식과 공개키 암호화 방식을 혼합한 하이브리드 암호 시스템을 사용한다. 대칭키를 공개키 암호화 방식으로 교환하고, 실제 데이터의 암호화에는 교환한 대칭키를 이용하여 암호화하는 방식이다.

> 여기서 SSL과 TLS은 사실상 같은 의미로 사용되는데, TLS는 SSL의 업그레이드 버전이며 현재 표준이다. Netscape 사에 의해 SSL이 발명되었고, 이것이 대중적으로 사용되다가 표준화 기구인 IETF가 관리하게 되면서 TLS라는 이름으로 변경되었다. 따라서 공식적으로는 TLS라는 이름을 사용하는 것이 적합하다. 그러나 여전히 SSL이라는 이름이 널리 사용되고 있다.

<br>

### HTTPS 동작 방식

#### 인증서 발급

TLS 프로토콜은 인증서라는 것을 사용한다. 이 인증서는 서버가 신뢰할 수 있는 서버인지 검증하는 수단으로 아래와 같은 정보를 포함하고 있다.

1. 서비스 정보(인증서 발급 CA, 서비스 도메인 등)
2. 서버 측 공개키(공개키, 공개키 암호화 방법 등)
3. 지문, 디지털 서명 등

이러한 인증서는 신뢰할 수 있는 발급 기관(CA)으로부터 발급 받은 경우 신뢰할 수 있는 사이트로 검증되며, 인증서는 있지만 CA로부터 발급 받지 못한 경우 신뢰할 수 없는 사이트로 표기된다. 즉, CA로부터 인증받지 못한 인증서라고 하더라도 TLS 통신은 가능하다.

**인증서 발급 과정**

1. Server가 CA에 인증서 발급을 요청한다. (도메인, 공개키 등을 제출)
2. CA는 해당 Server에 대한 자격 검증을 한다.
3. CA는 Server의 공개키를 SHA-256 알고리즘 등으로 해싱하여 인증서에 '지문(Finger Print)'으로 등록한다.
4. CA는 3에서 등록한 지문을 CA의 private key로 암호화 하여 인증서에 '서명(Digital Signing)'으로 등록한다.
5. CA가 Server에 인증서를 발급한다.

이때 지문을 암호화하는 과정에서 사용한 CA의 private key는 절대 유출되서는 안되는 정보이다.

<br>

#### TLS Handshaking

<img src="https://github.com/elegantstar/Network-Summary/blob/main/images/TLS%20Handshaking.png?raw=true">

##### Step 1. Client Hello (Client)

가장 첫 번째 스텝으로 클라이언트에서 서버에 `Client Hello`라는 패킷을 전송한다. 패킷 내부에는 필요한 데이터가 포함되어 있는데 아래와 같은 데이터가 포함된다.

- TLS version: TLS 프로토콜의 버전
- Cipher Suite List: Client가 지원하는 암호화 방식
- Client Random Data: 클라이언트에서 생성한 난수로 이후 대칭키를 생성할 때 사용한다.
- Session ID: Handshake를 한 번 진행하면 발행하는 SessionID
  - 매 연결마다 Full Handshake를 하는 것은 비효율적이기 때문에 최초 한 번의 Handshake에 대해서만 Full Handshake를 하고, 이후부터는 SessionID를 확인하여 절차를 간소화 한다.
- SNI(Server Name Indication): Client가 접속하고자 하는 서버의 도메인.
  - IP 주소 하나에 여러 개의 도메인이 연결되는 경우가 많기 때문에 도메인을 명시함으로써 서버에서는 어떤 도메인의 인증서가 필요한지 파악할 수 있다.

##### Step 2. Server Hello (Server)

서버에 클라이언트에 `Server Hello`라는 패킷을 전송한다. 패킷 내부에는 필요한 데이터가 포함되어 있는데 아래와 같은 데이터가 포함된다.

- TLS version
- Selected Suite: Client가 보낸 암호화 방식 중 서버가 지원 가능한 암호화 방식 중에서 가장 안전한 것을 선택
- Server Random Data: 서버에서 생성한 난수로, 클라이언트에서 생성한 난수화 조합하여 대칭키를 생성한다.
- Session ID: Client Hello에서 SessionID가 0으로 왔다면 새로운 SessionID를 생성하여 전송하고, SessionID가 0이 아닌 경우 SessionID가 유효한지 확인.
- SNI: Server에서는 SNI 항목을 비워서(empty) 전송한다.

##### Step 3. Server Certificate (Server) & Step 4. Server Hello Done (Server)

서버의 인증서를 클라이언트에게 전송한다. 클라이언트는 브라우저가 가지고 있는 CA의 공개키를 이용하여 인증서를 복호화하고, 복호화에 성공했다면 인증된 서버임이 확인 된 것이다.

클라이언트는 Client Random Number와 Server Random Number를 조합하여 `pre-master secret`이라는 키를 생성한다. 인증서를 복호화하면 서버의 공개키를 획득할 수 있는데, 클라이언트는 이렇게 얻은 서버의 공개키를 이용하여 pre-master secret을 암호화한다.

Server Hello Done은 서버가 클라이언트에게 전송할 메시지를 모두 전송했다는 메시지이다.

##### Step 5. Client Key Exchange (Client)

클라이언트는 만들어 둔 pre-master secret을 서버의 공개키로 암호화하여 서버에 전송한다.

##### Key Generation (Server & Client)

통신 과정은 아니고, 클라이언트와 서버가 서로 암/복호화에 사용할 대칭키를 성공적으로 공유한 시점.

##### Step 6. Cipher Spec Exchange (Server & Client)

이 시점부터는 모든 메시지를 협상된 암호화 알고리즘과 키를 이용하여 암호화하겠다는 메시지.

##### Step 7. Finished (Server & Client)

클라이언트와 서버가 TLS Handshake를 성공적으로 마치고 종료.

<br>
<hr>

## 쿠키와 세션

HTTP의 특성 중 Connectionless와 Stateless 특성은 서버의 효율을 높이고, 상태를 유지하기 위한 부담을 줄일 수 있도록 한다. 그러나 애석하게도 서버에서 클라이언트의 상태를 유지해야 하는 경우가 있는데, 대표적인 예가 바로 로그인이다. 쿠키와 세션은 HTTP 통신의 특성이자 제약을 극복하기 위해 도입된 기술이다.

### Cookie

- Cookie는 웹 브라우저에서 보관하는 key-value 형식의 데이터로, 클라이언트의 정보를 저장해 두었다가 서버에게 전달하는 방식으로 이전 요청 정보를 유지한다.
- 웹 브라우저는 서버로부터 발급 받은 Cookie를 보관하고 있다가, 이후의 요청 때마다 Http Header에 Cookie를 실어서 전송한다.
- 서버는 쿠기 정보를 이용하여 클라이언트와 연결을 유지하지 않고도 클라이언트의 인증 상태를 유지할 수 있다.
- 그러나 로그인 정보와 같이 보안이 중요한 데이터에 대해 Cookie를 사용하는 것은 치명적인 문제가 있다.
  - Cookie 데이터 변조 가능성 존재한다. 웹 브라우저의 개발자 도구 등의 기능을 통해서 Cookie 값을 임의로 변경할 수 있음. (Cookie에 저장된 데이터가 예측 가능한 값이라면..?)
  - 쿠키를 탈취당할 경우 중요한 정보가 유출될 수 있음. (Cookie에 패스워드 같은 정보를 저장하고 있을 경우)

### Session

- Session은 기존 쿠키의 단점을 보완하면서 클라이언트의 인증 상태를 유지하는 기술로서, 쿠키는 클라이언트의 브라우저에 데이터를 저장했다면 Session은 서버에 데이터를 저장한다.
- 즉, 쿠키를 사용하면서 데이터를 서버에서 유지하기 위한 방법.
- Server는 내부에 세션 저장소를 두고 클라이언트로부터 요청이 오면 Session을 생성하여 저장소에 저장한다.
- 세션 저장소는 SessionId와 Value를 저장할 수 있고, 클라이언트에게는 SessionId만 전송한다.
- 이 SessionId는 예측이 불가능한, 복잡한 랜덤 값을 사용한다.
- 클라이언트는 서버로부터 받은 SessionId를 브라우저의 쿠키로 저장하고 있다가, 이후의 요청 시마다 SessionId를 Http Header에 실어서 전송한다. (SessionId가 곧 쿠키 값)

#### Session 사용 시 장점

- 쿠키 변조 문제 해결.
  - 예측할 수 없는 값이기 때문.
- 탈취 시 정보 유출 리스크 감소.
  - 대단한 정보를 저장하고 있지 않기 때문에 가져가도 쿠키 자체로 할 수 있는 게 없음.
- 탈취 시 악용 가능성이 적음.
  - 세션 만료 시간을 짧게 설정하여 탈취한 쿠키를 장시간 보관해도 사용하지 못함.
  - 해킹으로 의심되는 경우 서버에서 세션을 삭제하여 악용 방지.

#### Session 사용 시 주의점

- Session은 기본적으로 메모리에 저장된다.
- 즉, 너무 많은 정보를 저장하면 메모리 사용량이 매우 커지기 때문에 꼭 필요한 최소한의 정보만 저장하도록 설계해야 한다.
- 또한, 만료 시간을 상황에 맞게 잘 설정하여 장시간 사용하지 않는 세션은 삭제하도록 설계해야 한다. (일반적으로 30분을 기본 값으로 한다.)

### 쿠키와 세션 비교

|             | 쿠키                                                               | 세션                                                          |
| :---------: | ------------------------------------------------------------------ | ------------------------------------------------------------- |
|  저장 위치  | 클라이언트 메모리 또는 디스크                                      | 서버 메모리                                                   |
|  만료 시점  | expires 속성으로 정의                                              | 클라이언트의 로그아웃 또는 설정 시간 동안 반응이 없을 시 만료 |
| 서버 리소스 | 서버 리소스 없음                                                   | 세션 생성 시마다 서버의 메모리에 적재                         |
|  용량 제한  | 도메인 당 20개, 쿠키 하나 당 4KB 제한(클라이언트 메모리 공격 방지) | 개수나 용량 제한 없음                                         |

<br>
<hr>

## DNS(Domain Name System)

일반적으로 웹 사이트에 접속할 때 외우기 어려운 IP 주소 대신 도메인 네임을 사용하는데, 실제 네트워크 통신 과정에서는 IP 주소가 필요하다. 따라서 **도메인 네임을 IP 주소로 변경하고, 해당 IP 주소로 접속하는 과정이 수반되는데 이러한 시스템을 `DNS(Domain Name System)`** 이라고 한다. 그리고 이렇게 도메인과 IP 주소 정보를 가지고 있는 서버를 DNS 서버라고 한다. DNS 서버는 계층형 구조를 가지고 있으며, 분산 데이터베이스 구조를 가진다.

### DNS 통신 과정

DNS 서버 통신은 DNS Protocol을 사용하며, 해당 프로토콜은 UDP 방식이다. 또한 기본적으로 53번 포트를 사용한다. 아래 내용은 Client에서 DNS 서버에 특정 도메인에 대한 IP 주소를 요청하여 응답 받기까지의 과정을 간소화한 것이다.

<img src="https://github.com/elegantstar/Network-Summary/blob/main/images/DNSLogic.png?raw=true">

1. 클라이언트는 `Local DNS Server`(기지국 DNS 서버 = 통신사)에 원하는 도메인에 대한 IP 주소를 요청한다.
2. Local DNS Server에 해당 도메인에 대한 IP 주소 정보가 존재하면 찾아서 응답한다.
3. 만약 Local DNS Server에 IP 주소 정보가 존재하지 않으면, Local DNS Server는 `Root DNS Server`에게 IP 주소를 요청한다.
4. `Root DNS Server`는 IP 주소를 가지고 있지 않으므로 `TLD(Top-Level Domain) Server`의 주소를 응답한다.
5. Local DNS Server는 Root DNS Server로부터 받은 TLD Server에 다시 IP 주소를 요청한다.
6. TLD Server에서는 2차 도메인에 대한 DNS 서버 주소를 찾아서 응답한다.
7. Local DNS Server는 이런 과정을 재귀적으로 반복하며 최종적으로 Client에서 요청한 도메인에 대한 IP 주소를 얻는다.
8. Local DNS Server는 해당 도메인에 대한 IP 주소를 저장(Cache)하고, Client에게 IP 주소를 응답한다.

**최상위 도메인**

- `국가 코드 최상위 도메인(Country Code Top-Level Domain, ccTLD)`: .kr, .jp, .US 등
- `일반 최상위 도메인(Generic Top-Level Domain, gTLD)`: .com, .net, .org 등
- 그 외 몇 가지 종류가 더 있으나 위 두 가지가 대표적인 최상위 도메인이다.

### DNS Cache

어떤 URL 주소로 방문할 때마다 DNS 서버에 IP 주소를 요청하는 과정을 반복하는 것은 매우 비효율적이고, DNS 서버에도 큰 부담을 주는 일이다. 따라서 PC는 방문한 Domain Name 정보를 캐싱하는데, 이때 사용하는 캐시를 `DNS Cache`라고 한다. 이렇게 DNS Cache에 저장되어 있는 도메인에 접근할 때에는 DNS에 IP 주소를 따로 요청할 필요 없이 바로 해당 IP 주소에 접근할 수 있다.

- window에서 cmd에 `ipconfig/displaydns`라는 명령어를 입력하면 DNS Cache에 저장된 데이터를 확인할 수 있다.
