# meta 하네스

Spec으로 성공 기준을 선언하면, AI가 테스트를 먼저 작성해 고정하고 모든 테스트를 통과할 때까지 구현한다.
Spec-Driven Development(SDD) 기반의 Claude Code 개발 하네스.

## 철학: "하지 마" 대신 "해"를 남긴다

"나가 살아. 서울대에 합격하면 돌아와."
이렇게 말하면 학원을 가든 독학을 하든, 스스로 방법을 찾아 될 때까지 해낸다.
"학원 가, 잠은 4시간만 자"를 더하면 방법이 묶여 자유도가 떨어진다.

그래서 문서는 이렇게 쓴다.
1. 성공한 모습만 선언한다. 방법은 AI에게 맡긴다
2. "하지 마"는 "이렇게 한다"로 바꾼다
3. 꼭 막아야 하는 것은 문장이 아니라 테스트로 확인한다
4. 되돌릴 수 없는 일은 장치로 처음부터 막는다
   - 문장은 긍정형이든 부정형이든 AI에게 하는 부탁일 뿐이다. 지켜지는 것은 장치만 보장한다

## 1. 설치

1. 하네스를 내려받는다.
   ```
   git clone https://github.com/meta-agent-project/metakit-sdd-harness.git
   ```
2. metakit-sdd-harness/.claude 폴더를 내 프로젝트에 복사한다.
   이미 .claude가 있으면 합친다. 내 파일은 그대로 남는다.
3. Claude Code에서 /meta-flow를 실행한다.

## 2. 사용법

한 단계씩 확인하며 진행
```
/meta-flow
```
준비가 안 됐으면 /meta-init부터 하고, 로드맵이 없으면 /meta-interview 대화를 시작한다.
그다음 단계마다 다음 차례를 보여주고 확인을 받아 이어간다.

단계 순서
```
/meta-init → /meta-interview → /meta-spec → /meta-tech → /meta-mockup
→ /meta-plan → /meta-task → /meta-tdd → /meta-implement → /meta-converge → /meta-wiki
```
각 스킬을 직접 실행해도 된다.
/meta-mockup은 시안을 보여주기 전에 /meta-design-consistency로 기존 화면과 맞춘다.

화면 전체의 일관성을 한 번에 맞출 때
```
/meta-design-consistency
```
모든 시안을 토큰, 디자인 시스템과 비교해 고친다. 구현 코드에서 다른 곳은 note.md에 적는다.

끝까지 자동으로
```
/meta-loop
```
휴대폰으로 질문을 받으려면 Remote Control을 연결해 둔다.
정한 시간 안에 답이 없으면 Claude와 Codex가 함께 정하고 .meta/loop/auto-decisions.md에 남긴다.

배포 전
```
/meta-live        실제 외부 서비스 연결 확인
```

## 3. 스킬

| 스킬 | 하는 일 |
|---|---|
| meta-init | 빈 문서, 폴더, git 준비 |
| meta-interview | 아이디어를 질문으로 구체화해 roadmap, contract 작성 |
| meta-spec | 기능 하나의 spec.md 작성 (무엇을) |
| meta-tech | 기술 선택, tech.md 작성 (어떻게) |
| meta-mockup | 화면 시안 3개 중 선택, 기존 화면과 일관성 맞추기 |
| meta-design-consistency | 시안을 토큰, 디자인 시스템, 기존 화면과 비교해 맞추기 |
| meta-plan | 상세 구현 계획 ai/plan.md |
| meta-task | 작업 순서와 병렬 묶음 ai/tasks.md |
| meta-tdd | 테스트를 먼저 쓰고 잠금 |
| meta-implement | 구현, 병렬 레인 합치기, 통합 테스트 |
| meta-converge | spec과 코드를 비교해 빠진 것 찾기 |
| meta-wiki | wiki 갱신, 기본 브랜치에 머지 |
| meta-flow | 지금 무엇을 할 차례인지 안내 |
| meta-loop | 처음부터 끝까지 자동 진행 |
| meta-live | 배포 전 실제 연동 테스트 |

## 4. 문서

사람이 보는 문서
짧게 쓴다. 사람은 이것만 읽고 판단한다.

프로젝트 전체 (.meta/)
- roadmap.md: 무엇을 어떤 순서로 만드나
- contract.md: 프로젝트 원칙, 제약, 공통 기술, 품질 기준
- note.md: 자유 메모. AI가 발견한 문제도 여기에 모인다

디자인 (.meta/design/)
- tokens.yaml: 색, 글꼴, 간격, 모서리, 레이아웃 값. 값의 원본
- components.yaml: 버튼, 입력창 같은 컴포넌트 값과 상태별 값. 토큰 이름을 가리킨다
- design-system.md: 페이지 골격, 화면 패턴, 컴포넌트 쓰는 규칙. 값 대신 이름을 쓴다

기능마다 (.meta/specs/002-signup/)
- spec.md: 무엇을 만드나. 사용자 스토리, 요구사항, 성공 기준
- tech.md: 무엇으로 만드나. 고른 기술과 이유
- mockups/mockup.html: 고른 화면 시안

구현 결과 (.meta/wiki/)
- 실제로 만들어진 내용을 meta-wiki가 정리한다
- architecture.md, erd.md(+ erd.svg 그림), glossary.md, screen.md, spec-index.md
- spec-index.md에서 기능마다 진행 상태를 본다

AI가 보는 문서
상세하게 쓴다. 사람은 스킬이 보여주는 요약만 본다.
- .meta/specs/002-signup/ai/research.md: 기술 후보 조사, 최신 사용법
- .meta/specs/002-signup/ai/plan.md: 상세 구현 계획
- .meta/specs/002-signup/ai/tasks.md: 작업 순서와 병렬 묶음

문서를 고칠 때
- 문서를 먼저 고치고, 코드가 문서를 따른다
- 동작이 바뀌면 spec.md (사용자 허락), 기술이 바뀌면 tech.md, 설계가 바뀌면 ai/plan.md
- 완료된 기능의 문서를 고쳤으면 /meta-wiki를 다시 실행한다
