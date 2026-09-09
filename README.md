# project-workspace-setup

Claude Code용 저장소 정리 스킬. 프로젝트 저장소를 **3분할 구조(dev / docs / works)** 로 정리하고, 사이트맵(`workspace.md`)·작업 규칙 문서(`PROJECT.md`)·에이전트 진입점(`AGENTS.md`)을 셋팅합니다.

> A Claude Code skill that organizes a project repository into a three-part layout (dev / docs / works) with a sitemap, a team-owned rules document, and an agent entry point.

## 무엇을 해주나

| 결과물 | 설명 |
| --- | --- |
| `dev/` `docs/` `works/` | 로컬 전용 / 문서·양식 / 1회성 요청작업 폴더 골격 |
| `.gitignore` 규칙 | `dev/`·`works/`를 git에서 제외 (문서 원본만 추적) |
| `workspace.md` | 저장소 사이트맵 — "이 파일 어디에 두지?"의 기준 문서 |
| `PROJECT.md` | **현재 팀이 소유하는 작업 규칙** — 환경 표(개발/운영)·절대 규칙·폴더 구조·자가점검 |
| `CLAUDE.md` | **본문 무수정.** 최상단에 `@PROJECT.md` 진입 블록만 삽입 |
| `AGENTS.md` | Claude Code · Codex 등 에이전트 공용 진입점. `PROJECT.md`(규칙)와 `CLAUDE.md`(맥락)를 함께 가리킴 |

**3분할 기준** — "다음 달에도 이 파일을 열어볼까?"

- 그렇다 → `docs/` (git 추적)
- 아니다 → `works/YYYYMMDD_작업제목/` (git 제외)
- 내 PC 환경에 묶인 것 → `dev/` (git 제외)

## 지침 3계층 — 남의 CLAUDE.md는 고치지 않는다

`CLAUDE.md`는 이전 담당자나 `/init`이 만든 문서인 경우가 많습니다. 거기에 우리 규칙을 섞으면 누가 쓴 것인지 구분이 사라지고, 원저자가 갱신할 때 충돌합니다. 그래서 이 스킬은 **소유권을 분리**합니다.

| 파일 | 로드 | 소유 | 내용 |
| --- | --- | --- | --- |
| `CLAUDE.md` | 항상 | 기존 작업자 | 원본 유지. 우리는 **최상단 진입 블록만** 추가 |
| `PROJECT.md` | 항상 (`@` 임포트) | **우리** | 환경 표·절대 규칙·폴더 구조·자가점검 |
| `docs/guide/*.md` | 필요시만 | project-md-slim 산출물 | 작업유형별 상세 (일반 경로로 참조) |

`CLAUDE.md` 최상단 진입 블록의 **`@PROJECT.md`에서 `@`는 반드시 유지**해야 합니다. `@`가 있어야 절대 규칙이 매 세션 자동 로드됩니다 — 지우거나 백틱으로 감싸면 가드레일이 조용히 꺼집니다. 반대로 `docs/guide/*.md`는 `@` 없이 일반 경로로 참조합니다(전부 로드되면 경량화가 무의미해짐).

레거시·폐쇄망 프로젝트(전자정부 프레임워크, 구형 JDK·WAS 고정 등)에는 `templates/claude-legacy-rules.md`의 **환경 표 + 절대 규칙 13개**를 `PROJECT.md`에 채웁니다. 운영 하한·버전 동결·폐쇄망 반입 제약처럼 코드에 드러나지 않아 `/init`이 만들지 못하는 층입니다.

## 설치

Claude Code가 설치되어 있어야 합니다.

### 개인용 (모든 프로젝트에서 사용)

**Windows (PowerShell)**

```powershell
git clone https://github.com/seamoon23/project-workspace-setup.git "$env:USERPROFILE\.claude\skills\project-workspace-setup"
```

**macOS / Linux**

```bash
git clone https://github.com/seamoon23/project-workspace-setup.git ~/.claude/skills/project-workspace-setup
```

### 팀·프로젝트용 (저장소를 쓰는 모든 팀원에게 적용)

프로젝트 저장소 안에 넣고 커밋하면, 그 저장소에서 Claude Code를 쓰는 팀원 모두가 사용할 수 있습니다.

```bash
git clone https://github.com/seamoon23/project-workspace-setup.git <프로젝트>/.claude/skills/project-workspace-setup
# .git 폴더는 제거 후 프로젝트에 커밋 (또는 파일만 복사)
```

### 업데이트

설치 폴더에서 `git pull` 하면 됩니다.

## 사용법

Claude Code 대화창에서:

| 명령 | 동작 | 파일 변경 |
| --- | --- | --- |
| `/project-workspace-setup` | 전체 셋팅. **계획표를 먼저 보여주고 승인 후에만 실행** | 승인 후에만 |
| `/project-workspace-setup dry-run` | 계획표 출력까지만. 처음이면 이것부터 | 없음 |
| `/project-workspace-setup audit` | 셋팅된 저장소가 규칙대로 유지되는지 점검 | 없음 |
| `/project-workspace-setup update` | `workspace.md`를 실제 폴더 상태와 동기화 | workspace.md만 |

## 안전 원칙

이 스킬은 다음을 보장하도록 설계되어 있습니다.

1. **승인 전 변경 없음** — 계획표(생성 목록 + 이동 계획)를 먼저 제시하고 승인받은 뒤에만 실행
2. **삭제 없음** — 파일은 이동과 추가만. 어떤 파일도 삭제하지 않음
3. **덮어쓰기 없음** — 기존 `AGENTS.md`·`workspace.md`·`PROJECT.md`·`.gitignore`가 있으면 병합안을 제시. **`CLAUDE.md`는 본문을 한 줄도 수정하지 않고 최상단 진입 블록만 추가**
4. **git 클린 상태에서 시작** — 문제가 생겨도 git으로 전부 복구 가능
5. **검증 가능한 보고** — 이동 내역 전체와 전후 파일 개수 대조를 보고에 포함

## 폴더 구성

```
project-workspace-setup/
├── SKILL.md                        스킬 본문 (절차·모드·안전 원칙)
└── templates/
    ├── AGENTS.md                   에이전트 공용 진입점 템플릿
    ├── PROJECT.md                  작업 규칙 템플릿 (환경 표·절대 규칙·확인된 충돌)
    ├── claude-entry-block.md       기존 CLAUDE.md 최상단에 넣는 @PROJECT.md 진입 블록
    ├── claude-legacy-rules.md      레거시·폐쇄망용 환경 표 + 절대 규칙 13개 (PROJECT.md에 채움)
    ├── workspace.md                사이트맵 템플릿
    └── gitignore-append.txt        .gitignore 추가분
```

## 관련 스킬 (project-* 세트)

- [project-md-slim](https://github.com/seamoon23/project-md-slim) — 무거워진 지침 문서(`PROJECT.md` 또는 `CLAUDE.md`)를 핵심 규칙 + 작업유형별 참조 문서(`docs/guide/`)로 분할하는 자매 스킬.
- [project-artifacts](https://github.com/seamoon23/project-artifacts) — SI 표준 산출물(테이블·컬럼·코드정의서, 프로그램설명서, ERD) 생성/갱신. 산출물은 이 스킬이 만드는 3분할 구조의 `docs/산출물/`에 쌓인다.
- [project-dev-tomcat](https://github.com/seamoon23/project-dev-tomcat) — 프로젝트별 로컬 Tomcat 인스턴스를 만들고(포트 자동 분리) 빌드·배포·기동·로그를 `tc` 한 명령으로 관리. Windows 전용.

## License

MIT
