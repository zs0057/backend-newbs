# What is Domain Name?

---

# [[What is a Domain Name?](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_domain_name)]

---

## Summary

- Any Internet-connected computer can be reached through a public IP address
    - not human-readable
    - IP address가 바뀔 수 있다

## Structure of domain names

![image](https://github.com/AFpine/backend-newbs/assets/55616895/2179e42d-72f5-406f-86c5-42e522298292)

- read from right to left ←

### TLD (Top-Level Domain)

- domain name이 가리키는 서비스의 일반적인 목적을 알려준다
    - .com, .org, .net
- 특정 TLD는 더 엄격한 정책을 사용하여 목적을 명확하게 만든다
    - .us, .fr 같은 local TLD는 특정 언어로 제공되고나 특정 국가에서 호스팅 되도록 요구할 수 있다
    - .gov를 포함하는 TLD는 정부 부서에서만 사용 가능
    - .edu TLD는 교육 및 학술 기관에서만 사용 가능
- TLD는 63자 이하의 특수문자와 라틴알파벳으로 이루어진다
    - 어떤 글에서는 [2, 6]길이의 알파벳 소문자라고 한다
    - 어떤 글에서는 [1, 63]길이의 알파벳 소문자라고 한다
        - [ICANN generic TLD](https://en.wikipedia.org/wiki/List_of_Internet_top-level_domains)는 보통 이걸 따르는 것처럼 보인다

### Label (or component)

- TLD가 아닌 label들
- [A-Z], [a-z], - 로 이루어진 63자 이하의 sequence
    - 처음과 끝에 ‘-’ 가 올 수 없다

## Buying a domain name

- domain name은 살 수 없다
    - domain name이 구매 가능한 것이 된다면, 사용되지 않는 잠겨있는 domain name으로 web이 빠르게 채워질 것
- 대신 일정 기간동안 대여를 하는 방식이다
- registrar(등록기관)이라 불리는 회사들에서 domain name을 구매(대여)할 수 있다

### Finding an available domain name

- registrar 웹사이트에서 확인 가능
- or shell에서 whois 명령으로 확인할 수 있다
    - whois naver.com
    
    ![image](https://github.com/AFpine/backend-newbs/assets/55616895/ffe7ca43-574d-4a22-8e2b-190e9a9891e9)


### DNS refreshing

- DNS database는 전 세계의 모든 DNS server에 저장된다
- registrar가 domain에 대한 정보를 업데이트하면, 모든 DNS database에서도 업데이트 되어야 한다
- DNS server는 authoritative server로부터 받은 정보를 일정 시간동안 저장하고 있고, 시간이 지나면 자동적으로 정보를 refresh 한다
    - 그러므로 DNS server에 domain information이 update 되기까지는 시간이 좀 걸릴 수 있다
