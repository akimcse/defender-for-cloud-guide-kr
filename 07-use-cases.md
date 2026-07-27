[🏠 전체 목차](./README.md)　·　**Part 3 · 실습·활용**　·　페이지 8 / 9

# 07 · 실무 활용 — 3대 축별 데모 시나리오

> [!NOTE]
> **이 페이지에서 얻는 것**
> - CSPM · CWP · DevSecOps 세 축을 **실제 운영 흐름**으로 따라가는 데모 시나리오
> - 각 단계에서 Defender for Cloud가 무엇을 보여주고 무엇을 조치하는지
> - 고객 데모·내부 교육에 바로 쓸 수 있는 **한국어 내레이션**
>
> ⏱️ 예상 소요 **30분+**　·　🎯 대상: 프리세일즈·컨설턴트·보안 엔지니어, 데모 준비자

이 페이지는 Microsoft의 공식 인터랙티브 가이드(*Protect your multi-cloud environment with Microsoft Defender for Cloud*)의 흐름을 **한국어 데모 시나리오로 재구성**한 것입니다. 원문 가이드는 영어이고 실제 데모 테넌트 화면 위에서 클릭하는 방식이므로, 여기서는 **말로 전달할 내레이션 + 화면 포인트**를 정리했습니다.

> [!TIP]
> **스크린샷 넣는 법** — 각 단계의 `🖼️ *화면: …*` 자리에 직접 캡처한 포털 화면을 넣으세요. Microsoft 가이드의 화면을 그대로 복제하기보다, **자사 데모 테넌트에서 캡처**하면 저작권상 안전하고 고객 환경과도 가깝습니다. 원문 인터랙티브 가이드는 각 시나리오 끝의 "더 해보기" 링크에서 직접 클릭해 볼 수 있습니다.

> [!IMPORTANT]
> 아래 시나리오의 일부 기능(공격 경로 분석, 클라우드 보안 탐색기, 규정 준수 표준 추가, 데이터 인식 보안 등)은 **Defender CSPM(유료)** 이, 위협 탐지·경고는 **워크로드 Defender 플랜(유료)** 이 필요합니다(→ [01 · 사전 준비](./01-prerequisites.md)).

---

## 시나리오 A · CSPM — 위험을 우선순위화하고 태세를 끌어올리기

**페르소나:** 클라우드 보안 엔지니어 / 보안 관리자
**목표:** 수많은 권장사항 중 **정말 위험한 것부터** 처리하고, 규정 준수와 거버넌스까지 연결한다.

### A-1. 개요 대시보드에서 현재 태세 읽기

Defender for Cloud 개요 페이지는 하이브리드·멀티클라우드 워크로드의 **보안 태세, 보안 경고, 커버리지**를 한 화면에 보여줍니다. 상단의 **보안 점수**가 출발점입니다.

> "점수가 높을수록 식별된 위험이 낮습니다. 지금 보이는 점수는 Azure 환경 기준이며, **AWS·GCP 필터**를 켜면 모든 클라우드를 합친 통합 점수를 볼 수 있습니다."

> 🖼️ *화면: 개요 대시보드 — 보안 점수 타일 + 환경 필터(Azure/AWS/GCP)*

### A-2. 공격 경로 분석으로 "진짜 위험" 골라내기

권장사항이 수백 개면 어디부터 손댈지 막막합니다. **클라우드 보안 그래프**와 **공격 경로 분석**이 잠재적 측면 이동 경로와 위험 컨텍스트를 근거로 **가장 치명적인 위험을 우선순위화**합니다.

대표 공격 경로 예시:
- **인터넷 노출 VM(고위험 취약점) → Key Vault 읽기 권한** — 노드(리소스)와 엣지(접근)로 표현된 그래프에서 공격 사슬을 시각적으로 확인
- **인터넷 도달 가능한 Amazon EC2(RCE 취약점) → AWS KMS 읽기 권한** — 멀티클라우드 에이전트리스 스캔 인사이트가 결합된 경로. CVE 레코드까지 드릴다운
- **민감 데이터가 담긴 Azure Blob 컨테이너가 인증 없이 공개 읽기 허용** — 데이터 인식 보안(DSPM)으로 민감 정보 유형·파일 샘플까지 확인

각 경로의 **Recommendations 탭**에서 권장사항을 수정하면 공격 사슬을 끊을 수 있습니다.

> 🖼️ *화면: 공격 경로 그래프(노드/엣지) + 경로별 권장사항 탭*

### A-3. 권장사항 수정 + 소유자 할당

권장사항 상세에서 **영향받는 리소스를 선택 → 소유자 지정 → 유예 기간(grace period)** 설정 후 저장합니다. 특정 리소스 노드(예: VM)를 선택하면 그 리소스에 걸린 권장사항만 모아 볼 수도 있습니다.

> 🖼️ *화면: 권장사항 상세 — 리소스 선택 + 소유자/유예기간 지정*

### A-4. 클라우드 보안 탐색기로 능동 헌팅

**클라우드 보안 탐색기(Cloud Security Explorer)** 는 그래프에 대한 쿼리 기반 도구입니다. 시작 템플릿과 커스텀 쿼리로 위험을 직접 찾습니다.

- 템플릿 예: **인터넷 노출 VM**, **HTTP 허용 + 민감 데이터 스토리지 계정**(→ Quick Fix 즉시 수정)
- 커스텀 쿼리 예: **민감 데이터를 포함하고 인터넷에 노출된 Azure SQL 서버** → 결과의 Resource health에서 데이터 분류·권장사항·경고 확인

> 🖼️ *화면: 클라우드 보안 탐색기 — 템플릿/쿼리 빌더 + 결과*

### A-5. 규정 준수 태세 개선

**규정 준수** 타일 → **MCSB** 컨트롤의 평가(pass/fail) 확인 → Quick Fix로 수정하거나, 재발 방지를 위해 **Deny** 정책으로 비준수 리소스 생성을 차단합니다. **PCI DSS** 등 표준을 추가하고, **감사 보고서** 탭에서 인증서(예: PCI DSS 인증서)를 다운로드해 이해관계자와 공유할 수 있습니다.

> 🖼️ *화면: 규정 준수 대시보드 + 감사 보고서 다운로드*

### A-6. 보안 거버넌스로 책임 명확화

**거버넌스 규칙**으로 권장사항에 **소유자와 기한**을 자동 할당하면 책임과 투명성이 생깁니다. 지정된 소유자와 관리자에게 **주간 이메일**로 미해결·기한 초과 항목이 통지됩니다.

> 🖼️ *화면: 거버넌스 규칙 설정 + 주간 알림 이메일 예시*

> [!NOTE]
> **핵심 메시지** — "권장사항을 다 처리하려 하지 말고, **공격 경로가 가리키는 소수의 치명적 위험**부터 소유자를 지정해 기한 내 처리하라. 그게 태세 점수를 가장 빨리 올리는 길이다."

🔗 **더 해보기(영문 인터랙티브):** [Manage your cloud security posture with Microsoft Defender for Cloud](https://mslearn.cloudguides.com/guides/Manage%20your%20cloud%20security%20posture%20with%20Microsoft%20Defender%20for%20Cloud)

---

## 시나리오 B · CWP — 위협을 예방·탐지하고 신속 대응하기

**페르소나:** SOC 분석가 / 워크로드 담당 엔지니어
**목표:** 멀티클라우드 워크로드를 온보딩하고, 실제 위협(악성코드·컨테이너 공격·API 이상)을 탐지·대응한다.

### B-1. 멀티클라우드 온보딩

**환경 설정 → 환경 추가**로 GCP/AWS 커넥터를 만들면 전체 클라우드를 **단일 창(single pane of glass)** 에서 모니터링합니다. GCP는 커넥터 이름 지정 → (단일 프로젝트 또는 조직) → Defender 플랜 선택 → **Cloud Shell 스크립트**로 액세스 구성. 비-Azure 클라우드 검색은 **약 4시간 주기**로 수행됩니다.

> 🖼️ *화면: 환경 추가 마법사(GCP 플랜 선택 화면)*

### B-2. 에이전트리스 스캔으로 취약성 가시성 확보

AWS 온보딩 2단계에서 **에이전트리스 스캔**을 토글하면, 에이전트 설치 없이 EC2의 취약성·자격 증명을 평가합니다. **인벤토리**에서 리소스를 열면 **설치된 애플리케이션 목록**이 보이고, **권장사항**에는 에이전트·에이전트리스 결과가 **통합**되어 나타납니다(Microsoft Defender 취약성 관리 연동).

> 🖼️ *화면: 인벤토리 리소스 상세(설치 SW) + 취약성 권장사항*

### B-3. 경고 상관 → 인시던트로 대응

**워크로드 보호 대시보드**의 보안 경고 그래프에서 심각도별 추이를 봅니다. 관련 경고들은 **kill chain 패턴**에 따라 **보안 인시던트**로 묶여, 진행 중인 공격의 점들을 연결해 줍니다. 인시던트의 **Alerts 탭**(상관된 경고)과 **Take action 탭**(완화·자동 대응)을 활용하고, 오탐이 잦은 경고는 **억제 규칙(Suppression rules)** 으로 노이즈를 줄입니다.

> 🖼️ *화면: 인시던트 상세 — 상관 경고 + Take action*

### B-4. Defender for Storage 악성코드 스캔 (EICAR 시연)

**환경 설정**에서 구독 수준으로 **Defender for Storage** 를 켜고, **악성코드 스캔**과 **민감 데이터 검색**을 구성합니다(월 스캔 GB 상한으로 비용 통제 가능).

데모 흐름:
1. 스토리지 컨테이너에 **정상 파일** 업로드 → 스캔 결과 **위협 없음**
2. **EICAR 테스트 파일**(악성코드 시뮬레이션) 업로드 → **근실시간으로 악성 탐지**
3. Defender for Cloud에 **보안 경고** 자동 생성 — 영향 리소스, 파일 해시, **MITRE ATT&CK** 전술 표시
4. SIEM(예: Microsoft Sentinel) 연계로 SOC가 조사, Event Grid로 격리 자동화 가능

> [!TIP]
> EICAR 문자열은 표준 안티바이러스 테스트 파일입니다: `X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*` — 텍스트 파일에 붙여 넣어 만듭니다. 실제 악성코드가 아니라 **탐지 동작 검증용**입니다.

> 🖼️ *화면: EICAR 업로드 후 스토리지 보안 경고(MITRE 전술 포함)*

### B-5. 컨테이너 위협 탐지 + 정책 강제

멀티클라우드 Kubernetes(AKS·EKS·GKE) 경고를 통합 뷰로 봅니다. 예: **Weave Scope 오용**으로 K8s 서비스가 인터넷에 노출(Initial Access), 악성 파일 다운로드 경고 등 — 모두 **MITRE ATT&CK 전술**로 분류됩니다.

예방은 **정책 강제**로: "신뢰할 수 있는 레지스트리에서만 이미지 배포" 권장사항에 **Deny**를 설정하면, 터미널에서 Docker Hub 이미지 배포를 시도해도 **차단**됩니다(데모에서 배포 성공→Deny 설정→배포 실패를 시연). Azure Policy 할당으로도 동일하게 강제할 수 있습니다.

> 🖼️ *화면: 컨테이너 경고(ATT&CK) + Deny 설정 후 배포 실패 터미널*

### B-6. Defender for APIs

Azure API Management의 API를 온보딩하면 **엔드포인트별 보안 인사이트**(인증 상태, 30일 무트래픽, 민감 데이터 분류)를 봅니다. **OWASP API Top 10** 기반 ML·위협 인텔 탐지가 이상 파라미터·비정상 트래픽을 경고로 올리고, **Take action**에서 자동 대응·억제 규칙을 구성합니다.

> 🖼️ *화면: API 보안 인사이트 + 이상 파라미터 경고*

> [!NOTE]
> **핵심 메시지** — "온보딩 한 번으로 멀티클라우드 워크로드가 단일 창에 모이고, **경고는 인시던트로 상관**되어 SOC가 공격의 전체 그림을 본다. 예방은 **Deny/정책**으로, 탐지는 **경고·MITRE 매핑**으로."

🔗 **더 해보기(영문 인터랙티브):** [Prevent, detect, and respond quickly to modern threats](https://mslearn.cloudguides.com/guides/Prevent%20detect%20and%20respond%20quickly%20to%20modern%20threats%20with%20Microsoft%20Defender%20for%20Cloud)

---

## 시나리오 C · DevSecOps — 코드에서 클라우드까지 보안 통합

**페르소나:** DevSecOps / 플랫폼 엔지니어, 애플리케이션 보안 담당
**목표:** GitHub·Azure DevOps 파이프라인을 온보딩하고, **배포 전 코드에서** 시크릿·취약점을 잡아 개발자에게 되돌린다.

### C-1. DevOps 환경 온보딩

Defender for Cloud 메인에서 **DevOps 보안(DevOps Security)** → **환경 추가**로 **GitHub** 커넥터(앱 설치, 리포지토리 접근 승인)와 **Azure DevOps** 커넥터(프로젝트/리포 선택, 자동 검색)를 만듭니다. 온보딩된 커넥터는 **환경 설정**에 나타납니다.

> 🖼️ *화면: DevOps 보안 — GitHub/Azure DevOps 커넥터 온보딩*

### C-2. 풀 리퀘스트(PR) 주석 구성

**DevOps 보안** 대시보드는 심각도별 취약점, 스캔 결과, 노출된 시크릿 수, 커버리지를 보여 줍니다. 리포를 선택해 **PR 주석(Pull Request Annotations)** 을 켭니다.

개발자 관점 데모:
1. Azure DevOps 리포에서 **PR 생성**
2. Defender for DevOps 도구가 백그라운드로 스캔 → **자격 증명 탐지로 빌드 실패**
3. **PR에 주석**으로 "Azure Storage Account Access Key 탐지" 표시 → 개발자가 즉시 수정 후 재제출

> [!IMPORTANT]
> 이 흐름의 가치는 **"보안을 왼쪽으로(shift-left)"** — 문제를 프로덕션이 아니라 **PR 단계**에서, 별도 도구가 아니라 **개발자가 보는 화면**에서 잡는다는 점입니다.

> 🖼️ *화면: 빌드 실패 + PR 주석(시크릿 탐지)*

### C-3. 파이프라인에 보안 도구 통합

**Microsoft Security DevOps 확장**을 파이프라인 YAML에 추가하면 코드·시크릿·오픈소스(OSS)·IaC 스캔 도구가 실행됩니다. Azure DevOps는 `MicrosoftSecurityDevOps` 빌드 태스크로, GitHub는 **Actions 워크플로**로 설정하며, 결과는 Defender for Cloud로 흘러 들어옵니다(GitHub Advanced Security 연동 시 Security 탭에도 표시).

> 🖼️ *화면: 파이프라인 YAML에 보안 태스크 추가 + 스캔 결과*

### C-4. DevOps 권장사항으로 취약점 완화

Defender for Cloud의 **권장사항**에 DevOps 결과가 두 갈래로 나타납니다.
- **취약점 수정(Remediate vulnerabilities)** — 코드 스캔 결과(파일 위치·도구·규칙), **시크릿 탐지**(예: ConfigData의 노출 자격 증명), **OSS 취약점**(CVSS 점수·GitHub Advisory 링크), **IaC 스캔**(terrascan·template analyzer)
- **향상된 보안 기능 사용(Enable enhanced security)** — GitHub 코드 스캔·시크릿 스캔·**Dependabot** 활성화. Dependabot은 수동 대신 **Logic App 트리거**로 여러 리포에 한 번에 켤 수 있음

**인벤토리**에서 Azure DevOps·GitHub 리포지토리를 필터링해 리포별 권장사항 상태를 추적합니다.

> 🖼️ *화면: DevOps 권장사항(시크릿/코드/OSS/IaC) 상세*

### C-5. DevOps 워크북으로 인사이트 통합

**Workbooks**의 Defender for DevOps 워크북은 리포·스캔 결과를 하나의 대시보드로 모읍니다 — 노출된 시크릿, 코드 스캔 취약점, Dependabot(심각도별), IaC 발견, 위협·전술 개요. 위협 헌터는 이 뷰로 조직의 노출 지점을 파악합니다.

> 🖼️ *화면: DevOps 워크북(시크릿/코드/Dependabot/IaC/위협 탭)*

> [!NOTE]
> **핵심 메시지** — "코드에서 클라우드까지 하나의 콘솔로. **PR 주석**으로 개발자와 보안팀의 소통을 줄이고, **파이프라인 스캔 + 권장사항 + 워크북**으로 배포 전에 위험을 끊는다."

🔗 **더 해보기(영문 인터랙티브):** [Unify DevOps security management with Microsoft Defender for Cloud](https://mslearn.cloudguides.com/guides/Unify%20DevOps%20security%20management%20with%20Microsoft%20Defender%20for%20Cloud)

---

## 데모 진행 팁 (프리세일즈용)

- **스토리 순서**: 태세(A) → 위협(B) → 코드(C) 순서가 자연스럽습니다. "구성이 잘못됐는지(A) → 지금 공격받는지(B) → 배포 전에 막았는지(C)".
- **시간 배분**: 각 축 8~10분. 시간이 짧으면 A-2(공격 경로), B-4(EICAR), C-2(PR 주석)만으로도 임팩트가 큽니다.
- **한국 고객 대응**: 화면은 자사 데모 테넌트에서 **한국어 UI**로 캡처하면 전달력이 높습니다. 규정 준수는 국내 관심이 큰 **ISO 27001 / PCI DSS**를 예로 드세요.
- **비용 질문 대비**: 무료 기초 CSPM과 유료 플랜의 경계, 30일 체험, 리소스 단위 과금을 미리 정리(→ [01 · 사전 준비](./01-prerequisites.md)).

---

## 참고 링크

- [멀티클라우드 환경 보호(허브 인터랙티브 가이드)](https://mslearn.cloudguides.com/guides/Protect%20your%20multi-cloud%20environment%20with%20Microsoft%20Defender%20for%20Cloud)
- [공격 경로 분석 / 클라우드 보안 그래프](https://learn.microsoft.com/en-us/azure/defender-for-cloud/concept-attack-path)
- [보안 경고 · 인시던트](https://learn.microsoft.com/en-us/azure/defender-for-cloud/alerts-overview)
- [Defender for Storage 개요](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-storage-introduction)
- [Defender for Containers 개요](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-containers-introduction)
- [Defender for APIs 개요](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-apis-introduction)
- [DevOps 보안 개요](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-devops-introduction)

---

### 다음 읽을거리

| ◀ 이전 | ▶ 다음 |
| :-- | --: |
| [06 · 핸즈온 랩](./06-handson-lab.md) | 99 · 부록 _(작성 예정)_ |

[🏠 전체 목차로 돌아가기](./README.md)
