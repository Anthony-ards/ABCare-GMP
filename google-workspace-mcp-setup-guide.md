# Google Workspace MCP 설정 가이드

Claude Code에서 Google Workspace (Gmail, Calendar, Drive, Sheets 등)를 사용하기 위한 MCP 서버 설정 가이드.

## 목차
1. [사전 요구사항](#사전-요구사항)
2. [Google Cloud Console 설정](#google-cloud-console-설정)
3. [Claude Code MCP 설정](#claude-code-mcp-설정)
4. [인증 및 테스트](#인증-및-테스트)
5. [사용 가능한 기능](#사용-가능한-기능)
6. [트러블슈팅](#트러블슈팅)

---

## 사전 요구사항

- Python 3.10 이상
- `uvx` (Python 패키지 실행기) 설치
- Google Cloud 프로젝트 접근 권한
- Claude Code 설치

```bash
# uvx 설치 (uv가 없는 경우)
curl -LsSf https://astral.sh/uv/install.sh | sh
```

---

## Google Cloud Console 설정

### 1. 프로젝트 생성 또는 선택

1. [Google Cloud Console](https://console.cloud.google.com) 접속
2. 상단의 프로젝트 선택 드롭다운 클릭
3. "새 프로젝트" 생성 또는 기존 프로젝트 선택
   - 예: `ards-anthony`

### 2. 필요한 API 활성화

**API 및 서비스 > 라이브러리**에서 다음 API들을 활성화:

| API 이름 | 용도 |
|----------|------|
| Google Sheets API | 스프레드시트 읽기/쓰기 |
| Google Drive API | 파일 관리 |
| Gmail API | 이메일 검색/읽기/전송 |
| Google Calendar API | 일정 관리 |
| Google Docs API | 문서 읽기/쓰기 |
| Google Slides API | 프레젠테이션 관리 |
| Google Forms API | 폼 관리 |
| Google Tasks API | 태스크 관리 |
| Google Chat API | 채팅 메시지 |
| Custom Search API | 웹 검색 (선택사항) |

**API 활성화 방법:**
1. 좌측 메뉴 → "API 및 서비스" → "라이브러리"
2. 원하는 API 검색 (예: "Google Sheets API")
3. "사용" 버튼 클릭
4. 각 API에 대해 반복

### 3. OAuth 동의 화면 설정

1. **API 및 서비스** → **OAuth 동의 화면**
2. 사용자 유형: **외부** 선택 (또는 조직 내부만 사용 시 **내부**)
3. 앱 정보 입력:
   - 앱 이름: `Claude Code Workspace` (원하는 이름)
   - 사용자 지원 이메일: 본인 이메일
   - 개발자 연락처 이메일: 본인 이메일
4. 범위(Scopes) 추가:
   ```
   https://www.googleapis.com/auth/gmail.readonly
   https://www.googleapis.com/auth/gmail.send
   https://www.googleapis.com/auth/calendar
   https://www.googleapis.com/auth/drive
   https://www.googleapis.com/auth/spreadsheets
   https://www.googleapis.com/auth/documents
   https://www.googleapis.com/auth/presentations
   https://www.googleapis.com/auth/forms
   https://www.googleapis.com/auth/tasks
   https://www.googleapis.com/auth/chat.messages
   ```
5. 테스트 사용자 추가 (외부 앱인 경우):
   - 본인 Google 계정 이메일 추가

### 4. OAuth 클라이언트 ID 생성

1. **API 및 서비스** → **사용자 인증 정보**
2. **+ 사용자 인증 정보 만들기** → **OAuth 클라이언트 ID**
3. 애플리케이션 유형: **데스크톱 앱**
4. 이름: `claude-code-workspace` (원하는 이름)
5. **만들기** 클릭
6. **중요**: 생성된 **클라이언트 ID**와 **클라이언트 보안 비밀번호** 복사해두기

```
클라이언트 ID: xxxxxxxxx.apps.googleusercontent.com
클라이언트 보안 비밀번호: GOCSPX-xxxxxxxxx
```

---

## Claude Code MCP 설정

### 방법 1: Claude Code 명령어 사용 (권장)

```bash
# Claude Code 실행 후
/mcp
```

MCP 설정 메뉴에서 google-workspace 추가.

### 방법 2: 설정 파일 직접 편집

`~/.claude.json` 파일을 열고 `mcpServers` 섹션에 추가:

```json
{
  "mcpServers": {
    "google-workspace": {
      "type": "stdio",
      "command": "uvx",
      "args": [
        "workspace-mcp",
        "--tool-tier",
        "core"
      ],
      "env": {
        "GOOGLE_OAUTH_CLIENT_ID": "YOUR_CLIENT_ID.apps.googleusercontent.com",
        "GOOGLE_OAUTH_CLIENT_SECRET": "GOCSPX-YOUR_CLIENT_SECRET"
      }
    }
  }
}
```

### Tool Tier 옵션

| Tier | 포함 기능 |
|------|----------|
| `core` | Calendar, Gmail, Drive, Sheets (기본) |
| `extended` | core + Docs, Slides, Forms, Tasks |
| `all` | extended + Chat, Custom Search |

필요에 따라 `--tool-tier` 값 변경:
```json
"args": [
  "workspace-mcp",
  "--tool-tier",
  "all"
]
```

---

## 인증 및 테스트

### 첫 번째 사용 시 인증 과정

1. Claude Code에서 Google Workspace 관련 명령 실행
2. 브라우저가 자동으로 열리며 Google 로그인 페이지 표시
3. Google 계정으로 로그인
4. 권한 요청 승인 ("고급" → "안전하지 않은 앱으로 이동" 클릭 필요할 수 있음)
5. 인증 완료 후 토큰이 로컬에 저장됨

### 테스트 명령어 예시

```
# Gmail 검색
"내 Gmail에서 최근 이메일 5개 보여줘"

# Calendar 조회
"오늘 일정 알려줘"

# Drive 파일 검색
"내 드라이브에서 '보고서' 파일 찾아줘"

# Sheets 읽기
"스프레드시트 ID가 xxx인 시트 내용 보여줘"
```

---

## 사용 가능한 기능

### Gmail
| 함수 | 설명 |
|------|------|
| `search_gmail_messages` | 이메일 검색 |
| `get_gmail_message_content` | 이메일 내용 조회 |
| `get_gmail_messages_content_batch` | 다중 이메일 조회 |
| `send_gmail_message` | 이메일 전송/답장 |

### Google Calendar
| 함수 | 설명 |
|------|------|
| `list_calendars` | 캘린더 목록 조회 |
| `get_events` | 일정 조회 |
| `create_event` | 일정 생성 |
| `modify_event` | 일정 수정 |

### Google Drive
| 함수 | 설명 |
|------|------|
| `search_drive_files` | 파일 검색 |
| `get_drive_file_content` | 파일 내용 조회 |
| `create_drive_file` | 파일 생성 |

### Google Sheets
| 함수 | 설명 |
|------|------|
| `read_sheet_values` | 시트 데이터 읽기 |
| `modify_sheet_values` | 시트 데이터 수정 |
| `create_spreadsheet` | 새 스프레드시트 생성 |

### Google Docs
| 함수 | 설명 |
|------|------|
| `get_doc_content` | 문서 내용 조회 |
| `create_doc` | 문서 생성 |
| `modify_doc_text` | 문서 수정 |

### Google Tasks
| 함수 | 설명 |
|------|------|
| `list_tasks` | 태스크 목록 조회 |
| `get_task` | 태스크 상세 조회 |
| `create_task` | 태스크 생성 |
| `update_task` | 태스크 수정 |

---

## 트러블슈팅

### 1. "API가 사용 설정되지 않음" 에러

```
해결: Google Cloud Console에서 해당 API 활성화 확인
```

### 2. "액세스가 차단됨" 에러

```
해결:
1. OAuth 동의 화면에서 테스트 사용자에 본인 이메일 추가
2. 또는 "앱 게시"로 상태 변경 (프로덕션 환경)
```

### 3. "Invalid client" 에러

```
해결:
1. 클라이언트 ID/Secret 값 재확인
2. ~/.claude.json에서 값 정확히 입력되었는지 확인
3. 따옴표나 공백 없는지 확인
```

### 4. 토큰 만료 에러

```
해결:
1. ~/.google-workspace-mcp/ 폴더 내 토큰 파일 삭제
2. Claude Code 재시작
3. 재인증 진행
```

### 5. uvx 명령어를 찾을 수 없음

```bash
# uv 재설치
curl -LsSf https://astral.sh/uv/install.sh | sh

# PATH 확인
export PATH="$HOME/.local/bin:$PATH"
```

---

## 보안 주의사항

1. **클라이언트 Secret 노출 주의**: `.claude.json` 파일을 git에 커밋하지 않기
2. **최소 권한 원칙**: 필요한 API 스코프만 활성화
3. **정기적 토큰 갱신**: 장기간 미사용 시 재인증 권장
4. **테스트 사용자 관리**: 외부 앱일 경우 필요한 사용자만 추가

---

## 참고 링크

- [Google Cloud Console](https://console.cloud.google.com)
- [workspace-mcp GitHub](https://github.com/QuantGeekDev/workspace-mcp)
- [Claude Code MCP 문서](https://docs.anthropic.com/en/docs/claude-code)

---

*작성일: 2025-12-15*
*작성자: Claude Code*
