---
name: meta-task
description: ai/plan.md를 보고 ai/tasks.md에 구현 순서를 정한다. 독립성이 확인된 레인은 병렬 묶음으로, 나머지는 순차 묶음으로 나눈다. "작업 나눠줘", "구현 순서", "meta-task" 요청에 사용.
---

# meta-task

plan.md를 보고 tasks.md에 구현 순서를 쓴다.
독립성이 확인된 레인만 병렬로 구현한다. 확실하지 않으면 순차로 둔다.

## 순서

1. 읽기
   ai/plan.md를 읽는다. 특히 3번 폴더 구조, 5번 모듈과 인터페이스, 7번 추적표, 8번 통합 시나리오.

2. 레인 나누기
   plan.md 5번의 모듈 하나를 레인 하나로 만든다. 모듈 크기와 상관없이 묶지 않는다.
   레인마다 맡을 파일을 정한다. 한 파일은 한 레인만 맡는다.

3. 공용 파일 정하기
   여러 레인이 함께 고쳐야 하는 파일은 레인에 주지 않고 공용 파일로 따로 적는다.
   예: 모듈을 모아 내보내는 index.ts, 라우트나 메뉴 목록, 공용 타입 파일, package.json과 잠금 파일
   공용 파일은 스킬이 다룬다. 뼈대 단계에서 미리 등록하거나, 통합 단계에서 한 번에 고친다.

4. 독립성 판단
   레인마다 세 가지를 확인한다.
   - 맡은 파일이 다른 레인과 겹치지 않는다
   - 다른 레인에 기대는 것은 뼈대의 인터페이스뿐이다. mock으로 대신할 수 있다
   - 공용 파일을 고칠 필요가 없다
   셋 다 확실하면 병렬 묶음, 하나라도 확실하지 않으면 순차 묶음에 넣는다.
   다른 레인의 실제 결과물이 있어야 시작할 수 있는 레인은 그 레인보다 뒤 순서로 순차 묶음에 넣는다.
   예: DB 스키마에서 자동 생성되는 타입을 쓰는 레인, 코드 생성 결과를 쓰는 레인

5. tasks.md 쓰기
   아래 형식대로 단계 0~4를 쓴다. 5단계는 비워 둔다.

6. 점검
   모든 FR이 어떤 작업에 들어 있는지
   모든 SC가 4단계 통합 작업에 들어 있는지
   같은 파일을 두 레인이 맡지 않는지
   공용 파일이 어느 레인에도 들어가 있지 않은지
   2단계와 3단계의 레인이 같은 모듈로 짝지어지는지

7. 마무리
   사용자에게 레인 개수, 병렬 묶음과 순차 묶음의 레인 수만 알린다.
   spec-index.md 상태를 "task 완료"로 바꾼다.
   다음 단계는 /meta-tdd라고 알린다.

## 단계

0. 준비: 스킬. 설치, 설정
1. 뼈대: 스킬. plan.md 5번을 타입, 인터페이스, stub으로 만들고 공용 파일을 등록한다
2. 테스트: 순서대로. 레인마다 테스트 에이전트를 하나씩 차례로 띄운다 (meta-tdd)
3. 구현: 병렬 묶음은 레인마다 별도 작업 폴더(worktree)에서 동시에, 순차 묶음은 하나씩 (meta-implement)
4. 통합: 스킬. 레인을 하나씩 합치고, 공용 파일을 마무리하고, mock 없이 SC 기준 통합 테스트 (meta-implement)
5. 수렴 추가: meta-converge가 빠진 작업을 여기에 붙인다

## 병렬 실행

병렬 묶음의 레인마다 구현 에이전트 하나를 붙인다.
동시에 도는 에이전트는 최대 4개다.
레인이 4개보다 많으면 나머지는 기다렸다가, 하나가 끝날 때마다 다음 레인을 시작한다.
레인이 하나뿐이면 묶음을 나누지 않고 바로 진행한다.

## 작업 형식

```
- [ ] T001 [P] [레인] 할 일
  - 파일: 경로
  - mock: 대신할 레인 (2단계만)
  - 근거: FR-001
  - 완료 조건: 확인할 수 있는 문장
```

[P]는 3단계 병렬 묶음의 작업에만 붙인다.

## tasks.md 예시 (일부)

```
# 002 회원가입 작업

## 레인
- schema: src/features/signup/schema.ts
- auth-client: src/lib/auth/client.ts
- signup-service: src/features/signup/service.ts
- signup-form: src/features/signup/SignupForm.tsx

## 공용 파일
- src/features/signup/index.ts: 뼈대 단계에서 모든 export를 등록한다
- src/app/routes.ts: 통합 단계에서 가입 화면 경로를 추가한다

## 묶음
- 병렬: schema, auth-client, signup-service, signup-form
- 순차: 없음

## 0. 준비
- [ ] T001 zod 3.x 설치
  - 완료 조건: package.json에 zod가 있다

## 1. 뼈대
- [ ] T002 plan.md 5번의 타입과 함수 시그니처를 만들고 공용 파일에 등록한다
  - 파일: 레인의 모든 파일, src/features/signup/index.ts
  - 근거: plan.md 5번
  - 완료 조건: 모든 함수가 "구현 안 됨" 오류를 던지고, 타입 검사가 통과한다

## 2. 테스트 (순서대로, meta-tdd)
- [ ] T003 [schema] 이메일 형식, 비밀번호 8자 검사 테스트
  - 파일: tests/unit/signup/schema.test.ts
  - 근거: FR-001, FR-002
  - 완료 조건: 테스트가 실행되고 모두 실패한다
- [ ] T004 [signup-service] 가입 성공, 중복 이메일, 네트워크 오류 테스트
  - 파일: tests/unit/signup/service.test.ts
  - mock: schema, auth-client
  - 근거: FR-003
  - 완료 조건: 테스트가 실행되고 모두 실패한다

## 3. 구현 (meta-implement)
- [ ] T007 [P] [schema] validateSignup 구현
  - 파일: src/features/signup/schema.ts
  - 완료 조건: T003 통과
- [ ] T008 [P] [signup-service] submitSignup 구현
  - 파일: src/features/signup/service.ts
  - 완료 조건: T004 통과

## 4. 통합 (meta-implement)
- [ ] T010 레인 합치기와 공용 파일 마무리
  - 파일: src/app/routes.ts
  - 완료 조건: 모든 레인이 합쳐지고 test, typecheck, lint가 통과한다
- [ ] T011 통합 시나리오 1: 정상 가입
  - 파일: tests/integration/signup.spec.ts
  - 근거: SC-001
  - 완료 조건: mock 없이 1분 안에 홈으로 이동한다

## 5. 수렴 추가
(meta-converge가 채운다)
```

## 쓰는 규칙

tasks.md는 AI가 보는 문서다. 아주 상세하게 쓴다.
