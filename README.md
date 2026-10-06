# meta 하네스

사람은 짧은 문서만 보고, 상세한 일은 AI가 하는 Claude Code 개발 하네스.

## 1. 설치

준비물
- Claude Code
- git
- Codex CLI (선택. 자동 모드에서 함께 결정할 때 씀)

설치
1. 하네스를 내려받는다.
   ```
   git clone {저장소 주소}
   ```
2. meta-harness/.claude 폴더를 내 프로젝트에 복사한다.
   이미 .claude가 있으면 합친다. 내 파일은 그대로 남는다.
3. Claude Code에서 /meta-flow를 실행한다.

업데이트
1. meta-harness 폴더에서 git pull
2. .claude 폴더를 다시 복사해서 덮어쓴다

## 2. 사용법

한 단계씩 확인하며 진행
```
/meta-flow
```
준비가 안 됐으면 /meta-init부터 하고, 로드맵이 없으면 /meta-interview 대화를 시작한다.
그다음 단계마다 다음 차례를 보여주고 확인을 받아 이어간다.

단계 순서
```
/meta-init → /meta-interview → /meta-spec → /meta-tech → /meta-screen
→ /meta-plan → /meta-task → /meta-tdd → /meta-implement → /meta-converge → /meta-wiki
```
각 스킬을 직접 실행해도 된다.

끝까지 자동으로
```
/meta-loop
```
휴대폰으로 질문을 받으려면 Remote Control을 연결해 둔다.
정한 시간 안에 답이 없으면 Claude와 Codex가 함께 정하고 .meta-loop/auto-decisions.md에 남긴다.

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
| meta-screen | 화면 시안 3개 중 선택, design-system 작성 |
| meta-plan | 상세 구현 계획 ai/plan.md |
| meta-task | 작업 순서와 병렬 묶음 ai/tasks.md |
| meta-tdd | 테스트를 먼저 쓰고 잠금 |
| meta-implement | 구현, 병렬 레인 합치기, 통합 테스트 |
| meta-converge | spec과 코드를 비교해 빠진 것 찾기 |
| meta-wiki | wiki 갱신, 기본 브랜치에 머지 |
| meta-flow | 지금 무엇을 할 차례인지 안내 |
| meta-loop | 처음부터 끝까지 자동 진행 |
| meta-live | 배포 전 실제 연동 테스트 |

## 문서

사람이 보는 문서
- roadmap.md, contract.md, design-system.md, note.md
- specs/번호/spec.md, tech.md
- wiki/ (직접 고치지 않는다. 스킬이 갱신한다)

AI가 보는 문서
- specs/번호/ai/
