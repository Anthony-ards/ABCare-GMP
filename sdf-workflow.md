# ABCare SDF 작업 방식

GMP SDF(소프트웨어 상세설계파일) 작성을 위한 작업 프로세스.

---

## 1. 문서 위치

### 1.1 Google Drive 원본
| 문서 | 형식 | 링크 |
|------|------|------|
| SRS (요구사항) | Google Docs | [ARDS-SRS-ABC](https://docs.google.com/document/d/1vBlcPlECSGd5fz0SxNnLcloS0kxP32SzyB1ILR3kFLc/edit) |
| SDF (상세설계) | Google Sheets | [ARDS-SDF-ABC](https://docs.google.com/spreadsheets/d/1X3_srci8M4RNIIFhasAY1o8ltA9vHkiSav_Zx4RyLpk/edit) |
| RTM (추적성) | Google Sheets | [ARDS-RTM-ABC](https://docs.google.com/spreadsheets/d/1rylN1236f_U6p2xHb_GGP2uhGtvK6vpZ0wbybfQ2Bus/edit) |

### 1.2 로컬 작업 경로
```
/Users/anthony/workspace/develop-doc/gmp/ABCare/
├── sdf/                          # SDF 초안 폴더
│   └── 마이페이지/
│       └── 복약기록-SDF-초안.md   # 복약기록 SDF 초안
├── sdf-workflow.md               # 이 문서
└── ...
```

---

## 2. 작업 프로세스

```
┌─────────────────────────────────────────────────────────────┐
│                    SDF 작성 워크플로우                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   Step 1                Step 2              Step 3          │
│   ──────────────────────────────────────────────────────    │
│                                                             │
│   코드 분석      →     로컬 초안 작성    →    컨펌 & 업데이트  │
│   (patient-app)        (sdf-draft.md)       (Google Sheets) │
│                                                             │
│   • 함수/컴포넌트       • 마크다운 형식     • Anthony 검토     │
│   • hooks              • SRS ID 매핑       • 수정 요청 반영   │
│   • API 호출           • 함수 설명 작성    • Sheets 직접 수정 │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 3. SDF 작성 규칙

### 3.1 ID 체계

**SRS ID 형식**: `SRS_PA_[기능그룹]_[번호]`
```
SRS_PA_MYPG_MEDL_001
│   │  │     │    │
│   │  │     │    └─ 순번
│   │  │     └────── 세부 기능 (MEDL = Medication Log)
│   │  └──────────── 기능 그룹 (MYPG = MyPage)
│   └─────────────── 플랫폼 (PA = Patient App)
└──────────────────── 문서 유형
```

**SDS ID 형식**: `SDS_PA_[기능그룹]_[번호]`
```
SDS_PA_MYPG_MEDL_001
│   │  │     │    │
│   │  │     │    └─ 순번 (SRS와 1:N 관계이므로 001, 002, 003...)
│   │  │     └────── 세부 기능
│   │  └──────────── 기능 그룹
│   └─────────────── 플랫폼
└──────────────────── 문서 유형 (SDS = Software Design Specification)
```

### 3.2 함수명 작성 규칙

| 유형 | 네이밍 패턴 | 예시 |
|------|------------|------|
| Screen 컴포넌트 | `[Screen명].render()` | `MedicationLogScreen.render()` |
| Custom Hook | `use[기능명]()` | `useMedicationHistory()` |
| 일반 컴포넌트 | `[컴포넌트명].render()` | `MedicationTimeline.render()` |
| 유틸 함수 | `[동사][명사]()` | `formatMedicationData()` |
| API 호출 | `fetch[자원명]()` / `mutation[동작]()` | `fetchMedicationLogs()` |

### 3.3 함수 기능 설명 작성

**좋은 예**:
- "사용자의 기간별 복약 기록 목록을 서버에서 조회하여 반환한다"
- "복약 기록 데이터를 타임라인 표시용 포맷으로 변환한다"

**나쁜 예**:
- "데이터를 가져온다"
- "처리한다"

### 3.4 SDF 작성 핵심 원칙 (중요!)

```
⚠️ SRS 원문 그대로만 작성할 것!
```

**절대 하면 안 되는 것**:
- 코드에서 본 기능을 SRS 요구사항인 것처럼 추가하기
- SRS에 없는 상세 내용 임의로 넣기 (색상, 캘린더, 계획 대비 등)

**이유**:
- 식약처 심사 시 SRS ↔ SDF ↔ 테스트 추적성 검증
- SRS에 없는 내용이 SDF에 있으면 → "이 기능 SRS에 있나?" → 추적성 결함
- 너무 상세하면 검증 통과 시 불필요한 문제 발생

**올바른 접근**:
```
SRS 원문: "기간 별 사용자의 약제 별, 통합 복약 횟수를 요약하여 제공한다"
↓
SDF: 위 원문 그대로 + 구현 함수 매핑만
```

---

## 4. 기능별 작업 순서

### 4.1 마이페이지 (MYPG)

| 순서 | SRS ID | 기능명 | 상태 |
|------|--------|--------|------|
| 1 | SRS_PA_MYPG_MEDL_001 | 복약 기록 요약 정보 | 🔄 작업중 |
| 2 | SRS_PA_MYPG_MEDL_002 | 복약 기록 타임라인 | ⏳ 대기 |
| 3 | SRS_PA_MYPG_PAIL_001 | 통증 기록 요약 정보 | ⏳ 대기 |
| 4 | SRS_PA_MYPG_PAIL_002 | 통증 추이 그래프 | ⏳ 대기 |
| 5 | SRS_PA_MYPG_PAIL_003 | 빠른 약제 복약 기록 | ⏳ 대기 |
| ... | ... | ... | ... |

### 4.2 홈 (HOME)
| 순서 | SRS ID | 기능명 | 상태 |
|------|--------|--------|------|
| 1 | SRS_PA_HOME_MED_001 | 복약 타이머 | ⏳ 대기 |
| 2 | SRS_PA_HOME_MED_002 | 오늘의 기록 요약 | ⏳ 대기 |
| ... | ... | ... | ... |

---

## 5. 컨펌 체크리스트

### 로컬 초안 검토 시
- [ ] SRS ID와 정확히 매핑되었는가?
- [ ] 함수명이 실제 코드와 일치하는가?
- [ ] 함수 설명이 명확하고 구체적인가?
- [ ] 누락된 함수가 없는가?

### Google Sheets 업데이트 전
- [ ] Anthony 컨펌 완료
- [ ] 수정 요청 사항 반영 완료
- [ ] 최종 검토 완료

---

## 6. 명령어 참고

### Claude Code에서 사용
```bash
# SDF 초안 보기
Read /Users/anthony/workspace/develop-doc/gmp/ABCare/sdf-draft.md

# Google Sheets 업데이트 (컨펌 후)
# → Claude가 modify_sheet_values로 직접 반영
```

---

*작성일: 2025-12-15*
*작성자: Claude Code*
