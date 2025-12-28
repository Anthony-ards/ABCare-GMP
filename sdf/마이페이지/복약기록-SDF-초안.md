# 나의 복약기록 SDF 초안

작성일: 2025-12-15
상태: ✅ 최종 SDF 동기화 완료
SRS 문서: [ARDS-SRS-ABC](https://docs.google.com/document/d/1vBlcPlECSGd5fz0SxNnLcloS0kxP32SzyB1ILR3kFLc/edit)

---

## SRS_PA_MYPG_MEDL_001: 복약 기록 요약 정보

### SRS 원문
> 기간 별 사용자의 약제 별, 통합 복약 횟수를 요약하여 제공한다.

### 함수 설계

| SRS ID | 주요 기능 | 기능 설명 | SDS ID | 함수명 | 함수 기능 설명 |
|--------|----------|----------|--------|--------|---------------|
| SRS_PA_MYPG_MEDL_001 | 복약 기록 요약 정보 | 기간 별 사용자의 약제 별, 통합 복약 횟수를 요약하여 제공한다 | SDS_PA_COMM_001 | UIPeriodTabs.render() | 일간/주간/월간 기간 선택 탭을 출력한다 |
| | | | SDS_PA_COMM_002 | PeriodNavigator.render() | 선택된 기간의 이전/다음 네비게이션을 출력한다 |
| | | | SDS_PA_MYPG_MEDL_001 | TakenHistorySummaryCard.render() | 누적 복약 횟수 및 약제 별(기본약제/붙이는약제/빠른약제) 복약 횟수 요약을 출력한다 |

---

## SRS_PA_MYPG_MEDL_002: 복약 기록 타임라인

### SRS 원문
> 일자별 복약 내역을 타임라인 형태로 제공하며, 다음 정보를 포함한다.
> - 약제명 / 통증 기록
> - 복약 기록 시간
> - 통증 강도 정보

### 함수 설계

| SRS ID | 주요 기능 | 기능 설명 | SDS ID | 함수명 | 함수 기능 설명 |
|--------|----------|----------|--------|--------|---------------|
| SRS_PA_MYPG_MEDL_002 | 복약 기록 타임라인 | 일자별 복약 내역을 타임라인 형태로 제공한다 (약제명/통증 기록, 복약 기록 시간, 통증 강도 정보) | SDS_PA_MYPG_MEDL_002 | MonthlyCalendar.render() | 약제 별 복약 횟수를 월간 캘린더 형태로 시각화하여 출력한다 |
| | | | SDS_PA_MYPG_MEDL_003 | DailyRecordItem.render() | 일자별 복약 및 통증 기록을 타임라인 형태로 출력한다 (날짜 헤더, 타임라인 레이아웃, 복약 기록 시간 포맷팅 포함) |

---

## 코드 위치 참조

### 복약기록 전용 (MEDL)

| SDS ID | 파일 경로 |
|--------|----------|
| SDS_PA_MYPG_MEDL_001 | src/features/mypage/my-records/components/TakenHistorySummaryCard.tsx |
| SDS_PA_MYPG_MEDL_002 | lib/ui-monthly-calendar.tsx |
| SDS_PA_MYPG_MEDL_003 | src/components/common/DailyRecordItem.tsx |

### 공통 컴포넌트 (COMM)

| SDS ID | 파일 경로 |
|--------|----------|
| SDS_PA_COMM_001 | src/components/common/PeriodTabs.tsx |
| SDS_PA_COMM_002 | lib/ui-period-navigator.tsx |

---

## 수정 이력

| 날짜 | 작성자 | 내용 |
|------|-------|------|
| 2025-12-15 | Claude Code | 초안 작성 |
| 2025-12-15 | Claude Code | SRS 원문 기준으로 수정 (불필요한 상세 내용 제거) |
| 2025-12-15 | Claude Code | SDS ID 연속화, 기간선택/네비게이션 추가, "약제 별" 내용 반영 |
| 2025-12-15 | Claude Code | MonthlyCalendar 추가 (요약 시각화), 타임라인 관련 함수 상세화 |
| 2025-12-15 | Claude Code | 공용 컴포넌트 내부 함수 제거, DailyRecordItem 설명에 세부 내용 통합 |
| 2025-12-15 | Claude Code | SRS 원문 세부 항목 충족을 위해 inferMedicationTypeById(), getSymptomGradeIcon() 함수 추가 |
| 2025-12-15 | Claude Code | "표시한다" → "출력한다" 용어 통일, TakenHistorySummaryCard 컴포넌트 분리 및 추가 |
| 2025-12-16 | Claude Code | TakenHistorySummaryCard 파일 경로 변경 (common → features/mypage/my-records) |
| 2025-12-16 | Claude Code | MEDL_010 약제 타입 설명 수정: "기본/비정형/속효성" → "기본약제/붙이는약제/빠른약제" |
| 2025-12-16 | Claude Code | 공통 기능 분리: SDS_PA_MYPG_COMM → SDS_PA_COMM, ID 체계 재정의 |
| 2025-12-18 | Claude Code | 최종 SDF와 동기화: MEDL_001~003, COMM_001~002로 간소화 (useGet*TakenHistory, getMedicationType*, getSymptomGrade* 제거) |
