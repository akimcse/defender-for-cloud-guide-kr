[🏠 전체 목차](./README.md)　·　**Part 3 · 실습·활용**　·　페이지 6 / 7

# 05 · 핸즈온 랩 — 처음부터 끝까지 30분 실습

> [!NOTE]
> **이 페이지에서 얻는 것**
> - Defender for Cloud를 켜고 보안 점수를 확인하는 첫 경험
> - 권장사항을 실제로 **수정(Quick Fix)** 해 점수를 올려 보기
> - 규정 준수·경고·자동화까지 한 바퀴 돌아보기
>
> ⏱️ 예상 소요 **30분**　·　🎯 대상: 처음 실습하는 모든 독자　·　필요: **Azure 구독 + 구독 Owner(또는 Security Admin)**

이 랩은 **실제 Azure 포털**에서 진행하는 최소 실습입니다. 각 축의 심화 데모는 [06 · 실무 활용](./06-use-cases.md)에서 다룹니다.

> [!IMPORTANT]
> 유료 Defender 플랜은 **첫 30일 무료 체험**이지만, 체험 종료 후 과금됩니다. 실습 뒤 불필요한 플랜은 **환경 설정에서 다시 꺼 두세요**(Step 6). Defender for Storage 악성코드 스캔 등 일부는 체험에서 제외됩니다(→ [01 · 사전 준비](./01-prerequisites.md)).

---

## 사전 확인

- [ ] Azure 구독 보유, **구독 Owner** 또는 **Security Admin** 역할
- [ ] Azure 포털 로그인 가능: https://portal.azure.com

---

## Step 1 · Defender for Cloud 열기 (기본 CSPM 자동 활성화)

1. Azure 포털에서 **Microsoft Defender for Cloud** 검색·선택
2. 좌측 메뉴 **개요(Overview)** 로 이동

이 순간 해당 구독에 **기본 CSPM(무료)** 이 자동 활성화됩니다 — 보안 점수, 자산 인벤토리, 권장사항, MCSB 규정 준수.

> 🖼️ *화면: Defender for Cloud 개요 대시보드 (보안 점수·경고·규정 준수 타일)*

## Step 2 · 보안 점수 확인

1. 개요에서 **보안 태세(Security posture)** 타일 선택
2. **보안 점수(Secure Score)** 와 컨트롤 목록 확인
3. (멀티클라우드 온보딩 시) **Azure / AWS / GCP** 필터로 환경별·통합 점수 비교

> [!TIP]
> 점수는 구독/커넥터별로 **8시간마다** 재계산됩니다. 방금 켠 직후에는 값이 아직 채워지는 중일 수 있습니다.

> 🖼️ *화면: 보안 점수와 보안 컨트롤 목록*

## Step 3 · 유료 플랜 체험 활성화 (선택)

경고·공격 경로 등 심화 기능을 보려면 유료 플랜이 필요합니다. 체험으로 켜 봅니다.

1. 좌측 **환경 설정(Environment settings)**
2. 실습용 **구독** 선택
3. **Defender CSPM** 과 필요 워크로드 플랜(예: **Servers**, **Storage**)을 **켜기(On)** → **저장(Save)**

> 🖼️ *화면: 환경 설정의 플랜 토글 목록*

## Step 4 · 권장사항 수정 (Quick Fix)

1. 좌측 **권장사항(Recommendations)**
2. **Quick Fix** 배지가 있는 권장사항 하나 선택
3. 설명·수정 단계·영향받는 리소스 확인
4. 리소스를 선택하고 **Fix** 실행

수정 후 해당 컨트롤의 상태가 개선되고, 다음 재계산 때 **보안 점수가 오릅니다**.

> [!NOTE]
> 재발 방지가 필요하면 권장사항 상단의 **Deny**(비준수 리소스 생성 차단) 또는 **Enforce**(정책 강제)를 고려하세요.

> 🖼️ *화면: 권장사항 상세 + Quick Fix*

## Step 5 · 규정 준수 대시보드

1. 개요에서 **규정 준수(Regulatory compliance)** 타일 선택
2. 기본 표준 **Microsoft 클라우드 보안 벤치마크(MCSB)** 의 컨트롤 → 평가(pass/fail) 확인
3. (유료) **규정 준수 정책 관리**에서 NIST·PCI DSS·ISO 27001 등 추가 → **감사 보고서** 탭에서 인증서 다운로드

> 🖼️ *화면: 규정 준수 대시보드(MCSB 컨트롤 상태)*

## Step 6 · 정리 (과금 방지)

실습이 끝나면:

1. **환경 설정** → 실습 구독 → Step 3에서 켠 유료 플랜을 **끄기(Off)** → **저장**
2. 기본 CSPM(무료)은 그대로 둬도 과금되지 않습니다.

> [!WARNING]
> 용량/플랜을 켜 두면 체험 종료 후 과금됩니다. 반드시 정리하세요.

---

## 더 나아가기 (선택 챌린지)

- **멀티클라우드**: 환경 설정 → **환경 추가**로 AWS 또는 GCP 커넥터 연결 (→ [01 · 사전 준비](./01-prerequisites.md))
- **워크플로 자동화**: High 심각도 경고 발생 시 Teams/메일 알림을 보내는 Logic App 연결
- **공격 경로**: (Defender CSPM 필요) 개요 → 공격 경로 분석에서 인터넷 노출→핵심 자산 경로 탐색

각 축의 실제 운영 시나리오는 [06 · 실무 활용](./06-use-cases.md)에서 스토리로 이어집니다.

---

## 참고 링크

- [Azure 구독에서 Defender for Cloud 사용](https://learn.microsoft.com/en-us/azure/defender-for-cloud/connect-azure-subscription)
- [보안 점수](https://learn.microsoft.com/en-us/azure/defender-for-cloud/secure-score-security-controls)
- [권장사항 검토·수정](https://learn.microsoft.com/en-us/azure/defender-for-cloud/review-security-recommendations)
- [규정 준수 대시보드](https://learn.microsoft.com/en-us/azure/defender-for-cloud/regulatory-compliance-dashboard)

---

### 다음 읽을거리

| ◀ 이전 | ▶ 다음 |
| :-- | --: |
| [04 · DevSecOps](./04-devsecops.md) | [06 · 실무 활용](./06-use-cases.md) |

[🏠 전체 목차로 돌아가기](./README.md)
