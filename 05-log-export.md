[🏠 전체 목차](./README.md)　·　**Part 3 · 운영 통합**　·　페이지 6 / 7

# 05 · 로그 내보내기 · 조회하기

> [!NOTE]
> **이 페이지에서 얻는 것**
> - Defender 보안 데이터를 외부(SIEM·Event Hub·Log Analytics)로 내보내는 **두 경로**(MDC Continuous Export vs Defender XDR Streaming API)
> - **조건에 맞는 로그만** 골라 보내는 방법 — Continuous Export 필터 + **Logic App**
> - 수집된 데이터가 **어떤 테이블·컬럼**으로 적재되고, KQL로 **어떻게 가져와 조회·활용하는지**
>
> ⏱️ 예상 소요 **10분**　·　🎯 대상: SOC 엔지니어, SIEM 연동 담당, 플랫폼 엔지니어

Defender for Cloud의 경고·권장사항 같은 보안 데이터를 조직의 SIEM이나 데이터 파이프라인으로 보내는 방법과, **전체가 아니라 필요한 로그만 필터링**해 보내는 방법을 정리합니다.

---

## 1. 내보내기 두 경로 — MDC vs Defender XDR

Defender 보안 신호를 외부로 스트리밍하는 경로는 크게 **두 가지**이며, 데이터 소스와 스키마가 다릅니다.

| | **A. MDC Continuous Export** | **B. Defender XDR Streaming API** |
| --- | --- | --- |
| **주체** | Microsoft Defender for Cloud | Microsoft Defender XDR |
| **데이터** | 보안 **경고·권장사항·보안 점수·규정 준수·attack path** 등 자산·태세 중심 | **Advanced Hunting 이벤트 테이블**(원시 이벤트) |
| **대표 대상** | `SecurityAlert`, `SecurityRecommendation`, `SecurityRegulatoryCompliance`, `SecureScore` | `AlertInfo`, `AlertEvidence`, `DeviceEvents`, `DeviceProcessEvents`, `CloudAppEvents`, `EmailEvents`, `IdentityLogonEvents` 등 |
| **대상 목적지** | Log Analytics workspace, **Event Hub**, 제3자 SIEM/SOAR | **Event Hub**, Azure Storage |
| **설정 위치** | Defender for Cloud → **환경 설정 → Continuous export** | Defender 포털(security.microsoft.com) → 설정 → Streaming API |
| **스코프** | 구독/관리 그룹(Azure Policy로 대규모 배포) | 테넌트 |
| **실시간성** | **Streaming**(상태 변경 시) 또는 **Snapshot**(주 1회) | 이벤트 발생 시 준실시간 스트리밍 |

### 언제 무엇을 쓰나

- **MDC Continuous Export** — "Defender for Cloud의 **경고·권장사항·규정 준수**를 SIEM으로" 보낼 때. 태세·자산 관점 데이터. 구독 단위로 Azure Policy 대규모 배포 가능.
- **XDR Streaming API** — "엔드포인트·ID·메일·클라우드앱의 **원시 이벤트(Advanced Hunting)**"를 커스텀 파이프라인·데이터 레이크로 대량 스트리밍할 때.
- **둘 다 Event Hub로** 보낼 수 있어, Splunk·QRadar 등 제3자 SIEM은 Event Hub를 구독해 수집합니다. (Microsoft Sentinel을 쓰면 별도 커넥터로 더 간단히 연동됩니다.)

> [!TIP]
> Defender for Cloud 경고는 **Defender XDR에도 자동 통합**됩니다. 그래서 "경고만" 필요하면 XDR의 `AlertInfo`/`AlertEvidence`로도 받을 수 있고, "권장사항·규정 준수·보안 점수"처럼 태세 데이터가 필요하면 **MDC Continuous Export**를 써야 합니다.

참고: [Continuous export](https://learn.microsoft.com/en-us/azure/defender-for-cloud/continuous-export) · [Defender XDR Streaming API](https://learn.microsoft.com/en-us/defender-xdr/streaming-api) · [지원 이벤트 타입](https://learn.microsoft.com/en-us/defender-xdr/supported-event-types)

---

## 2. Continuous Export 설정 · 내보낼 데이터 고르기

MDC 자체 내보내기는 **데이터 종류별로 필터를 걸 수 있습니다**(전량이 아니라 원하는 것만).

**설정 절차**
1. Defender for Cloud → **환경 설정(Environment settings)** → 구독 선택
2. **Continuous export** → **Event Hubs** 또는 **Log Analytics workspace** 탭 선택
3. 내보낼 **데이터 타입 선택**(보안 경고·권장사항·규정 준수·보안 점수·attack path 등) → 타입별 **필터** 지정 — 예: **High 심각도 경고만**
4. **내보내기 빈도** 선택:
   - **Streaming** — 리소스 상태가 변경될 때 전송(변경 없으면 미전송)
   - **Snapshot** — 선택 데이터의 현재 상태를 **주 1회** 스냅샷 전송(`IsSnapshot` 필드로 식별)
5. 저장

- 권한: **Security Admin 또는 리소스 그룹 Owner**, Event Hub 정책 Write 권한. 대규모는 **Azure Policy(DeployIfNotExists)** 로 배포.

> [!NOTE]
> Continuous Export의 필터는 "**어떤 종류·심각도를 보낼지**" 고르는 수준입니다. 임의 조건(KQL)으로 **원본은 전량 보존하면서 조건 히트분만** 별도 전송하려면 아래 **Logic App** 방식을 씁니다.

참고: [Continuous export](https://learn.microsoft.com/en-us/azure/defender-for-cloud/continuous-export) · [Event Hub로 내보내기](https://learn.microsoft.com/en-us/azure/defender-for-cloud/continuous-export-event-hub)

---

## 3. Logic App으로 조건 필터링 (원본 전량 보존)

"원본은 workspace에 전부 두되, **조건 히트분만** Event Hub/Storage로"가 필요하면 **예약 쿼리(Recurrence) → Azure Monitor Logs 커넥터 → 대상 커넥터** 로 Logic App을 구성합니다. `where`·`summarize` 등 **필터·집계가 자유롭고 과거 데이터도** 가능합니다(단, 실시간이 아니라 예약 주기 기반이며 로그 쿼리 제한을 받습니다).

### 워크플로 구성 단계

1. **Logic App 생성** — 포털에서 Logic App(Consumption) 생성 → 디자이너 열기. 만든 계정은 workspace **읽기 권한** + 대상(Storage/Event Hub) **쓰기 권한** 필요.
2. **트리거 추가: Recurrence** — **Schedule** 범주의 **Recurrence**(Built-in) 트리거 선택 → Frequency·Interval 지정(예: 1시간마다).
3. **액션: Azure Monitor Logs** — `+ New step` → **Azure Monitor Logs** → **"Azure Log Analytics – Run query and list results"** → 구독·리소스 그룹·workspace 선택 → **KQL 쿼리** 입력.
4. **결과 파싱(선택)** — **Parse JSON** 으로 결과를 필드화.
5. **대상 전송** — **Event Hubs 커넥터(Send event)** 또는 **Azure Blob Storage 커넥터(Create blob)** 로 결과 전송.

### 쿼리 작성 요령 — `ingestion_time()` 로 지연 데이터 누락 방지

예약 실행은 "지난 주기에 **적재된** 데이터"를 정확히 잡아야 합니다. `TimeGenerated`(발생 시각) 대신 **`ingestion_time()`**(적재 시각) 기준으로 창을 잡으면 네트워크·플랫폼 지연으로 늦게 들어온 데이터도 다음 실행에서 포함됩니다.

```kql
// 1시간 주기 예약 실행용 — 직전 1시간에 '적재된' High 경고만 추출
let dt = now();
let startTime = make_datetime(datetime_part('year',dt), datetime_part('month',dt), datetime_part('day',dt), datetime_part('hour',dt), 0) - 1h;
let endTime = startTime + 1h - 1tick;
SecurityAlert
| where ingestion_time() between (startTime .. endTime)
| where AlertSeverity == "High"
| project TimeGenerated, AlertName, AlertSeverity, CompromisedEntity, Tactics, Status
```

> [!TIP]
> - **Time Range**는 쿼리의 시간 창보다 넉넉히(예: "Last 4 hours") 잡아, `ingestion_time`이 `TimeGenerated`보다 늦은 레코드까지 포함시키세요.
> - 쿼리에서 미리 **필터·`project`(필요 컬럼만)** 로 데이터를 줄이면 커넥터 제한에 안 걸립니다.

> [!WARNING]
> **커넥터 제한** — 로그 쿼리는 ①**50만 행** ②**64MB** ③**10분** 을 넘을 수 없고, Azure Monitor Logs 커넥터는 **분당 100회** 호출 제한이 있습니다. 대량이면 주기를 짧게(예: 1시간) 잡아 매 실행량을 줄이세요. 대규모 과거 데이터 일괄 내보내기는 **Export job(Preview)** 이 더 적합합니다.

참고: [Logic App으로 로그 내보내기](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/logs-export-logic-app) · [Azure Monitor Logs 커넥터](https://learn.microsoft.com/en-us/connectors/azuremonitorlogs)

---

## 4. 수집된 데이터 — 테이블 구조 · 활용

Continuous Export로 Log Analytics에 보내면 아래 테이블에 적재됩니다. **한 행 = 하나의 경고/권장사항** 이며, KQL로 조회·필터·알림을 만듭니다.

### SecurityAlert — 보안 경고

한 행이 하나의 보안 경고입니다. 분석가가 자주 쓰는 주요 컬럼:

| 컬럼 | 의미 |
| --- | --- |
| `AlertName` · `AlertType` | 경고 이름 · 유형 ID |
| `AlertSeverity` | 심각도(High/Medium/Low/Informational) — **가장 흔한 필터 기준** |
| `CompromisedEntity` | 영향받은 리소스(예: VM 이름) |
| `Tactics` · `Techniques` | **MITRE ATT&CK** 전술·기법 매핑 |
| `Status` | 경고 상태(New/Active/Resolved 등) |
| `Description` · `RemediationSteps` | 설명 · 수정 단계 |
| `Entities` · `ExtendedProperties` | 관련 개체·확장 속성 (**JSON 문자열** → 파싱 필요) |
| `StartTime` · `EndTime` · `TimeGenerated` | 발생·수집 시각 |

> `Entities`·`ExtendedProperties`는 JSON 문자열이라 `parse_json()`으로 풀어 세부 필드를 뽑습니다.

### SecurityRecommendation — 권장사항

| 컬럼 | 의미 |
| --- | --- |
| `RecommendationName` · `RecommendationDisplayName` | 권장사항 ID · 표시 이름 |
| `RecommendationSeverity` | 심각도 |
| `RecommendationState` | 상태(Healthy/Unhealthy/NotApplicable) |
| `AssessedResourceId` | 평가된 리소스 |
| `Description` | 설명 |
| `IsSnapshot` | 스냅샷 데이터 여부 |

### 실전 KQL — 데이터 가져와 쓰기

```kql
// ① 최근 24시간 High 심각도 경고
SecurityAlert
| where TimeGenerated > ago(24h)
| where AlertSeverity == "High"
| project TimeGenerated, AlertName, CompromisedEntity, Tactics, Status
| order by TimeGenerated desc
```

```kql
// ② 경고 이름별 발생 건수 (Top 10)
SecurityAlert
| where TimeGenerated > ago(7d)
| summarize Count = count() by AlertName
| top 10 by Count
```

```kql
// ③ ExtendedProperties JSON 파싱해 세부 필드 추출
SecurityAlert
| where TimeGenerated > ago(24h)
| extend props = parse_json(ExtendedProperties)
| project TimeGenerated, AlertName, IP = props.["Client IP"], props
```

```kql
// ④ 수정이 필요한(Unhealthy) High 권장사항
SecurityRecommendation
| where TimeGenerated > ago(1d)
| where RecommendationState == "Unhealthy" and RecommendationSeverity == "High"
| summarize by RecommendationDisplayName, AssessedResourceId
```

```kql
// ⑤ 시간대별 경고 추세 (1시간 단위)
SecurityAlert
| where TimeGenerated > ago(7d)
| summarize Count = count() by bin(TimeGenerated, 1h), AlertSeverity
| render timechart
```

```kql
// ⑥ 가장 많이 공격받은 리소스 Top 10
SecurityAlert
| where TimeGenerated > ago(30d)
| summarize Alerts = count() by CompromisedEntity
| top 10 by Alerts
```

### 활용 방법

- **Log Analytics 포털**에서 위 쿼리를 실행하거나, **Workbook**으로 대시보드화
- 쿼리에 **예약 경고 규칙(Scheduled alert rule)** 을 걸어 조건 충족 시 자동 알림·티켓
- **Microsoft Sentinel**을 쓰면 이 테이블들이 그대로 헌팅·분석 규칙의 소스가 됩니다

> [!TIP]
> `TimeGenerated`는 **수집 시각** 기준입니다. 경고 자체의 발생 시각은 `StartTime`을 보세요. 컬럼명은 **대소문자를 구분**하며, 데이터에는 수집 지연(latency)이 있어 최근 몇 분 데이터는 아직 안 보일 수 있습니다.

참고: [SecurityAlert 스키마](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/securityalert) · [SecurityRecommendation 스키마](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/securityrecommendation) · [KQL 시작하기](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/get-started-queries)

---

## 한눈에 — 무엇을 언제 쓰나

| 목표 | 방법 |
| --- | --- |
| Defender for Cloud **경고·권장사항·규정 준수**를 SIEM/Event Hub로 | **MDC Continuous Export**(타입·심각도 필터 가능) |
| 엔드포인트·ID·메일 **원시 이벤트**를 대량 스트리밍 | **Defender XDR Streaming API** |
| 원본 전량 보존 + **조건에 맞는 로그만** 별도 전송 | **Logic App**(예약 쿼리 → Event Hub/Storage) |
| 적재된 데이터 조회·대시보드·알림 | **KQL**(SecurityAlert·SecurityRecommendation) + Workbook/Sentinel |

---

## 참고 링크

- [Continuous export](https://learn.microsoft.com/en-us/azure/defender-for-cloud/continuous-export) · [Event Hub로 내보내기](https://learn.microsoft.com/en-us/azure/defender-for-cloud/continuous-export-event-hub) · [SIEM·SOAR 연동](https://learn.microsoft.com/en-us/azure/defender-for-cloud/export-to-siem)
- [Defender XDR Streaming API](https://learn.microsoft.com/en-us/defender-xdr/streaming-api) · [지원 이벤트 타입](https://learn.microsoft.com/en-us/defender-xdr/supported-event-types)
- [Logic App으로 로그 내보내기](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/logs-export-logic-app) · [Azure Monitor Logs 커넥터](https://learn.microsoft.com/en-us/connectors/azuremonitorlogs)
- [SecurityAlert 스키마](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/securityalert) · [KQL 시작하기](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/get-started-queries)

---

| ◀ 이전 | ▶ 다음 |
| :-- | --: |
| [04 · DevSecOps](04-devsecops.md) | [06 · 경고 알림 자동화](06-notifications.md) |

[🏠 전체 목차로 돌아가기](./README.md)
