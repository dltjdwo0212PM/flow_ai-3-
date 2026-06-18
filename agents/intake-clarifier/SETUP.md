# intake-clarifier 루틴 만들기 (자동 게시)

[claude.ai/code/routines](https://claude.ai/code/routines)에서 만듭니다. 새 기획요청에
**자동으로 사전질문 댓글**을 답니다.

## 절차

1. **New routine** 클릭.
2. **Name**: `기획요청 되묻기`
3. **Prompt** (그대로 복사):
   ```
   이 저장소의 .claude/agents/intake-clarifier.md 지침대로, [PM] 엔터 기획 요청(1551252)의
   2026-06-18 이후 새 글에만 기획 사전 되묻기 질문 댓글을 게시해줘. 기존 글과 이미 🧭 마커가
   달린 글은 건드리지 마. [내부 분석]은 게시하지 말고 실행 로그로만 남겨줘.
   ```
4. **Select repositories**: `dltjdwo0212PM/flow_ai-3-` (기본 브랜치에 에이전트 정의 포함).
5. **Environment**: Default(Trusted)로 충분 — MCP는 Anthropic 경유라 egress 추가 불필요.
6. **Select a trigger → Schedule**: **hourly** (정시마다).
7. 하단 **Connectors**: **Flow MCP만 남기고** 나머지 제거. (이 커넥터에 `flow_create_comment`가 포함돼야 게시 가능)
8. **Permissions**: 코드 푸시 안 하므로 그대로.
9. **Create** → detail 페이지에서 **Run now**로 즉시 테스트.

## 평일 업무시간만 돌리려면 (선택)

`hourly`는 하루 24회라 **일일 루틴 실행 한도**를 많이 씁니다. 평일 9~19시만 돌리려면,
웹에서 hourly로 만든 뒤 CLI에서:
```
/schedule update
```
로 크론을 `0 9-19 * * 1-5` 로 좁히세요. (최소 간격 1시간)

## 끄기

detail 페이지 **Repeats** 토글로 **일시정지(Pause)**, 또는 삭제.

## ⚠️ 주의

- **댓글은 삭제 불가** — 자동 게시라 잘못 달려도 못 지웁니다. 켠 첫 1~2일은 새 글에 달린 댓글을 확인하세요.
- 상태가 초록이어도 "성공"이 아니라 "에러 없이 종료"입니다. 실행 세션을 열어 실제 게시 여부를 확인하세요.
- 질문 톤·개수·5축 강조점은 `.claude/agents/intake-clarifier.md` 에서 조정.
