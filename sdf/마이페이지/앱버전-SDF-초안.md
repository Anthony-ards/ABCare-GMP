# 앱 버전 안내 SDF 초안

작성일: 2025-12-22
상태: 작성중
SRS 문서: [ARDS-SRS-ABC](https://docs.google.com/document/d/1vBlcPlECSGd5fz0SxNnLcloS0kxP32SzyB1ILR3kFLc/edit)
SDF 스프레드시트: [ARDS-SDF-ABC](https://docs.google.com/spreadsheets/d/1X3_srci8M4RNIIFhasAY1o8ltA9vHkiSav_Zx4RyLpk/edit?gid=1166114657#gid=1166114657)

> **주의**: 스프레드시트 입력 시 반드시 **anthony-dummy** 탭에 추가할 것

---

## SRS_PA_MYPG_VER_001: 앱 버전 안내

### SRS 원문
> 현재 앱의 버전 정보를 제공한다.
> 최신 버전이 존재할 경우 업데이트 기능을 제공한다.

### 함수 설계

| SDS ID | 함수명 | 함수 기능 설명 |
|--------|--------|----------------|
| SDS_PA_MYPG_VER_001 | ServiceVersionRow.render | 현재 앱 버전("서비스 버전 V{version}")과 업데이트 상태 출력 |
| SDS_PA_MYPG_VER_002 | handleUpdatePress | 업데이트 버튼 클릭 시 확인 팝업 표시 후 앱스토어/플레이스토어로 이동 |

---

## 코드 위치 참조

| SDS ID | 파일 경로 |
|--------|----------|
| SDS_PA_MYPG_VER_001 | src/features/mypage/components/ServiceVersionRow.tsx |
| SDS_PA_MYPG_VER_002 | src/features/mypage/components/ServiceVersionRow.tsx |

---

## 컴포넌트 상세

### ServiceVersionRow

**Props:**
```typescript
interface ServiceVersionRowProps {
  /** 업데이트 가능 여부 */
  hasUpdate?: boolean;
}
```

**렌더링 로직:**
- 항상 표시: "서비스 버전" 레이블 + 현재 버전 (V{version})
- hasUpdate === true: "업데이트" 버튼 표시 (클릭 시 스토어 이동)
- hasUpdate === false: "최신 버전입니다." 텍스트 표시

**업데이트 플로우:**
1. 사용자가 "업데이트" 버튼 클릭
2. 확인 팝업 표시 ("최신 버전으로 업데이트하시겠습니까?")
3. "업데이트" 선택 시 앱스토어/플레이스토어로 이동

---

## 수정 이력

| 날짜 | 작성자 | 내용 |
|------|-------|------|
| 2025-12-22 | Claude Code | 초안 작성 |
