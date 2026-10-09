---
name: meta-design-consistency
description: 화면 시안을 브라우저로 열어 잰 값을 tokens.yaml, components.yaml, ui.css, design-system.md, 기존 mockup.html과 비교해 값, 컴포넌트 변형, 페이지 골격, 패턴을 맞춘다. meta-mockup이 시안마다 안에서 쓰고, 직접 실행하면 모든 기능의 시안을 한 번에 맞춘다. "디자인 일관성", "화면 통일", "토큰 정리", "meta-design-consistency" 요청에 사용.
---

# meta-design-consistency

모든 화면이 같은 값, 같은 컴포넌트, 같은 골격을 쓰게 한다.

방식은 두 가지다.
- 시안 검사: meta-mockup이 시안을 만들 때마다 안에서 실행한다
- 전체 검사: 사용자가 /meta-design-consistency를 직접 실행한다

## 네 파일

.meta/design/ 안에 있다.

tokens.yaml이 기본 값의 원본이다.
components.yaml이 컴포넌트 값의 원본이다.
design-system.md가 규칙과 패턴의 원본이다. 값을 쓰지 않고 토큰과 컴포넌트 이름만 쓴다.
ui.css는 두 yaml을 화면에 옮긴 공통 CSS다. 모든 mockup.html이 이 파일을 불러온다.

tokens.yaml
- 기본 값(color, font, space, height, radius, layout)만 쓴다
- 크기 값(font.size, space, height, radius)은 종류마다 5단계 이하다
- 글자 크기는 역할 이름으로 쓴다: title, section, body, small
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
  size: { title: 24, section: 18, body: 16, small: 13 }
space: [4, 8, 16, 24, 32]
height: [32, 40, 48]
radius: { control: 8, card: 12 }
layout:
  header-height: 64
  content-max-width: 1200
  page-padding: 24
  form-width: 480
```

components.yaml
- 맨 위 키가 컴포넌트 종류다. 버튼, 입력창, 체크박스, 라디오, 표, 카드, 네비게이션 모두 같은 틀로 쓴다
- 값은 tokens.yaml의 이름(color.primary)이나 단계 안의 숫자로 쓴다
- variants: 용도별 변형이다. 이름은 용도로 짓고, 기본과 다른 값만 적는다
- sizes: 크기별 변형이다. 용도와 따로 쓰고, 기본과 다른 값만 적는다
- same: 모든 화면에서 같아야 하는 속성이다. 기준값 없이 화면끼리 비교한다
- 상태(hover, disabled, error)는 컴포넌트 아래 키로 쓰고, 기본 상태와 다른 값만 적는다
- 테스트가 이 파일을 직접 읽는다

```yaml
button:
  height: 40
  radius: radius.control
  variants:
    primary: { background: color.primary, text: color.background }
    secondary: { background: color.background, text: color.text, border: color.text-sub }
    danger: { background: color.error, text: color.background }
  sizes:
    small: { height: 32 }
  disabled: { background: color.text-sub }
input:
  height: 40
  border: color.text-sub
  radius: radius.control
  variants:
    default: {}
  error: { border: color.error }
nav:
  width: 240
  variants:
    default: {}
  same: [width, height]
```

ui.css
- 토큰은 CSS 변수로 쓴다. 예: --color-primary, --space-3
- 컴포넌트는 .{종류}-{변형} 클래스로 쓴다. 크기는 .{종류}-{크기} 클래스를 더한다. 예: .button-primary, .button-small
- 두 yaml과 값이 같다

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
- 입력 폼: 한 칸으로 쌓기, 너비 layout.form-width, 저장 버튼은 폼 아래 오른쪽. 쓰인 곳: 002
- 확인 대화상자: 가운데, 버튼은 오른쪽에 보조 버튼, 주 버튼 순서

컴포넌트 사용
- 화면 하나에 button-primary는 하나
- 삭제는 button-danger
- button-primary 쓰인 곳: 002, 004
```

## 비교하는 것

1. 클래스: 컴포넌트는 ui.css의 클래스를 쓴다. 없으면 components.yaml에 추가하고 ui.css에 반영한 뒤 쓴다
2. 값: 색, 글자 크기, 간격, 높이, 모서리가 tokens.yaml에 있는 값이다. 새로 만든 컴포넌트도 같다
3. 글자 역할: 글자 크기는 title, section, body, small 중 역할에 맞는 것이다
4. 변형: 컴포넌트는 components.yaml의 variants 중 용도에 맞는 것이다. 새 변형은 용도 이름을 먼저 정하고 만든다
5. 여백: 부품 안쪽 여백은 부품의 padding이 정한다. 부품 사이 간격은 부모의 gap을 기본으로 한다
6. 같은 값: same에 적은 속성은 모든 화면에서 같다. 한 줄에 놓인 컨트롤은 높이가 같다
7. 골격: 헤더, 페이지 제목, 주 버튼 위치, 본문 너비가 design-system.md의 페이지 골격과 같다
8. 패턴: 이미 있는 패턴과 같은 종류의 화면이면, 그 패턴이 쓰인 mockup.html을 참고하고 콘텐츠와 작업 흐름에 맞게 조정한다

## 확인하는 법

시안을 로컬 서버로 띄워 브라우저로 열고, 실제 적용된 값을 재서 위 항목과 비교한다.
- 1, 2, 5, 6, 7은 잰 값과 위치로 판정한다
- 컴포넌트마다 잰 값이 components.yaml에서 그 변형과 크기를 합친 값과 같은지 본다
- ui.css와 두 yaml이 서로 같은지도 본다. 다르면 yaml이 원본이다
- 3, 4, 8은 추측이라 "확인 필요"로 보고한다. 예: "삭제" 글자인데 danger가 아님, 한 화면에 primary가 둘

## 어긋난 곳을 찾았을 때

- 토큰과 2px 이하로 다르고 가장 가까운 토큰이 하나면(예: 15 → 16) 그 토큰으로 바꾸고, 바꾼 것을 한 줄씩 알린다. 전체 검사에서도 보여주기 전에 바꾼다
- 그보다 크게 다른 값, 토큰에 없는 값이나 새 컴포넌트가 정말 필요해 보이면 사용자에게 묻는다
  - "새 토큰으로 추가할까요, 기존 {토큰 이름}으로 바꿀까요?"
  - 추천과 이유를 함께 보여준다

## 시안 검사 1: 보여주기 전

meta-mockup 4번에서 실행한다. 토큰이 비어 있으면(첫 기능) 건너뛴다.

1. mockups/의 a.html, b.html, c.html을 "비교하는 것"으로 비교한다
2. 어긋난 곳을 고친다
3. 고친 내용을 시안마다 한 줄로 알린다

## 시안 검사 2: 고른 뒤

meta-mockup 7번에서 실행한다.

토큰이 비어 있었을 때 (첫 기능)
- mockup.html에서 색, 글꼴, 간격, 높이, 모서리, 레이아웃 값을 뽑아 tokens.yaml을 쓴다. 크기 값은 종류마다 5단계 이하로 모은다
- 컴포넌트를 종류마다 용도별 variants와 상태별 값으로 뽑아 components.yaml을 쓴다. 네비게이션처럼 화면마다 같아야 하는 속성은 same에 적는다
- 두 yaml로 ui.css를 쓰고, mockup.html이 ui.css를 불러오게 바꾼다
- 페이지 골격, 패턴, 컴포넌트 사용을 뽑아 design-system.md를 쓴다

토큰이 있었을 때
- 선택한 첫 시안을 공통 디자인의 기준으로 삼는다. 이후 화면은 이 기준을 우선 따른다
- 새로 생긴 토큰, 컴포넌트, 변형, 패턴을 추가한다
- 기준을 바꾸는 게 나으면 제안만 하고, 전체 검사에서 기준과 관련 화면을 함께 고친다
- 추가한 것은 ui.css에도 반영한다
- 쓰인 곳에 이번 기능 번호를 더한다

## 전체 검사

사용자가 /meta-design-consistency를 직접 실행할 때다.

1. 브랜치 확인
   기본 브랜치에서 실행한다.
   머지하지 않은 spec 브랜치가 있으면 멈추고, 그 기능이 끝난 뒤에 하라고 알린다.
   다른 기능의 파일까지 고치면 머지할 때 섞이기 때문이다.

2. 비교
   .meta/specs/*/mockups/mockup.html을 모두 "비교하는 것"으로 비교한다.
   여러 화면에서 같은 역할에 다른 값이 쓰였으면 따로 모은다.

3. 보여주기
   모든 화면의 스크린샷을 한 페이지에 모아 브라우저로 연다. 위계, 밀도, 분위기는 사람이 여기서 본다.
   아래 형식으로 보여주고 고칠지 묻는다.
   여러 화면에서 다르게 쓰인 값은 무엇으로 합칠지 묻는다.

4. 고치기
   mockup.html, tokens.yaml, components.yaml, ui.css, design-system.md를 고친다.

5. 구현 코드 확인
   구현된 화면 코드에서 tokens.yaml, components.yaml과 다른 값을 찾는다.
   코드는 고치지 않는다. issues.md의 "AI가 발견한 문제"에 적는다.

6. 마무리
   커밋한다. 커밋 메시지: "docs: design consistency"
   완료된 기능의 mockup.html을 고쳤으면 /meta-wiki를 다시 돌리라고 알린다.

## 보여주는 형식

```
디자인 일관성 검사
- 002-signup: 안내 글자 15 → font.size.body(16)로 바꿈
- 002-signup: 주 버튼 높이 44 → button-primary(40)로 바꿀까요?
- 002-signup: 검색창 32, 옆 버튼 40 → 한 줄 높이 40으로 맞출까요?
- 004-course-list: nav 넓이 220 → same nav.width(240)로 바꿀까요?
- 004-course-list: 페이지 제목이 가운데 → 골격대로 왼쪽으로 바꿀까요?
- 확인 필요: 004 "삭제" 버튼이 button-secondary
- 여러 화면에서 다름: 카드 모서리 10(002), 12(004) → radius.card(12)로 합칠까요?

구현 코드에서 다른 곳 (issues.md에 적음)
- src/pages/signup.css:12 주 버튼 높이 44
```

## 루프 모드

사용자에게 묻는 지점에서는 .meta/loop/question.md에 쓰고 멈춘다.
