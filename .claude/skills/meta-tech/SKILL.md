---
name: meta-tech
description: spec.md를 보고 어떻게 만들지 기술을 정해 tech.md를 쓴다. "기술 정해줘", "기술 스택", "meta-tech" 요청에 사용. 자료조사 에이전트로 기존 무료 API, 오픈소스, 최신 버전을 먼저 찾는다.
---

# meta-tech

spec.md를 보고 어떻게 만들지 정한다. 직접 만들기 전에 이미 있는 것부터 찾는다.

## 순서

0. 브랜치
   기본 브랜치에서 spec/{기능 번호} 브랜치를 만들고 옮긴다. 이미 있으면 그 브랜치로 옮긴다.
   머지하지 않은 다른 spec 브랜치가 있으면 멈추고 알린다. 기능은 한 번에 하나씩 진행한다.

1. 읽기
   spec.md, contract.md, .meta/wiki/architecture.md를 읽는다.
   이미 쓰고 있는 기술이 있으면 그것을 먼저 고려한다.

2. 정할 항목 뽑기
   요구사항을 보고 기술 결정이 필요한 항목만 뽑는다.
   contract.md의 기술에 이미 있는 것은 다시 정하지 않는다. 그대로 쓴다.
   이번 기능에만 필요한 것만 정한다. 예: 결제 서비스, PDF 라이브러리

3. 1차 조사
   meta-researcher 에이전트에게 항목 목록을 넘긴다.
   찾을 것: 무료 API, 오픈소스, 후보의 최신 버전과 유지보수 상태.
   결과는 ai/research.md의 "후보 조사"에 저장된다.

4. 정하기
   항목마다 AskUserQuestion으로 하나씩 묻는다.
   추천안을 첫 번째에 두고, 직접 만들기는 마지막에 둔다.

5. tech.md 쓰기
   아래 형식대로 쓴다.

6. 2차 조사
   확정한 기술과 버전을 meta-researcher 에이전트에게 넘긴다.
   context7로 최신 사용법을 찾아 ai/research.md의 "사용법"에 저장한다.

7. 마무리
   spec-index.md 상태를 "tech 완료"로 바꾼다.
   다음 단계는 /meta-screen이라고 알린다.

## tech.md 형식

```
# 002 회원가입 기술

선택
- 인증: Supabase Auth
- 저장: Supabase Postgres
- 입력 검사: zod

이유
- 이메일 가입과 중복 검사를 직접 만들지 않아도 된다
- 무료 요금제로 충분하다

검토했지만 안 쓴 것
- Firebase Auth: 저장소가 둘로 나뉜다
- 직접 구현: 비밀번호 저장과 세션 관리가 위험하다
```

## 쓰는 규칙

설치 방법, 코드, 설정값은 tech.md에 쓰지 않는다. 그건 ai/research.md에 둔다.
tech.md가 40줄을 넘으면 줄인다.
