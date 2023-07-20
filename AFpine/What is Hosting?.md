# What is Hosting?

---

# [[What is Hosting?](https://www.hostinger.com/tutorials/what-is-web-hosting/)]

---

## What is Web Hosting?

- 인터넷에 website를 게시할 수 있게 하는 online service

## How Does Web Hosting Work?

![image](https://github.com/AFpine/backend-newbs/assets/55616895/6c306c10-c5a8-48b2-83aa-6eaa82796853)

# [[What is the difference between webpage, website, web server, and search engine?](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/Pages_sites_servers_and_search_engines)]

---

## Summary

- web page (just “page”)
    - web browser에서 표시할 수 있는 문서
- website (just “site”)
    - 여러 방식으로 함께 연결되는 web page의 모음
- web server
    - 인터넷에서 website를 hosting하는 컴퓨터
- search engine
    - 다른 web page를 찾는데 도움을 주는 web 서비스
    - google, bing, yahoo

## Deeper dive

### Web page

- web browser에서 표시할 수 있는 간단한 document
    - HTML로 작성되어있다
    - HTML이 아닌 문서도(PDF, image) 표시할 수 있지만, 이는 web page라는 용어보다 그냥 document라고 부른다
- 다양한 type의 resource를 포함할 수 있다
    - style information : controlling a page’s look-and-feel
    - scripts : interactivity to the page
    - media : images, sounds, and videos
- 모든 웹페이지는 고유한 주소를 통해 연결할 수 있다
    - URL

### Website

- 같은 domain name을 공유하는 web page의 모음
- 주소창에 domain name을 입력하면, website의 기본 web page로 연결된다
- single-page website도 가능하다

### Web server

- 하나 이상의 website를 hosting 할 수 있다
- 사용자 요청에 따라 hosting 하는 website의 web page를 사용자의 browser에 보낸다

### Search engine

- 다른 web page를 찾는걸 도와주는 **website**
    - browser : web page를 검색하고 표시하는 **software**

# [[What is a web server?](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_web_server)]

---

## Summary

- web server라는 말은 hardware, software 둘 다 가리킬 수 있는 용어

### Hardware side

- web server의 software와 website의 구성요소 파일들을 저장하는 컴퓨터
    - HTML document, image, JavaScript file
- 인터넷에 연결되어 web에 연결된 다른 device와 물리적 데이터를 교환

### Software side

- 사용자가 hosting된 파일에 접근하는 것을 제어하는 software
- At a minimum, this is an HTTP server
- HTTP server는 URL 및 HTTP를 이해하는 software

![image](https://github.com/AFpine/backend-newbs/assets/55616895/2edf7adf-7536-4ec3-b209-2a7aef2c74ce)

## Deeper dive

### Hosting files

- web server는 website의 모든 HTML 문서 및 관련 asset을 저장해야 한다
- 기술적으로는 모든 파일을 사용자의 컴퓨터에 hosting 할 수 있지만, web server에 파일을 모두 저장하는 것이 더 편리하다
    - 가동 중지시간 및 시스템 문제 제외, 항상 인터넷에 연결되어 있다
    - dedicated web server는 항상 동일한 IP 주소를 가질 수 있다
    - 일반적으로 타사에서 유지 관리한다

### Communicating through HTTP

- web server는 HTTP에 대한 지원을 제공한다
    - Textual : 모든 command는 일반 text이며 human-readable 하다
    - Stateless : server와 client 둘 다 이전 통신을 기억하지 않는다
- web server에서, HTTP server는 request를 처리하고 응답한다
    1. request를 받으면 HTTP server는 URL이 기존 파일과 일치하는지 확인한다
    2. 그렇다면 파일을 browser에 보낸다. 그렇지 않다면 server는 동적으로 파일을 생성해야 하는지 확인한다
    3. 둘 다 가능하지 않다면, 오류 메세지를 반환한다

### Static vs Dynamic content

- server는 정적 또는 동적 콘텐츠를 제공할 수 있다
- static content
    - static website는 설정이 쉽다
- dynamic content
    - server가 content를 처리하거나 database에서 즉석으로 content를 생성한다
    - more flexibility, more complex technical stack
