> 태그: #claude #cli #korean #writing #skill #prompt
> 출처: 여러 GitHub 레포·논문·커뮤니티 종합 (2026-09 리서치 시점)
> 상태: 참고용
> 노션: <개인 워크스페이스 페이지 — .claude/notion-config.local.md 참조>
> 관련 프롬프트: [korean-writing-rules](../prompts/korean-writing-rules.md)

# 클로드코드 한국어 작문 자연스럽게 만드는 법 — 2026 리서치

**요약**: Claude Code 한국어 번역투/AI 티 제거 종합 리서치. **예방+치료 2단** — ① 생성단계 `fluent-korean`(output-style)으로 번역투 차단 → ② `DaleSeo/korean-skills` 또는 `im-not-ai`로 후처리 윤문. 스킬 없이 쓸 CLAUDE.md 문체 규칙 블록(§3), KatFishNet(ACL 2025) 근거, 모델 비교 포함.

## TL;DR
- **결론부터**: 지금 당장 제일 효과 좋은 조합은 ① 생성 단계에서 `snflkd/fluent-korean`(output-style, 스타 762) 깔아서 애초에 번역투 안 나오게 막고 ② 이미 나온 AI 글은 `DaleSeo/korean-skills`(3개 스킬 3.3k 설치) 또는 `epoko77-ai/im-not-ai`(스타 5.3k대) 스킬로 후처리(윤문)하는 "예방+치료" 2단 구성이다. 셋 다 진짜 존재하고 MIT 라이선스로 공짜.
- **왜 어색하냐면** 대부분 영어 번역투 때문이다. "~를 통해 / ~에 대해 / ~에 있어서", 이중 피동("되어진다"), "가지고 있다", "할 수 있다" 남발, "그/그녀" 강박, 기계적 "첫째·둘째·셋째", "결론적으로/시사하는 바가 크다" 같은 AI 관용구가 핵심 범인. ACL 2025 논문 KatFishNet으로 언어학적으로 검증된 패턴이다.
- **바로 쓸 수 있는 것**: 플러그인 설치 명령어 + CLAUDE.md에 복붙할 한국어 문체 규칙 프롬프트 완성본. 스킬 설치 귀찮으면 CLAUDE.md 규칙만 복붙해도 절반은 먹고 들어간다.

## Key Findings
1. **한국어 특화 스킬 생태계가 2025~2026년에 폭발적으로 커졌다.** Anthropic이 2025-10-16 'Introducing Agent Skills'로 Agent Skills(SKILL.md 포맷)를 출시하고 2026-08-19 Claude API에 정식 출시한 이후, 한국어 humanizer/윤문 스킬이 여러 개 등장했고 몇 개는 스타 수천 개를 찍었다.
2. **가장 근본적 해결책은 "후처리 윤문"이 아니라 "생성 단계 제어"다.** fluent-korean은 output-style로 시스템 프롬프트에 박혀서 Claude가 애초에 깨진 한국어를 안 쓰게 사전 규율한다. humanizer류는 이미 나온 글을 고친다. 둘은 보완 관계.
3. **AI 어색함의 정체가 규명됨.** KatFishNet 논문(ACL 2025 main, arXiv:2503.00032)이 띄어쓰기 패턴·품사 다양성·쉼표 사용을 검토해 인간 글과 LLM 글의 언어학적 차이를 규명했고, 기존 최고 탐지법 대비 평균 19.78% 높은 AUROC 달성. 스킬들이 검출 규칙으로 흡수.
4. **모델 선택도 변수다.** 글로벌 모델(Claude Sonnet 4.5/4.6) 한국어가 좋아져 "한국어=무조건 국산"은 옛말. 다만 한국 고유명사·마케팅 톤은 HyperCLOVA X 강점.

## Details

### 1. Claude Skills / 플러그인

**A. `snflkd/fluent-korean` — 생성 단계 예방책 (가장 추천)**
- URL: https://github.com/snflkd/fluent-korean
- 스타 762, 포크 54, MIT. output-style 플러그인.
- Claude Code가 깨진 기계 한국어(조사·어미 생략, 전보식 명사 나열, 비유로 치환된 어휘)를 쓰는 걸 시스템 프롬프트 층위에서 막는다. 수려한 문체보다 "확실한 의미 전달"이 목표.
- 국어국문학 전공자가 한국어 지침을 손으로 작성했다고 명시.
- 두 버전: `fluent-korean`(코딩 지침 유지), `fluent-korean-not-coding`(순수 글쓰기용).
- 설치: `/plugin marketplace add snflkd/fluent-korean` → `/plugin install fluent-korean@fluent-korean` → `/config`에서 output-style 선택 → 새 세션 or `/clear`.

**B. `DaleSeo/korean-skills` — 후처리 3종 세트 (가장 정돈됨)**
- URL: https://github.com/DaleSeo/korean-skills
- MIT. 193 스타(2026-05-05). 3개 스킬 총 3.3k 설치(humanizer 1.5k, grammar-checker 920, style-guide 866).
- 스킬 3개: `humanizer`, `grammar-checker`(맞춤법·띄어쓰기·문법), `style-guide`(문서 일관성).
- humanizer는 6개 카테고리 40가지 패턴, S1/S2/S3 심각도 + A~D 자연도 등급으로 AI 티를 잡는다. KatFishNet 기반(자체 표기 94.88% AUC).
- 권장 파이프라인: `/humanizer` → `/grammar-checker` → `/style-guide`.
- 설치: `npx skills add daleseo/korean-skills`
- 예시: "인공지능 기술의 발전은 빠르게 진행되고 있으며, 다양한 산업 분야에 적용되고 있습니다." → "인공지능 기술은 빠르게 발전하고 있으며 여러 산업 분야에 적용되고 있습니다."

**C. `epoko77-ai/im-not-ai` (Humanize KR) — 가장 정교하고 인기 많음**
- URL: https://github.com/epoko77-ai/im-not-ai
- 5,303 스타·564 포크(2026-09), v2.3.2(2026-08-18), MIT.
- 10대 카테고리 × 70개 서브패턴을 S1/S2/S3 심각도로 스팬 단위 탐지 후 윤문. "내용은 한 글자도 안 건드리고" 문체만 고침(4대 철칙: 의미 불변, 근거 기반, 장르 유지, 과윤문 금지 — 변경률 30% 초과 경고·50% 초과 강제 중단).
- route_hint 3경로: light/standard/heavy.
- 한국 번역학계 8유형 + Toral 2019 post-editese 이론 흡수. 학술적으로 제일 두껍다.
- 설치: `/plugin marketplace add epoko77-ai/im-not-ai` → `/plugin install humanize-korean@im-not-ai` → `/humanize`.
- Pebblous 2026-08 티어다운 리뷰에서 외부 검증됨.

**D. `NomaDamas/k-skill` 안의 korean-humanizer**
- URL: https://github.com/NomaDamas/k-skill (7.3k+ 스타)
- 종합 스킬 모음 안에 `korean-humanizer` + `korean-spell-check`(바른한글=부산대 맞춤법 검사기 연동). 맞춤법은 실제 부산대 검사기 호출이라 LLM 판단보다 정확.

**E. `hjongc/humanizer-kr`** — Codex/Claude Code용. 번역투·과한 홍보문구·명사화·수동표현·어색한 높임·챗봇식 전개를 줄임.

**F. 큐레이션/디렉토리**
- `J-nowcow/awesome-korean-agent-skills`: 400+ 한국어 에이전트 스킬 큐레이션.
- claude-skills.bdnhost.net: 257개 스킬 자동 색인.
- 공식 `anthropics/skills`에는 한국어 특화 글쓰기 스킬 없음(전부 커뮤니티 제작).

### 2. AI 한국어가 어색한 진짜 이유 (패턴)
- **번역투(제일 큰 범인)**: "~를 통해", "~에 대해", "~에 있어서", 이중 조사, 피동 "~에 의해", 이중 피동 "되어진다", 잉여 "가지고 있다", "그/그녀" 강박.
- **가능/완곡 남발**: "~할 수 있다", "~할 수 있을 것으로 보인다".
- **형식명사 과다**: "것이다", "점", "수", "바", "~할 필요가 있다".
- **구조적 패턴**: 기계적 "첫째·둘째·셋째", 과한 불릿·헤딩·이모지, 콜론 부제 헤딩, 연결어미 뒤 쉼표(인간 대비 4.84배).
- **AI 관용구**: "결론적으로", "시사하는 바가 크다", "주목할 만하다", "혁신적/획기적".
- **리듬 균일성**: 문장 길이 비슷, 동일 종결어미 반복, 주어·목적어 생략 없음.
- **경어체 일관성**: "~합니다"/"~해요" 혼용.

### 3. CLAUDE.md 규칙 → 별도 '프롬프트' 항목으로 분리 저장함
스킬 설치가 귀찮으면 규칙 블록을 `~/.claude/CLAUDE.md` 또는 프로젝트 CLAUDE.md에 복붙. 실제 텍스트는 프롬프트 파일 참고 → [korean-writing-rules](../prompts/korean-writing-rules.md). 대원칙·문체 일관성은 fluent-korean README 지침 블록 verbatim, 나머지는 im-not-ai/korean-skills 패턴 규칙화.

### 4. 커뮤니티 실전 프롬프트
- **브런치 @maven/527**: 주어·목적어 생략, 도치법, 문장 길이 변주 3가지 핵심. "사람처럼 써줘"는 효과 없고 구체 규제 필요.
- **브런치 @seeyonglee/275**: '하다'체, 과도한 현재진행형 삼가기, 직역 후 의역.
- **maily.so/airecipe**: 기계적 표현 제거 + 문장 구조 최적화 템플릿.
- **나무위키 Claude 문서**: 명사 나열·엠대시 번역투 있음, "영어 번역투 교정하라" 명시하면 개선.

### 5. 한국어 규범 자료 (스킬로 변환 가능)
- **이오덕 『우리글 바로쓰기』**: 번역투 교정 고전. "-에 있어서/에서의" 지양, 토 '의' 줄이기, 피동·-화 명사화 지양.
- **국립국어원 우리말 다듬기(malteo.korean.go.kr)**.
- **부산대 맞춤법 검사기(바른한글)**: k-skill 연동.

### 6. 모델 선택 (참고)
- Claude Sonnet 4.5/4.6: 번역체 적고 해요체 유지 좋음, 장문 일관성 강점.
- HyperCLOVA X: 한국 고유명사·마케팅 톤 강점(입력 $5로 비쌈).
- EXAONE 3.5(LG), Solar(Upstage, Apache 2.0).
- 결론: Claude + 위 스킬/규칙으로 번역투 잡는 게 현실적으로 최선.

## Recommendations
1. **(지금, 5분)** fluent-korean 설치 → `/config`에서 `fluent-korean-not-coding` → `/clear`.
2. **(후처리)** DaleSeo/korean-skills(`npx skills add daleseo/korean-skills`) 또는 im-not-ai.
3. **(스킬 없이)** CLAUDE.md 규칙 블록 복붙.
4. **(품질)** few-shot이 제일 강력 — 잘 쓴 글 5~10개 붙여넣고 "이 말투로 써줘".

**언제 뭘 바꾸나**: 후처리해도 번역투면 → 생성 단계 강화(예방>치료) / 맞춤법 문제면 → 부산대 검사기 연동 / 마케팅 카피면 → HyperCLOVA X 병행 / 소설·대본이면 → fluent-korean 예외 처리.

## Caveats
- 스타 수·버전은 2026-09 시점 기준, 소스마다 숫자 다름. 설치 전 최신 확인.
- humanizer류는 문체만 고침, 사실관계·출처 확인 안 함. 변경률 50% 넘으면 사람 검토.
- **AI 탐지기 우회 목적으로 쓰지 말 것.** 목적은 자연스러운 글쓰기지 표절 회피 아님.
- 오탐 존재(im-not-ai가 2020년 사람 에세이 오판 반례, 이후 수정).
- output-style/CLAUDE.md는 토큰 조금 더 씀. 긴 세션·다중 에이전트에선 지침 안 지켜질 수 있으니 출력 전 자체 점검 조항 넣기.
- **"94.88% AUC"는 DaleSeo humanizer 자체 표기**, 논문 초록은 "기존 대비 평균 19.78% 높은 AUROC". 혼동 금지.
