[🏠 전체 목차](./README.md)　·　**Part 1 · 시작하기**　·　페이지 1 / 8

# 00 · 개요 — Microsoft Defender for Cloud란?

> [!NOTE]
> **이 페이지에서 얻는 것**
> - Defender for Cloud가 무엇이고 왜 필요한지
> - CNAPP의 3대 축(CSPM · CWP · DevSecOps)이 어떻게 나뉘는지
> - 멀티클라우드·하이브리드를 어떻게 보호하며, 어디서 접근하는지
> - 프롬프트가 아닌 "리소스 → 신호 → 조치"로 이어지는 동작 원리
>
> ⏱️ 예상 소요 **7분**　·　🎯 대상: 모든 독자(입문)

처음 읽는 분을 위해, 이 페이지는 **Defender for Cloud가 무엇이고, 어떻게 동작하며, 무엇을 할 수 있는지**를 설명합니다.
(각 용어의 실체는 [02 · 핵심 개념](./02-concepts.md)부터, 각 축의 상세 기능은 Part 2에서 깊이 있게 다룹니다.)

---

## 한 문장으로

Microsoft Defender for Cloud는 **CNAPP(Cloud Native Application Protection Platform)** 로, **여러 클라우드 보안 도구를 하나로 결합해 애플리케이션을 전체 수명 주기에 걸쳐 보호**하는 통합 솔루션입니다.

즉, **개발 단계부터(코드) → 클라우드 구성(인프라) → 실행 중인 워크로드(런타임)** 까지, 서로 다른 계층의 보안을 한 콘솔에서 다룹니다. 대상은 SOC 분석가, 클라우드 보안 엔지니어, IT/보안 관리자, DevSecOps 담당자, CISO 등 다양합니다.

## 왜 필요한가? (해결하는 문제)

클라우드 보안은 **환경 분산, 구성 오류, 런타임 위협, 도구 파편화**에 시달립니다.

- **멀티클라우드/하이브리드 복잡성** — 여러 클라우드 제공업체에 자산이 퍼지면서 보안을 중앙화하기 어렵습니다. Defender for Cloud는 **모든 환경의 보호를 하나의 대시보드에서** 관리합니다.
- **코드에서 런타임까지** — 애플리케이션은 코드·인프라·런타임 계층 모두에서 보안이 필요합니다. Defender for Cloud는 **운영에 배포된 뒤가 아니라, 개발 파이프라인 앞단에 보안을 내재화**해 배포 전에 구성 오류·위험을 찾아냅니다.
- **태세 + 워크로드 보호** — "제대로 구성됐는가(태세)"와 "지금 공격받고 있는가(위협)"를 하나의 CNAPP 경험으로 통합합니다.

참고: [Defender for Cloud 소개](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-cloud-introduction)

## CNAPP의 3대 축

Defender for Cloud는 세 가지 축을 하나로 묶습니다. **Part 2에는 이 세 축을 각각 한 문서로 나눠** 설명합니다.

| 축 | 한 줄 정의 | 답하는 질문 | Part |
| --- | --- | --- | --- |
| **CSPM** — 클라우드 보안 태세 관리 | 클라우드 리소스의 보안 태세를 점검·개선 | "내 리소스가 올바르게 구성됐는가?" | Part 2 · 03 |
| **CWP / CWPP** — 클라우드 워크로드 보호 | VM·컨테이너·스토리지·DB·서버리스 등 워크로드를 위협으로부터 방어 | "지금 누가 내 리소스를 공격하고 있는가?" | Part 2 · 04 |
| **DevSecOps** — 개발 보안 운영 | 멀티 파이프라인 환경 전반의 코드 수준 보안 관리 | "배포 전 코드·인프라에 위험이 있는가?" | Part 2 · 05 |

> [!IMPORTANT]
> **CSPM은 "예방(구성·컴플라이언스)", CWP는 "탐지·대응(런타임 위협)"** 입니다. 성격이 완전히 다르며, 과금 방식도 다릅니다(→ [01 · 사전 준비](./01-prerequisites.md)).

### 축 ① CSPM — 클라우드 보안 태세 관리

Defender for Cloud는 Azure 구독, AWS 계정, GCP 프로젝트를 보안 표준에 맞춰 **지속적으로 평가**하고, 구성 오류·위험을 줄이는 **보안 권장사항**을 제공합니다. CSPM은 두 계층으로 제공됩니다.

- **기본 CSPM(무료)** — 보안 점수, 자산 인벤토리, 보안 권장사항, 멀티클라우드 커버리지, 중앙 정책 관리. **온보딩 시 기본 활성화.**
- **Defender CSPM(유료)** — 에이전트리스 취약성 스캔, **공격 경로 분석**, 클라우드 보안 탐색기/그래프, 데이터 보안 태세 관리(DSPM), 규정 준수 평가, AI 보안 태세, 거버넌스 규칙, 위험 우선순위화 등.

참고: [CSPM 개요](https://learn.microsoft.com/en-us/azure/defender-for-cloud/concept-cloud-security-posture-management)

### 축 ② CWP / CWPP — 클라우드 워크로드 보호

환경이 위협받으면 **보안 경고가 즉시 위협의 성격과 심각도**를 알려 대응을 계획하게 합니다. CWP는 **워크로드별 Defender 플랜**으로 구성됩니다.

| Defender 플랜 | 보호 대상 |
| --- | --- |
| Defender for Servers | Azure·AWS·GCP·온프렘의 Windows/Linux VM |
| Defender for Containers | Kubernetes 클러스터·노드, 컨테이너 이미지·레지스트리 |
| Defender for Storage | 악성코드 업로드, 민감 데이터 유출, SAS 토큰 오용 |
| Defender for Databases | Azure SQL, 오픈소스 관계형 DB, Cosmos DB |
| Defender for App Service | Azure App Service의 웹앱·API |
| Defender for Key Vault | Key Vault 접근 이상 징후 |
| Defender for Resource Manager | 비정상 리소스 관리 작업 |
| Defender for APIs | API 가시성·태세·실시간 위협 탐지 |
| Defender for AI Services | 생성형 AI 워크로드 위협 탐지 |

참고: [Defender for Cloud 소개](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-cloud-introduction)

### 축 ③ DevSecOps — 개발 보안 운영

Defender for Cloud의 DevOps 보안은 하나의 중앙 콘솔에서, Azure DevOps·GitHub·GitLab 등 여러 파이프라인 환경에 걸쳐 보안팀이 코드에서 클라우드까지(code to cloud) 애플리케이션과 리소스를 보호하도록 돕습니다.

세 가지 핵심 역량:

1. **DevOps 보안 태세 통합 가시성** — 멀티 파이프라인·멀티클라우드 전반의 DevOps 인벤토리와 프로덕션 이전 코드 보안(코드·시크릿·오픈소스 종속성 스캔) 가시성.
2. **개발 수명 주기 전반의 클라우드 구성 강화** — IaC 템플릿·컨테이너 이미지를 사전에 보안하여 프로덕션에 도달하는 구성 오류를 최소화.
3. **코드 내 중대 이슈 우선순위 수정** — 코드 투 클라우드 컨텍스트로 우선순위화하고, **PR(풀 리퀘스트) 주석**으로 개발자에게 소유권 할당.

참고: [DevOps 보안 소개](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-devops-introduction)

## 멀티클라우드 · 하이브리드 지원

Defender for Cloud는 Azure 네이티브이면서, 커넥터로 **다른 클라우드**를, Azure Arc로 **온프렘**을 지원합니다.

| 환경 | 지원 방식 |
| --- | --- |
| **Azure** | 네이티브 내장 — 커넥터 불필요 |
| **AWS** | 네이티브 커넥터 (AWS CloudFormation 또는 Terraform) |
| **GCP** | 네이티브 커넥터 (GCP 온보딩 스크립트) |
| **온프렘 / 기타 클라우드** | **Azure Arc 지원 서버**로 온보딩 |

- 커넥터는 **페더레이션과 단기 자격 증명**을 사용해 장기 시크릿을 저장하지 않습니다.
- AWS·GCP 커넥터는 EC2/GCE 인스턴스의 **Azure Arc 배포를 자동으로 처리**합니다.

참고: [멀티클라우드 계획](https://learn.microsoft.com/en-us/azure/defender-for-cloud/plan-multicloud-security-get-started) · [AWS 온보딩](https://learn.microsoft.com/en-us/azure/defender-for-cloud/quickstart-onboard-aws) · [온프렘/Arc 온보딩](https://learn.microsoft.com/en-us/azure/defender-for-cloud/quickstart-onboard-machines)

## 어디서 접근하는가 (포털 · XDR 통합)

Defender for Cloud는 **두 포털**에서 접근합니다.

| 포털 | 주소 | 특징 |
| --- | --- | --- |
| **Azure 포털** | portal.azure.com → *Microsoft Defender for Cloud* | 전통적·1차 접근점. 플랜 활성화, 환경 설정, 개요 대시보드(보안 점수·경고·권장사항·규정 준수·자산 인벤토리) |
| **Microsoft Defender 포털** | security.microsoft.com | **통합 XDR 경험**. MDC가 이 포털로 기능을 확장 중 |

**Defender XDR 통합 핵심**

- MDC의 **모든 인시던트·경고**(멀티클라우드·내부·외부 공급자)가 Defender 포털의 경고 큐에 통합됩니다.
- 리소스는 Azure / Amazon / Google Cloud 리소스로 명확히 식별됩니다.
- 경고 상태 변경은 MDC ↔ Defender XDR 간 **양방향 동기화**됩니다.
- **고급 헌팅(Advanced hunting)** 이 `CloudAuditEvents`, `CloudProcessEvents`, `CloudStorageAggregatedEvents` 테이블로 확장됩니다.
- 알림 피로를 줄이기 위해 **정보성(Informational) 경고는 Defender 포털로 통합되지 않습니다**(설계상).

> [!NOTE]
> **XDR 통합 사전 요구사항** — ① Azure 구독에 Defender for Cloud 활성화, ② Defender 포털에서 보이는 경고는 **활성화된 Defender 플랜에 따라** 달라짐, ③ **Defender XDR 통합 RBAC** 역할(또는 Entra ID **전역 관리자·보안 관리자**)이 있어야 경고·상관을 조회할 수 있습니다. 자세한 요건은 [01 · 사전 준비](./01-prerequisites.md)를 참고하세요.

참고: [Defender XDR 통합](https://learn.microsoft.com/en-us/azure/defender-for-cloud/concept-integration-365)

## 어떻게 동작하는가 (아키텍처)

Defender for Cloud의 처리 흐름은 다음과 같습니다.

```
① 리소스 온보딩 — Azure(네이티브) / AWS·GCP(커넥터) / 온프렘(Azure Arc)
        ▼
② 데이터 수집 — 에이전트리스 스캔(디스크 스냅샷) + 에이전트 기반(MDE) + Azure Policy 평가
        ▼
③ 지속 평가 — MCSB 등 보안 표준 대비 8시간마다 재계산 → 보안 권장사항·보안 점수
        ▼
④ 위협 탐지 — 유료 Defender 플랜이 위협 인텔리전스·행위 분석·ML로 보안 경고 생성
        ▼
⑤ 컨텍스트화 — 클라우드 보안 그래프 → 공격 경로 분석, 경고를 인시던트로 상관
        ▼
⑥ 조치·통합 — 포털 조치, 워크플로 자동화(Logic Apps), Defender XDR·Sentinel(SIEM)로 스트리밍
```

- **에이전트리스 스캔** — VM 디스크의 스냅샷을 떠 대역 외(out-of-band)로 심층 분석하고, 메타데이터 추출 후 스냅샷을 **즉시 삭제**합니다(수 분 내). 에이전트 설치·네트워크 연결이 필요 없고 성능 영향이 없습니다.
- **Azure Policy** — 각 MCSB 권장사항은 Azure Policy 정의로 매핑되어 평가를 뒷받침합니다.
- **무료 vs 유료** — 기본 CSPM(무료)은 태세 가시성을, 유료 플랜은 공격 경로·규정 준수·런타임 위협 탐지를 제공합니다.

참고: [에이전트리스 스캔](https://learn.microsoft.com/en-us/azure/defender-for-cloud/concept-agentless-data-collection) · [모니터링 구성 요소(에이전트/데이터 수집)](https://learn.microsoft.com/en-us/azure/defender-for-cloud/monitoring-components) · [AWS 온보딩](https://learn.microsoft.com/en-us/azure/defender-for-cloud/quickstart-onboard-aws) · [GCP 온보딩](https://learn.microsoft.com/en-us/azure/defender-for-cloud/quickstart-onboard-gcp) · [온프렘/Arc 온보딩](https://learn.microsoft.com/en-us/azure/defender-for-cloud/quickstart-onboard-machines) · [보안 점수·MCSB 평가](https://learn.microsoft.com/en-us/azure/defender-for-cloud/secure-score-security-controls) · [보안 정책·Azure Policy](https://learn.microsoft.com/en-us/azure/defender-for-cloud/security-policy-concept) · [보안 경고(위협 탐지)](https://learn.microsoft.com/en-us/azure/defender-for-cloud/alerts-overview) · [클라우드 보안 그래프·공격 경로](https://learn.microsoft.com/en-us/azure/defender-for-cloud/concept-attack-path) · [워크플로 자동화](https://learn.microsoft.com/en-us/azure/defender-for-cloud/workflow-automation) · [Defender XDR 통합](https://learn.microsoft.com/en-us/azure/defender-for-cloud/concept-integration-365)

## 주요 기능 및 가치

| 성과 | 설명 | 필요 플랜 |
| --- | --- | --- |
| **보안 점수(Secure Score)** | 보안 상태를 단일 지표로 집계 — 높을수록 위험 낮음 | 기본 CSPM |
| **보안 경고·인시던트** | 워크로드 플랜이 위협 탐지, 관련 경고를 인시던트로 상관 | 유료 워크로드 플랜 |
| **공격 경로 분석** | 클라우드 보안 그래프로 외부에서 핵심 자산까지의 악용 경로 식별 | Defender CSPM |
| **규정 준수** | NIST·PCI DSS·ISO 27001·CIS 등 표준 대비 지속 평가 | Defender CSPM |
| **DevOps 보안** | 코드 투 클라우드 가시성, PR 주석 | 기본 CSPM / Defender CSPM |

> [!NOTE]
> Defender for Cloud는 **AI/생성형 AI 워크로드**까지 CNAPP 범위를 확장했습니다 — AI 보안 태세 관리(AI-SPM)와 AI 위협 보호(프롬프트 인젝션·탈옥·데이터 유출 탐지). ([참고](https://learn.microsoft.com/en-us/azure/defender-for-cloud/ai-threat-protection))

---

## 이 코스에서 경험하는 것

```
사전 준비(플랜·역할) ─▶ 온보딩(멀티클라우드) ─▶ 태세 관리(CSPM) ─▶ 워크로드 보호(CWP) ─▶ 코드 보안(DevSecOps) ─▶ 조사·자동화·XDR
```

Part 2에서 3대 축을 각각 깊이 다루고, Part 3에서 실제 포털 실습으로 이어집니다.

---

## 참고 링크

- [Microsoft Defender for Cloud 소개](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-cloud-introduction)
- [CSPM 개요](https://learn.microsoft.com/en-us/azure/defender-for-cloud/concept-cloud-security-posture-management)
- [DevOps 보안 소개](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-devops-introduction)
- [멀티클라우드 보안 계획](https://learn.microsoft.com/en-us/azure/defender-for-cloud/plan-multicloud-security-get-started)
- 온보딩: [AWS](https://learn.microsoft.com/en-us/azure/defender-for-cloud/quickstart-onboard-aws) · [GCP](https://learn.microsoft.com/en-us/azure/defender-for-cloud/quickstart-onboard-gcp) · [온프렘/Arc](https://learn.microsoft.com/en-us/azure/defender-for-cloud/quickstart-onboard-machines)
- 데이터 수집: [에이전트리스 스캔](https://learn.microsoft.com/en-us/azure/defender-for-cloud/concept-agentless-data-collection) · [모니터링 구성 요소](https://learn.microsoft.com/en-us/azure/defender-for-cloud/monitoring-components)
- 평가·탐지: [보안 점수·MCSB](https://learn.microsoft.com/en-us/azure/defender-for-cloud/secure-score-security-controls) · [보안 정책·Azure Policy](https://learn.microsoft.com/en-us/azure/defender-for-cloud/security-policy-concept) · [보안 경고](https://learn.microsoft.com/en-us/azure/defender-for-cloud/alerts-overview)
- 컨텍스트·조치: [클라우드 보안 그래프·공격 경로](https://learn.microsoft.com/en-us/azure/defender-for-cloud/concept-attack-path) · [워크플로 자동화](https://learn.microsoft.com/en-us/azure/defender-for-cloud/workflow-automation)
- [Defender XDR 통합](https://learn.microsoft.com/en-us/azure/defender-for-cloud/concept-integration-365)

---

### 다음 읽을거리

| ◀ 이전 | ▶ 다음 |
| :-- | --: |
| [🏠 전체 목차](./README.md) | [01 · 사전 준비](./01-prerequisites.md) |

[🏠 전체 목차로 돌아가기](./README.md)
