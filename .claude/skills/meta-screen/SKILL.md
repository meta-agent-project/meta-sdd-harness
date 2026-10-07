---
name: meta-screen
description: spec.md의 화면을 HTML 시안 3개로 보여주고 하나를 고르게 한다. 고른 시안으로 design-system.md를 채운다. "화면 시안", "디자인 골라줘", "meta-screen" 요청에 사용.
---

# meta-screen

화면 시안 3개를 HTML로 보여주고 하나를 고르게 한다.
고른 시안을 기준으로 design-system.md를 완성한다.

## 순서

1. 읽기
   spec.md, tech.md, design-system.md를 읽는다.

2. 화면 목록 뽑기
   사용자 스토리 P1을 보고 필요한 화면만 뽑는다.
   예: 가입 화면, 가입 완료 화면

   새 화면이나 화면 변경이 없으면 건너뛴다.
   - 예: PDF 내보내기, 백엔드만 있는 기능, 기존 화면에 버튼 하나 추가
   - 시안을 만들지 않는다. design-system.md도 고치지 않는다.
   - spec-index.md 상태를 "screen 건너뜀"으로 바꾸고, 다음 단계는 /meta-plan이라고 알린다.
   - 건너뛸지 애매하면 사용자에게 묻는다.

3. 시안 3개 만들기
   .meta/specs/번호/screens/에 a.html, b.html, c.html을 만든다.
   파일 하나에 그 기능의 모든 화면을 담는다.
   외부 파일 없이 브라우저로 바로 열리게 만든다.

   design-system.md가 비어 있을 때 (첫 기능)
   - 세 시안의 색, 글꼴, 배치가 확실히 달라야 한다.

   design-system.md가 있을 때 (두 번째 기능부터)
   - 색, 글꼴, 간격, 컴포넌트는 design-system.md를 그대로 따른다.
   - 화면 배치만 다르게 한다.

4. 보여주기
   세 파일을 브라우저로 연다.
   AskUserQuestion으로 하나를 고르게 한다.
   섞고 싶다는 요청이 오면 섞어서 다시 만든다.

5. 정리
   고른 시안은 screen.html로 이름을 바꾼다. 나머지는 지운다.

6. design-system.md 채우기
   비어 있었으면 고른 시안에서 색, 글꼴, 간격, 모서리, 버튼과 입력창 모양을 뽑아 쓴다.
   이미 있었으면 새로 생긴 컴포넌트만 추가한다. 기존 내용은 바꾸지 않는다.

7. 마무리
   spec-index.md 상태를 "screen 완료"로 바꾼다.
   다음 단계는 /meta-plan이라고 알린다.

## design-system.md 형식

```
# 디자인 시스템

색
- 기본: #2F5BEA
- 배경: #FFFFFF
- 글자: #1A1A1A
- 보조 글자: #6B7280
- 오류: #DC2626

글꼴
- Pretendard
- 제목 24px, 본문 16px, 작은 글자 13px

간격
- 4의 배수만 쓴다. 8, 16, 24, 32

모서리
- 버튼, 입력창 8px
- 카드 12px

버튼
- 주 버튼: 기본색 배경, 흰 글자, 높이 44px
- 보조 버튼: 흰 배경, 회색 테두리

입력창
- 높이 44px, 회색 테두리
- 오류일 때 테두리를 오류색으로, 아래에 이유 한 줄
```

## 쓰는 규칙

design-system.md가 50줄을 넘으면 줄인다.
