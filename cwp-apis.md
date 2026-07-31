[🏠 전체 목차](README.md)　·　**Part 2 · 핵심 기능**　·　[03 · CWP](03-cwp.md)　·　**Defender for APIs**

# Defender for APIs

> [!NOTE]
> **보호 대상**: **Azure API Management**에 게시된 **REST API**. (현재 REST만 지원, self-hosted gateway·API Management workspaces는 온보딩 제외.)
> API는 가장 빈번한 공격 벡터입니다. Defender for APIs는 **발견 → 강화 → 런타임 보호 → DevOps 사전 테스트** 4단계로 API를 보호합니다.
>
> ⏱️ 예상 소요 **5분**

> [!TIP]
> **공격 경로 분석**(백엔드 워크로드·데이터·AI 앱으로의 측면 이동·데이터 유출 경로)은 **Defender CSPM**의 Cloud Security Graph 기능입니다. Defender for APIs를 CSPM과 함께 켜면 조직 전반의 API 노출 위험을 그래프로 분석할 수 있습니다. → [02 · CSPM](02-cspm.md)

## ① API 발견 · 태세 이해

- **가시성 향상** — Azure API Management에 게시된 API를 **통합 대시보드**(API 컬렉션·엔드포인트·서비스별)로 발견.
- **API 위험 선제 완화** — 외부·휴면(30일 미사용)·미인증 API를 식별하고, 위험 컨텍스트로 수정 우선순위화.
- **민감 데이터 분류** — 민감 데이터를 주고받는 API를 분류하고, 위험 우선순위화에 반영.

![API 인벤토리 화면](assets/api/inventory.png)
<small class="cap">엔드포인트별 인증 상태·30일 미사용·민감 정보 유형을 한눈에</small>

## ② 구성 강화 · 위험 우선순위화

- **보안 권장사항** — 위험에 노출된 API 표면을 강화하는 권장사항을 검토·적용.
- **애플리케이션 위험 수정** — 측면 이동·데이터 유출로 이어지는 API 주도 위험 해소.
- **MITRE 매핑** — 전체 **MITRE ATT&CK 킬체인** 매핑으로 API 공격 표면 가시성 확보.

## ③ 런타임 위협 탐지 · 대응

- **OWASP API Top 10 커버리지** — 상위 OWASP API 위협을 지속 탐지·대응.
- **ML·룰 기반 이상 탐지** — 트래픽 모니터링·위협 인텔리전스로 활성 API 위협·의심 사용 패턴을 통합 뷰로.
- **SIEM 통합** — Microsoft Sentinel 등과 연동해 SOC가 API 인시던트를 신속 분류.

대표 시나리오: **누출(leaky) API** — 공개·미인증 API의 민감 데이터 노출을 태세 관리로 사전 식별하고, 페이로드 급증 등 이상을 경고로 탐지. **깨진 개체 수준 인가(BOLA)** — 사용자 ID 열거(예: `/userId/12/accountdetails` … `/99/…`) 같은 파라미터 열거를 행위 이상으로 탐지.

![API 런타임 보안 경고 화면](assets/api/runtime-alert.png)
<small class="cap">단일 IP의 API 트래픽 급증 등 이상을 MITRE 매핑 경고로 조사</small>

## ④ DevOps 사전 스캔

- **API 사전 테스트·강화** — CI/CD 파이프라인에서 정적·동적 테스트로 **OWASP API 위협**과 **OpenAPI 명세 모범 사례**에 대비해 API를 사전 강화.
- **파트너 솔루션 연동** — Azure Marketplace 파트너 솔루션 결과를 Defender for Cloud로 통합.

## 과금 모델

구독 수준에서 **월간 모니터링된 API 트래픽(호출 수)** 기준으로 과금됩니다. **5단계 요금제**가 있으며 각 플랜의 엔타이틀먼트 한도가 다릅니다. 기본은 **Plan 1**(월 100만 호출 한도)이므로, 트래픽이 많은 구독은 상위 플랜 선택으로 초과 과금을 방지하세요.

---

## 활성화 · 온보딩

> [!IMPORTANT]
> **플랜 토글만으로는 단 하나의 API도 보호되지 않습니다.** 활성화는 **2단계**(플랜 활성화 → API 온보딩)입니다.

### 전제조건

- Defender for Cloud가 활성화된 **Azure 구독**
- **Azure API Management** 인스턴스 1개 이상 + 지원 API **1개 이상 임포트**
- 보호 대상은 **REST API**만 (GraphQL·SOAP·gRPC 미지원). **self-hosted gateway·API Management workspaces** 로 노출된 API는 제외
- 권한: **API Management Service Contributor** + Defender 플랜 활성화 권한(Security Admin 또는 구독 Owner/Contributor)

### 공통 1단계 — 플랜 활성화 (구독 수준)

1. Azure 포털 → **Microsoft Defender for Cloud** → **환경 설정(Environment settings)**
2. 보호할 API가 있는 **구독** 선택
3. **APIs** 플랜 행의 **Details** 클릭 → **Plan 1~5 중 선택**
4. **저장(Save)**

> [!WARNING]
> 기본값은 **Plan 1(월 100만 API 호출)** 입니다. 청구는 **구독 전체의 월간 API 트래픽 합산** 기준이며, 엔타이틀먼트 초과분에 **overage 요금**이 부과됩니다. 플랜 선택 전 **APIM → Metrics**(Requests·Sum·최근 30일)로 트래픽을 측정해 적정 플랜을 고르세요.

### 2단계 — API 온보딩 (필수, 실제 보호 시작점)

1. Defender for Cloud → **Recommendations(권장사항)** 이동
2. 검색창에 `Defender for APIs` 입력
3. 권장사항 **"Azure API Management APIs should be onboarded to Defender for APIs"** 선택
4. **Unhealthy resources**(미온보딩)에서 보호할 API 선택
5. **Fix** → **Fix resources** 클릭
6. 성공 알림 확인

> [!TIP]
> 온보딩 후 반영에는 시간이 걸립니다 — 권장사항 갱신 최대 **50분**, API security 대시보드 인사이트 약 **40분**, 첫 보안 인사이트 약 **30분**.

> [!NOTE]
> APIM 인스턴스가 고부하 상태일 때 **모든 API를 한 번에 온보딩하면 인스턴스 장애(outage)** 가 발생할 수 있습니다. capacity 지표를 보며 **단계적으로** 온보딩하세요.

### 대체 경로 — API Management 포털에서 온보딩

APIM 인스턴스 → **Security → Defender for Cloud** → "Enable Defender on the subscription (recommended)" → APIs 플랜 On → 이후 동일하게 미보호 API 온보딩.

참고: [APIs 배포·온보딩](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-apis-deploy) · [전제조건·지원 범위](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-apis-prepare) · [APIM에서 온보딩](https://learn.microsoft.com/en-us/azure/api-management/protect-with-defender-for-apis)

---

## 참고 링크

- [Defender for APIs 개요](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-apis-introduction)
- [Defender for APIs 배포·요금제](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-apis-deploy) · [API 경고 참조](https://learn.microsoft.com/en-us/azure/defender-for-cloud/alerts-api)

---

### 다음 읽을거리

| ◀ 이전 | ▶ 다음 |
| :-- | --: |
| [Containers](cwp-containers.md) | [AI Services →](cwp-ai.md) |

[🏠 전체 목차로 돌아가기](README.md)
