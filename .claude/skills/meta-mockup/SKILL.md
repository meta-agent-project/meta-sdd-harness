---
name: meta-mockup
description: spec.md의 화면을 HTML 시안 3개로 보여주고 하나를 고르게 한다. 시안은 meta-design-consistency로 기존 화면과 맞춘 뒤 보여주고, 고른 시안으로 tokens.yaml, components.yaml, design-system.md를 채운다. "화면 시안", "목업", "디자인 골라줘", "meta-mockup" 요청에 사용.
---

# meta-mockup

화면 시안 3개를 HTML로 보여주고 하나를 고르게 한다.
일관성 검사와 토큰, 디자인 시스템 갱신은 meta-design-consistency가 한다.

## 순서

1. 읽기
   spec.md, tech.md, tokens.yaml, components.yaml, design-system.md를 읽는다.
   design-system.md의 "쓰인 곳"에 나온 기존 mockup.html도 읽는다.

2. 화면 목록 뽑기
   사용자 스토리 P1을 보고 필요한 화면만 뽑는다.
   예: 가입 화면, 가입 완료 화면

   새 화면이나 화면 변경이 없으면 건너뛴다.
   - 예: PDF 내보내기, 백엔드만 있는 기능, 기존 화면에 버튼 하나 추가
   - 시안을 만들지 않는다. 토큰과 디자인 시스템도 고치지 않는다.
   - spec-index.md 상태를 "mockup 건너뜀"으로 바꾸고, 다음 단계는 /meta-plan이라고 알린다.
   - 건너뛸지 애매하면 사용자에게 묻는다.

3. 시안 3개 만들기
   .meta/specs/번호/mockups/에 a.html, b.html, c.html을 만든다.
   파일 하나에 그 기능의 모든 화면을 담는다.
   외부 파일 없이 브라우저로 바로 열리게 만든다.

   tokens.yaml이 비어 있을 때 (첫 기능)
   - 세 시안의 색, 글꼴, 배치가 확실히 달라야 한다.

   tokens.yaml이 있을 때 (두 번째 기능부터)
   - 값은 tokens.yaml, 컴포넌트는 components.yaml을 그대로 쓴다.
   - 페이지 골격과 패턴은 design-system.md와 기존 mockup.html을 그대로 따른다.
   - 세 시안은 본문 안의 배치만 다르게 한다. 예: 표, 카드 목록, 두 칸 나누기

4. 시안 검사
   meta-design-consistency의 "시안 검사 1: 보여주기 전"을 실행한다.
   토큰이 비어 있으면 건너뛴다.

5. 보여주기
   세 파일을 브라우저로 연다.
   AskUserQuestion으로 하나를 고르게 한다.
   섞고 싶다는 요청이 오면 섞어서 다시 만들고 4번부터 다시 한다.

6. 정리
   고른 시안은 mockup.html로 이름을 바꾼다. 나머지는 지운다.

7. 토큰과 디자인 시스템 갱신
   meta-design-consistency의 "시안 검사 2: 고른 뒤"를 실행한다.

8. 마무리
   spec-index.md 상태를 "mockup 완료"로 바꾼다.
   커밋한다. 커밋 메시지: "docs: 002-signup mockup"
   다음 단계는 /meta-plan이라고 알린다.
