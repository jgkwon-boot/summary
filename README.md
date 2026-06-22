# summary 플러그인

AI 뉴스 URL을 받아 팀원용 아침 뉴스 요약 레포트를 자동 생성하는 Claude Code 플러그인.  
추출→요약→검증 3단계 파이프라인으로 고품질 한국어 요약 제공.

## 기능

### `/summary:summary` 슬래시 명령

AI 뉴스 URL 1개 이상을 입력하면 팀원용 한국어 요약 레포트(`reports/news_{YYYYMMDD}_{HHMM}.md`)를 자동 생성.

```
/summary:summary https://techcrunch.com/... https://aitimes.kr/...
```

### 처리 파이프라인

```
URL ──▶ [추출가] ──▶ [요약가] ──▶ [검증가] ──▶ 레포트 저장
         원문 수집      500자 요약    환각/누락
         메타 추출      한국어 번역   왜곡 검증
```

| 에이전트 | 역할 | 담당자 |
|---------|------|--------|
| `news-extractor` | 원문 수집·파싱·핵심 발췌 추출 | 서연 |
| `news-summarizer` | 500자 이내 한국어 요약 초안 작성 | 준호 |
| `news-verifier` | 환각·누락·왜곡 검증 및 품질 게이트 | 다은 |

### 출력 형식

```markdown
# 오늘의 AI 뉴스 브리핑 🤖 — YYYY.MM.DD

## 1. 뉴스 제목(YYYY.M.D)
요약 내용 (500자 이내, 쉽고 재미있는 문체)

*출처: URL 목록*
```

## 설치

```bash
# 마켓플레이스 등록
claude plugin marketplace add summary-team https://github.com/jgkwon-boot/summary

# 플러그인 설치
/plugins install summary@summary-team

# 로컬 경로에서 설치
/plugins install ./path/to/summary
```

## 조회

```bash
# 설치된 플러그인 목록 확인
/plugins list

# 특정 플러그인 상세 정보
/plugins info summary
```

## 업그레이드

```bash
# 플러그인 업그레이드
/plugins upgrade summary

# 전체 플러그인 업그레이드
/plugins upgrade --all
```

## 삭제

```bash
# 플러그인 비활성화
/plugins disable summary

# 플러그인 완전 삭제
/plugins uninstall summary
```

## 디렉토리 구조

```
summary/
├── .claude-plugin/
│   ├── plugin.json        # 플러그인 매니페스트
│   └── marketplace.json   # 마켓플레이스 등록 정보
├── commands/
│   └── summary.md         # /summary:summary 슬래시 명령
├── agents/
│   ├── news-extractor.md  # 원문 수집·추출 에이전트
│   ├── news-summarizer.md # 요약 초안 작성 에이전트
│   └── news-verifier.md   # 검증·품질게이트 에이전트
├── skills/
│   └── summary/
│       ├── SKILL.md       # 파이프라인 오케스트레이션 스킬
│       └── evals/
│           └── evals.json # 스킬 평가 케이스
└── README.md
```

## 사용 예시

```
/summary:summary https://www.technologyreview.com/2025/06/22/ai-update
```

실행 후 `reports/news_20250622_0930.md` 파일이 생성됨.
