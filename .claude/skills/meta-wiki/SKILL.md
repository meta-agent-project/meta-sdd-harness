---
name: meta-wiki
description: converge가 끝난 기능을 wiki 문서에 반영한다. 위키 에이전트가 architecture, erd, glossary, screen, spec-index를 갱신한다. "위키 갱신", "wiki", "meta-wiki" 요청에 사용.
---

# meta-wiki

converge가 끝난 기능을 wiki에 반영한다.
wiki에는 실제로 구현된 내용만 담는다.

## 순서

1. 확인
   spec-index.md에서 이 기능의 상태가 "converge 완료"인지 확인한다.
   아니면 멈추고 /meta-converge를 먼저 하라고 알린다.

2. 갱신 (meta-wiki-writer 에이전트)
   위키 에이전트에게 맡긴다.
   넘길 것: 기능 번호, 바뀐 파일 목록

3. 충돌 확인
   에이전트가 기존 wiki와 충돌한다고 보고하면 사용자에게 묻는다.
   예: 같은 용어가 다른 뜻으로 쓰였다

4. 마무리
   문서마다 바뀐 줄 수만 알린다.
   spec-index.md 상태를 "완료"로 바꾸고 커밋한다.

5. 머지
   기본 브랜치로 옮겨 spec/{기능 번호}를 --no-ff로 머지한다.
   머지 커밋 메시지: "merge: 002-signup"
   머지가 끝나면 spec 브랜치를 지운다.
   충돌이 나면 머지를 취소하고 멈춘 뒤 사용자에게 알린다.
   다음 할 일은 /meta-flow로 확인하라고 알린다.
