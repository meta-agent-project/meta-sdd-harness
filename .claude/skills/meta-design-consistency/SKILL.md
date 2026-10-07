---
name: meta-design-consistency
description: 화면 시안을 tokens.yaml, components.yaml, design-system.md, 기존 mockup.html과 비교해 값, 컴포넌트, 페이지 골격, 패턴을 맞춘다. meta-mockup이 시안마다 안에서 쓰고, 직접 실행하면 모든 기능의 시안을 한 번에 맞춘다. "디자인 일관성", "화면 통일", "토큰 정리", "meta-design-consistency" 요청에 사용.
---

# meta-design-consistency

모든 화면이 같은 값, 같은 컴포넌트, 같은 골격을 쓰게 한다.

방식은 두 가지다.
- 시안 검사: meta-mockup이 시안을 만들 때마다 안에서 실행한다
- 전체 검사: 사용자가 /meta-design-consistency를 직접 실행한다

## 세 문서

.meta/design/ 안에 있다.

tokens.yaml이 기본 값의 원본이다.
components.yaml이 컴포넌트 값의 원본이다.
design-system.md가 규칙과 패턴의 원본이다. 값을 쓰지 않고 토큰과 컴포넌트 이름만 쓴다.

tokens.yaml
- 기본 값(color, font, space, radius, layout)만 쓴다
- 단위는 px이고 쓰지 않는다
- 테스트가 이 파일을 직접 읽는다

```yaml
color:
  primary: "#2F5BEA"
  background: "#FFFFFF"
  text: "#1A1A1A"
  text-sub: "#6B7280"
  error: "#DC2626"
font:
  family: Pretendard
  size: { title: 24, body: 16, small: 13 }
space: [4, 8, 16, 24, 32]
radius: { control: 8, card: 12 }
layout:
  header-height: 64
  content-max-width: 1200
  page-padding: 24
```

components.yaml
- 맨 위 키가 컴포넌트 이름이다
- 값은 숫자를 직접 쓰거나 color.primary처럼 tokens.yaml의 이름을 가리킨다
- 상태(hover, disabled, error)는 컴포넌트 아래 키로 쓰고, 기본 상태와 다른 값만 적는다
- 테스트가 이 파일을 직접 읽는다

```yaml
button-primary:
  background: color.primary
  text: color.background
  height: 44
  radius: radius.control
  disabled: { background: color.text-sub }
input:
  height: 44
  border: color.text-sub
  radius: radius.control
  error: { border: color.error }
```

design-system.md
- 페이지 골격, 패턴, 컴포넌트 사용 세 부분
- 컴포넌트와 패턴마다 "쓰인 곳"에 기능 번호를 적는다. 새 시안을 만들 때 참고할 mockup.html을 여기서 찾는다
- 50줄을 넘으면 줄인다

```
# 디자인 시스템

페이지 골격
- 모든 화면: 위에 헤더(layout.header-height), 본문은 가운데 정렬, 최대 너비 layout.content-max-width
- 페이지 제목은 본문 맨 위 왼쪽, 주 버튼은 제목 줄 오른쪽 끝

패턴
- 목록 화면: 제목 줄, 검색과 필터 한 줄, 표. 쓰인 곳: 004
- 입력 폼: 한 칸으로 쌓기, 너비 480, 저장 버튼은 폼 아래 오른쪽. 쓰인 곳: 002
- 확인 대화상자: 가운데, 버튼은 오른쪽에 보조 버튼, 주 버튼 순서

컴포넌트 사용
- 화면 하나에 주 버튼은 하나만
- 위험한 동작은 오류색 버튼
- button-primary 쓰인 곳: 002, 004
```

## 비교하는 것

1. 값: 색, 글자 크기, 간격, 모서리가 tokens.yaml에 있는 값이다
2. 컴포넌트: 버튼, 입력창, 표, 카드가 components.yaml과 같은 모양이다. 상태별 모양도 같다. 같은 역할인데 모양이 다르면 기존 컴포넌트로 바꾼다
3. 골격: 헤더, 페이지 제목, 주 버튼 위치, 본문 너비가 design-system.md의 페이지 골격과 같다
4. 패턴: 이미 있는 패턴과 같은 종류의 화면이면, 그 패턴이 쓰인 mockup.html과 배치가 같다

## 어긋난 곳을 찾았을 때

- 토큰과 거의 같은 값(예: 15, 17)이면 가장 가까운 토큰으로 바꾼다
- 토큰에 없는 값이나 새 컴포넌트가 정말 필요해 보이면 사용자에게 묻는다
  - "새 토큰으로 추가할까요, 기존 {토큰 이름}으로 바꿀까요?"
  - 추천과 이유를 함께 보여준다

## 시안 검사 1: 보여주기 전

meta-mockup 4번에서 실행한다. 토큰이 비어 있으면(첫 기능) 건너뛴다.

1. mockups/의 a.html, b.html, c.html을 "비교하는 것" 네 가지로 비교한다
2. 어긋난 곳을 고친다
3. 고친 내용을 시안마다 한 줄로 알린다

## 시안 검사 2: 고른 뒤

meta-mockup 7번에서 실행한다.

토큰이 비어 있었을 때 (첫 기능)
- mockup.html에서 색, 글꼴, 간격, 모서리, 레이아웃 값을 뽑아 tokens.yaml을 쓴다
- 버튼, 입력창 같은 컴포넌트와 상태별 값을 뽑아 components.yaml을 쓴다
- 페이지 골격, 패턴, 컴포넌트 사용을 뽑아 design-system.md를 쓴다

토큰이 있었을 때
- 새로 생긴 토큰, 컴포넌트, 패턴만 추가한다. 기존 값은 바꾸지 않는다
- 쓰인 곳에 이번 기능 번호를 더한다

## 전체 검사

사용자가 /meta-design-consistency를 직접 실행할 때다.

1. 브랜치 확인
   기본 브랜치에서 실행한다.
   머지하지 않은 spec 브랜치가 있으면 멈추고, 그 기능이 끝난 뒤에 하라고 알린다.
   다른 기능의 파일까지 고치면 머지할 때 섞이기 때문이다.

2. 비교
   .meta/specs/*/mockups/mockup.html을 모두 "비교하는 것" 네 가지로 비교한다.
   여러 화면에서 같은 역할에 다른 값이 쓰였으면 따로 모은다.

3. 보여주기
   아래 형식으로 보여주고 고칠지 묻는다.
   여러 화면에서 다르게 쓰인 값은 무엇으로 합칠지 묻는다.

4. 고치기
   mockup.html, tokens.yaml, components.yaml, design-system.md를 고친다.

5. 구현 코드 확인
   구현된 화면 코드에서 tokens.yaml, components.yaml과 다른 값을 찾는다.
   코드는 고치지 않는다. note.md의 "AI가 발견한 문제"에 적는다.

6. 마무리
   커밋한다. 커밋 메시지: "docs: design consistency"
   완료된 기능의 mockup.html을 고쳤으면 /meta-wiki를 다시 돌리라고 알린다.

## 보여주는 형식

```
디자인 일관성 검사
- 002-signup: 주 버튼 높이 40 → button-primary(44)
- 004-course-list: 페이지 제목이 가운데 → 골격대로 왼쪽
- 여러 화면에서 다름: 카드 모서리 10(002), 12(004) → radius.card(12)로 합칠까요?

구현 코드에서 다른 곳 (note.md에 적음)
- src/pages/signup.css:12 주 버튼 높이 40
```

## 루프 모드

사용자에게 묻는 지점에서는 .meta/loop/question.md에 쓰고 멈춘다.
