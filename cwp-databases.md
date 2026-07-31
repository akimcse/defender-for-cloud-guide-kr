[🏠 전체 목차](README.md)　·　**Part 2 · 핵심 기능**　·　[04 · CWP](04-cwp.md)　·　**Defender for Databases**

# Defender for Databases

> [!NOTE]
> **보호 대상**: Azure SQL, 오픈소스 관계형 DB(PostgreSQL·MySQL·MariaDB), Azure Cosmos DB — PaaS·IaaS·멀티클라우드(Azure·AWS·GCP·온프렘) 전반.
> Microsoft의 DB 보안도 **"안전하게 시작 → 안전하게 유지"** 로, ① 태세 위험을 찾아 우선순위화하고 ② 위협을 탐지·대응합니다. 프레임워크는 **활성화(원클릭) → 예방(취약성 평가) → 탐지 → 대응(경고·자동화)** 입니다.

## ① 태세 위험 발견 · 우선순위화

### 섀도우 DB · 구성 오류 발견

환경의 **섀도우 데이터베이스**를 발견하고, 관리형·비관리형 DB의 구성 오류를 멀티클라우드 전반에서 자동 검색합니다. 리소스 수준의 세분화된 가시성과 민감 데이터 인사이트를 제공하며, 위험 우선순위화 엔진으로 비즈니스 영향과 함께 맥락화해 단계별 수정 가이드를 제시합니다.

> [!TIP]
> 리소스 수준 민감 데이터 인사이트·위험 우선순위화는 **Defender CSPM과 Defender for SQL을 함께 활성화**해야 제공됩니다.

![Defender for Cloud DB 권장사항 화면](assets/database/recommendations.png)
<small class="cap">MySQL/MariaDB/PostgreSQL·SQL 서버의 권장사항과 비정상 리소스 수를 확인</small>

### 컨텍스트 기반 공격 경로

**공격 경로 분석**으로 공격자가 가장 중요한 데이터에 도달하는 경로를 시뮬레이션해, 우선순위화된 수정으로 값비싼 유출을 예방합니다.

## ② 위협 탐지 · 대응

### 조기 침해 징후 탐지

- **접근 이상(ML)** — 비정상 위치·의심 IP·데이터센터/도메인 이상 등 비정상 접근 패턴
- **브루트포스 탐지** — 반복 로그인 시도(유효 사용자 대상·성공 포함)
- **쿼리 이상** — 잠재적 **SQL 인젝션**, 스크립트 실행(난독화·비정상 외부 소스)
- **측면 이동** — 침해된 워크로드에서 DB로의 이동
- **데이터 유출** — 비정상 규모의 데이터 추출
- Microsoft 위협 인텔리전스로 지속 업데이트, **MITRE ATT&CK 매핑** 경고 제공

![보안 경고 큐 화면](assets/database/security-alerts.png)
<small class="cap">브루트포스·악성 업로드 등 경고를 MITRE ATT&CK 전술과 함께 조회</small>

### 통합 대응

경고는 **Microsoft Sentinel(SIEM)** 및 **Defender XDR**에 네이티브 통합되어, 단일 화면에서 조사·자동 대응이 가능합니다.

## 하위 플랜별 상세

### Defender for Azure SQL Databases

- **대상**: Azure SQL DB·탄력적 풀·관리형 인스턴스·Synapse 전용 SQL 풀, SQL Server 2012~2022(Azure VM·Arc·AWS EC2·GCP GCE·온프렘 포함).
- **기능**: 취약성 평가(PaaS·IaaS) + 위협 방어 — SQL 인젝션, 브루트포스(성공/실패 분리), 침해 머신의 의심 접근.

### Defender for Open-Source Relational Databases

- **대상**: Azure PostgreSQL/MySQL Flexible Server(모든 티어), Azure/AWS의 MariaDB·MySQL·PostgreSQL, Amazon RDS(**Preview**).
- **탐지**: 이상 접근·쿼리 패턴(브루트포스), 침해 머신의 의심 활동. *PaaS만 지원, Arc 머신 미지원.*

### Defender for Azure Cosmos DB

- **대상**: Cosmos DB **NoSQL API 전용**(Cassandra·MongoDB·Table·Gremlin 미지원).
- **탐지**: SQL 인젝션 변형, 이상 접근(Tor·악성 IP·비정상 위치), 의심스러운 키 목록 조회·데이터 추출. 계정 데이터에 직접 접근하지 않아 **성능 영향 없음**.

---

## 참고 링크

- [Defender for Azure SQL](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-sql-introduction)
- [오픈소스 관계형 DB](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-databases-introduction) · [Cosmos DB](https://learn.microsoft.com/en-us/azure/defender-for-cloud/concept-defender-for-cosmos)
- [SQL 경고 참조](https://learn.microsoft.com/en-us/azure/defender-for-cloud/alerts-sql-database-and-azure-synapse-analytics)

---

### 다음 읽을거리

| ◀ 이전 | ▶ 다음 |
| :-- | --: |
| [Storage](cwp-storage.md) | [Containers →](cwp-containers.md) |

[🏠 전체 목차로 돌아가기](README.md)
