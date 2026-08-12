# workspace-setup

Claude Code용 저장소 정리 스킬. 프로젝트 저장소를 **3분할 구조(dev / docs / works)** 로 정리하고, 사이트맵(`workspace.md`)과 에이전트 진입점(`AGENTS.md`)을 셋팅합니다.

> A Claude Code skill that organizes a project repository into a three-part layout (dev / docs / works) with a sitemap and agent entry point.

## 무엇을 해주나

| 결과물 | 설명 |
| --- | --- |
| `AGENTS.md` | Claude Code · Codex 등 에이전트 공용 진입점. `CLAUDE.md`를 단일 원천으로 참조 |
| `dev/` `docs/` `works/` | 로컬 전용 / 문서·양식 / 1회성 요청작업 폴더 골격 |
| `.gitignore` 규칙 | `dev/`·`works/`를 git에서 제외 (문서 원본만 추적) |
| `workspace.md` | 저장소 사이트맵 — "이 파일 어디에 두지?"의 기준 문서 |
| `CLAUDE.md` 병합 | 폴더 구조 절 추가 (기존 내용은 건드리지 않음) |

**3분할 기준** — "다음 달에도 이 파일을 열어볼까?"

- 그렇다 → `docs/` (git 추적)
- 아니다 → `works/YYYYMMDD_작업제목/` (git 제외)
- 내 PC 환경에 묶인 것 → `dev/` (git 제외)

## 설치

Claude Code가 설치되어 있어야 합니다.

### 개인용 (모든 프로젝트에서 사용)

**Windows (PowerShell)**

```powershell
git clone https://github.com/seamoon23/workspace-setup.git "$env:USERPROFILE\.claude\skills\workspace-setup"
```

**macOS / Linux**

```bash
git clone https://github.com/seamoon23/workspace-setup.git ~/.claude/skills/workspace-setup
```

### 팀·프로젝트용 (저장소를 쓰는 모든 팀원에게 적용)

프로젝트 저장소 안에 넣고 커밋하면, 그 저장소에서 Claude Code를 쓰는 팀원 모두가 사용할 수 있습니다.

```bash
git clone https://github.com/seamoon23/workspace-setup.git <프로젝트>/.claude/skills/workspace-setup
# .git 폴더는 제거 후 프로젝트에 커밋 (또는 파일만 복사)
```

### 업데이트

설치 폴더에서 `git pull` 하면 됩니다.

## 사용법

Claude Code 대화창에서:

| 명령 | 동작 | 파일 변경 |
| --- | --- | --- |
| `/workspace-setup` | 전체 셋팅. **계획표를 먼저 보여주고 승인 후에만 실행** | 승인 후에만 |
| `/workspace-setup dry-run` | 계획표 출력까지만. 처음이면 이것부터 | 없음 |
| `/workspace-setup audit` | 셋팅된 저장소가 규칙대로 유지되는지 점검 | 없음 |
| `/workspace-setup update` | `workspace.md`를 실제 폴더 상태와 동기화 | workspace.md만 |

## 안전 원칙

이 스킬은 다음을 보장하도록 설계되어 있습니다.

1. **승인 전 변경 없음** — 계획표(생성 목록 + 이동 계획)를 먼저 제시하고 승인받은 뒤에만 실행
2. **삭제 없음** — 파일은 이동과 추가만. 어떤 파일도 삭제하지 않음
3. **덮어쓰기 없음** — 기존 `AGENTS.md`·`workspace.md`·`CLAUDE.md`·`.gitignore`가 있으면 병합안을 제시
4. **git 클린 상태에서 시작** — 문제가 생겨도 git으로 전부 복구 가능
5. **검증 가능한 보고** — 이동 내역 전체와 전후 파일 개수 대조를 보고에 포함

## 폴더 구성

```
workspace-setup/
├── SKILL.md                        스킬 본문 (절차·모드·안전 원칙)
└── templates/
    ├── AGENTS.md                   에이전트 진입점 템플릿
    ├── workspace.md                사이트맵 템플릿
    ├── gitignore-append.txt        .gitignore 추가분
    └── claude-section.md           CLAUDE.md에 병합할 폴더 구조 절
```

## License

MIT
