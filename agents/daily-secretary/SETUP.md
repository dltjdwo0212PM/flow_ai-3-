# 데일리 비서를 루틴으로 거는 법

이 에이전트는 **Claude Code 루틴**으로 매일 자동 실행됩니다. 루틴 생성은 컨테이너 안이 아니라
**claude.ai/code 웹 UI**에서 해야 합니다(저는 컨테이너 안이라 루틴을 직접 만들 수 없어요).

## 1. 루틴 만들기

1. [claude.ai/code](https://claude.ai/code) 접속 → **구름(cloud) 아이콘** 자리에서 **Routine**(루틴) 생성.
2. **저장소/브랜치**: `dltjdwo0212pm/flow_ai-3-` · `claude/admiring-darwin-37twsn` (이 에이전트가 들어있는 곳).
3. **환경(Environment)**: Flow MCP 커넥터가 켜진 환경을 선택.
   - 루틴 설정에서 **Flow MCP 커넥터를 ON** 으로. (MCP 트래픽은 Anthropic 서버를 경유하므로
     `api.flow.team`를 egress allowlist에 추가할 필요가 **없습니다**.)
4. **스케줄**: 평일 아침 추천 — 예) 월~금 **08:30 (KST)**. (타임존을 한국으로 맞추세요.)

## 2. 루틴 프롬프트

프롬프트 칸에는 아래 한 줄만 넣는 걸 권장합니다(프롬프트 파일을 고치면 자동 반영됨):

```
이 저장소의 agents/daily-secretary/ROUTINE_PROMPT.md 를 읽고, 적힌 절차를 그대로 수행해
오늘자 데일리 브리핑을 만들어줘.
```

> 또는 `ROUTINE_PROMPT.md`의 본문 전체를 그대로 붙여넣어도 됩니다.

## 3. 결과 받기

- 루틴이 끝나면 세션 결과로 브리핑이 남습니다. **Claude 모바일 앱**에서 알림으로 확인하거나,
  웹에서 해당 세션을 열어 보세요.
- 브리핑 마지막의 제안(①멘션 답글 초안 ②마감 재설정 ③회의 준비 업무 생성)에서 하나 고르면,
  그 세션을 이어받아 **확인 후** 실제 작업까지 진행할 수 있습니다.

## 4. 조정 포인트

| 바꾸고 싶은 것 | 어디서 |
|---|---|
| 마감 임박 기준 일수, Top N 개수 | `ROUTINE_PROMPT.md`의 도구 파라미터(`imminentDays`, `topN`) |
| 출력 형식/톤 | `ROUTINE_PROMPT.md` 3번 섹션 |
| 실행 시간/요일 | 루틴 스케줄 설정 |
| 회의 준비 깊이 | `ROUTINE_PROMPT.md` 1-4번(검색 size/키워드) |

## 참고: 로컬/직접 API로 돌리고 싶다면

MCP 대신 클론한 `flow-team-skill`의 스크립트(`npm run brief`)로도 비슷한 브리핑이 가능합니다.
단, 그 경로는 `api.flow.team` 직접 호출이라 **egress allowlist에 `api.flow.team` 추가**가 필요합니다.
(이 루틴 방식은 MCP 경유라 그 설정이 필요 없습니다.)
