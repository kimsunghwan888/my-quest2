---
version: 1.0
last_updated: 2026-09-07
---

# 퀘스트 4 — My ChatGPT (Network + AI)

**만든 것**: 성격을 정해준 AI와 이야기하는 채팅창 — 「사서 김목록 상담실」
**AI 캐릭터**: 동네 공공도서관에서 40년 일한 사서. 존댓말, 짧은 문장, 이모지 없음. 대답 끝에 반드시 책 한 권을 추천.

**바로 써보기**: https://claude.ai/code/artifact/f84f8c82-d8e9-4a3c-921b-4bf291f2df7a

## 미션 3가지를 어떻게 채웠나

| 미션 | 어디에 |
|---|---|
| AI 프로필(성격·말투·전문분야) 설정 | 왼쪽 «AI 인물 카드» — 4칸 모두 **화면에서 직접 고칠 수 있음**. 고친 즉시 AI에게 가는 글이 바뀌고, "AI에게 실제로 전달되는 글 보기"로 확인 가능 |
| 프로필대로 AI가 답변 | 인물 설정문을 대화 앞에 붙여 보냄 (`personaText()` → `buildTurns()`) |
| 대화 기록이 화면에 쌓임 | `history` 배열에 누적 → 말풍선으로 렌더 + `localStorage` 저장. 창을 닫았다 열어도 남음 |

## AI에 연결하는 두 가지 길

| 상황 | 방법 | 열쇠 |
|---|---|---|
| 클로드 화면 안에서 열 때 | `claude.use("sample")` | 필요 없음 |
| GitHub 등 다른 곳에서 열 때 | `fetch` → `api.anthropic.com/v1/messages` | 사용자가 본인 API 키 입력 |

키는 **localStorage에만** 저장되고 저장소 파일에는 들어가지 않습니다.

요청 설정: 모델 `claude-opus-5`, `max_tokens` 4096, `output_config.effort: "low"`(잡담형 대화라 낮게), `fallbacks: "default"` + 베타 헤더 `server-side-fallback-2026-07-01`, 브라우저 직접 호출용 `anthropic-dangerous-direct-browser-access` 헤더. `stop_reason: "refusal"`도 따로 처리합니다.

## 파일

| 파일 | 내용 |
|---|---|
| `my-chatgpt.html` | 앱 전체 (HTML·CSS·JS 한 파일) |
| `screenshots/app-ui.png` | 앱 첫 화면 |
| `screenshots/chat-working.png` | 실제 대화 장면 |

## 시행착오 (되풀이하지 않기 위해)

- `claude.use("sample")`에는 **system 자리가 없다.** 인물 설정은 첫 user 메시지 앞에 붙여야 한다. API 직접 호출 쪽은 `system` 필드를 쓴다 — 두 경로의 프롬프트 조립이 다르다.
- `window.claude`는 **클로드 화면 안에서만** 존재한다. 로컬 서버나 GitHub Pages에서 열면 없다. 그래서 두 경로를 모두 만들어야 실제로 쓸 수 있는 앱이 된다.
- 게시한 아티팩트는 **비공개**라 로그인 없이는 열리지 않는다. 동작 스크린샷은 계정 주인이 직접 찍어야 한다.
- 배포 전 점검은 가짜 `window.claude`를 붙인 임시 파일로 했다 (프로필 반영·스트리밍·기록 누적 확인 후 삭제).
