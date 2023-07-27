# What is HTTP?

---

# [[Everything you need to know about HTTP](https://cs.fyi/guide/http-in-depth)]

---

## What is HTTP?

- TCP/IP-based application layer communication protocol

## HTTP/0.9 - The One Liner (1991)

- HTTP의 처음 version
- request : single method GET
    
    ```html
    GET /index.html
    ```
    
- response : server는 HTML로 응답하고, 전송되는 즉시 연결을 닫는다
    
    ```html
    (response body)
    (connection closed)
    ```
    
- 특징
    - No headers
    - ‘GET’ 라는 유일한 method
    - response는 반드시 HTML

## HTTP/1.0 - 1996

- image, video files, plain text 등 다른 콘텐츠 타입으로 응답 가능
- request : HTTP header 포함
    
    ```html
    GET / HTTP/1.0
    Host: cs.fyi
    User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_5)
    Accept: */*
    ```
    
    - 개인 정보, 필수 응답 타입 등을 보냈다
- response : HTTP header, 상태 코드, 문자 지원, 캐싱, 등 포함
    
    ```html
    HTTP/1.0 200 OK 
    Content-Type: text/plain
    Content-Length: 137582
    Expires: Thu, 05 Dec 1997 16:00:00 GMT
    Last-Modified: Wed, 5 August 1996 15:55:28 GMT
    Server: Apache 0.84
    
    (response body)
    (connection closed)
    ```
    
    - HTTP/1.0 (버전 번호) + 200 OK (상태 코드, 설명)
- HTTP/1.0의 주요 단점 중 하나는, 하나의 connection이 multiple request를 처리할 수 없다는 것
    - request 마다 새로운 TCP 연결을 열고 닫아야 한다

### Three-way Handshake

- TCP 연결에서 data를 주고받기 전에 three-way handshake를 수행
- 과정
    - SYN
        - client가 랜덤한 숫자 x를 server에게 보낸다
    - SYN ACK
        - server가 랜덤한 숫자 y와 x+1로 이루어진 ACK packet을 client에게 보낸다
    - ACK
        - client가 y+1로 이루어진 ACK packet을 server에게 보낸다
- 완료되면 client와 server간의 data 공유가 시작될 수 있다
    - client는 마지막 ACK packet을 보내자마자 data를 보낼 수 있다
    - 하지만 server는 마지막 ACK를 받아야 data를 받을 수 있다

![image](https://github.com/AFpine/backend-newbs/assets/55616895/5b912121-fe38-41ff-a971-94234ef81050)

- HTTP는 stateless protocol
    - server는 client에 대한 정보를 유지하지 않는다
    - 각 request는 이전 request와 관계없이 필수 정보들을 포함해야 한다
    - 즉, 중복된 data를 보내야 한다

## HTTP/1.1 - 1997

- 새로운 method - PUT, PATCH, OPTIONS, DELETE
- Host header 필수(Hostname Identification)
- Persistent Connection 도입
    - connection은 기본적으로 닫히지 않는다
    - multiple sequential request 처리
    - Connection: close 헤더를 사용해 connection을 닫는다

## SPDY - 2009

- google의 altenative protocol
    - 약어가 아닌 상표
- bandwidth를 늘려서 얻는 성능 향상은 줄어드는 구간이 온다
- latency를 늘려서 얻는 성능 향상은 constant 하다
- HTTP/2를 만들고 SPDY를 HTTP에 병합

## HTTP/2 - 2015

- low latency를 위해 설계
    - SPDY가 이의 전신
- 기능적 차이
    - binary instead of textual
    - multiplexing - single connection에서의 multiple asynchronous HTTP
    - HPACK을 이용한 header 압축
    - server push - single request에 대한 multiple responses
    - request prioritization
    - 보안

### 1. Binary Protocol

- low latency, easier to parsing, not human-readable
- frames and streams
    - HTTP 메시지는 이제 하나 이상의 frame으로 구성
        - HEADERS frame, DATA frame, etc
    - frame의 모음이 stream
        - 각 frame은 자신이 속한 stream을 식별하는 고유한 stream ID 를 가지고 있다

### 2. Multiplexing

- TCP connection이 열리면, request들이 asynchronously하게 전달
- server도 asynchronously하게 response
    - 즉, 응답에 순서가 정해져 있지 않다
- client는 시간이 오래 걸리는 request에 대한 답변을 기다릴 필요가 없으며, 다른 request는 계속 처리된다

### 3. Header Compression

- 이전엔, 동일한 client가 지속적으로 server에 연결할 때, header에 많은 중복 data가 있다. 이를 최적화 하는 것이 별도의 RFC가 하는 일이었다
- HTTP/2 header compression은 다음과 같다
    - literal value가 Huffman code로 encoding되고, header table이 client와 server에 둘 다 남아있다
    - server는 header table을 사용해 subsequent request들에 대한 반복적인 header를 생략한다

### 4. Server Push

- server가 client가 특정 resource를 요청할 것을 미리 알고, request 전에 그에 대한 response를 client에 push 할 수 있다
- server가 data를 push하여 roundtrip을 줄일 수 있다
- server가 PUSH_PROMISE라는 frame을 전송하여 client의 request를 막는다

### 5. Request Prioritization

- client는 frame의 HEADERS frame에 우선순위 정보를 포함하여 stream에 우선순위를 지정할 수 있다
- PRIORITY frame을 전송하여 stream의 우선순위를 변경할 수 있다
- 우선순위 정보가 없으면 server는 asynchronously하게 처리

### 6. Security

- HTTP/2에 대한 보안은 필수가 아니다
    - TLS를 통한 보안
- 하지만 대부분의 공급 업체는 TLS를 통해 사용될 때만 HTTP/2를 지원할 것이라고 밝혔다
- 따라서 HTTP/2는 사양상 암호화를 요구하지는 않지만, 기본적으로 의무화 되어있다
