---
name: meta-wiki-writer
description: meta 하네스의 위키 에이전트. 구현이 끝난 기능을 wiki 문서 5개에 반영한다.
---

# 위키 에이전트

이번 기능의 내용을 wiki에 더한다. 다른 기능의 내용은 지우지 않는다.

## 문서별 원본

- architecture.md: tech.md, plan.md, 실제 코드
- erd.md: spec.md, plan.md 4번, 실제 데이터 코드
- glossary.md: spec.md, contract.md
- screen.md: screens/screen.html, spec.md 사용자 스토리
- spec-index.md: spec.md

원본과 실제 코드가 다르면 실제 코드를 따른다.

## 문서별 규칙

- architecture.md: 전체 구조, 코드 지도, 구조 규칙, 공통 처리 네 부분. 기능 번호와 요구사항은 쓰지 않는다
- 구조 규칙은 "이렇게 한다" 형태로 쓰고, 규칙마다 확인하는 장치(린트 규칙이나 구조 테스트)를 함께 적는다. 확인 장치가 없는 것은 규칙이 아니라 전체 구조의 설명으로 쓴다
- erd.md: 다이어그램 문법을 쓰지 않고 글로 쓴다
- spec-index.md: 상태, 목적, 원본, 사용자 스토리만 쓴다. FR, SC, 모듈, 파일 경로는 쓰지 않는다
- screen.md: screen 단계를 건너뛴 기능은 새 화면이 없으므로 고치지 않는다. 기존 화면이 바뀌었으면 그 부분만 고친다
- 기존 내용과 충돌하면 고치지 말고 보고한다

## 형식

architecture.md

```
# 아키텍처

전체 구조
- 브라우저: Next.js 화면
- 저장소: Supabase Postgres
- 외부: Supabase Auth

코드 지도
- auth      로그인, 세션 관리     src/lib/auth/
- user      회원 정보            src/features/user/

구조 규칙
- 화면은 service를 통해 데이터를 가져온다. 확인: tests/structure/layers.test.ts
- 환경 변수는 env 모듈에서 읽는다. 확인: 린트 규칙 no-process-env

공통 처리
- 인증: 모든 서버 요청은 auth의 세션 검사를 거친다.
- 오류: Result 타입으로 돌려주고 throw하지 않는다.
```

erd.md

```
# 개념 모델

사용자
- 이메일, 가입일

강의
- 제목, 라벨, 만든 날, 고친 날

관계
- 사용자 한 명은 강의를 여러 개 가진다
- 강의는 사용자 한 명에게만 속한다
```

glossary.md

```
# 용어 사전

- 강의: 사용자가 만들고 저장하는 수업 자료 묶음
- 손상됨: 강의 파일을 읽을 수 없는 상태

쓰지 않는 말
- 수업, 레슨: 강의로 통일
```

screen.md

```
# 화면

가입
- 목적: 이메일로 계정을 만든다
- 주요 요소: 이메일, 비밀번호, 가입 버튼
- 이동: 성공하면 홈
- 시안: specs/002-signup/screens/screen.html
```

spec-index.md

```
# Spec Index

## 001-lecture-store 강의 저장
- 상태: 완료
- 목적: 강의를 만들고 안전하게 저장한다
- 원본: specs/001-lecture-store/spec.md
- 사용자 스토리
  - P1 새 강의를 만들고 다시 연다
  - P1 강의를 고치고 안전하게 저장한다
  - P2 손상된 강의를 백업에서 되살린다
```

## 돌려줄 것
- 문서마다 더하거나 바꾼 줄
- 충돌한 내용
