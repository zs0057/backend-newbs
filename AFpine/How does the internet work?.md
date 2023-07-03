# How does the Internet work?

# [[How does the Internet work?](https://cs.fyi/guide/how-does-internet-work)]

## Introduction to the Internet

- network : group of connected devices

> Internet is a network of networks
> 

## How the Internet Works: An overview

- standardized protocols
- data는 작은 packet들로 나누어져 router들을 통해 destination까지 간다

## Basic Concepts and Terminology

- Packet : internet을 통해 전송되는 data의 작은 단위
- Router : 다른 network들의 사이를 연결하여 packet을 전달하는 기기
- IP Address : network상의 device에 대입되는 식별자 (unique)
- Domain Name : website를 특정하기 위해 사람이 읽기 쉽게 만든 이름이다
    - google.com
- DNS(Domain Name System) : domain name을 IP 주소로 번역
- HTTP(Hypertext Transfer Protocol) : client와 server 사이에 data를 주고받기 위한 protocol
    - web browser ↔ website
- HTTPS(HTTP Secure) : encrypted version of HTTP. 보안 추가
- SSL/TLS : Secure Sockets Layer, Transport Layer Security protocols. internet상의 통신에서 보안을 제공하기 위해 사용

## The Role of Protocols in Internet

- IP, TCP, UDP, DNS, HTTP 등 굉장히 많은 protocol들이 존재한다
- IP(Internet Protocol)
    - destination correctness
- TCP(Transmission Control Protocol), UDP(User Datagram Protocol)
    - packet reliability(order), efficiency
- standardized protocol을 사용해서 얻는 중요한 장점 중 하나는, 서로 다르게 설계된 device나 system들의 통신이 가능하다는 것이다

## Understanding IP Addresses and Domain Names

- IP address : data를 올바른 destination에 전달하기 위해 사용된다. 보통 점으로 구분된 4개의 정수로 나타낸다 (192.168.1.1)
- Domain names : 사람이 읽기(기억하기) 쉬운 IP address. 점으로 구분된 두 개 이상의 부분으로 이루어져있다(google.com). DNS를 통해 IP주소로 번역된다.
- DNS : domain name으로 website에 접근하면, DNS server에 쿼리를 날리고 그에 맞는 IP주소를 받는다. 그리고 그 주소로 접속한다

## Introduction to HTTP and HTTPS

- HTTP는 client와 server간의 data 전송에 대한 protocol이다
- HTTPS는 전송간에 SSL/TLS를 사용하여 암호화한다
    - 로그인 정보, 결제 정보, 개인 정보 등

## Building Applications with TCP/IP

- TCP/IP는 internet-based 통신에서 가장 많이 쓰이는 통신 protocol
    - relieable, ordered, error-checked
- Ports : device안에서 application이나 서비스를 특정하는 번호
- Sockets : IP주소와 port 번호의 결합. 통신의 endpoint를 나타낸다
- Connections : 두 socket이 통신을 원할 때 생긴다. connection이 일어나면, device들은 여러 parameter들을 정한다
    - maximum segment size, window size, how data transmission
- Data transfer : connection이 생기면, 두 device 사이에 data를 주고받을 수 있다. data는 segment형태로 전달된다
    - sequence number, other metadata to ensure reliable delivery

## Secureing Internet Communication with SSL/TLS

- 로그인, 결제, 개인 정보 등을 위해 secure connection을 사용한다
- Certificates(인증서) : certificates에는 서버의 identity에 대한 정보가 포함되어 있다. 제 3자(Certificate Authority)가 서명하여 보증한다
- Handshake : server와 client는 (secure connection을 위한)암호화 알고리즘과 다른 parameter들을 조정하기 위해 정보를 교환한다.
- Encryption : secure connection이 설정되면, data는 서로 합의된 알고리즘으로 암호화되어 전달된다

## The Future : Emerging Trends and Technologies

- 5G
- IoT
- AI
- Blockchain
- Edge computing

# [[The internet, explained](https://www.vox.com/2014/6/16/18076282/the-internet)]

## Where is the internet?

- The last mile : 가정이나 작은 회사에서 사용하는 internet
- Data centers : data center는 서버로 가득찬 곳이다. 보통 땅값과 전기세가 저렴한 곳에 위치한다
- The backbone : data center와 consumer를 연결하는 아주 긴 network. 보통 광케이블로 이루어져있다

## What is IPv6

- IPv4 주소는 이미 부족하다
- 새로 만든 standard

## How does wireless internet work?

### Wifi

- 무료로 사용할 수 있는 전자기 주파수인 unlicensed spectrum 사용
- 이웃의 네트워크와 간섭하는 것을 방지하기 위해 범위 제한이 있다

### Cellular

- 서비스 지역을 cell로 나누어 작동
- 인구밀도에 따라 셀의 크기가 다르다
    - 높은 밀도의 지역에는 셀이 더 작다
    - 셀이 서비스를 제공할 수 있는 이용자의 수에 제한이?
- 각 셀에는 서비스를 제공하는 타워가 중앙에 존재
- wifi보다 범위를 훨씬 넓게 제공해야 하므로, 저전력 unlicensed spectrum을 사용할 수 없다
- 그러므로 통신사마다 사용할 수 있는 주파수를 독점해서 사용한다
    - 보통 경매를 통해 낙찰된다

## What is the cloud?

- 서버에 파일을 저장하고 인터넷을 통해 소프트웨어를 제공하는 것
- 사용자에게 더 간단하고 안정적인 환경을 제공
- 서버 관리에 관한 업무를 클라우드 회사에 맡길 수 있다

# [[How does the Internet Work?](http://web.stanford.edu/class/msande91si/www-spr04/readings/week1/InternetWhitepaper.htm)]

## Protocol Stacks and Packets

- Application Protocol Layer : www, mail, ftp 등
- TCP Protocol Layer : 포트번호를 사용
- IP Protocol Layer : IP 주소를 사용
- Hardware Protocol Layer : binary data ↔ network signal

## The Internet Routing Hierarchy

- 패킷이 전송되는 과정
    - 컴퓨터들은 다른 모든 컴퓨터들의 위치를 알지 못한다
    - 단순히 모든 컴퓨터에 broadcast 되지 않는다
- 패킷이 라우터에 도착하면, 라우팅 테이블을 통해 전송할 네트워크를 찾는다
    - 찾지 못한다면 backbone을 향해 **위쪽**으로 라우팅된다
    - backbone으로 올라갈 수록 더 큰 라우팅 테이블을 보유

## Domain Names and Address Resolution

- IP routing hierarchy 구조와 유사한 구조로 구성
- domain name을 확인할 수 있는 DNS server를 찾을 때 까지 **위쪽**으로

## Transmission Control Protocol

- connection-oriented, relieble, byte stream service
- 포트번호를 사용해 해당 어플리케이션으로 라우팅
- TCP 헤더에는 IP주소가 없다

## Internet Protocol

- unrelieble, connectionless protocol
- packet을 다른 컴퓨터로 전송하고 라우팅
