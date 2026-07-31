[🏠 전체 목차](./README.md)　·　**Part 1 · 시작하기**　·　페이지 2 / 5

# 01 · 사전 준비 — 플랜 · 온보딩 · 역할/RBAC

> [!NOTE]
> **이 페이지에서 얻는 것**
> - 기본 MDC 기능과 유료 Defender 플랜의 차이, 그리고 전체 플랜 목록
> - Azure·AWS·GCP·온프렘(Arc) 온보딩 경로
> - Defender for Cloud를 다루는 데 필요한 **Azure RBAC 역할**
> - **Microsoft Defender XDR 통합**에 필요한 권한·조건
> - 데이터 지역·리전, 정부 클라우드 제약
>
> ⏱️ 예상 소요 **11분**　·　🎯 대상: 보안/IT 관리자, 도입 담당자, 클라우드 엔지니어

Defender for Cloud 온보딩은 **① 구독에서 MDC 열기(기본 CSPM 자동 활성화) → ② 필요한 유료 플랜 켜기 → ③ 멀티클라우드/온프렘 커넥터 구성** 순으로 진행합니다. 각 단계에는 적절한 **Azure RBAC 역할**이 전제되므로(§4), 시작 전 역할 배분을 함께 계획하세요. 이 페이지는 온보딩 사전 준비를 정리합니다.

---

## 1. 활성화 기본 — 무료 vs 유료

### 무료: MDC 기본 기능 (자동)

<img width="1835" height="855" alt="image" src="https://github.com/user-attachments/assets/7d78a239-f636-42a2-92e3-2fd5ec3cd79f" />

Microsoft Defender for Cloud를 켜면 별도 구독 없이 **기본 CSPM(태세 관리)** 과 **Microsoft Defender XDR 접근**이 자동으로 제공됩니다. 아래 기능이 여기에 포함되며, 공격 경로 분석·DSPM·워크로드 위협 탐지 같은 심화 기능은 유료 플랜(**Defender CSPM** 또는 워크로드 Defender 플랜)에서 제공됩니다.

- 보안 점수(Secure Score)
- 자산 인벤토리(Asset inventory)
- 보안 권장사항(Recommendations)
- Workbooks
- 규정 준수 — **Microsoft 클라우드 보안 벤치마크(MCSB)**
- Microsoft Defender XDR 접근

참고: [Azure 구독 연결](https://learn.microsoft.com/en-us/azure/defender-for-cloud/connect-azure-subscription)

### 유료: Defender 플랜 (30일 무료 체험)

- 대부분의 유료 플랜은 **첫 30일 무료 체험**(또는 사용 한도 도달 시점까지).
- **예외:** Defender for Storage의 **악성코드 스캔(Malware scanning)** 은 체험에 포함되지 않고 **첫날부터 과금**됩니다.

> [!TIP]
> 정확한 단가는 정책상 여기서 재현하지 않습니다. 최신 금액은 [Defender for Cloud 가격 페이지](https://azure.microsoft.com/pricing/details/defender-for-cloud/)와 [비용 계산기](https://learn.microsoft.com/en-us/azure/defender-for-cloud/cost-calculator)에서 확인하세요.

### 전체 Defender for Cloud 플랜 목록

각 플랜의 상세 기능은 Part 2(CSPM·CWP·DevSecOps)에서 다룹니다.

| 플랜 | 보호 대상 · 핵심 | 상태 |
| --- | --- | --- |
| **기본 CSPM** (무료) | 자산 인벤토리, 보안 점수, MCSB 권장/규정 준수, 멀티클라우드 커버리지, DevOps 기본 권장 | GA |
| **Defender CSPM** (유료) | 에이전트리스 취약성/시크릿 스캔, **공격 경로 분석**, 클라우드 보안 탐색기, DSPM, AI-SPM, 거버넌스 규칙, 위험 우선순위화, EASM, PR 주석 | GA |
| **Defender for Servers** — Plan 1 | Windows/Linux VM용 EDR(MDE 통합), 에이전트 기반 취약성 스캔, 소프트웨어 인벤토리 | GA |
| **Defender for Servers** — Plan 2 | P1 + 에이전트리스 취약성/악성코드/시크릿 스캔, DNS 경고, OS 기준선 평가, **FIM**, **JIT VM 액세스**, 일 500MB 무료 수집 | GA |
| **Defender for Storage** | Blob/Files/Data Lake — 악성코드 스캔, 민감 데이터 위협 탐지, 활동 모니터링, 해시 평판 분석 | GA |
| **Defender for Databases** — Azure SQL | Azure SQL DB·탄력적 풀·MI·Synapse·SQL on VM/Arc — 취약성 평가 + 위협 보호 | GA |
| **Defender for Databases** — 오픈소스 관계형 DB | Azure PostgreSQL/MySQL Flexible Server, Amazon RDS(**AWS는 Preview**) | GA / Preview |
| **Defender for Databases** — Cosmos DB | Cosmos DB(**NoSQL API 전용**) — SQL 인젝션·이상 접근 탐지 | GA |
| **Defender for Containers** | AKS·EKS·GKE·Arc K8s — 태세, 취약성 평가, 런타임 위협, 공급망 보호 (**Arc 센서는 Preview**) | GA / Preview |
| **Defender for App Service** | Azure App Service 웹앱·API — 공격 탐지, dangling DNS 탐지 | GA |
| **Defender for Key Vault** | Key Vault 접근 이상·탈취 자격 증명 시나리오 탐지 | GA |
| **Defender for Resource Manager** | ARM 관리 작업 — 악용 툴킷(Microburst, PowerZure), 측면 이동 탐지 | GA |
| **Defender for APIs** | Azure API Management 게시 API — 인벤토리·태세·OWASP API Top 10 탐지 | GA |
| **Defender for AI Services** | 생성형 AI(Azure OpenAI 등) — 탈옥·프롬프트 인젝션·데이터 유출 실시간 탐지 (**상용 클라우드 전용**) | GA |

참고: [Defender for Cloud 소개](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-cloud-introduction)

> [!NOTE]
> **Defender for Servers Plan 1 vs Plan 2 배포 범위**
> - **P1**: 리소스(서버) 수준으로 켜고 끌 수 있음
> - **P2**: 구독 수준으로 활성화, 리소스별로 끌 수는 있으나 리소스 수준으로만 켤 수는 없음. **에이전트리스 스캔·악성코드 스캔·FIM·JIT** 등 고급 기능 포함
> 참고: [Defender for Servers 개요](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-servers-overview)

### 유료 플랜 활성화 방법 (Azure 포털)

<img width="1572" height="776" alt="image" src="https://github.com/user-attachments/assets/a22530c7-39b0-49bb-8423-724df09eb09f" />

1. **Azure 포털**(portal.azure.com) 로그인 → **Microsoft Defender for Cloud** 검색·선택
2. 메뉴에서 **환경 설정(Environment settings)**
3. 보호할 **구독 또는 워크스페이스** 선택
4. **모두 사용(Enable all)** 클릭, 또는 개별 플랜 토글
5. **저장(Save)**

> [!NOTE]
> **워크스페이스 수준**으로 켤 수 있는 플랜: **Defender for Servers**, **Defender for SQL servers on machines**. Storage·SQL·오픈소스 관계형 DB는 **구독 또는 리소스 수준**으로 켤 수 있습니다.

참고: [Azure 구독 연결](https://learn.microsoft.com/en-us/azure/defender-for-cloud/connect-azure-subscription) · [향상된 보안 사용](https://learn.microsoft.com/en-us/azure/defender-for-cloud/enable-enhanced-security)

## 2. 멀티클라우드 온보딩

### AWS 커넥터 (GA — 정부 클라우드 제외)

**사전 요건:** MDC가 활성화된 Azure 구독, AWS 계정 접근, 구독의 **Contributor** 권한. (CIEM 사용 시 Security Admin + `Application.ReadWrite.All`)

<img width="1806" height="781" alt="image" src="https://github.com/user-attachments/assets/2d1b58cb-ab88-4363-95be-c067ab234a36" />
<img width="826" height="511" alt="image" src="https://github.com/user-attachments/assets/d56ee7c6-8f55-4ad7-ba40-09e7b8d1aaf6" />
<img width="3270" height="1755" alt="image" src="https://github.com/user-attachments/assets/b8196b2a-b27c-4a3c-b27f-11c4695dc8db" />
<img width="1295" height="836" alt="image" src="https://github.com/user-attachments/assets/baca088d-d424-44af-bc39-5677ac6932b1" />

1. Azure 포털 → MDC → **환경 설정** → **환경 추가** → **Amazon Web Services**
2. 커넥터 유형 선택: **Management account**(하위 계정 자동 프로비저닝) 또는 **Single account**
3. AWS 리전·Azure 구독·리소스 그룹·위치, **스캔 주기(4/6/12/24시간)** 설정
4. AWS 계정 ID 입력 → **플랜 선택**
5. **액세스 구성** → **기본 액세스** 또는 **최소 권한 액세스**
6. 배포 방식: **AWS CloudFormation** 또는 **Terraform** — 생성된 템플릿을 AWS에서 실행(IAM 역할 생성)

**인증:** 연합 신뢰 + 단기 자격 증명(장기 시크릿 미저장).

참고: [AWS 온보딩](https://learn.microsoft.com/en-us/azure/defender-for-cloud/quickstart-onboard-aws)

### GCP 커넥터 (GA)

**사전 요건:** MDC 활성 Azure 구독, GCP 프로젝트/조직 접근, 구독의 **Contributor** 권한.

<img width="2525" height="1120" alt="image" src="https://github.com/user-attachments/assets/3d16bd2f-7e21-419e-b080-7c84fabffdfa" />
<img width="1860" height="915" alt="image" src="https://github.com/user-attachments/assets/87a97e1f-8bff-4b7b-9ac2-4945760d8486" />
<img width="2582" height="1862" alt="image" src="https://github.com/user-attachments/assets/e46d8491-2438-493b-9ab5-57dc09f08a6a" />

1. Azure 포털 → MDC → **환경 설정** → **환경 추가** → **Google Cloud Platform**
2. Azure 구독·리소스 그룹·위치·스캔 주기 설정
3. 조직 수준(조직 ID) 또는 프로젝트 수준(프로젝트 번호+ID) 선택
4. **플랜 선택** → **액세스 구성**(기본/최소 권한)
5. GCP Cloud Shell에서 생성된 **`gcloud` 스크립트** 실행 — 워크로드 자격 증명 풀·서비스 계정·정책 바인딩 생성

**인증:** 워크로드 자격 증명 연합 + 서비스 계정 위임(장기 자격 증명 미사용).

참고: [GCP 온보딩](https://learn.microsoft.com/en-us/azure/defender-for-cloud/quickstart-onboard-gcp)

### 온프렘 / 기타 클라우드 — Azure Arc (권장)

- Azure Arc로 연결하면 온프렘 머신이 **Azure VM처럼 MDC에 표시**됩니다.
- AWS·GCP 커넥터는 EC2/GCE의 **Arc 배포를 자동 처리**합니다.
- Arc 없이 직접 온보딩(direct onboarding) 시 **Defender for Servers Plan 2의 전체 기능에 접근할 수 없습니다.**

참고: [온프렘/Arc 온보딩](https://learn.microsoft.com/en-us/azure/defender-for-cloud/quickstart-onboard-machines)

## 3. 데이터 수집 · Log Analytics 워크스페이스

Defender for Cloud는 세 가지 방식으로 데이터를 수집합니다.

- **에이전트리스 스캔** (Defender CSPM / Defender for Servers P2) — 디스크 스냅샷 기반, 에이전트·네트워크 불필요. 취약성·악성코드·시크릿·소프트웨어 인벤토리.
- **Microsoft Defender for Endpoint(MDE) 에이전트** — Defender for Servers의 실시간 위협 탐지·FIM·취약성 평가의 기본 수단.
- **Azure Monitor Agent(AMA)** — Defender for SQL servers on machines, 그리고 Servers P2의 **일 500MB 무료 수집** 혜택에 사용.

> [!WARNING]
> **레거시 Log Analytics 에이전트(MMA)는 2024년에 은퇴**했습니다. 기존 MMA 기반 기능은 MDE 통합 또는 에이전트리스 스캔으로 전환되었습니다. FIM·JIT 등은 이제 **MDE 기반**입니다.

**Log Analytics 워크스페이스가 필요한 경우**

- **파일 무결성 모니터링(FIM)** (Servers P2)
- **Defender for SQL servers on machines** (AMA를 워크스페이스에 배포)
- **일 500MB 무료 수집** 혜택 (AMA + 워크스페이스 연결)

참고: [모니터링 구성 요소](https://learn.microsoft.com/en-us/azure/defender-for-cloud/monitoring-components) · [Servers 데이터 워크스페이스 계획](https://learn.microsoft.com/en-us/azure/defender-for-cloud/plan-defender-for-servers-data-workspace)

## 4. 역할 및 권한 (Azure RBAC)

Defender for Cloud는 **Azure RBAC**를 사용합니다. 전용 역할 2종과 표준 Azure 역할(Contributor/Owner)이 함께 쓰입니다.

| 역할 | 할 수 있는 일 |
| --- | --- |
| **Security Reader** | 읽기 전용 — 권장사항·경고·보안 정책·상태 조회. **변경 불가** |
| **Security Admin** | Security Reader + 보안 정책 업데이트, 경고·권장사항 무시(dismiss), **Defender 플랜 사용/해제** |
| **Contributor** (리소스 그룹/구독) | 권장사항 적용(Fix), 이메일 알림 구성, 경고 무시, GitHub 이슈 생성/할당 |
| **Owner** (구독) | 위 전부 + 이니셔티브 추가/할당, 보안 정책 편집, **플랜의 모든 기능 활성화** |

### 역할–작업 매트릭스 (요약)

| 작업 | Security Reader | Security Admin | Contributor (구독) | Owner (구독) |
| --- | --- | --- | --- | --- |
| 경고·권장사항 조회 | ✅ | ✅ | ✅ | ✅ |
| 권장사항 적용(Fix) | ❌ | ❌ | ✅ | ✅ |
| **Defender 플랜 사용/해제** | ❌ | ✅ | ✅ | ✅ |
| 경고 무시(dismiss) | ❌ | ✅ | ✅ | ✅ |
| 이니셔티브(규정 표준) 추가/할당 | ❌ | ✅ | ❌ | ✅ |
| 보안 정책 편집 | ❌ | ✅ | ❌ | ✅ |
| 권장사항 예외(exempt) 처리 | ❌ | ✅ | ❌ | ✅ |

> [!IMPORTANT]
> 세 역할(Security Admin, 구독 Contributor, 구독 Owner)로 플랜을 켜고 끌 수는 있지만, **플랜의 "모든 기능"을 활성화하려면 Owner 역할이 필요**합니다.
> 참고: [권한(Permissions)](https://learn.microsoft.com/en-us/azure/defender-for-cloud/permissions)

### 구성 요소별 추가 권한

| 구성 요소 | 필요 역할 |
| --- | --- |
| Azure Monitor Agent(AMA) 배포 | Owner (구독 수준) |
| MDE 통합 사용/해제 | Security Admin 또는 Owner |
| 취약성 평가(Vulnerability Assessment) | Owner (구독 수준) |
| Defender for Containers 확장(AKS/Arc) | Owner 또는 User Access Administrator |
| CIEM(AWS·GCP 권한 관리) | Security Admin + 테넌트 `Application.ReadWrite.All` |

참고: [모니터링 구성 요소](https://learn.microsoft.com/en-us/azure/defender-for-cloud/monitoring-components)

## 5. Microsoft Defender XDR 통합 요구사항

Defender for Cloud를 켜면 경고·인시던트가 **Microsoft Defender 포털(security.microsoft.com)** 의 통합 XDR 경험으로 자동 연동됩니다(→ [00 · 개요](./00-overview.md)). 이 통합을 실제로 조회·활용하려면 다음이 필요합니다.

| 요구사항 | 내용 |
| --- | --- |
| **① Defender for Cloud 활성화** | Azure 구독에서 Defender for Cloud가 활성화되어 있어야 함 |
| **② 플랜에 따른 경고 접근** | Defender 포털에서 보이는 경고는 **활성화된 Defender(워크로드) 플랜에 따라** 달라짐 — 유료 플랜이 있어야 해당 위협 경고가 표시 |
| **③ 권한(RBAC)** | **Defender XDR 통합 RBAC** 역할을 적용해야 사용 가능. 이 역할 없이 보려면 Entra ID의 **전역 관리자(Global Administrator)** 또는 **보안 관리자(Security Administrator)** 필요 |

> [!NOTE]
> 경고·상관 조회 권한은 **테넌트 전체에 자동 부여**되며, **특정 구독만 제한 조회는 지원되지 않습니다.** 구독별로 보려면 경고·인시던트 큐에서 **`alert subscription ID` 필터**를 사용하세요.

> [!TIP]
> "Defender 포털에 경고가 안 보인다"는 대부분 ② 플랜 미활성 또는 ③ 권한 부족이 원인입니다. 도입 시 두 항목을 먼저 점검하세요.

참고: [Defender XDR 통합](https://learn.microsoft.com/en-us/azure/defender-for-cloud/concept-integration-365)

## 6. 데이터 지역 · 리전 · 정부 클라우드

- **자산 데이터 저장 위치**는 VM 지리에 따라 가장 가까운 워크스페이스로 라우팅됩니다. (예: **한국(Korea) → Asia Pacific**, 영국 → 영국, 유럽(영국 제외) → 유럽)
- **태세 데이터**는 테넌트 위치 기준으로 저장됩니다(유럽 테넌트는 유럽 위치).
- **클라우드 보안 그래프** 원시 데이터는 14일, 계산된 데이터(공격 경로)는 추가 14일, AI 위협 보호(프롬프트/응답)는 30일 보존.

> [!WARNING]
> **정부/제한 클라우드 제약**: AWS 커넥터, AMA 자동 프로비저닝, AI 위협 보호, Containers용 Defender 센서, MDE Linux 통합 등은 **Azure Government / 21Vianet 운영 Azure에서 제공되지 않습니다.** 또한 **Azure 중국 리전의 MDC는 2026년 10월 1일 은퇴** 예정입니다.

참고: [데이터 보안](https://learn.microsoft.com/en-us/azure/defender-for-cloud/data-security) · [모니터링 구성 요소](https://learn.microsoft.com/en-us/azure/defender-for-cloud/monitoring-components)

---

## 사전 준비 체크리스트

- [ ] 보호할 **Azure 구독** 확인, MDC 열어 기본 CSPM(무료) 활성화
- [ ] 필요한 **유료 Defender 플랜** 결정(Servers/Storage/Databases/Containers/…) 및 30일 체험 인지
- [ ] **Owner / Security Admin / Contributor** 역할 배분 (플랜 전체 기능은 Owner 필요)
- [ ] 멀티클라우드 대상 시: **AWS/GCP 커넥터** 사전 요건(Contributor, CIEM용 추가 권한) 확보
- [ ] 온프렘 대상 시: **Azure Arc** 온보딩 계획(직접 온보딩 시 P2 제한 인지)
- [ ] FIM·SQL on machines·무료 수집 필요 시 **Log Analytics 워크스페이스** 준비
- [ ] **Defender XDR 통합** 조회 권한 확보(전역/보안 관리자 또는 XDR 통합 RBAC) 및 플랜별 경고 접근 인지
- [ ] **데이터 지역/리전** 및 정부·중국 클라우드 제약 검토

---

## 참고 링크

- [Azure 구독 연결 / 활성화](https://learn.microsoft.com/en-us/azure/defender-for-cloud/connect-azure-subscription)
- [향상된 보안 기능 사용](https://learn.microsoft.com/en-us/azure/defender-for-cloud/enable-enhanced-security)
- [권한(Permissions)](https://learn.microsoft.com/en-us/azure/defender-for-cloud/permissions)
- [모니터링 구성 요소](https://learn.microsoft.com/en-us/azure/defender-for-cloud/monitoring-components)
- [AWS 온보딩](https://learn.microsoft.com/en-us/azure/defender-for-cloud/quickstart-onboard-aws) · [GCP 온보딩](https://learn.microsoft.com/en-us/azure/defender-for-cloud/quickstart-onboard-gcp) · [온프렘/Arc](https://learn.microsoft.com/en-us/azure/defender-for-cloud/quickstart-onboard-machines)
- [가격](https://azure.microsoft.com/pricing/details/defender-for-cloud/) · [비용 계산기](https://learn.microsoft.com/en-us/azure/defender-for-cloud/cost-calculator)

---

### 다음 읽을거리

| ◀ 이전 | ▶ 다음 |
| :-- | --: |
| [00 · 개요](./00-overview.md) | [02 · CSPM](./02-cspm.md) |

[🏠 전체 목차로 돌아가기](./README.md)
