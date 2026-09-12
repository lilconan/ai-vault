# 노션 설정 (예시 템플릿)

> 이 파일을 복사해 `.claude/notion-ids.md` 를 만들고 실제 값으로 채운다.
> `.claude/notion-ids.md` 는 `.gitignore` 로 제외됨(커밋 금지). 이 예시(`*.example.md`)만 커밋한다.
> DB 스키마는 `.claude/rules/notion-schema.md` 참고.

## 워크스페이스
- 이름: <워크스페이스명>
- 이메일: <your@email>
- WORKSPACE_ID: <32자리 16진수 또는 UUID>

## 홈 페이지
- "AI 아카이브": <PAGE_ID>
  - https://app.notion.com/p/<PAGE_ID>

## DB (2개)
- **아카이브 DB** (모든 항목, Type 구분)
  - DB URL: https://app.notion.com/p/<ARCHIVE_DB_ID>
  - DATA_SOURCE_ID: <ARCHIVE_DS_ID>
- **도구·레시피 DB** (설치해 쓰는 것만)
  - DB URL: https://app.notion.com/p/<TOOLS_DB_ID>
  - DATA_SOURCE_ID: <TOOLS_DS_ID>

## 항목 페이지 (백링크용, 선택)
- 예: 노트/프롬프트/도구 페이지 URL

## 폐기됨 (휴지통 — 선택, 헷갈림 방지용 기록)
- 예: 옛 DB/페이지 ID
