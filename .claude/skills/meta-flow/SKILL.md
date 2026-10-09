---
name: meta-flow
description: 처음부터 끝까지 한 단계씩 안내하고 실행한다. meta-init이 안 됐으면 바로 하고, 로드맵이 없으면 meta-interview를 시작하고, 이후 단계마다 다음 차례를 보여주고 확인을 받아 이어간다. "다음 뭐 해", "어디까지 했지", "진행 상황", "시작하자", "meta-flow" 요청에 사용.
---

# meta-flow

지금 무엇을 할 차례인지 알려주고, 확인을 받으면 그 스킬을 실행한다.
한 단계가 끝나면 다음 차례를 다시 보여주고 묻는다. 사용자가 멈추라고 할 때까지 이어간다.
meta-flow 자신은 파일을 고치지 않는다. 고치는 일은 실행한 스킬이 한다.

## 순서

1. 시작 전 확인
   공통 파일이 없으면 묻지 않고 바로 /meta-init을 실행한다.
   roadmap.md가 비어 있으면 /meta-interview를 시작한다.
   사용자가 요구사항을 바꾸자고 하거나 버그를 말하면 /meta-requirement-change를 실행한다.

2. 진행 중인 기능 찾기
   머지하지 않은 spec/ 브랜치가 있으면 그 기능이 진행 중인 기능이다.
   그 브랜치의 spec-index.md에서 상태를 읽는다. 기본 브랜치의 spec-index.md는 머지 전까지 옛 상태다.
   spec 브랜치가 없으면 기본 브랜치의 spec-index.md에서 상태가 "완료"가 아닌 기능을 찾는다.
   여러 개면 모두 보여주고, 번호가 가장 작은 것을 먼저 하라고 추천한다.

3. 상태로 다음 스킬 정하기
   spec 완료                          /meta-tech
   tech 완료                          /meta-mockup
   mockup 완료                        /meta-plan
   mockup 건너뜀                      /meta-plan
   plan 완료                          /meta-task
   task 완료                          /meta-tdd
   tdd 완료                           /meta-implement
   implement 완료                     /meta-converge
   converge N회차                     /meta-tdd
   converge N회차 tdd 완료            /meta-implement
   converge N회차 implement 완료      /meta-converge
   converge 완료                      /meta-wiki
   막힘                               다음 스킬을 안내하지 않는다. 막힌 이유를 보여주고 사람이 정하게 한다

4. 진행 중인 기능이 없을 때
   issues.md에 "다시 열기 대기" 항목이 있으면 먼저 그 기능의 /meta-requirement-change를 추천한다.
   roadmap.md의 기능 순서에서 spec-index에 아직 없는 첫 기능을 찾는다: /meta-spec
   roadmap의 기능이 모두 완료됐으면: 배포 전이면 /meta-live, 기능을 더하려면 /meta-requirement-change

5. 상태와 파일이 맞는지 확인
   상태에 비해 있어야 할 파일이 없으면 알린다.
   - spec 완료 이후: spec.md
   - tech 완료 이후: tech.md, ai/research.md
   - mockup 완료 이후: mockups/mockup.html (mockup 건너뜀이면 없어도 된다)
   - plan 완료 이후: ai/plan.md
   - task 완료 이후: ai/tasks.md
   - tdd 완료 이후: 잠금 태그 lock/{기능 번호}
   빠진 파일이 있으면 다음 스킬 대신 빠진 단계를 다시 하라고 안내한다.

6. 알리고 실행하기
   아래 형식으로 보여주고 "바로 시작할까요?"라고 묻는다.
   좋다고 하면 그 스킬을 실행한다.

7. 이어가기
   스킬이 끝나면 2번부터 다시 해서 다음 차례를 보여주고 묻는다.
   사용자가 멈추라고 하거나, 막힘이거나, roadmap의 기능이 모두 완료되면 멈춘다.

## 보여주는 형식

```
진행 상황
- 001-lecture-store 강의 저장: 완료
- 002-slide-editor 슬라이드 편집: converge 1회차 implement 완료
- 남은 기능: export 내보내기
- issues.md: AI가 발견한 문제 2개, 다시 열기 대기 0개

다음 차례
002-slide-editor의 /meta-converge

바로 시작할까요?
```

파일이 빠졌을 때

```
확인이 필요합니다
- 002-slide-editor: 상태는 "plan 완료"인데 ai/plan.md가 없습니다

다음 차례
002-slide-editor의 /meta-plan을 다시 실행
```
