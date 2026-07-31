[🏠 전체 목차](README.md)　·　**Part 2 · 핵심 기능**　·　[03 · CWP](03-cwp.md)　·　**Defender for Storage**

# Defender for Storage

> [!NOTE]
> **보호 대상**: Azure Blob·Files·Data Lake (에이전트 불필요, Azure 네이티브).
> Defender for Storage는 스토리지 계정의 **런타임 위협**을 보호합니다 — ① 컨트롤·데이터 플레인 활동을 분석해 위협을 **탐지·대응**하고, ② 업로드·온디맨드 **악성코드 스캔**으로 확산을 차단하며, 민감 데이터 대상 활동을 별도로 탐지합니다.
>
> ⏱️ 예상 소요 **6분**

> [!TIP]
> 스토리지 계정의 **태세 위험**(잘못된 구성·과도한 권한·공격 경로·인터넷 노출·민감 데이터 발견)은 이 플랜이 아니라 **Defender CSPM / DSPM** 소관입니다. → [02 · CSPM](02-cspm.md) 참조. Defender for Storage와 함께 켜면 위협 탐지에 민감 데이터 컨텍스트가 더해집니다.

## ① 위협 탐지 · 대응

### 데이터 플레인 가시성

Azure Storage 트래픽의 **약 67%가 액세스 키·SAS 토큰**으로 이뤄집니다(컨트롤 플레인만 보면 사각지대). Defender for Storage는 **컨트롤 + 데이터 플레인 로그를 자동 분석**해, 신원 없이 진입하는 엔터티를 탐지합니다. **진단 로그 활성화가 필요 없습니다.**

### 위협 탐지 패밀리

- **악성 콘텐츠** — 악성코드 업로드·다운로드, VM/엔드포인트/앱으로의 확산, 피싱 콘텐츠 호스팅
- **민감 데이터 노출** — 인터넷 노출, 공개 컨테이너 스캐닝, 민감 데이터에 대한 비정상 익명 접근
- **의심스러운 접근 패턴** — Tor 종료 노드, 악성 IP, 의심 앱, 비정상 접근(위치·앱·인증)
- **의심스러운 동작** — 비정상 데이터 추출·접근 수준 조사·탐색·삭제 작업

### 신원 없는 엔터티 탐지 (SAS 토큰)

유출·오용된 액세스 토큰을 탐지합니다 — 내부에서만 쓰이던 과도한 권한 토큰의 외부 IP 접근(토큰 유출 의심), 외부 IP의 비정상 작업, 내부용 토큰의 외부 접근 등.

### 민감 데이터 노출·유출 탐지

스토리지 접근 수준 변경(인증 없는 공개 접근 허용)과 민감 데이터 포함 컨테이너의 의심 활동을 탐지해, SOC가 잠재적 데이터 유출을 신속히 분류·대응하게 합니다. **추가 비용 없이** 켤 수 있으며 Purview SIT·민감도 레이블을 상속합니다.

### 고급 헌팅 (Defender 포털)

Defender XDR 통합으로 `CloudStorageAggregatedEvents` 등 스토리지 이벤트를 환경 전반에서 상관·헌팅합니다. **집계 이벤트**로 로그 노이즈를 줄이고, **데이터 + 컨트롤 플레인 전체 가시성**을 진단 로그 설정 없이 KQL로 조회합니다.

![고급 헌팅 화면](assets/storage/advanced-hunting.png)
<small class="cap">CloudStorageAggregatedEvents를 KQL로 조회해 SAS 사용·작업 패턴 헌팅</small>

## ② 악성코드 스캔

### 근실시간 악성코드 스캔

Microsoft Defender Antivirus(MDAV) 기반으로 **업로드 시 + 온디맨드** 스캔합니다. **모든 파일 유형**에서 **다형성(polymorphic)·변성(metamorphic)** 악성코드까지 탐지하며, **파일당 최대 50GB**를 지원합니다. 파일 **경로·유형·크기로 스캔 범위를 필터링**할 수 있습니다.

![악성코드 스캔 화면](assets/storage/malware-scanning.png)
<small class="cap">업로드 시·온디맨드 스캔 상태와 월 스캔 GB·상한을 확인</small>

> [!IMPORTANT]
> **업로드 시 스캔은 Azure Files에 미지원**입니다(Files는 온디맨드만). Append/Page Blob, 레거시 v1 스토리지, NFS 3.0 업로드도 스캔 대상에서 제외됩니다.

### 스캔 결과 활용

- **Blob index tags**(기본) — 모든 스캔 blob에 `Scan result`·`Scan time(UTC)` 태그가 기록되어, 앱이 **클린 파일만 읽도록** 구성할 수 있습니다.
- **Event Grid** — 스캔 결과 이벤트로 자동 대응(삭제·격리·경보)을 트리거합니다.
- **Log Analytics** — `StorageMalwareScanningResults` 테이블로 결과를 집계·조회합니다.

### 비용 통제 · 자동 대응

- **GB당 과금** — 월 스캔 상한 기본값은 **계정당 10,000GB/월**(조정 가능). 상한의 **75% 도달**·**100% 도달** 시 경고가 발생합니다.
- **자동 대응** — 기본 제공 **소프트 삭제(soft-delete)** 또는 Logic Apps/Function Apps 기반 **커스텀 워크플로(격리·이동)** 로 악성 파일을 처리합니다.

### 해시 평판 분석

업로드된 파일 해시를 알려진 악성코드 해시와 비교해 전체 스캔 없이 신속 탐지합니다(*Put Block/Put Block List·SMB 파일 공유 미지원*).

## 활성화 · 온보딩

> [!NOTE]
> 일반 활성화 절차는 [01 · 사전 준비](01-prerequisites.md)를 참고하세요. 아래는 **Storage 플랜 고유** 설정입니다.

- **원클릭 활성화** — 구독 또는 스토리지 계정 수준으로 켜면 기존·미래 계정이 자동 보호됩니다. **스토리지 계정 생성 과정에 통합**되어 체크박스 하나로 신규 계정을 곧바로 보호할 수 있고, Terraform·Bicep·ARM·PowerShell·REST API·Azure Policy로 대규모 자동화가 가능합니다. 켜면 **활동 모니터링·악성코드 스캔·민감 데이터 위협 탐지가 기본 ON**입니다.
- **악성코드 스캔 비용 통제** — **GB당 과금**이라 월 스캔 상한(기본 **10,000GB/계정**)을 확인하세요. 활성화 시 **Event Grid 시스템 토픽·StorageDataScanner** 등이 자동 배포되며, 삭제·수정하면 스캔이 멈출 수 있습니다.
- **스캔 결과 연동(선택)** — 자동화가 필요하면 **Event Grid 커스텀 토픽**(동일 리전·공개 액세스)이나 **Log Analytics 워크스페이스**를 별도 구성합니다. 악성 파일 **소프트 삭제**도 선택 활성화입니다.
- **계정별 재정의** — 구독 설정과 다르게 하려면 계정 설정에서 **"구독 수준 설정 재정의"를 먼저 켜야** 합니다(안 켜면 구독 설정으로 덮어써짐).
- **클래식 플랜 마이그레이션** — 2025-02-05 이후 클래식 플랜은 신규 활성화 불가. 사용 중이면 신규 플랜으로 마이그레이션(구형 정책 비활성화 선행)이 필요합니다.

참고: [Storage 활성화](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-storage-azure-portal-enablement) · [악성코드 스캔 고급 설정](https://learn.microsoft.com/en-us/azure/defender-for-cloud/advanced-configurations-for-malware-scanning) · [클래식 마이그레이션](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-storage-classic-migrate)

---

## 참고 링크

- [Defender for Storage 개요](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-storage-introduction)
- [악성코드 스캔](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-storage-malware-scan) · [민감 데이터 위협 탐지](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-storage-data-sensitivity)
- [보안 경고(스토리지)](https://learn.microsoft.com/en-us/azure/defender-for-cloud/alerts-azure-storage)

---

### 다음 읽을거리

| ◀ 이전 | ▶ 다음 |
| :-- | --: |
| [Servers](cwp-servers.md) | [Databases →](cwp-databases.md) |

[🏠 전체 목차로 돌아가기](README.md)
