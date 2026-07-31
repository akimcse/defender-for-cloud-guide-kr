[🏠 전체 목차](README.md)　·　**Part 2 · 핵심 기능**　·　[03 · CWP](03-cwp.md)　·　**Defender for Containers**

# Defender for Containers

> [!NOTE]
> **보호 대상**: Kubernetes 클러스터·노드·워크로드·레지스트리·이미지 — AKS·EKS·GKE·Arc K8s.
> **코드에서 런타임까지** 컨테이너 수명 주기 전 단계를 보호합니다. 빌드 시 스캔·게이팅으로 안전한 이미지를 배포하고, 에이전트리스로 태세를 관리하며, 런타임에서 위협을 탐지·차단하고 Defender XDR로 조사·대응합니다.
>
> ⏱️ 예상 소요 **7분**

## ① 안전한 공급망 (빌드 시)

### 빌드 시 이미지 스캔 · 게이팅

- **조기 스캔** — CI/CD 파이프라인(Azure·서드파티)을 CLI 도구로 스캔해 취약 이미지를 조기에 탐지·차단.
- **레지스트리 스캔** — 퍼블릭 클라우드·서드파티 레지스트리(ACR·ECR·GAR/GCR·Docker Hub·JFrog) 이미지 스캔.
- **소프트웨어 컴포넌트 가시성** — 컨테이너 내부 패키지·라이브러리(SBOM 수준)를 심층 분석해 공급망 위험을 축소.
- **SDLC 이미지 진행 제어** — 클러스터 관리자를 위한 파이프라인 전반의 중앙 뷰(AKS), 고위험 구성 오류 **차단**·배포 시 보안 정책 강제(게이팅).
- **코드 투 클라우드 추적** — 런타임 위험·레지스트리 취약성을 소스 코드까지 역추적해 정밀 수정, 실제 악용 가능성(exploitability) 기반 권장사항 제공.

## ② 태세 관리 (에이전트리스)

### 발견 · 위험 평가 · 컴플라이언스

- **종합 가시성·발견** — 에이전트리스로 Kubernetes·레지스트리 자산을 제로 풋프린트로 탐색.
- **위험 평가** — 에이전트리스 취약성 평가(레지스트리 이미지·실행 컨테이너·노드), 취약성 데이터를 클라우드 보안 그래프에 반영. *실행 컨테이너 런타임 VA는 Windows 노드·AKS 임시(ephemeral) OS 디스크에서는 미지원.*
- **컴플라이언스** — 업계 표준(CIS·PCI DSS·NIST 등) 기준 벤치마킹으로 감사 간소화·규정 준수 확인.

> [!TIP]
> **공격 경로 분석**과 **클라우드 보안 탐색기**(그래프 기반 위험 헌팅)는 **Defender CSPM** 플랜의 기능입니다. Defender for Containers는 취약성 데이터를 그래프에 제공하며, CSPM을 함께 켜면 컨테이너 자산의 공격 경로를 분석할 수 있습니다. → [02 · CSPM](02-cspm.md)

## ③ 런타임 위협 방어 (센서)

### 탐지 · 조사 · 대응 (Defender XDR 통합)

- **다층 위협 탐지** — 워크로드는 **MDE 엔진으로 구동되는 eBPF 센서**로, 호스트·클러스터는 **에이전트리스 맬웨어 탐지**로, 제어 평면은 **Kubernetes 감사 로그**(에이전트리스)로 탐지합니다. 60+ K8s 인식 분석, **MITRE ATT&CK for Containers** 매핑.
- **심층 조사** — 클라우드 제공자·프로세스 데이터로 인시던트 근본 원인 추적, 인시던트 그래프, **컨테이너 인시던트 위협 분석 리포트**.
- **신속 대응** — 침해된 파드를 **격리·종료**하고, **AI 기반 가이드 수정** 제안으로 무단 접근·측면 이동을 즉시 차단.
- **바이너리 드리프트 탐지·차단** — 이미지에 없던 비인가 프로세스 탐지·차단.

![컨테이너 인시던트 조사 화면](assets/container/xdr-investigation.png)
<small class="cap">K8s 인시던트 그래프·경고를 Defender XDR에서 조사, 파드 격리 등 대응</small>

## 플랜 구성

| | Defender CSPM (에이전트리스) | Defender for Containers (에이전트리스 + 센서) |
| --- | :---: | :---: |
| K8s·레지스트리 에이전트리스 발견 | ✅ | ✅ |
| 에이전트리스 취약성 평가 | ✅ | ✅ |
| 빌드 시 이미지 스캔 | ✅ | ✅ |
| 코드 투 클라우드 이미지 매핑 | ✅ | ✅ |
| 취약성 컨텍스트를 클라우드 보안 그래프에 반영 | ✅ | ✅ |
| **공격 경로 분석 · 클라우드 보안 탐색기** | ✅ | ❌ |
| **런타임 위협 탐지**(드리프트·심층 OS 가시성) | ❌ | ✅ |
| **관리 계층 에이전트리스 런타임 탐지** | ❌ | ✅ |
| **K8s 구성 오류 식별·차단**(Azure Policy) | ❌ | ✅ |
| **CDR** — 런타임 위협·인시던트 조사·대응 | ❌ | ✅ |

- **원클릭 온보딩** — Defender 센서는 **AKS 보안 프로필**로 네이티브 배포되어 즉시 커버리지를 제공합니다.
- 센서 기반 추가 기능: **안티맬웨어**, **DNS 탐지**. AKS는 GA, **Arc K8s 센서는 Preview**. 취약성 발견 결과는 **Microsoft 인증서로 서명**됩니다.

---

## 활성화 · 온보딩

> [!NOTE]
> 일반 활성화 절차는 [01 · 사전 준비](01-prerequisites.md)를 참고하세요. 플랜 토글은 **시작점**일 뿐, 런타임 보호는 **센서 배포**가 있어야 작동합니다. 환경(AKS/EKS/GKE/Arc)별 작업량이 크게 다릅니다.

### 공통 1단계 — 플랜 활성화

1. Azure 포털 → **Defender for Cloud** → **환경 설정** → 구독 선택
2. **Containers** 상태를 **On**
3. **Settings**에서 구성 요소를 개별 On: **Agentless scanning · Defender sensor**(+ Security Gating·Runtime Anti-Malware) **· Azure Policy 애드온 · K8s API access · Registry access**
4. **Continue → Save**

> [!NOTE]
> **Registry access의 Security findings**는 Azure Policy로는 켤 수 없고 반드시 이 Settings에서 직접 토글해야 합니다.

### AKS (Azure) — 별도 커넥터 불필요

- 위 Settings에서 **Defender 센서**(AKS Security profile/DaemonSet)와 **Azure Policy 애드온**을 켜면 **자동 배포**됩니다(수동 배포는 CLI·Helm도 가능). 설치 완료까지 수 시간 걸릴 수 있습니다.
- **감사 로그 수집은 완전 자동** — Azure 관리형 컨트롤 플레인 통합이라 클러스터에서 감사 로깅을 켤 필요가 없고, **프라이빗 AKS 클러스터도 추가 네트워크·권한 설정이 없습니다.**
- 권한: 플랜 활성화·확장 배포에 **Owner** 또는 **User Access Administrator**

### EKS (AWS) — 커넥터·CloudFormation·Arc 다단계

1. **환경 설정 → 환경 추가 → Amazon Web Services**로 **AWS 커넥터 생성**(계정 유형·스캔 주기·AWS Account ID, Containers 플랜 선택)
2. 생성된 **CloudFormation 템플릿을 AWS에 배포** → IAM 역할 자동 생성(`MDCContainersAgentlessDiscoveryK8sRole`, `MDCContainersImageAssessmentRole`, 감사로그용 CloudWatch→Kinesis→S3 역할 등)
3. Defender for Cloud가 EKS 클러스터를 **Azure Arc-enabled Kubernetes로 자동 등록**
4. Auto-provisioning이 **Arc 확장으로 Defender 센서·Azure Policy 배포**
5. **프라이빗 클러스터**는 EKS API 서버에 MDC IP 대역(`172.212.245.192/28`, `48.209.1.192/28`) 허용
6. 감사 로그 파이프라인(**SQS·Kinesis Data Firehose·S3**)은 AWS에 자동 구성

### GKE (GCP) — 커넥터·gcloud 스크립트·Arc 다단계

1. **환경 설정 → 환경 추가 → Google Cloud Platform**으로 **GCP 커넥터 생성**(프로젝트 ID/번호, Containers 플랜 선택)
2. 생성된 **gcloud 스크립트를 GCP Cloud Shell에서 실행** → Workload Identity Pool·서비스 계정·역할(`MDCGkeClusterWriteRole` 등) 생성
3. GKE 클러스터가 **Arc-enabled Kubernetes로 자동 등록**되고 **Arc 확장으로 센서·정책 배포**
4. **프라이빗 클러스터**는 Master Authorized Networks에 MDC IP 대역 추가
5. 감사 로그는 **Cloud Logging → Pub/Sub**로 자동 구성
- 필수 GCP API: `iam`·`sts`·`cloudresourcemanager`·`iamcredentials`·`compute`.googleapis.com

### Arc (온프렘·기타)

1. `az connectedk8s connect`로 클러스터를 **먼저 Azure Arc에 연결**
2. Defender 센서·Azure Policy를 **Arc Kubernetes 확장**으로 배포(자동 프로비저닝 지원)
- Linux 커널 **5.4 이상** 필요. **Arc 센서·정책은 Preview** 상태

> [!IMPORTANT]
> **센서를 배포하지 않으면** 이미지 취약성 평가·태세 관리·제어 평면 탐지 같은 **에이전트리스 기능만** 작동하고, **런타임 위협 탐지·바이너리 드리프트·안티맬웨어·DNS 탐지·XDR 통합**은 비활성화됩니다.

참고: [Containers 활성화](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-containers-enable-plan) · [배포 계획](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-containers-deployment-planning) · [AWS 온보딩](https://learn.microsoft.com/en-us/azure/defender-for-cloud/quickstart-onboard-aws) · [GCP 온보딩](https://learn.microsoft.com/en-us/azure/defender-for-cloud/quickstart-onboard-gcp) · [네트워크 요구사항](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-containers-network-access)

---

## 참고 링크

- [Defender for Containers 개요](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-containers-introduction)
- [컨테이너 아키텍처](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-containers-architecture) · [이미지 취약성 평가](https://learn.microsoft.com/en-us/azure/defender-for-cloud/agentless-vulnerability-assessment-azure)
- [Kubernetes 경고 참조](https://learn.microsoft.com/en-us/azure/defender-for-cloud/alerts-containers)

---

### 다음 읽을거리

| ◀ 이전 | ▶ 다음 |
| :-- | --: |
| [Databases](cwp-databases.md) | [APIs →](cwp-apis.md) |

[🏠 전체 목차로 돌아가기](README.md)
