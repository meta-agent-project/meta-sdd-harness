---
name: meta-init
description: meta 하네스의 공통 파일과 폴더를 빈 상태로 만들고 git 저장소와 병렬 작업 설정을 준비한다. "meta-init", "하네스 시작", "프로젝트 초기화" 요청에 사용. 문서 내용은 채우지 않는다.
---

# meta-init

공통 파일과 폴더를 빈 상태로 만들고 git 저장소를 준비한다. 문서 내용은 채우지 않는다.

## 만들 것

하네스 문서는 모두 .meta/ 안에 만든다. 프로젝트 코드와 섞이지 않게 하기 위해서다.

프로젝트 루트
- .gitignore
- .worktreeinclude

.meta/
- roadmap.md
- contract.md
- backlog.md
- issues.md

.meta/design/
- tokens.yaml
- components.yaml
- ui.css
- design-system.md

.meta/specs/ (빈 폴더)

.meta/wiki/
- architecture.md
- erd.md
- glossary.md
- screen.md
- spec-index.md

.meta/loop/
- auto-decisions.md

## CLAUDE.md에 넣을 줄

```
@.claude/meta-rules.md
```

모든 세션이 시작할 때 공통 규칙을 자동으로 읽게 하는 연결이다. CLAUDE.md에는 이 한 줄만 넣는다.

## .gitignore에 넣을 줄

```
.meta/loop/question.md
.meta/loop/settings.md
.meta/loop/logs/
.claude/worktrees/
.env
```

## .worktreeinclude에 넣을 줄

```
.env
```

구현 레인의 작업 폴더(worktree)는 git에 올라간 파일만 가져온다. 이 파일에 적은 것은 작업 폴더로 함께 복사된다.

## .claude/settings.json에 넣을 값

```json
{ "worktree": { "baseRef": "head" } }
```

구현 레인의 작업 폴더가 지금 있는 spec 브랜치에서 출발하게 한다. 이 값이 없으면 기본 브랜치에서 출발해서 뼈대와 테스트가 없다.

## 순서

1. 옛 문서 옮기기
   .meta/design-tokens.yaml이 있으면 옮긴다.
   - component: 아래 내용은 한 단계 내어 .meta/design/components.yaml에 쓴다
   - 나머지는 .meta/design/tokens.yaml에 쓴다
   - .meta/design-tokens.yaml은 지운다
   - .meta/design-system.md는 git mv로 .meta/design/design-system.md로 옮긴다
   .meta/note.md가 있으면 나눠 옮긴다.
   - "AI가 발견한 문제" 구역은 .meta/issues.md에 쓴다
   - 나머지 줄은 사용자에게 보여주고 backlog.md, issues.md 중 어디로 옮길지 묻는다. 둘 다 아니면 지운다
   - .meta/note.md는 지운다
2. 이미 있는 파일은 건드리지 않는다. 없는 것만 만든다.
3. 문서는 모두 빈 파일로 만든다. .meta/design/, .meta/wiki/, .meta/loop/ 안의 파일도 마찬가지다.
4. 이미 있는 파일에는 빠진 것만 더한다.
   - .gitignore, .worktreeinclude: 위의 줄 중 없는 것만 추가한다
   - CLAUDE.md: 없으면 위의 한 줄로 만든다. 있으면 그 줄이 없을 때만 맨 아래에 추가한다
   - .claude/settings.json: 없으면 위의 값으로 만든다. 있으면 worktree.baseRef만 추가한다. 다른 설정은 그대로 둔다
5. git 저장소가 아니면 git init을 한다.
6. 만든 파일을 커밋한다. 커밋 메시지: "chore: meta 하네스 초기화"
7. 만든 것, 옮긴 것, 건너뛴 것을 목록으로 알린다.
8. 다음 할 일
   .meta/roadmap.md가 비어 있으면 "지금 /meta-interview로 로드맵을 만들까요?"라고 묻는다.
   좋다고 하면 바로 /meta-interview를 시작한다.
   .meta/design/의 네 파일은 /meta-mockup에서 채운다고 알린다.
