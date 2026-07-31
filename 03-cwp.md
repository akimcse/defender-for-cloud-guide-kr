[🏠 전체 목차](./README.md)　·　**Part 2 · 핵심 기능**　·　페이지 4 / 5

# 03 · CWP — 클라우드 워크로드 보호

> [!NOTE]
> **이 페이지에서 얻는 것**
> - 워크로드별 Defender 플랜 9종이 각각 무엇을, 어떻게 보호하는지
> - Servers Plan 1 vs Plan 2의 기능 경계
> - 경고·인시던트가 생성·상관되고 MITRE ATT&CK에 매핑되는 원리
>
> ⏱️ 예상 소요 **15분**　·　🎯 대상: SOC 분석가, 워크로드 담당 엔지니어, 보안 관리자

CWP는 **"지금 누가 내 리소스를 공격하고 있는가?"** 에 답하는 축입니다. **워크로드별 Defender 플랜**이 위협을 탐지해 보안 경고를 생성하고, 이를 인시던트로 상관합니다. 각 플랜은 독립 활성화하며, 경고는 MITRE ATT&CK에 매핑되어 SIEM으로 내보낼 수 있습니다.

---

## 1. Defender for Servers (Plan 1 · Plan 2)

**보호 대상**: Azure·AWS·GCP·온프렘의 Windows/Linux VM (하이브리드·멀티클라우드 통합 뷰). 에이전트 기반(MDE)과 에이전트리스를 결합해 **가시성·컴플라이언스 / 공격 표면 축소 / 고급 탐지·대응** 세 축으로 보호합니다.

- **Plan 1** = MDE(EDR) 통합 중심 엔트리 레벨. 리소스 수준 켜기/끄기 가능.
- **Plan 2** = P1 전체 + 에이전트리스 스캔·FIM·JIT·MCSB 기준·DNS/네트워크 탐지 등. 구독 수준 활성화.

주요 기능: 소프트웨어 인벤토리·취약성 평가(MDVM), 파일 무결성 모니터링(FIM), OS 패치(Azure Update Manager), 하드닝 기준선(MCSB), 시크릿 스캔, JIT VM 액세스, 제어 평면/DNS/네트워크 탐지, 에이전트리스 악성코드 스캔, NGAV·EDR.

▶ **[Defender for Servers 상세 →](cwp-servers.md)**

참고: [Defender for Servers 개요](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-servers-overview)

## 2. Defender for Storage

**보호 대상**: Azure Blob·Files·Data Lake (에이전트 불필요).

| 기능 | 설명 |
| --- | --- |
| **활동 모니터링** | 데이터/컨트롤 플레인 로그 분석 — 악성 IP·Tor·위험 앱, SAS 토큰 오용(신원 없는 엔터티) 탐지. **진단 로그 불필요** |
| **악성코드 스캔**(유료 add-on) | Microsoft Defender Antivirus 기반 **업로드 시 + 온디맨드** 스캔. **GB당 과금**(기본 월 1만 GB 캡), 75%/100% 도달 경고, Event Grid 자동화(삭제·격리) |
| **민감 데이터 위협 탐지**(추가 비용 없음) | Purview SIT·레이블 상속, 비정상 접근·유출 탐지 |
| **해시 평판 분석**(모든 플랜) | 업로드 해시를 알려진 악성코드 해시와 비교. *Put Block/Put Block List·SMB 파일 공유 미지원* |

구독 수준 활성화 권장(기존·미래 계정 자동 보호). **클래식 플랜** 사용 중이면 신규 기능 위해 **신규 플랜 마이그레이션** 필요.

▶ **[Defender for Storage 상세 →](cwp-storage.md)**

참고: [Defender for Storage](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-storage-introduction)

## 3. Defender for Databases

세 하위 플랜으로 구성됩니다.

### 3-1. Defender for Azure SQL Databases

- **대상**: Azure SQL DB·탄력적 풀·관리형 인스턴스·Synapse 전용 SQL 풀, SQL Server 2012~2022(Azure VM·Arc 포함).
- **기능**: 취약성 평가 + 위협 방어 — **SQL 인젝션**, **브루트포스**(성공/실패 분리 경고), 침해 머신에서의 의심 접근.

### 3-2. Defender for Open-Source Relational Databases

- **대상**: Azure PostgreSQL/MySQL Flexible Server(모든 티어). AWS RDS(Aurora PostgreSQL·MySQL, PostgreSQL·MySQL·MariaDB)는 **Preview**.
- **탐지**: 이상 접근·쿼리 패턴(브루트포스), 침해 머신의 의심 활동. *PaaS만 지원, Arc 머신 미지원.*

### 3-3. Defender for Azure Cosmos DB

- **대상**: Cosmos DB **NoSQL API 전용**(Cassandra·MongoDB·Table·Gremlin 미지원).
- **탐지**: SQL 인젝션 변형, 이상 접근(Tor·악성 IP·비정상 위치), 의심스러운 키 목록 조회·데이터 추출. 계정 데이터에 직접 접근하지 않아 **성능 영향 없음**.

▶ **[Defender for Databases 상세 →](cwp-databases.md)**

참고: [Azure SQL](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-sql-introduction) · [오픈소스 DB](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-databases-introduction) · [Cosmos DB](https://learn.microsoft.com/en-us/azure/defender-for-cloud/concept-defender-for-cosmos)

## 4. Defender for Containers

**보호 대상**: Kubernetes 클러스터·노드·워크로드·레지스트리·이미지 — AKS·EKS·GKE·Arc K8s.

**5개 핵심 도메인**
1. **보안 태세 관리** — 에이전트리스 K8s 탐색, 컨트롤 플레인 강화, **Azure Policy for Kubernetes**(허용 전 검증·위반 차단), 취약성 컨텍스트를 클라우드 보안 그래프에 반영
2. **취약성 평가** — 에이전트리스로 레지스트리 이미지·실행 컨테이너·노드 스캔(ACR·ECR·GAR/GCR), 일일 재스캔, 결과는 그래프에 반영. 멀티클라우드 실행 컨테이너 스캔은 **Public Preview**. 발견 결과는 Microsoft 인증서로 서명
3. **런타임 위협 방어** — 60+ K8s 인식 분석·AI·이상 탐지, **MITRE ATT&CK for Containers** 매핑(노출 대시보드/서비스, 고권한 역할, 민감 마운트 등), Defender XDR 통합
4. **소프트웨어 공급망 보호** — **게이트된 배포**(취약성 정책 미충족 이미지 감사/차단)
5. **배포·모니터링** — 센서 누락 클러스터 탐지·대규모 배포

**센서 기반 추가**: 안티맬웨어, DNS 탐지, **바이너리 드리프트 탐지·차단**(비인가 프로세스).

▶ **[Defender for Containers 상세 →](cwp-containers.md)**

참고: [Defender for Containers](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-containers-introduction)

## 5. Defender for App Service

- **대상**: Azure App Service의 모든 웹앱·API(에이전트 불필요, Azure 네이티브).
- **탐지**: 클라우드 규모 가시성으로 요청/응답·내부 로그 분석 → **분산 공격**(여러 호스트의 소규모 IP가 유사 엔드포인트 크롤링) 탐지. MITRE 전술별 — 사전 공격(스캐너), 초기 접근(악성 IP의 FTP 연결), 실행(고권한 명령·파일리스·크립토마이닝).
- **Dangling DNS**: 사이트 해제 후 잔류 DNS 항목 탐지 → **서브도메인 탈취** 방지(Azure DNS·외부 등록기관 모두, Windows·Linux).

참고: [Defender for App Service](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-app-service-introduction)

## 6. Defender for Key Vault

- **대상**: Azure Key Vault(키·인증서·시크릿).
- **탐지**: 비정상·유해한 접근/악용 시도, **도난 자격 증명** 시나리오. 경고에 Object ID, UPN/IP 포함.
- **대응 4단계**: ① 소스 식별(테넌트 내부 여부) → ② 대응(미인가 IP는 방화벽, 미인가 앱/사용자는 접근 정책에서 제거) → ③ 영향 측정 → ④ **영향받은 시크릿·키·인증서 즉시 교체**.

참고: [Defender for Key Vault](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-key-vault-introduction)

## 7. Defender for Resource Manager

- **대상**: ARM 레이어의 **모든 리소스 관리 작업**(포털·REST API·CLI·PowerShell).
- **탐지**: 의심스러운 관리 작업(악성 IP, 안티맬웨어 비활성화, VM 확장의 의심 스크립트), **악용 툴킷**(MicroBurst·PowerZure), 관리 레이어 → 데이터 플레인 **측면 이동**.

참고: [Defender for Resource Manager](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-resource-manager-introduction)

## 8. Defender for APIs

- **대상**: Azure API Management에 게시된 API.
- **기능**: 인벤토리, 보안 결과(외부/미사용/미인증 API), 보안 태세, **데이터 분류**, 런타임 위협 탐지(**OWASP API Top 10** — 데이터 유출·볼류메트릭·비정상 파라미터·트래픽/IP 이상), Defender CSPM(그래프) 통합, SIEM 연계.

▶ **[Defender for APIs 상세 →](cwp-apis.md)**

참고: [Defender for APIs](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-apis-introduction)

## 9. Defender for AI Services

- **대상**: Azure OpenAI·Azure AI Model Inference 모델(현재 **텍스트 토큰만**).
- **탐지**: 데이터 유출·포이즈닝, **탈옥(Jailbreak)**, 자격 증명 도난, **프롬프트 인젝션**. Azure AI Content Safety **Prompt Shields** + Microsoft 위협 인텔리전스, Defender XDR 통합.
- **가용성**: 상용 클라우드만(Azure Government·21Vianet·AWS 연결 계정 미지원). 30일 체험(월 750억 토큰 캡). 활성화에 구독 Owner 필요.

▶ **[Defender for AI 상세 →](cwp-ai.md)**

참고: [AI 위협 보호](https://learn.microsoft.com/en-us/azure/defender-for-cloud/ai-threat-protection)

## 10. 보안 경고 · 인시던트

**경고**는 유료 워크로드 플랜이 위협 탐지 시 생성 — 영향 리소스·설명·수정 단계 포함, 포털에 **90일** 표시(리소스 삭제 후에도).

| 심각도 | 의미 | 예시 |
| --- | --- | --- |
| **High** | 침해 가능성 높음 | Mimikatz 등 악성 도구 |
| **Medium** | 의심 활동(ML/이상) | 비정상 위치 로그인 |
| **Low** | 무해/차단된 공격 가능성 | 로그 삭제 |
| **Informational** | 단독으론 낮으나 인시던트 맥락서 의미 | 상관 신호 |

**인시던트**는 관련 경고를 kill chain 패턴으로 묶은 것 — AI로 상관해 **알림 피로**를 줄이고 공격 스토리를 재구성(테넌트 간 분석 포함).

**탐지 스택** · **매핑** · **내보내기**

- **탐지 스택**: Microsoft 글로벌 위협 인텔리전스 + 행위 분석(ML) + 이상 탐지(배포별 기준선).
- **MITRE ATT&CK 매핑** — Containers는 ATT&CK for Containers 특화.
- **SIEM 내보내기**: CSV / 지속 내보내기(Event Hubs·Log Analytics) / **Microsoft Sentinel 커넥터**.

참고: [보안 경고 개요](https://learn.microsoft.com/en-us/azure/defender-for-cloud/alerts-overview)

---

## 워크로드 플랜 요약 (GA / Preview)

| 플랜 | 상태 | 비고 |
| --- | --- | --- |
| Servers P1/P2 · Storage · App Service · Key Vault · Resource Manager · APIs | GA | — |
| Azure SQL · Cosmos DB(NoSQL) · 오픈소스 DB(Azure) | GA | — |
| 오픈소스 DB(AWS RDS) | **Preview** | — |
| Containers | GA | 멀티클라우드 실행 컨테이너 스캔은 **Preview** |
| AI Services | GA | 텍스트 토큰만, 상용 클라우드 |

---

## 참고 링크

- [Defender for Servers](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-servers-overview)
- [Defender for Storage](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-storage-introduction)
- [Defender for Databases](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-databases-introduction)
- [Defender for Containers](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-containers-introduction)
- [Defender for APIs](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-apis-introduction)
- [AI 위협 보호](https://learn.microsoft.com/en-us/azure/defender-for-cloud/ai-threat-protection)
- [보안 경고 개요](https://learn.microsoft.com/en-us/azure/defender-for-cloud/alerts-overview)

---

### 다음 읽을거리

| ◀ 이전 | ▶ 다음 |
| :-- | --: |
| [02 · CSPM](./02-cspm.md) | [04 · DevSecOps](./04-devsecops.md) |

[🏠 전체 목차로 돌아가기](./README.md)
