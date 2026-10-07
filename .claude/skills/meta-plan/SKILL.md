---
name: meta-plan
description: spec, tech, research, screen을 토대로 ai/plan.md에 상세 구현 계획을 쓴다. 모듈 경계와 인터페이스를 정해 병렬 구현이 가능하게 한다. "계획 세워줘", "상세 계획", "meta-plan" 요청에 사용.
---

# meta-plan

AI가 구현할 수 있는 상세 계획을 ai/plan.md에 쓴다.
사람에게는 요약만 보여준다.

## 순서

1. 읽기
   spec.md, tech.md, ai/research.md, design-system.md, contract.md를 읽는다.
   screens/screen.html이 있으면 읽는다. screen 단계를 건너뛴 기능에는 없다.

2. 기존 구조 확인
   .meta/wiki/architecture.md의 전체 구조, 구조 규칙, 공통 처리를 읽는다.
   코드 지도에서 이번 기능과 관련된 모듈만 고른다.
   고른 모듈의 공개 인터페이스(함수, 타입)는 서브에이전트가 실제 코드에서 확인해 목록으로 돌려준다.
   내부 구현은 읽지 않는다. 기준은 실제 코드다.

3. 빈 곳 채우기
   구현에 필요한 정보가 research.md에 없으면 meta-researcher 에이전트를 부른다.

4. contract.md 점검
   계획이 contract.md 규칙을 어기지 않는지 확인한다.
   어기는 부분이 있으면 사용자에게 묻는다.

5. plan.md 쓰기
   아래 형식대로 쓴다. 섹션을 빼지 않는다.

6. 점검
   모든 FR이 담당 모듈에 연결됐는지
   모든 SC가 통합 시나리오에 연결됐는지
   모든 모듈에 인터페이스가 있는지
   모듈 사이에 순환 의존이 없는지
   계획이 contract.md를 지키는지 다시 확인한다.

7. 마무리
   사용자에게 5줄 이내로 요약을 보여준다.
   spec-index.md 상태를 "plan 완료"로 바꾼다.
   다음 단계는 /meta-task라고 알린다.

## plan.md 형식

1. 요약
2. 기술 맥락: 언어, 버전, 주요 라이브러리, 저장소, 테스트 도구, 실행 환경
3. 폴더 구조: 실제로 만들 소스 폴더와 파일. 이미 있는 파일은 표시한다.
4. 데이터 모델: 엔티티, 필드, 타입, 검증 규칙, 상태 변화
5. 모듈과 인터페이스: 모듈마다 책임, 제공하는 함수와 타입, 의존하는 모듈
6. 외부 연동: 외부 API 호출, 요청과 응답 형태, 실패 처리
7. 요구사항 추적표: FR별 담당 모듈, SC별 통합 시나리오
8. 통합 시나리오: SC마다 준비, 실행, 기대 결과
9. 위험과 결정: 복잡해진 부분, 그 이유, 버린 대안

5번 모듈과 인터페이스는 meta-tdd가 뼈대 코드로 그대로 옮긴다.
함수 이름, 인자, 반환 타입, 오류 타입까지 빠짐없이 쓴다.

8번 통합 시나리오의 기대 결과에는 SC의 응답 시간 기준을 그대로 옮긴다.
느려질 위험이 있는 부분은 9번에 적는다. 예: 목록 전체를 한 번에 읽기, 반복문 안의 DB 호출

## plan.md 예시 (일부)

```
# 002 회원가입 구현 계획

## 1. 요약
이메일과 비밀번호로 가입한다. Supabase Auth로 계정을 만들고,
zod로 입력을 검사한 뒤 성공하면 세션을 저장하고 홈으로 이동한다.

## 2. 기술 맥락
- 언어: TypeScript 5.x
- 프레임워크: Next.js 15 (App Router)
- 인증: @supabase/supabase-js 2.x
- 검증: zod 3.x
- 테스트: Vitest (단위), Playwright (통합)
- 실행 환경: 브라우저, Vercel

## 3. 폴더 구조
src/
  features/signup/
    schema.ts        입력 검증 규칙
    service.ts       가입 처리
    SignupForm.tsx   화면
  lib/auth/
    client.ts        Supabase 클라이언트 래퍼 (이미 있음)
tests/
  unit/signup/
  integration/signup.spec.ts

## 4. 데이터 모델
SignupInput
- email: string, 이메일 형식, 앞뒤 공백 제거
- password: string, 8자 이상

계정 상태
- 없음, 가입됨, 로그인됨

## 5. 모듈과 인터페이스
### schema (의존 없음)
- validateSignup(input: unknown): Result<SignupInput, FieldError[]>

### auth-client (의존: Supabase)
- signUp(email: string, password: string): Promise<Result<Session, AuthError>>
- AuthError = "EMAIL_TAKEN" | "NETWORK" | "UNKNOWN"

### signup-service (의존: schema, auth-client)
- submitSignup(raw: unknown): Promise<SignupOutcome>
- SignupOutcome = { ok: true } | { ok: false, errors: FieldError[] }

### SignupForm (의존: signup-service)
- 입력값은 실패해도 유지한다

## 7. 요구사항 추적표
| 요구사항 | 모듈 |
|---|---|
| FR-001 | schema, SignupForm |
| FR-002 | schema |
| FR-003 | auth-client, signup-service |
| FR-004 | SignupForm |
| SC-001 | 통합 시나리오 1 |
| SC-002 | 통합 시나리오 2 |

## 8. 통합 시나리오
### 1. 정상 가입 (SC-001)
- 준비: 비어 있는 테스트 프로젝트
- 실행: 가입 화면에서 이메일, 비밀번호 입력 후 가입
- 기대: 1분 안에 홈으로 이동, 세션 있음

### 3. 가입 응답 시간 (SC-003)
- 준비: 가짜 응답을 쓰는 테스트 환경
- 실행: 가입 버튼을 누른다
- 기대: 0.5초 안에 결과가 나온다
```

## 쓰는 규칙

plan.md는 AI가 보는 문서다. 구체적으로 쓴다.
요약은 사람용이므로 짧게 쓴다.
