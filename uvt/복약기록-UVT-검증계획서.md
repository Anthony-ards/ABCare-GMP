# 복약기록 유닛테스트 검증계획서

작성일: 2025-12-19
상태: 작성중
테스트 파일: `src/__tests__/medication-record-snapshot.test.tsx`

---

## 테스트 개요

복약기록 관련 UI 컴포넌트의 스냅샷 테스트 및 검증 포인트 테스트

### 테스트 대상 SDS
| SDS ID | 함수명 | 테스트 범위 |
|--------|--------|------------|
| SDS_PA_MYPG_MEDL_001 | TakenHistorySummaryCard.render | 스냅샷 3개 + 검증포인트 2개 |
| SDS_PA_MYPG_MEDL_003 | DailyRecordItem.render | 스냅샷 3개 + 검증포인트 3개 |

---

## SDS_PA_MYPG_MEDL_001: TakenHistorySummaryCard

### UVT_PA_MEDL_SNAP_001: 기본 복약 요약 카드 렌더링

**입력값:**
```typescript
{
  total: 45,
  basic: 30,
  irregular: 10,
  fast: 5
}
```

**기대 출력 (스냅샷 핵심 발췌):**
```jsx
<View testID="taken-history-summary-card">
  <View testID="total-section">
    <Text>누적 복약 기록</Text>
    <Text testID="total-count">45회</Text>
  </View>
  <View testID="medication-types">
    <Text>기본 약제</Text><Text testID="basic-count">30회</Text>
    <Text>붙이는 약제</Text><Text testID="irregular-count">10회</Text>
    <Text>빠른 약제</Text><Text testID="fast-count">5회</Text>
  </View>
</View>
```

---

### UVT_PA_MEDL_SNAP_002: 계획 횟수 포함된 복약 요약 카드 렌더링

**입력값:**
```typescript
{
  total: 45,
  basic: 30,
  basicScheduled: 35,
  irregular: 10,
  irregularScheduled: 12,
  fast: 5
}
```

**기대 출력 (스냅샷 핵심 발췌):**
```jsx
<View testID="taken-history-summary-card">
  <Text testID="total-count">45회</Text>
  <View testID="basic-medication">
    <Text testID="basic-count">30회</Text>
    <Text>/ 35회</Text>  <!-- 계획 횟수 표시 -->
  </View>
  <View testID="irregular-medication">
    <Text testID="irregular-count">10회</Text>
    <Text>/ 12회</Text>  <!-- 계획 횟수 표시 -->
  </View>
</View>
```

---

### UVT_PA_MEDL_SNAP_003: 모든 값이 0인 경우 렌더링

**입력값:**
```typescript
{
  total: 0,
  basic: 0,
  irregular: 0,
  fast: 0
}
```

**기대 출력 (스냅샷 핵심 발췌):**
```jsx
<View testID="taken-history-summary-card">
  <Text testID="total-count">0회</Text>
  <Text testID="basic-count">0회</Text>
  <Text testID="irregular-count">0회</Text>
  <Text testID="fast-count">0회</Text>
</View>
```

---

### UVT_PA_MEDL_VP_001: 총 복약 횟수 검증

**검증 방법:** `getByTestId`, `getByText`

**검증 포인트:**
- `getByTestId('total-count')` → 존재 확인
- `getByText('45회')` → 총 복약 횟수 표시
- `getByText('누적 복약 기록')` → 레이블 텍스트

---

### UVT_PA_MEDL_VP_002: 약제별 복약 횟수 검증

**검증 방법:** `getByText`

**검증 포인트:**
- `getByText('기본 약제')` + `getByText('30회')` → 기본 약제 횟수
- `getByText('붙이는 약제')` + `getByText('10회')` → 붙이는 약제 횟수
- `getByText('빠른 약제')` + `getByText('5회')` → 빠른 약제 횟수

---

## SDS_PA_MYPG_MEDL_003: DailyRecordItem

### UVT_PA_MEDL_SNAP_004: 복약 기록만 있는 타임라인 렌더링

**입력값:**
```typescript
{
  dateTitle: '1월 15일',
  records: [
    { record_type: 'TAKEN_RECORD', is_am: true, hour: 9, minute: 0, product_name: '뉴신타서방정50mg' },
    { record_type: 'TAKEN_RECORD', is_am: false, hour: 9, minute: 0, product_name: '뉴신타서방정50mg' }
  ]
}
```

**기대 출력 (스냅샷 핵심 발췌):**
```jsx
<View testID="daily-record-item">
  <Text testID="date-title">1월 15일</Text>
  <View testID="records-list">
    <View testID="record-0">
      <Text testID="record-0-name">뉴신타서방정50mg</Text>
      <Text testID="record-0-time">오전 9:00</Text>
    </View>
    <View testID="record-1">
      <Text testID="record-1-name">뉴신타서방정50mg</Text>
      <Text testID="record-1-time">오후 9:00</Text>
    </View>
  </View>
</View>
```

---

### UVT_PA_MEDL_SNAP_005: 통증 기록 포함된 타임라인 렌더링

**입력값:**
```typescript
{
  dateTitle: '1월 15일',
  records: [
    { record_type: 'TAKEN_RECORD', is_am: true, hour: 9, minute: 0, product_name: '뉴신타서방정50mg' },
    { record_type: 'PAIN_RECORD', is_am: false, hour: 2, minute: 30, pain_score: 5 }
  ]
}
```

**기대 출력 (스냅샷 핵심 발췌):**
```jsx
<View testID="daily-record-item">
  <Text testID="date-title">1월 15일</Text>
  <View testID="record-0">
    <Text testID="record-0-name">뉴신타서방정50mg</Text>
    <Text testID="record-0-time">오전 9:00</Text>
  </View>
  <View testID="record-1">
    <Text testID="record-1-name">통증 기록</Text>
    <Text testID="record-1-time">오후 2:30</Text>
    <Text testID="record-1-pain-score">5점</Text>
  </View>
</View>
```

---

### UVT_PA_MEDL_SNAP_006: 빈 기록 렌더링

**입력값:**
```typescript
{
  dateTitle: '1월 16일',
  records: []
}
```

**기대 출력 (스냅샷 핵심 발췌):**
```jsx
<View testID="daily-record-item">
  <Text testID="date-title">1월 16일</Text>
  <View testID="records-list" />  <!-- 빈 리스트 -->
</View>
```

---

### UVT_PA_MEDL_VP_003: 날짜 타이틀 검증

**검증 방법:** `getByTestId`

**검증 포인트:**
- `getByTestId('date-title').props.children` === `'1월 15일'`

---

### UVT_PA_MEDL_VP_004: 복약 기록 시간 검증

**검증 방법:** `getByTestId`

**검증 포인트:**
- `getByTestId('record-0-time').props.children` === `'오전 9:00'`
- `getByTestId('record-0-name').props.children` === `'뉴신타서방정50mg'`

---

### UVT_PA_MEDL_VP_005: 통증 점수 검증

**검증 방법:** `getByTestId`

**검증 포인트:**
- `getByTestId('record-0-pain-score').props.children` === `[5, '점']`
- `getByTestId('record-0-name').props.children` === `'통증 기록'`

---

## Google Sheets 입력용 간소화 버전

| UVT ID | 테스트명 | 입력값 | 기대 출력 (스냅샷 핵심) |
|--------|---------|--------|------------------------|
| UVT_PA_MEDL_SNAP_001 | 기본 복약 요약 카드 | total:45, basic:30, irregular:10, fast:5 | `<Text testID="total-count">45회</Text>` + 약제별 횟수 표시 |
| UVT_PA_MEDL_SNAP_002 | 계획 횟수 포함 요약 | basicScheduled:35, irregularScheduled:12 추가 | 횟수 + `/ 계획횟수` 형태 표시 |
| UVT_PA_MEDL_SNAP_003 | 모든 값 0 | total:0, basic:0, irregular:0, fast:0 | 모든 횟수 `0회` 표시 |
| UVT_PA_MEDL_SNAP_004 | 복약 기록 타임라인 | TAKEN_RECORD 2건 | 날짜, 약제명, 오전/오후 시간 표시 |
| UVT_PA_MEDL_SNAP_005 | 통증 포함 타임라인 | TAKEN + PAIN_RECORD | 복약 + 통증기록(점수) 표시 |
| UVT_PA_MEDL_SNAP_006 | 빈 기록 | records: [] | 날짜만 표시, 빈 리스트 |

---

## 수정 이력

| 날짜 | 작성자 | 내용 |
|------|-------|------|
| 2025-12-19 | Claude Code | 스냅샷 핵심 발췌 형식으로 초안 작성 |
