# Browsers and how they work?

---

# [[How browsers work](https://web.dev/howbrowserswork/)]

---

## The browser’s main functionality

- main function : 사용자가 선택한 web resource 제공
    - 일반적으로 HTML 문서. 하지만 PDF, 이미지, 등 존재
    - resourece는 URI(Uniform Resource Identifier)를 사용하여 사용자가 지정
- browser가 HTML 파일을 해석하고 표시하는 방식에는 표준이 있다
    - 예전에는 잘 지켜지지 않았지만, 오늘날엔 대부분 지킨다
- 일반적인 사용자 인터페이스
    - URI를 넣을 주소 창
    - 앞, 뒤로 가기 버튼
    - 북마크
    - 새로 고침, 중지 버튼
    - 홈 버튼
- 사용자 인터페이스는 표준이 없다

## The browser’s high level structure

- user interface : 요청한 페이지가 표시되는 창을 제외한 모든 부분
- browser engine : UI와 rendering engine간의 작업을 marshalling
    - marshalling : 한 객체의 메모리에서 표현방식을 저장 또는 전송에 적합한 다른 데이터 형식으로 변환하는 과정 (반대는 demarshalling)
- rendering engine : 요청된 콘텐츠를 display
- networking : HTTP같은 network call에 대해, platform-independent interface 뒤에서 platform마다 다른 구현을 사용
- UI backend : combo box, 창 같은 기본 위젯을 그린다
- JavaScript interpreter : JavaScript 코드를 parse, execute
- Data storage : persistence layer. browser는 쿠키와 같은 모든 종류의 데이터를 로컬에 저장해야 할 수 있다
    - localStorage, IndexedDB, WebSQL, FileSystem같은 저장 메커니즘 지원

![image](https://github.com/AFpine/backend-newbs/assets/55616895/34e05fbb-1486-451a-b4dc-2588fd213189)

- Chrome같은 브라우저는 rendering engine의 instance를 tab 당 하나씩 사용한다. 각 탭은 별도의 프로세스에서 실행된다

## The rendering engine

- rendering engine은 요청된 콘텐츠를 잘 display 해야한다
- 기본적으로 HTML, XML 문서와 이미지를 display 할 수 있다
    - 플러그인 또는 확장을 통해 다른 유형의 데이터를 display 한다
- browser마다 다른 rendering engine을 사용한다
    - Trident, Gecko, WebKit, Opera, Blink, etc…

### The main flow

![image](https://github.com/AFpine/backend-newbs/assets/55616895/8f5caba7-a343-4fc1-ac47-5aabf91a7fdf)

- HTML parsing을 시작하고 DOM tree 생성
    - style data parsing
- render tree 생성 (style information을 사용해)
    - 색상, 치수 같은 시각적 속성이 있는 rectangle 포함
    - rectangle은 display되는 올바른 순서
- layout processing
    - display 되어야하는 정확한 좌표를 각 node에 제공
- painting
    - render tree를 순회하면서 UI backend layer를 사용해 paint

![image](https://github.com/AFpine/backend-newbs/assets/55616895/3852b712-04f8-4a12-80b2-f26247041ead)

- 가능한 한 빨리 콘텐츠를 display하기 위해, 전체 HTML이 parsing되기 까지 기다리지 않는다

# [[Populating the page: how browsers work](https://developer.mozilla.org/en-US/docs/Web/Performance/How_browsers_work)]

---

## Navigation

- web 성능의 목표 중 하나는 navigation 시간 최소화

### DNS Lookup

- 어떤 HTML page를 domain name으로 접근하려면, DNS lookup을 통해 IP 주소를 알아내야 한다
- DNS lookup은 일반적으로 hostname당 한 번만 수행
    - 한 페이지 안에 font, image, ads 같은 항목들의 hostname이 전부 다르면 각각 DNS lookup을 수행해야 한다

### TCP Handshake

- IP 주소를 얻으면 browser는 3-way handshake를 통해 connection을 설정
    - TCP connection의 parameter negotiation
- SYN / SYN-ACK / ACK
    - 첫 SYN에 대한 ACK와 두 번째 SYN을 같이 보낸다

### TLS Negotiation

- HTTPS를 통한 connection에서는 또 다른 handshake 필요
- 통신을 암호화 하는데 사용할 암호를 결정

![image](https://github.com/AFpine/backend-newbs/assets/55616895/4f453da0-c0ce-4276-92de-8b229e9f8abb)

## Response

- conncection이 설정되면, browser는 초기 HTTP GET 요청을 보낸다
- 서버가 이 요청을 받으면 관련 응답 헤더와 HTML content를 보낸다
- TTFB(Time to First Byte) : first request - response 시간

### TCP Slow Start / 14KB rule

- TCP slow start : 네트워크 연결 속도의 균형을 맞추는 알고리즘
- 첫 response packet은 크기는 14KB로 시작
- 결정된 임계값에 도달하거나 정체가 발생할 때 까지 2배씩 증가

![image](https://github.com/AFpine/backend-newbs/assets/55616895/0c940f7b-af89-4f4e-b15c-74a2629d03f4)

### Congestion control

- ACK를 사용해 전송 속도를 결정

## Parsing

- browser가 수신한 데이터를 DOM 및 CSSOM으로 전환하기 위해 수행하는 단계
    - DOM(Document Object Model) : 모든 HTML, XML 문서를 나타내고 상호작용하는 API. 문서를 노드 트리로 나타내는 문서 모델.
    - CSSOM(CSS Object Model) : 문서의 스타일 관련 정보를 읽고 수정하기 위한 API 세트.
- renderer가 page를 display 하는데 사용

## Critical rendering path

- 중요한 rendering 경로 다섯 단계

### 1. Building the DOM tree

- HTML parsing은 tokenization, tree construction으로 나뉜다
    - tokenizer : 문서를 token들로 나눈다
    - parser : token들로 parse tree를 만든다
- DOM tree는 문서의 내용을 설명
    - <html>이 루트

![image](https://github.com/AFpine/backend-newbs/assets/55616895/d45e36f3-e39b-439f-af5e-d51e66a63d20)

### Preload scanner

- browser가 DOM tree를 만드는 동안 우선순위가 높은 리소스를 요청
    - CSS, JavaScript, 웹 글꼴 등
- HTML parser가 요청받은 asset에 도달할 때까지 리소스가 이미 실행 중이거나 다운로드 되었을 수 있도록 background에서 리소스를 검색
    - 최적화

### 2. Building the CSSOM

- CSS를 처리하고 CSSOM tree를 구축
    - CSS rules → map of styles
- DOM과 유사. 하지만 독립적은 자료 구조

### Other Processes

- JavaScript Compilation
    - CSS가 parsing되고 CSSOM이 생성되는 동안, JavaScript 파일을 비롯한 다른 asset이 다운로드된다(preload scanner)
    - abstract syntax tree로 parsing 된다
- Building the Accessibility Tree
    - browser가 accessibility tree를 구축
    - AOM은 DOM의 semantic version

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
