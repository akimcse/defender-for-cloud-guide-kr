[🏠 전체 목차](README.md)　·　**Part 2 · 핵심 기능**　·　[04 · CWP](04-cwp.md)　·　**Defender for APIs**

# Defender for APIs

> [!NOTE]
> **보호 대상**: Azure API Management에 게시된 API(및 Function Apps·Logic Apps 발견).
> API는 가장 빈번한 공격 벡터입니다. Defender for Cloud는 **발견 → 강화 → 런타임 보호 → DevOps 사전 테스트** 4단계로 API를 보호하고, CNAPP 컨텍스트(공격 경로·민감 데이터)와 결합합니다.

## ① API 발견 · 태세 이해

- **가시성 향상** — Azure Function Apps·Logic Apps·API Management 전반의 API를 **통합 뷰**로 발견.
- **API 위험 선제 완화** — 외부·휴면(30일 미사용)·미인증 API를 식별하고, 상관된 위험 컨텍스트로 수정 우선순위화.
- **민감 데이터 발견** — 민감 데이터를 전달하는 API를 식별하고, **Microsoft Purview** 연동으로 데이터 노출 관리.
- **API 위험 탐색** — 백엔드 클라우드·AI 앱으로의 측면 이동·데이터 유출 위험을 공격 경로로 분석.

![API 인벤토리 화면](assets/api/inventory.png)
<small class="cap">엔드포인트별 인증 상태·30일 미사용·민감 정보 유형을 한눈에</small>

## ② 구성 강화 · 위험 우선순위화

- **API 위험의 컨텍스트 상관** — 게이트웨이 API → 백엔드 워크로드·데이터 저장소·AI 앱까지의 노출 위험을 맥락화.
- **애플리케이션 위험 수정** — 측면 이동·데이터 유출로 이어지는 API 주도 공격 경로 해소.
- **MITRE 매핑** — 전체 **MITRE ATT&CK 킬체인** 매핑으로 API 공격 표면 가시성 확보.

## ③ 런타임 위협 탐지 · 대응

- **OWASP API Top 10 커버리지** — 상위 OWASP API 위협을 지속 탐지·대응.
- **ML 기반 이상 탐지** — 트래픽 모니터링·위협 인텔리전스로 활성 API 위협·의심 사용 패턴을 통합 뷰로.
- **SIEM 통합** — Microsoft Sentinel 등과 연동해 SOC가 API 인시던트를 신속 분류.

대표 시나리오: **누출(leaky) API** — 공개·미인증 API의 민감 데이터 노출을 태세 관리로 사전 식별하고, 페이로드 급증 등 이상을 경고로 탐지. **깨진 개체 수준 인가(BOLA)** — 사용자 ID 열거(예: `/userId/12/accountdetails` … `/99/…`) 같은 파라미터 열거를 행위 이상으로 탐지.

![API 런타임 보안 경고 화면](assets/api/runtime-alert.png)
<small class="cap">단일 IP의 API 트래픽 급증 등 이상을 MITRE 매핑 경고로 조사</small>

## ④ DevOps 사전 스캔

- **API 사전 테스트·강화** — CI/CD 파이프라인에서 정적·동적 테스트로 OWASP 위협 대비 API를 사전 강화.
- **파트너 솔루션 연동** — 42Crunch·Bright Security 등 Azure Marketplace 파트너 결과를 Defender for Cloud로 통합.

---

## 참고 링크

- [Defender for APIs 개요](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-apis-introduction)
- [API 보안 인사이트](https://learn.microsoft.com/en-us/azure/defender-for-cloud/api-security-posture) · [API 경고 참조](https://learn.microsoft.com/en-us/azure/defender-for-cloud/alerts-api)

---

### 다음 읽을거리

| ◀ 이전 | ▶ 다음 |
| :-- | --: |
| [Containers](cwp-containers.md) | [AI Services →](cwp-ai.md) |

[🏠 전체 목차로 돌아가기](README.md)
