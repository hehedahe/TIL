# **BIND9로 NS 구축**

  

---

  

## **1. BIND9**

  

- **개요**: BIND9(Berkeley Internet Name Domain 9)는 DNS 서버 소프트웨어 중 하나로 ISC에서 관리중인 오픈 소스 프로젝트입니다.

  

- **기능**:

  

- Authoritative DNS Server: 특정 도메인에 대한 질의 응답 권한이 있는 DNS 서버 역할

- Recursive DNS Resolver: 질의에 대한 답을 갖고 있지 않으면 다른 DNS 서버로 질의를 전달하여 결과를 리턴

- Zone Files: 도메인 이름과 IP 주소간의 매핑 정보를 포함한 records를 정의

  

- **구성**:

  

- named.conf

- Zone Files

### **주요 기능**