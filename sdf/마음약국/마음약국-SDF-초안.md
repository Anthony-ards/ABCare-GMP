# 마음약국 SDF 초안

작성일: 2025-12-17
최종수정: 2025-12-18
상태: ✅ 최종 SDF 동기화 완료
SRS 문서: [ARDS-SRS-ABC](https://docs.google.com/document/d/1vBlcPlECSGd5fz0SxNnLcloS0kxP32SzyB1ILR3kFLc/edit)
SDF 스프레드시트: [ARDS-SDF-ABC](https://docs.google.com/spreadsheets/d/1X3_srci8M4RNIIFhasAY1o8ltA9vHkiSav_Zx4RyLpk/edit?gid=1166114657#gid=1166114657)

> **주의**: 스프레드시트 입력 시 반드시 **anthony-dummy** 탭에 추가할 것

---

## SDF 작성 지침

### 포함 대상
- SRS 요구사항에 직접 대응하는 UI 렌더 컴포넌트/함수
- 챗봇 대화 인터페이스의 각 블록 타입별 컴포넌트

### 제외 대상
- 서버 데이터 조회 함수 (useGet*, useMutation 등) → 서버 SDF에서 관리
- 전체 화면 렌더 함수 (XxxScreen.render)
- 내부 헬퍼/포맷팅 함수 (convert*, format*, get* 내부 로직)
- 애니메이션/스타일 헬퍼 함수

### 함수 기능 설명 작성 규칙
- **간결하게**: "~한다" 제거 → "출력", "표시" 등으로 끝내기
- **핵심만**: 상세 항목 나열 최소화, 어떻게 출력하는지 중심으로
- **실제 코드 기준**: 함수명은 실제 코드베이스와 일치해야 함

### SRS 원문 규칙
- **절대 수정 금지**: SRS 기능 설명은 원본 그대로 유지
- 줄바꿈, 불릿 포인트 등 형식도 그대로

---

## SRS_PA_MIND_MAN_001: 교육 이수 완료 현황

### SRS 원문
> 4주간 진행되는 인지행동치료 교육 진행 상태를 표시한다.

### 함수 설계

| SDS ID | 함수명 | 함수 기능 설명 |
|--------|--------|-----------------|
| SDS_PA_MIND_MAN_001 | CbtProgress.render | 4주 교육 진행률(완료/전체 세션 백분율) 출력 |

---

## SRS_PA_MIND_MAN_002: 교육 모아보기

### SRS 원문
> 주차 별 교육 콘텐츠 목록을 제공한다.

### 함수 설계

| SDS ID | 함수명 | 함수 기능 설명 |
|--------|--------|-----------------|
| SDS_PA_MIND_MAN_002 | LessonCard.render | 주차별 레슨 카드(제목, 세션 리스트 확장) 출력 |
| SDS_PA_MIND_MAN_003 | SessionItem.render | 세션 항목(제목, 진행상태 아이콘) 출력 |

---

## SRS_PA_MIND_EDU_001: 교육 진행

### SRS 원문
> 인지행동치료 기반 교육 과정을 챗봇과 대화 형식으로 학습하며, 다음의 인터렉티브 요소를 포함한다.
> - 교육형 텍스트 블록
> - 이미지형 설명 블록
> - 참여형 질문 답변 블록
> - 피드백형 시스템 메시지
>
> 참여형 질문 답변 블록 내 답변 선택 결과를 서버에 전송하여 문진에 반영한다.

### 함수 설계

| SDS ID | 함수명 | 함수 기능 설명 |
|--------|--------|-----------------|
| SDS_PA_MIND_EDU_001 | renderSessionStep | 콘텐츠 유형별 챗 UI 말풍선 출력 |
| SDS_PA_MIND_EDU_002 | SystemMessageBubble.render | 교육형 텍스트 블록(챗봇 메시지) 출력 |
| SDS_PA_MIND_EDU_003 | ImageStep.render | 이미지형 설명 블록(터치 시 전체화면 확대) 출력 |
| SDS_PA_MIND_EDU_004 | ChoiceStep.render | 참여형 질문 답변 블록(질문+선택지+제출) 출력 |
| SDS_PA_MIND_EDU_005 | ChoiceButton.render | 선택지 버튼(단일/다중 선택 지원) 출력 |
| SDS_PA_MIND_EDU_006 | UserAnswerBubble.render | 사용자 선택 답변 말풍선 출력 |

---

## 코드 위치 참조

### 마음약국 메인 (MAN)

| SDS ID | 파일 경로 |
|--------|----------|
| SDS_PA_MIND_MAN_001 | src/components/cbt/CbtProgress.tsx |
| SDS_PA_MIND_MAN_002 | src/components/cbt/LessonCard.tsx |
| SDS_PA_MIND_MAN_003 | src/components/cbt/SessionItem.tsx |

### 교육 진행 (EDU)

| SDS ID | 파일 경로 |
|--------|----------|
| SDS_PA_MIND_EDU_001 | app/(tabs)/cbt/session.tsx (인라인) |
| SDS_PA_MIND_EDU_002 | src/features/cbt/components/SystemMessageBubble.tsx |
| SDS_PA_MIND_EDU_003 | src/features/cbt/components/ImageStep.tsx |
| SDS_PA_MIND_EDU_004 | src/features/cbt/components/ChoiceStep.tsx |
| SDS_PA_MIND_EDU_005 | src/features/cbt/components/ChoiceButton.tsx |
| SDS_PA_MIND_EDU_006 | src/features/cbt/components/UserAnswerBubble.tsx |

---

## 수정 이력

| 날짜 | 작성자 | 내용 |
|------|-------|------|
| 2025-12-17 | Claude Code | 초안 작성 (MAN 2개, EDU 1개 → 10개 SDS 함수 매핑) |
| 2025-12-17 | Claude Code | SubmitButton, EndStep 제거 (허가용 문서 기능 명확성 기준) → 8개 SDS 함수 |
| 2025-12-17 | Claude Code | renderSessionStep 추가 및 순서 조정 (첫 번째로 이동) → 9개 SDS 함수 |
