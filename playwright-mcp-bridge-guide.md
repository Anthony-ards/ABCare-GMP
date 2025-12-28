# Playwright MCP Bridge Extension 가이드

작성일: 2025-12-16

---

## 1. 개요

Playwright MCP Bridge는 **이미 로그인된 Chrome 브라우저**에서 Playwright 자동화를 실행할 수 있게 해주는 확장 프로그램이다.

### 왜 필요한가?

기본 Playwright MCP는 새 브라우저 인스턴스를 실행함:
- 로그인 상태 없음 (Google, GitHub 등 재로그인 필요)
- 쿠키/세션 없음
- 매번 인증 필요

**Extension 모드**를 사용하면:
- 기존 Chrome 프로필의 로그인 상태 유지
- 쿠키, 세션, 인증 토큰 그대로 사용
- Google Sheets 같은 로그인 필요 서비스 바로 접근 가능

---

## 2. 동작 원리

### 2.1 아키텍처

```
┌─────────────────────────────────────────────────────────────────┐
│                        동작 흐름도                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   Claude Code          Playwright MCP         Chrome Browser     │
│   ───────────────────────────────────────────────────────────    │
│                                                                  │
│   mcp__playwright__*  ──→  MCP Server     ──→  Extension        │
│   (도구 호출)             (npx @playwright/mcp)  (Service Worker) │
│                              │                      │            │
│                              │    WebSocket 연결    │            │
│                              └──────────────────────┘            │
│                                                                  │
│   Extension이 브라우저 탭에 대한 CDP 접근 권한 제공                  │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### 2.2 핵심 컴포넌트

| 컴포넌트 | 역할 |
|----------|------|
| **Service Worker** | 확장 프로그램의 백그라운드 스크립트. WebSocket 서버 역할로 MCP 서버와 통신 |
| **CDP (Chrome DevTools Protocol)** | Chrome 내부 API. 탭 조작, DOM 접근, 스크린샷 등 모든 브라우저 제어 담당 |
| **Extension Token** | MCP 서버 ↔ 확장 프로그램 간 인증. 무단 연결 방지 |

### 2.3 통신 흐름

```
1. Claude Code에서 mcp__playwright__browser_navigate 호출
2. Playwright MCP 서버가 명령 수신
3. MCP 서버 → Extension (WebSocket으로 연결)
4. Extension → Chrome CDP API 호출
5. Chrome이 실제 페이지 조작 수행
6. 결과를 역순으로 반환
```

### 2.4 Service Worker 상세

Chrome 확장 프로그램의 Service Worker는:
- **Manifest V3** 기반 (최신 Chrome 확장 표준)
- **지속적 백그라운드 실행** (브라우저가 열려있는 동안)
- **WebSocket 리스닝** (MCP 서버의 연결 대기)
- **chrome.debugger API** 사용 (CDP 접근)

```javascript
// Service Worker 동작 예시 (개념적)
chrome.runtime.onInstalled.addListener(() => {
  // WebSocket 서버 시작
  startWebSocketServer(port);
});

// MCP 서버로부터 명령 수신
ws.onmessage = async (event) => {
  const { action, params } = JSON.parse(event.data);

  // CDP를 통해 브라우저 제어
  const result = await chrome.debugger.sendCommand(
    { tabId: activeTab },
    action,
    params
  );

  ws.send(JSON.stringify(result));
};
```

---

## 3. 설치 및 설정

### 3.1 확장 프로그램 설치

**방법 1: GitHub 릴리스에서 다운로드**
1. https://github.com/microsoft/playwright-mcp/releases 접속
2. 최신 버전의 `extension.zip` 다운로드
3. 압축 해제

**방법 2: 소스에서 빌드** (개발자용)
```bash
git clone https://github.com/microsoft/playwright-mcp.git
cd playwright-mcp/extension
npm install && npm run build
```

### 3.2 Chrome에 로드

1. `chrome://extensions/` 접속
2. 우측 상단 **"개발자 모드"** 활성화
3. **"압축해제된 확장 프로그램을 로드합니다"** 클릭
4. 다운로드/빌드한 extension 폴더 선택

### 3.3 토큰 복사

1. Chrome 툴바에서 Playwright MCP Bridge 아이콘 클릭
2. **PLAYWRIGHT_MCP_EXTENSION_TOKEN** 값 복사
3. Claude Code 설정에 추가

---

## 4. Claude Code 설정

### 4.1 글로벌 설정 파일 위치

```
~/.claude/.mcp.json
```

### 4.2 설정 내용

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": [
        "@playwright/mcp@latest",
        "--extension"
      ],
      "env": {
        "PLAYWRIGHT_MCP_EXTENSION_TOKEN": "여기에_토큰_붙여넣기"
      }
    }
  }
}
```

### 4.3 현재 설정 확인

```bash
cat ~/.claude/.mcp.json
```

### 4.4 설정 적용

설정 변경 후 **Claude Code 재시작** 필요:
```bash
# 현재 세션 종료 후 다시 시작
claude
```

---

## 5. 사용법

### 5.1 기본 사용

Extension 모드가 활성화되면, Claude Code에서 playwright 도구 호출 시:
1. 확장 프로그램이 탭 선택 다이얼로그 표시
2. 사용할 탭 선택 (또는 새 탭 생성)
3. 해당 탭에서 자동화 실행

### 5.2 토큰 설정 시 (자동 연결)

환경변수에 토큰이 설정되어 있으면:
- 연결 승인 다이얼로그 자동 스킵
- 바로 탭 선택 또는 자동화 시작

### 5.3 주의사항

- Chrome이 **반드시 실행 중**이어야 함
- 확장 프로그램이 **활성화** 상태여야 함
- 개발자 모드가 **켜져** 있어야 함

---

## 6. 트러블슈팅

### Q: 새 브라우저가 열리는 경우

**원인**: `--extension` 플래그 누락 또는 MCP 설정 미적용

**해결**:
1. `~/.claude/.mcp.json` 확인 - `"--extension"` 있는지
2. Claude Code 재시작
3. `/mcp` 명령으로 playwright 서버 상태 확인

### Q: 연결 실패 (Connection refused)

**원인**: 확장 프로그램 미실행 또는 토큰 불일치

**해결**:
1. Chrome에서 확장 프로그램 활성화 확인
2. 확장 프로그램의 토큰과 설정 파일 토큰 일치 확인
3. Chrome 재시작

### Q: 탭 선택 다이얼로그가 안 뜸

**원인**: 토큰이 설정되어 자동 승인됨

**해결**: 정상 동작. 토큰 없이 테스트하려면 env에서 토큰 제거

### Q: "Extension not found" 에러

**원인**: 확장 프로그램 ID 변경 또는 재설치

**해결**:
1. Chrome에서 확장 프로그램 제거 후 재설치
2. 새 토큰 복사하여 설정 업데이트

---

## 7. 참고 자료

- [Playwright MCP GitHub](https://github.com/microsoft/playwright-mcp)
- [Extension README](https://github.com/microsoft/playwright-mcp/blob/main/extension/README.md)
- [Chrome Extensions Manifest V3](https://developer.chrome.com/docs/extensions/mv3/)
- [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/)

---

## 수정 이력

| 날짜 | 작성자 | 내용 |
|------|-------|------|
| 2025-12-16 | Claude Code | 초안 작성 |
