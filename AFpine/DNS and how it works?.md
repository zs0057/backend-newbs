# DNS and how it works?
---

# [[What is DNS?](https://www.cloudflare.com/en-gb/learning/dns/what-is-dns/)]

---

## What is DNS?

- Internet의 전화번호부
- domain name → IP address

## There are 4 DNS servers involved in loading a webpage

### DNS recursor(recursive resolver)

- client와 DNS nameserver 사이의 중개자
- web client로부터 DNS query를 받는다
- 캐시에 요청받은 정보가 있다면 바로 응답
    - authoritative nameserver에서 받은 정보를 캐시
- 아니라면 root → TLD → authoritative nameserver로 요청을 보낸다

![image](https://github.com/AFpine/backend-newbs/assets/55616895/06b3883b-07ee-40b8-99a1-4739cab9ef2a)

### Root nameserver

![image](https://github.com/AFpine/backend-newbs/assets/55616895/3c1bbf2b-8d61-4436-8074-08550082fe9c)

- DNS zone : 특정 조직이나 관리자가 관리하는 DNS namespace의 일부
    - DNS 영역은 여러 하위 도메인을 포함할 수 있다
    - 여러 영역이 동일한 서버에 존재할 수 있다

![image](https://github.com/AFpine/backend-newbs/assets/55616895/111698a4-d0c9-4e4b-850f-c8369508c1e2)

- root 영역에서 작동하는 DNS nameserver
    - root 영역은 DNS 계층 구조의 맨 위에 있다
- 모든 DNS 조회는 root 영역에서 시작된다
- DNS root 영역을 제공하는 13개의 서로 다른 IP 주소가 있다
    - DNS 아키텍처의 제한 사항으로 root 영역에는 최대 13개의 서버 주소가 있어야 한다
    - 전 세계적으로 수백개의 사본 서버 존재, anycast 라우팅 사용
        - 어떤 (unique)요청을 다양한 노드로 라우팅 할 수 있는 방법
        - 일반적으로 대기 시간이 줄어들도록 최적화

### TLD(Top-Level Domain) nameserver

- URL의 마지막 점 뒤에 오는 확장자를 공유하는 domain name에 대한 정보 유지
- 일반 TLD : .com, .org, .net, .edu, .gov, 등
- 국가 코드 TLD : .us, .jp, .kr, 등

### Authoritative nameserver

- 서비스를 제공하는 domain name의 특정한 정보가 포함
    - DNS A record에서 IP address를 찾아서 recursive resolver에게 제공

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

![image](https://github.com/AFpine/backend-newbs/assets/55616895/ebebf78d-6575-42ba-85d7-02ca639554d8)

## What are the types of DNS queries?

### Recursive query

- 한 DNS server에서 다른 여러 DNS server와 통신하여 IP 주소를 찾아내고 client에 반환한다
- client는 **한 번의 직접적인 요청**만 한다

### Iterative query

- DNS resolver가 **가능한 최상의 답변을 반환**하도록 허용한다
    - 일치하는 항목이 없으면, 답이 있을 수 있는 다른 DNS server를 추천해준다
- 즉, 응답을 찾을 때 까지 DNS resolver가 여러 DNS server에 요청한다

### Non-recursive query

- 캐시 안에 정보가 있을 때

## DNS caching

- 요청에 대한 성능 및 안정성을 향상시키는 위치에 데이터를 임시 저장
- browser DNS caching
    - 정해진 시간동안 browser에서 DNS record를 caching
- OS level caching
    - stub resolver가 애플리케이션에서 요청을 받으면 캐시를 확인
        - stub resolver : OS 내부의 프로세스
