# 마이페이지 SDF 초안

작성일: 2025-12-16
최종수정: 2025-12-18
상태: ✅ 최종 SDF 동기화 완료
SRS 문서: [ARDS-SRS-ABC](https://docs.google.com/document/d/1vBlcPlECSGd5fz0SxNnLcloS0kxP32SzyB1ILR3kFLc/edit)
SDF 스프레드시트: [ARDS-SDF-ABC](https://docs.google.com/spreadsheets/d/1X3_srci8M4RNIIFhasAY1o8ltA9vHkiSav_Zx4RyLpk/edit?gid=1166114657#gid=1166114657)

> **주의**: 스프레드시트 입력 시 반드시 **anthony-dummy** 탭에 추가할 것

---

## SDF 작성 지침

### 포함 대상
- SRS 요구사항에 직접 대응하는 UI 렌더 컴포넌트/함수
- 공통 컴포넌트(UIPeriodTabs, PeriodNavigator 등)는 **사용하는 모든 SRS에 포함**

### 제외 대상
- 서버 데이터 조회 함수 (useGet*, useMutation 등) → 서버 SDF에서 관리
- 전체 화면 렌더 함수 (XxxScreen.render)
- 내부 헬퍼/포맷팅 함수 (convert*, format*, get* 내부 로직)
- 장식용 차트 (WeeklyBarChart 등) → 핵심 정보 표시 함수만 포함

### 함수 기능 설명 작성 규칙
- **간결하게**: "~한다" 제거 → "출력", "표시" 등으로 끝내기
- **핵심만**: 상세 항목 나열 최소화, 어떻게 출력하는지 중심으로
- **실제 코드 기준**: 함수명은 실제 코드베이스와 일치해야 함

### SRS 원문 규칙
- **절대 수정 금지**: SRS 기능 설명은 원본 그대로 유지
- 줄바꿈, 불릿 포인트 등 형식도 그대로

---

## SRS_PA_MYPG_MAN_001: 사용자 프로필

### SRS 원문
> 사용자 개인 정보 내역을 확인할 수 있으며, 다음 정보를 포함한다.
> - 사용자 프로필 이미지
> - 사용자 이름
> - 마지막 처방 정보

### 함수 설계

| SDS ID | 함수명 | 함수 기능 설명 |
|--------|--------|---------------|
| SDS_PA_MYPG_MAN_001 | ProfileImage.render | 사용자 프로필 이미지 출력 |
| SDS_PA_MYPG_MAN_002 | UserName.render | 사용자 이름 출력 |
| SDS_PA_MYPG_MAN_003 | PrescriptionInfo.render | 처방 정보(n차 처방, 처방일) 출력 |

---

## SRS_PA_MYPG_MAN_002: 알림센터

### SRS 원문
> 사용자가 수신한 알림 내역을 확인할 수 있다.
> 새로운 알림이 발생한 경우 신규 알림을 표시한다.

### 함수 설계

| SDS ID | 함수명 | 함수 기능 설명 |
|--------|--------|---------------|
| SDS_PA_MYPG_MAN_004 | NotificationItem.render | 개별 알림 항목(시간, 메시지, 읽음상태) 출력 |
| SDS_PA_MYPG_MAN_005 | IconBellNotification.render | 알림 아이콘과 미읽은 알림 갯수 배지 출력 |

---

## SRS_PA_MYPG_MAN_003: ABC 습관점수 현황 요약

### SRS 원문
> 사용자가 획득한 주간 평균 습관점수를 표시한다.

### 함수 설계

| SDS ID | 함수명 | 함수 기능 설명 |
|--------|--------|---------------|
| SDS_PA_MYPG_MAN_006 | HabitScoreSummaryTitle.render | 습관점수 요약 카드의 제목과 기간 정보 출력 |
| SDS_PA_MYPG_MAN_007 | HabitScoreSummaryValue.render | 현재 주간 습관점수 값 출력 |
| SDS_PA_MYPG_MAN_008 | HabitScoreSummaryDiff.render | 지난주 대비 점수 증감 출력 |

---

## SRS_PA_MYPG_HABT_001: ABC 습관점수 요약

### SRS 원문
> 서비스 내 사용자 활동 내역(앱 접속, 복약 기록, 마음약국 교육)을 점수화하여
> 표시한다.

### 함수 설계

| SDS ID | 함수명 | 함수 기능 설명 |
|--------|--------|---------------|
| SDS_PA_COMM_001 | UIPeriodTabs.render | 일간/주간 기간 선택 탭 출력 |
| SDS_PA_COMM_002 | PeriodNavigator.render | 선택된 기간의 이전/다음 네비게이션 출력 |
| SDS_PA_MYPG_HABT_001 | ScoreHeader.render | 습관점수 헤더(제목, 점수, 지난주 대비) 출력 |
| SDS_PA_MYPG_HABT_002 | DailyProgressBar.render | 일간 습관점수 프로그레스 바와 범례 출력 |
| SDS_PA_MYPG_HABT_003 | StatusMessage.render | 점수 상태에 따른 안내 메시지 출력 |

---

## SRS_PA_MYPG_HABT_002: ABC 습관점수 상세

### SRS 원문
> 각 활동 내역 별 기준 점수와 획득 점수를 확인할 수 있다.

### 함수 설계

| SDS ID | 함수명 | 함수 기능 설명 |
|--------|--------|---------------|
| SDS_PA_MYPG_HABT_004 | HabitScoreList.render | 습관점수 항목별 기준/획득 점수 리스트 출력 |
| SDS_PA_MYPG_HABT_005 | WeeklyScoreDetail.render | 주간 캘린더와 선택 날짜의 점수 상세 출력 |

---

## SRS_PA_MYPG_MEDL_001: 복약 기록 요약 정보

### SRS 원문
> 기간 별 사용자의 약제 별, 통합 복약 횟수를 요약하여 제공한다.

### 함수 설계

| SDS ID | 함수명 | 함수 기능 설명 |
|--------|--------|---------------|
| SDS_PA_COMM_001 | UIPeriodTabs.render | 일간/주간 기간 선택 탭 출력 |
| SDS_PA_COMM_002 | PeriodNavigator.render | 선택된 기간의 이전/다음 네비게이션 출력 |
| SDS_PA_MYPG_MEDL_001 | TakenHistorySummaryCard.render | 복약 요약 카드(총 복약횟수, 약제별 횟수) 출력 |

---

## SRS_PA_MYPG_MEDL_002: 복약 기록 타임라인

### SRS 원문
> 일자별 복약 내역을 타임라인 형태로 제공하며, 다음 정보를 포함한다.
> - 약제명 / 통증 기록
> - 복약 기록 시간
> - 통증 강도 정보

### 함수 설계

| SDS ID | 함수명 | 함수 기능 설명 |
|--------|--------|---------------|
| SDS_PA_MYPG_MEDL_002 | MonthlyCalendar.render | 월간 복약 기록 캘린더 출력 |
| SDS_PA_MYPG_MEDL_003 | DailyRecordItem.render | 일별 복약 기록 항목(약제명, 복약시간, 통증점수) 출력 |

---

## SRS_PA_MYPG_PAIL_001: 통증 기록 요약 정보

### SRS 원문
> 기간 별 사용자의 통증 강도 기록을 요약하여 제공한다.

### 함수 설계

| SDS ID | 함수명 | 함수 기능 설명 |
|--------|--------|---------------|
| SDS_PA_COMM_001 | UIPeriodTabs.render | 일간/주간 기간 선택 탭 출력 |
| SDS_PA_COMM_002 | PeriodNavigator.render | 선택된 기간의 이전/다음 네비게이션 출력 |
| SDS_PA_MYPG_PAIL_001 | PainIntensityChartWithStats.render | 통증 통계 카드(평균통증, 최고통증, 복약기록) 출력 |

---

## SRS_PA_MYPG_PAIL_002: 통증 추이 그래프

### SRS 원문
> 기간 별 통증 기록 중 최고·최저 강도 범위를 확인할 수 있다.

### 함수 설계

| SDS ID | 함수명 | 함수 기능 설명 |
|--------|--------|---------------|
| SDS_PA_COMM_001 | UIPeriodTabs.render | 일간/주간 기간 선택 탭 출력 |
| SDS_PA_COMM_002 | PeriodNavigator.render | 선택된 기간의 이전/다음 네비게이션 출력 |
| SDS_PA_MYPG_PAIL_002 | DailyPainChart.render | 일간 통증 강도 차트(시간별 통증 점수) 출력 |
| SDS_PA_MYPG_PAIL_003 | WeeklyMonthlyPainChart.render | 주간/월간 통증 추이 차트 출력 |

---

## SRS_PA_MYPG_PAIL_003: 빠른 약제 복약 기록 정보

### SRS 원문
> 빠른 약제 복약기록 빈도를 확인할 수 있다.

### 함수 설계

| SDS ID | 함수명 | 함수 기능 설명 |
|--------|--------|---------------|
| SDS_PA_MYPG_PAIL_004 | MedicationChart.render | 빠른 약제 복약 빈도 차트 출력 |

---

## 코드 위치 참조

### 마이페이지 메인 (MAN)

| SDS ID | 파일 경로 |
|--------|----------|
| SDS_PA_MYPG_MAN_001~003 | app/(tabs)/mypage/index.tsx (인라인) |
| SDS_PA_MYPG_MAN_004 | app/(tabs)/mypage/notifications.tsx |
| SDS_PA_MYPG_MAN_005 | src/components/common/IconBellNotification.tsx |
| SDS_PA_MYPG_MAN_006~008 | app/(tabs)/mypage/index.tsx (인라인) |

### 습관점수 (HABT)

| SDS ID | 파일 경로 |
|--------|----------|
| SDS_PA_MYPG_HABT_001 | src/features/mypage/habit-score/components/ScoreHeader.tsx |
| SDS_PA_MYPG_HABT_002 | src/features/mypage/habit-score/components/DailyProgressBar.tsx |
| SDS_PA_MYPG_HABT_003 | src/features/mypage/habit-score/components/StatusMessage.tsx |
| SDS_PA_MYPG_HABT_004 | src/components/common/HabitScoreList.tsx |
| SDS_PA_MYPG_HABT_005 | src/features/mypage/habit-score/components/WeeklyScoreDetail.tsx |

### 복약기록 (MEDL)

| SDS ID | 파일 경로 |
|--------|----------|
| SDS_PA_MYPG_MEDL_001 | src/features/mypage/my-records/components/TakenHistorySummaryCard.tsx |
| SDS_PA_MYPG_MEDL_002 | lib/ui-monthly-calendar.tsx |
| SDS_PA_MYPG_MEDL_003 | src/components/common/DailyRecordItem.tsx |

### 통증기록 (PAIL)

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

## SRS_PA_MYPG_PRE_001: 차수 별 처방 정보

### SRS 원문
> 각 처방 차수 별 약제 목록과 복약 조건을 표시한다.

### 함수 설계

| SDS ID | 함수명 | 함수 기능 설명 |
|--------|--------|---------------|
| SDS_PA_MYPG_PRE_001 | CycleAccordion.render | 처방 차수 카드(복약기간 포함) 및 약제 목록 출력 |
| SDS_PA_MYPG_PRE_002 | MedicationCard.render | 약제 정보 카드(타입/복용정보/약제명) 출력 |

---

## SRS_PA_MYPG_PRE_002: 처방 정보 등록

### SRS 원문
> 신규 처방 발생 시 처방 내역을 등록할 수 있도록 하며, 다음 정보를 입력할 수 있어야 한다.
> - 처방전 발급 일자
> - 약제 별 상세 처방 내역: 약제명 / 1회 투약량 / 1일 투약횟수 / 총 투약일수
> - 처방전 사진

### 함수 설계

| SDS ID | 함수명 | 함수 기능 설명 |
|--------|--------|---------------|
| SDS_PA_MYPG_PRE_003 | PrescriptionIssueDateScreen.handleDateCardPress | 발급일 입력용 휠피커 토글 |
| SDS_PA_MYPG_PRE_004 | MedicationSelectScreen.handleSelectMedication | 검색 결과에서 약제 선택 |
| SDS_PA_MYPG_PRE_005 | MedicationFormBasic.render | 기본 약제 복용 횟수 및 시간/복용량 폼 출력 |
| SDS_PA_MYPG_PRE_006 | MedicationFormPatch.render | 패치 약제 교체 주기/시간 폼 출력 |
| SDS_PA_MYPG_PRE_007 | MedicationFormRapid.render | 속효성 약제 복용량 폼 출력 |
| SDS_PA_MYPG_PRE_008 | MedicationDetailForm.handleTotalDaysChange | 총 투약일수 증감 입력 |
| SDS_PA_MYPG_PRE_009 | PrescriptionPhotoScreen.handleSelectFromAlbum | 처방전 사진 앨범에서 선택 |
| SDS_PA_MYPG_PRE_010 | PrescriptionPhotoScreen.handleTakePhoto | 처방전 사진 촬영 |

---

### 처방 관리 (PRE)

| SDS ID | 파일 경로 |
|--------|----------|
| SDS_PA_MYPG_PRE_001 | src/features/mypage/prescription/components/CycleAccordion.tsx |
| SDS_PA_MYPG_PRE_002 | src/features/mypage/prescription/components/MedicationCard.tsx |
| SDS_PA_MYPG_PRE_003 | app/(tabs)/mypage/prescription/prescription-issue-date.tsx |
| SDS_PA_MYPG_PRE_004 | app/(tabs)/mypage/prescription/medication-select.tsx |
| SDS_PA_MYPG_PRE_005 | src/features/mypage/prescription/components/MedicationFormBasic.tsx |
| SDS_PA_MYPG_PRE_006 | src/features/mypage/prescription/components/MedicationFormPatch.tsx |
| SDS_PA_MYPG_PRE_007 | src/features/mypage/prescription/components/MedicationFormRapid.tsx |
| SDS_PA_MYPG_PRE_008 | src/features/mypage/prescription/components/MedicationDetailForm.tsx |
| SDS_PA_MYPG_PRE_009~010 | app/(tabs)/mypage/prescription/prescription-photo.tsx |

---

## 수정 이력

| 날짜 | 작성자 | 내용 |
|------|-------|------|
| 2025-12-16 | Claude Code | 초안 작성 |
| 2025-12-17 | Claude Code | 10개 SRS 전체 포함 (MAN 3개, HABT 2개, MEDL 2개, PAIL 3개) |
| 2025-12-17 | Claude Code | SDF 작성 지침 확정 (SRS 원문 유지, 공통 컴포넌트 중복 포함, 간결한 설명) |
| 2025-12-17 | Claude Code | 함수명 검증 완료 (실제 코드베이스와 일치 확인) |
| 2025-12-17 | Claude Code | 알림 항목에서 카테고리 제거, PAIL_002에 기간탭 추가 |
| 2025-12-18 | Claude Code | PRE_001 (차수별 처방정보), PRE_002 (처방정보 등록) 추가 - 총 10개 SDS |
