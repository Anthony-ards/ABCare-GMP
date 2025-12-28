# 나의 통증기록 SDF 초안

작성일: 2025-12-16
상태: ✅ 최종 SDF 동기화 완료
SRS 문서: [ARDS-SRS-ABC](https://docs.google.com/document/d/1vBlcPlECSGd5fz0SxNnLcloS0kxP32SzyB1ILR3kFLc/edit)

---

## SRS_PA_MYPG_PAIL_001: 통증 기록 요약 정보

### SRS 원문
> 기간 별 사용자의 통증 강도 기록을 요약하여 제공한다.

### 함수 설계

| SRS ID | 주요 기능 | 기능 설명 | SDS ID | 함수명 | 함수 기능 설명 |
|--------|----------|----------|--------|--------|---------------|
| SRS_PA_MYPG_PAIL_001 | 통증 기록 요약 정보 | 기간 별 사용자의 통증 강도 기록을 요약하여 제공한다 | SDS_PA_COMM_001 | UIPeriodTabs.render() | 일간/주간 기간 선택 탭 출력 |
| | | | SDS_PA_COMM_002 | PeriodNavigator.render() | 선택된 기간의 이전/다음 네비게이션 출력 |
| | | | SDS_PA_MYPG_PAIL_001 | PainIntensityChartWithStats.render() | 통증 통계 카드(평균통증, 최고통증, 복약기록) 출력 |

---

## SRS_PA_MYPG_PAIL_002: 통증 추이 그래프

### SRS 원문
> 기간 별 통증 기록 중 최고·최저 강도 범위를 확인할 수 있다.

### 함수 설계

| SRS ID | 주요 기능 | 기능 설명 | SDS ID | 함수명 | 함수 기능 설명 |
|--------|----------|----------|--------|--------|---------------|
| SRS_PA_MYPG_PAIL_002 | 통증 추이 그래프 | 기간 별 통증 기록 중 최고·최저 강도 범위를 확인할 수 있다 | SDS_PA_COMM_001 | UIPeriodTabs.render() | 일간/주간 기간 선택 탭 출력 |
| | | | SDS_PA_COMM_002 | PeriodNavigator.render() | 선택된 기간의 이전/다음 네비게이션 출력 |
| | | | SDS_PA_MYPG_PAIL_002 | DailyPainChart.render() | 일간 통증 강도 차트(시간별 통증 점수) 출력 |
| | | | SDS_PA_MYPG_PAIL_003 | WeeklyMonthlyPainChart.render() | 주간/월간 통증 추이 차트 출력 |

---

## SRS_PA_MYPG_PAIL_003: 빠른 약제 복약 기록 정보

### SRS 원문
> 빠른 약제 복약기록 빈도를 확인할 수 있다.

### 함수 설계

| SRS ID | 주요 기능 | 기능 설명 | SDS ID | 함수명 | 함수 기능 설명 |
|--------|----------|----------|--------|--------|---------------|
| SRS_PA_MYPG_PAIL_003 | 빠른 약제 복약 기록 정보 | 빠른 약제 복약기록 빈도를 확인할 수 있다 | SDS_PA_MYPG_PAIL_004 | MedicationChart.render() | 빠른 약제 복약 빈도 차트 출력 |

---

## 코드 위치 참조

### 통증기록 전용 (PAIL)

| SDS ID | 파일 경로 |
|--------|----------|
| SDS_PA_MYPG_PAIL_001 | src/features/mypage/pain/components/PainIntensityChartWithStats.tsx |
| SDS_PA_MYPG_PAIL_002 | src/features/mypage/pain/components/DailyPainChart.tsx |
| SDS_PA_MYPG_PAIL_003 | src/features/mypage/pain/components/WeeklyMonthlyPainChart.tsx |
| SDS_PA_MYPG_PAIL_004 | src/features/mypage/pain/components/MedicationChart.tsx |

### 공통 컴포넌트 (COMM)

| SDS ID | 파일 경로 |
|--------|----------|
| SDS_PA_COMM_001 | src/components/common/PeriodTabs.tsx |
| SDS_PA_COMM_002 | lib/ui-period-navigator.tsx |

---

## 수정 이력

| 날짜 | 작성자 | 내용 |
|------|-------|------|
| 2025-12-16 | Claude Code | 초안 작성 |
| 2025-12-16 | Claude Code | 리팩토링 완료: 컴포넌트를 src/features/mypage/pain/으로 분리, 파일 경로 업데이트 |
| 2025-12-16 | Claude Code | 공통 기능 분리: SDS_PA_COMM ID 체계 적용, PAIL 전용 ID 재정의 |
| 2025-12-18 | Claude Code | 최종 SDF와 동기화: PAIL_001~004로 간소화 (useGet*PainHistory, PainManagementScreen, PainIntensityChart, getSymptomGradeIcon 제거) |
