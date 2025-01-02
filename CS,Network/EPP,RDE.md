# **EPP와 RDE 차이**

  

## **EPP 데몬**

  

**EPP (Extensible Provisioning Protocol) 데몬**은 도메인 등록과 관리 작업을 수행하는 데 사용되는 프로토콜을 구현한 시스템입니다.

  

### **1. EPP 주요 역할**

  

1) **도메인 관리**

  

- 도메인 등록, 갱신, 이전, 삭제, 업데이트와 같은 작업을 처리.

- 레지스트라(등록기관)와 레지스트리(도메인 레벨 관리자) 간의 통신을 담당.

  

2) **EPP 프로토콜 기반 통신**

  

- EPP는 XML 기반의 프로토콜로, 등록기관과 레지스트리 간의 표준화된 방식으로 데이터를 교환.

- 클라이언트(레지스트라)가 보내는 EPP 명령을 처리하고 레지스트리에 전달.

  

3) **실시간 작업 처리**

  

- 사용자(등록자)가 도메인에 대해 작업을 요청하면, 이를 실시간으로 레지스트리와 상호작용하여 처리.

  

4) **상태 관리**

- 도메인의 상태(등록, 만료, 삭제 대기 등)를 관리하고 이를 클라이언트에게 반환.

  

### **2. EPP 데몬의 작동 방식**

  

- **입력**: 클라이언트의 요청(EPP 명령, 예: 도메인 등록).

- **처리**: 요청을 XML 형식으로 변환하고, 레지스트리에 전달.

- **출력**: 레지스트리에서 반환된 응답을 클라이언트에게 전달.

  

### **3. 정리**

  

- 특정 도메인을 등록하거나 갱신.

- 도메인 이름의 소유권 이전 처리.

- 네임 서버 정보 업데이트.

  

---

  

## **RDE 데몬**

  

**RDE (Registrar Data Escrow) 데몬**은 등록 데이터를 **에스크로(보관)**하기 위해 설계된 시스템입니다. 이 데몬은 ICANN(Internet Corporation for Assigned Names and Numbers) 및 레지스트리 정책을 준수하기 위해 필수적으로 사용됩니다.

  

### **1. RDE 주요 역할**

  

1) **데이터 백업 및 보관**

  

- 도메인 등록 데이터(등록자 정보, 도메인 상태 등)를 정기적으로 에스크로 데이터 파일로 생성.

- 해당 데이터를 에스크로 기관(예: 제3자 보관 업체)으로 전송.

  

2) **ICANN 정책 준수**

  

- ICANN은 레지스트라가 등록 데이터를 정기적으로 백업하여 데이터 유실에 대비하도록 요구.

- 데이터 손실이나 서비스 중단 시 복구를 지원.

  

3) **정기적인 데이터 제출**

  

- 매일, 매주, 분기별로 도메인 등록 데이터를 수집하고, 파일을 준비해 에스크로 서버로 업로드.

  

4) **데이터 무결성 보장**

- 보관된 데이터의 정확성과 무결성을 검증.

- 암호화된 데이터 전송과 디지털 서명을 통해 보안 강화.

  

### **2. RDE 데몬의 작동 방식**

  

- **입력**: 도메인 등록 데이터(DB에서 추출).

- **처리**: 데이터를 ICANN 규격에 맞는 형식으로 가공(예: CSV 파일 생성).

- **출력**: 생성된 파일을 SFTP(보안 파일 전송 프로토콜)를 통해 에스크로 서버로 업로드.

  

### **3. 사용 사례**

  

- 도메인 등록 데이터의 에스크로 보관.

- 데이터 손실 발생 시 에스크로 데이터로 복구.

- ICANN 규제 준수를 위한 주기적 데이터 제출.

  

---

  

## **EPP 데몬과 RDE 데몬의 비교 정리**

  

| **항목** | **EPP 데몬** | **RDE 데몬** |

| ------------------ | ---------------------------------------------------- | ------------------------------------------------------ |

| **주요 목적** | 도메인 등록, 갱신, 삭제, 이전 등의 실시간 작업 처리. | 도메인 등록 데이터를 보관하고 에스크로 서버에 제출. |

| **통신 대상** | 레지스트리(도메인 관리 기관)와 통신. | 에스크로 서비스 제공자(제3자 데이터 보관 업체)와 통신. |

| **데이터 형식** | XML(EPP 프로토콜 표준). | ICANN 에스크로 데이터 사양에 따른 CSV 파일. |

| **작업 특성** | 실시간 트랜잭션 처리(사용자 요청 즉시). | 정기적 데이터 백업 및 전송(일간, 주간, 분기별). |

| **보안 메커니즘** | TLS/SSL 암호화를 통해 레지스트리와 안전한 통신. | 데이터 암호화, 디지털 서명 및 SFTP를 통한 안전한 전송. |

| **ICANN 요구사항** | EPP 프로토콜 준수. | 에스크로 데이터 제출 요구사항 준수. |

| **주요 출력** | 트랜잭션 처리 결과(등록 성공, 오류 등). | 에스크로 데이터 파일 및 업로드 상태. |

  

---

  

### **flow**

  

1. **EPP 데몬 작업**

  

- 사용자가 도메인(`example.com`)을 등록합니다.

- EPP 데몬은 레지스트리와 통신해 도메인을 등록하고 성공 여부를 반환합니다.

  

2. **RDE 데몬 작업**

- 주기적으로 RDE 데몬은 `example.com`의 등록 정보를 포함한 전체 도메인 데이터를 수집합니다.

- ICANN 사양에 맞게 데이터를 CSV 파일로 생성하고, 에스크로 서버로 전송합니다.

  

---

  

### **결론**

  

- **EPP 데몬**은 도메인 등록과 관련된 **실시간 트랜잭션 처리**를 담당하며, 레지스트리와 클라이언트 간의 실시간 상호작용을 지원합니다.

- **RDE 데몬**은 도메인 등록 데이터를 **백업 및 보관**하여 데이터 유실에 대비하고 ICANN 규정을 준수합니다.

- 두 데몬은 도메인 생태계의 안정성과 신뢰성을 보장하기 위해 각기 다른 역할을 수행하지만, 함께 작동하여 완전한 레지스트라 플랫폼을 구현하는 데 필수적입니다.

  

---

  

## clTRID

  

**clTRID**(Client Transaction Identifier)는 EPP(Extensible Provisioning Protocol)에서 사용되는 **클라이언트 트랜잭션 식별자**입니다. 이는 EPP 명령(도메인 등록, 갱신, 삭제 등)에 대해 클라이언트가 제공하는 **고유한 식별자**로, 클라이언트(예: 레지스트라)가 보낸 요청과 서버(레지스트리)가 반환하는 응답을 서로 연결하고 추적하는 데 사용됩니다.

  

EPP 트랜잭션에서 클라이언트가 요청과 응답을 추적하고 관리하는 데 중요한 **식별자**이므로 시스템 간 데이터 흐름의 명확성을 유지하고, 동시성 처리와 디버깅을 지원하며, 안정적인 트랜잭션 관리의 핵심 요소로 작용합니다.

  

---

  

### **clTRID의 역할**

  

1. **요청 및 응답 추적**

  

- 클라이언트가 보낸 각 요청에 고유한 `clTRID`를 포함시킵니다.

- 서버는 요청 처리 후 응답 메시지에 동일한 `clTRID`를 포함하여 반환합니다.

- 이를 통해 클라이언트는 어떤 요청에 대한 응답인지 식별할 수 있습니다.

  

2. **트랜잭션 구분**

  

- EPP 통신은 여러 요청이 동시에 처리될 수 있기 때문에, 각 요청을 식별하기 위해 `clTRID`가 필요합니다.

- 동일한 클라이언트에서 발생한 여러 트랜잭션을 구분합니다.

  

3. **문제 해결 및 디버깅**

- 특정 트랜잭션의 성공 여부를 확인하거나 에러가 발생한 요청을 추적할 때 유용합니다.

- 로그 및 감사 시스템에서 `clTRID`를 참조하여 특정 요청의 세부 정보를 확인할 수 있습니다.

  

---

  

### **clTRID의 구성**

  

`clTRID`는 클라이언트가 정의하는 문자열로, 일반적으로 다음과 같은 규칙을 따릅니다:

  

1. **고유성 보장**

  

- 클라이언트 내에서 각 요청의 `clTRID`는 고유해야 합니다.

- 이는 중복 트랜잭션 식별을 방지하고 명확성을 유지합니다.

  

2. **읽기 쉬운 형식(권장)**

  

- 일반적으로 `clTRID`는 다음과 같은 형식으로 구성됩니다:

- **`클라이언트ID-타임스탬프-난수`**

- 예: `client123-20241229T123456-001`

  

3. **길이 제한**

- EPP 프로토콜은 `clTRID`의 길이를 일반적으로 64자로 제한합니다.

  

---

  

### **clTRID의 동작 흐름 예시**

  

#### **1. 클라이언트 → 서버: EPP 요청 전송**

  

클라이언트가 도메인 등록 요청을 전송합니다:

  

```xml

<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">

<command>

<create>

<domain:create xmlns:domain="urn:ietf:params:xml:ns:domain-1.0">

<domain:name>example.com</domain:name>

<domain:registrant>123456</domain:registrant>

</domain:create>

</create>

<clTRID>client123-20241229T123456-001</clTRID>

</command>

</epp>

```

  

- 여기서 `clTRID`는 `client123-20241229T123456-001`입니다.

  

#### **2. 서버 → 클라이언트: EPP 응답 반환**

  

서버는 요청을 처리한 후, 응답 메시지에 동일한 `clTRID`를 포함합니다:

  

```xml

<epp xmlns="urn:ietf:params:xml:ns:epp-1.0">

<response>

<result code="1000">

<msg>Command completed successfully</msg>

</result>

<trID>

<clTRID>client123-20241229T123456-001</clTRID>

<svTRID>registry123-20241229T123500-002</svTRID>

</trID>

</response>

</epp>

```

  

- **`clTRID`**: 클라이언트가 보낸 요청의 식별자.

- **`svTRID`**: 서버가 생성한 고유 트랜잭션 ID.

  

---

  

### **clTRID의 주요 이점**

  

1. **동기화 보장**

  

- 요청과 응답을 명확히 매핑할 수 있으므로, 트랜잭션이 올바르게 처리되었는지 확인 가능.

  

2. **다중 요청 처리**

  

- 여러 요청이 동시에 처리되는 환경에서 `clTRID`를 사용해 각 요청을 구분.

  

3. **로그 추적 및 감사**

- 서버와 클라이언트 모두 `clTRID`를 기반으로 요청 및 응답을 기록하고 문제를 분석.

  

---

  

### **EPP 트랜잭션 관리에서의 중요성**

  

- **트랜잭션 실패 처리**:

만약 특정 요청이 실패하거나 중복 요청이 발생하면, 클라이언트는 `clTRID`를 확인하여 이를 재시도하거나 롤백할 수 있습니다.

- **통합 로깅**:

클라이언트와 서버 모두 로그 파일에서 `clTRID`를 참조하여 트랜잭션 세부 사항을 추적하고 분석할 수 있습니다.

  

---

  

## SFTP 서버와 SFTP 프로토콜

  

---

  

### **SFTP 서버란?**

  

**SFTP 서버**는 SFTP 프로토콜을 사용해 파일을 업로드하거나 다운로드할 수 있는 **파일 관리 시스템**입니다.

  

**SFTP (Secure File Transfer Protocol)**은 파일 전송을 위한 **네트워크 프로토콜**이자 **파일 전송 서버**로 사용됩니다. 이는 SSH(Secure Shell) 프로토콜을 기반으로 설계되어, **보안**이 강화된 파일 전송을 제공합니다.

  

- 클라이언트는 SFTP 서버에 접속하여 파일을 업로드하거나 다운로드할 수 있습니다.

- 서버는 SSH 프로토콜을 사용해 데이터의 전송 과정에서 **암호화**와 **보안 인증**을 보장합니다.

  

SFTP는 단순히 프로토콜만이 아니라, 파일 전송 작업을 처리하는 서버(예: SFTP 서버)와 클라이언트가 함께 동작하는 구조를 의미하기도 합니다.

  

#### **SFTP 서버의 역할**

  

1. **파일 전송**

  

- 파일 업로드, 다운로드, 삭제, 이름 변경, 디렉토리 생성 등의 작업을 처리.

  

2. **보안**

  

- 데이터 전송 시 암호화를 제공해, 중간에 전송 데이터를 가로채더라도 내용을 읽을 수 없도록 보호.

  

3. **사용자 인증**

  

- SFTP 서버는 사용자 인증(예: 비밀번호, SSH 키 기반 인증)을 통해 허가된 사용자만 파일에 접근할 수 있도록 제한.

  

4. **파일 관리**

- 클라이언트가 서버의 디렉토리와 파일 구조를 탐색하고, 필요한 작업(파일 읽기/쓰기 등)을 수행할 수 있도록 지원.

  

---

  

### **SFTP와 FTP의 차이**

  

SFTP는 FTP(File Transfer Protocol)와 비슷해 보이지만, **근본적으로 다른 프로토콜**입니다.

  

| **특징** | **SFTP** | **FTP** |

| --------------- | ---------------------------------------- | ------------------------------------- |

| **보안** | 데이터와 인증 정보를 암호화 (SSH 기반) | 평문 전송 (보안 없음) |

| **포트** | 기본적으로 **22번 포트** 사용 | 기본적으로 **21번 포트** 사용 |

| **데이터 채널** | 단일 암호화된 데이터 채널 사용 | 별도의 제어 채널과 데이터 채널 |

| **사용 사례** | 민감한 데이터 전송 및 보안이 필요한 환경 | 보안이 중요하지 않은 간단한 파일 전송 |

| **구현 복잡성** | SSH 기반으로 구현 (상대적으로 복잡) | 간단한 전송 구현 가능 |

  

---

  

### **SFTP 서버 사용 사례**

  

1. **RDE 데몬에서 데이터 전송**

  

- 레지스트라 플랫폼의 RDE 데몬은 도메인 등록 데이터를 **SFTP 서버**를 통해 ICANN 에스크로 데이터 저장소로 전송합니다.

- 이 과정에서 SSH 기반의 암호화를 사용해 **보안**과 **데이터 무결성**을 보장합니다.

  

2. **기업 내부 파일 공유**

  

- 기업 내부에서 SFTP 서버를 사용해 직원들 간의 파일 공유를 안전하게 수행.

  

3. **클라우드 백업**

  

- SFTP 서버를 사용하여 중요한 데이터를 원격 서버로 백업.

  

4. **개발 환경 배포**

- 애플리케이션 배포 시 SFTP를 통해 서버에 파일을 업로드.

  

---

  

### **SFTP의 주요 구성 요소**

  

1. **클라이언트**

  

- SFTP 서버에 접속해 파일 작업을 수행하는 시스템(예: FileZilla, WinSCP).

  

2. **서버**

  

- SFTP 프로토콜을 구현하여 클라이언트 요청을 처리하는 시스템.

  

3. **SSH 키**

- 비밀번호 대신 안전한 인증을 위해 사용하는 암호화 키.

  

---

  

### **SFTP 서버의 작동 방식**

  

1. 클라이언트가 SFTP 서버에 **SSH 프로토콜(22번 포트)**을 통해 연결.

2. 사용자 인증을 수행(예: 비밀번호 인증 또는 SSH 키 인증).

3. 인증 후 클라이언트는 파일 업로드, 다운로드, 삭제, 디렉토리 생성 등의 작업 수행.

4. 전송되는 모든 데이터는 SSH 암호화를 통해 보호.

  

---