# ABCare GMP 문서화 프로젝트

ABCare 디지털치료제 앱의 식약처 인증을 위한 GMP(Good Manufacturing Practice) 문서화 작업.

---

## 문서 개요

| 문서 | 설명 | 링크 |
|------|------|------|
| [GMP 개념 가이드](./gmp-concept-guide.md) | GMP 기본 개념, 국제표준, 문서체계 설명 | 개발자 필독 |
| [개발자 역할 가이드](./developer-gmp-role.md) | 개발자가 담당하는 GMP 문서와 작성법 | 개발자 필독 |
| [문서 작성 계획](./abcare-gmp-document-plan.md) | ABCare 인증을 위한 문서 작성 계획 | 전체 로드맵 |
| [SDF 워크플로우](./sdf-workflow.md) | SDF 작성 프로세스 및 규칙 | 작업 시 참고 |
| [IRB 개념 가이드](./irb-concept-guide.md) | IRB 심의와 임상시험 관련 개념 | 임상시험 계획 시 |

---

## 주요 외부 문서 (Google Drive)

| 문서 | 형식 | 링크 | 작업 탭 |
|------|------|------|---------|
| SRS (요구사항 명세서) | Google Docs | [ARDS-SRS-ABC](https://docs.google.com/document/d/1vBlcPlECSGd5fz0SxNnLcloS0kxP32SzyB1ILR3kFLc/edit) | - |
| SDF (상세 설계 파일) | Google Sheets | [ARDS-SDF-ABC](https://docs.google.com/spreadsheets/d/1X3_srci8M4RNIIFhasAY1o8ltA9vHkiSav_Zx4RyLpk/edit) | `anthony-dummy` |
| UVT (유닛테스트 검증계획서) | Google Sheets | [UVT 스프레드시트](https://docs.google.com/spreadsheets/d/1z3qiZKsj2DBIS5dVq00ZHxrUDF53dJrTw7LxJ9OMais/edit) | `DummyAnthony` |
| RTM (추적성 매트릭스) | Google Sheets | [ARDS-RTM-ABC](https://docs.google.com/spreadsheets/d/1rylN1236f_U6p2xHb_GGP2uhGtvK6vpZ0wbybfQ2Bus/edit) | - |

---

## 폴더 구조

```
/develop-doc/gmp/ABCare/
├── README.md                         # 이 파일
├── gmp-concept-guide.md              # GMP 기본 개념 가이드
├── developer-gmp-role.md             # 개발자 GMP 역할 가이드
├── abcare-gmp-document-plan.md       # 문서 작성 계획서
├── sdf-workflow.md                   # SDF 작업 프로세스
├── irb-concept-guide.md              # IRB 개념 가이드
├── google-workspace-mcp-setup-guide.md  # MCP 설정 가이드 (작업용)
├── playwright-mcp-bridge-guide.md    # Playwright 설정 가이드 (작업용)
│
├── sdf/                              # SDF 초안 산출물
│   ├── SDF-작업-가이드.md             # SDF 작성 상세 가이드
│   ├── 마이페이지/                    # 마이페이지 기능 SDF
│   │   ├── 마이페이지-SDF-초안.md     # 마이페이지 전체 SDF 초안
│   │   ├── 복약기록-SDF-초안.md       # 복약기록 상세 (참고용)
│   │   ├── 통증기록-SDF-초안.md       # 통증기록 상세 (참고용)
│   │   └── 앱버전-SDF-초안.md         # 앱 버전 안내 SDF
│   └── 마음약국/                      # 마음약국(CBT) 기능 SDF
│       └── 마음약국-SDF-초안.md       # 마음약국 전체 SDF 초안
│
└── uvt/                              # 유닛테스트 검증계획서
    └── 복약기록-UVT-검증계획서.md     # 복약기록 스냅샷 테스트 검증계획
```

---

## SDF 산출물 (`sdf/` 폴더)

### 마이페이지 SDF 현황

마이페이지 기능에 대한 SDF 초안 작성 완료. 최종 산출물은 Google Sheets에 반영.

| SRS 그룹 | 항목 수 | 상태 |
|---------|--------|------|
| MAN (메인) | 3개 | ✅ 완료 |
| HABT (습관점수) | 2개 | ✅ 완료 |
| MEDL (복약기록) | 2개 | ✅ 완료 |
| PAIL (통증기록) | 3개 | ✅ 완료 |
| PRE (처방관리) | 2개 | ✅ 완료 |

**총 12개 SRS → 36개 SDS 함수 매핑**

### 마음약국 SDF 현황

마음약국(CBT/인지행동치료) 기능에 대한 SDF 초안 작성 완료.

| SRS 그룹 | 항목 수 | 상태 |
|---------|--------|------|
| MAN (메인) | 2개 | ✅ 완료 |
| EDU (교육진행) | 1개 | ✅ 완료 |

**총 3개 SRS → 9개 SDS 함수 매핑**

### SDF 작성 규칙 요약

1. **SRS 원문 유지**: 기능 설명은 SRS 원문 그대로
2. **함수명 검증**: 실제 코드베이스와 일치하는 함수명 사용
3. **공통 컴포넌트**: 사용하는 모든 SRS에 중복 포함
4. **간결한 설명**: "~한다" 제거, "출력", "표시" 등으로 끝내기

상세 가이드: [`sdf/SDF-작업-가이드.md`](./sdf/SDF-작업-가이드.md)

---

## 인증 프로세스 흐름

```
┌─────────────────────────────────────────────────────────────────┐
│                    의료기기 소프트웨어 인증 흐름                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   1. 제품 개발                                                   │
│      └─ ABCare patient-app 개발                                 │
│                                                                 │
│   2. GMP 문서 작성                                               │
│      ├─ SRS (요구사항 명세서)                                    │
│      ├─ SDF (상세 설계 파일)        ← 현재 진행중                │
│      ├─ STP/STR (테스트 계획/결과)                               │
│      └─ RMP (위험관리)                                          │
│                                                                 │
│   3. 임상시험 (필요시)                                           │
│      ├─ IRB 승인                                                │
│      └─ 임상시험 실시                                           │
│                                                                 │
│   4. 식약처 기술문서 심사                                        │
│      └─ GMP 문서 + 임상 결과 제출                                │
│                                                                 │
│   5. 인허가 획득 → 시판                                          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 준수 규격

| 규격 | 설명 |
|------|------|
| IEC 62304:2006+A1:2015 | 의료기기 소프트웨어 수명주기 프로세스 |
| ISO 14971:2019 | 의료기기 위험관리 |
| ISO 13485:2016 | 의료기기 품질경영시스템 |
| IEC 62366-1:2015 | 사용적합성 엔지니어링 |

---

## 작업 참고

- **SDF 작업 시**: Google Sheets `anthony-dummy` 탭에 먼저 입력 후 검토
- **코드 분석**: `/Users/anthony/workspace/ABC-DTx/patient-app/` 기준
- **Claude Code 활용**: Google Workspace MCP로 문서 직접 수정 가능

---

*작성일: 2025-12-17*
*담당: Anthony (모바일 앱)*
