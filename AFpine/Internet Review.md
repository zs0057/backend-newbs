# Internet Review

---

# HTTP

---

## Three-way Handshake

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

![image](https://github.com/AFpine/backend-newbs/assets/55616895/4c9fffd2-5b40-4aa7-b06b-af972f572e9d)

- HTTP는 stateless protocol
    - server는 client에 대한 정보를 유지하지 않는다
    - 각 request는 이전 request와 관계없이 필수 정보들을 포함해야 한다
    - 즉, 중복된 data를 보내야 한다

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

# Browser

---

## Critical rendering path

- 중요한 rendering 경로 다섯 단계

### 1. Building the DOM tree

- HTML parsing은 tokenization, tree construction으로 나뉜다
    - tokenizer : 문서를 token들로 나눈다
    - parser : token들로 parse tree를 만든다
- DOM tree는 문서의 내용을 설명
    - <html>이 루트

![image](https://github.com/AFpine/backend-newbs/assets/55616895/09a5499f-cf98-48f4-a797-8abf4107b787)

### Preload scanner

- browser가 DOM tree를 만드는 동안 우선순위가 높은 리소스를 요청
    - CSS, JavaScript, 웹 글꼴 등
- HTML parser가 요청받은 asset에 도달할 때까지 리소스가 이미 실행 중이거나 다운로드 되었을 수 있도록 background에서 리소스를 검색
    - 최적화

### 2. Building the CSSOM

- CSS를 처리하고 CSSOM tree를 구축
    - CSS rules → map of styles
- DOM과 유사. 하지만 독립적인 자료 구조

## Render

- parsing 단계에서 생성된 CSSOM, DOM tree는 render tree로 결합되어 화면에 그려진다

### 3. Style

- 계산된 style tree 또는 render tree는 DOM tree의 루트부터 시작해 보이는 각 노드를 통과한다
- 각 노드에는 해당 CSSOM 규칙이 적용된다

### 4. Layout

- render tree에 있는 모든 node의 너비, 높이, 위치가 처음 결정되는 것
- render tree가 만들어지면, layout을 수행한다
    - 루트부터 탐색하면서
- node의 크기나 위치를 다시 계산하는 것을 reflow라 한다

### 5. Paint

- 개별 node를 화면에 painting 하는 것
- layout 단계에서 계산된 각 상자의 화면을 실제 픽셀로 변환
- 텍스트, 색상, 테두리, 그림자, 버튼, 이미지 등을 그린다
    - 즉, 이 작업을 빠르게 수행해야한다
    - 부드러우려면 16.67ms (60 fps)
- 다시 그리는 것을 repaint라 한다
    - reflow가 일어나지 않고 repaint만 일어날 수 있다

### Compositing

- 화면을 처음부터 다시 그리기에는 오랜 시간이 걸리기 때문에, 일반적으로 화면을 여러 layer로 나누어서 그린다
    - layer들은 tree 형태로 구성된다
- layer가 서로 겹치는 경우 올바른 순서로 화면에 그려져야 한다
- layer는 성능을 향상시키지만, 메모리 관리에 비용이 많이 든다
- 다시 합성하는 것을 re-composite라 한다

# DNS

---

## What are the steps in a DNS lookup?

1. 사용자가 web browser에 ‘example.com’을 입력하면, query가 인터넷으로 이동하여 DNS recursive resolver에게 전달된다
2. resolver는 DNS root nameserver에 요청한다
3. root server가 TLD DNS server의 주소로 응답한다
    1. .com TLD
4. resolver는 .com TLD에 요청한다
5. TLD server 는 domain’s nameserver인 example.com의 IP 주소로 응답한다
6. 마지막으로, recursive resolver가 domain’s nameserver에 query를 보낸다
7. example.com의 IP 주소가 nameserver에서 resolver로 전달된다
8. DNS resolver가 web browser에게 IP 주소를 응답한다

![image](https://github.com/AFpine/backend-newbs/assets/55616895/ecb6834f-f74e-433f-ad93-201c99932054)

## What are the types of DNS queries?

### Recursive query

- 한 DNS server에서 다른 여러 DNS server와 통신하여 IP 주소를 찾아내고 client에 반환한다
- client는 **한 번의 직접적인 요청**만 한다

### Iterative query

- DNS resolver가 **가능한 최상의 답변을 반환**하도록 허용한다
    - 일치하는 항목이 없으면, 답이 있을 수 있는 다른 DNS server를 추천해준다
- 즉, 응답을 찾을 때 까지 DNS resolver가 여러 DNS server에 요청한다

# Hosting

---

## Web server

- web server라는 말은 hardware, software 둘 다 가리킬 수 있는 용어

### Hardware side

- web server의 software와 website의 구성요소 파일들을 저장하는 컴퓨터
    - HTML document, image, JavaScript file
- 인터넷에 연결되어 web에 연결된 다른 device와 물리적 데이터를 교환

### Software side

- 사용자가 hosting된 파일에 접근하는 것을 제어하는 software
- At a minimum, this is an HTTP server
- HTTP server는 URL 및 HTTP를 이해하는 software
![image](https://github.com/AFpine/backend-newbs/assets/55616895/bec1a4e3-8b81-4f14-b85a-685767f327fa)
