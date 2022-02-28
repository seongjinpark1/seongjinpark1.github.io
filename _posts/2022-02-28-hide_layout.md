---
title: 'layout을 숨기는 속성들의 차이점'
tags: [TIL]
style: fill
color: info
description: 'display:none, visibility:hidden, opacity: 0의 차이점에 대해서 알아보고자 한다.'
---

> 이번에 과제용으로 페이지를 하나 만들면서 display:none, visibility:hidden, opacity: 0의 차이점에 대해서 알고 싶어졌습니다.

#### display: none

- 화면에 보이지 않고 영역을 차지하지 않습니다.

display: none은 렌더링트리에 포함되지 않아 상태를 비교할 값이 없어 transition 속성이 먹히질 않는다.

#### visibility: hidden

- 화면에 보이지 않지만 영역을 차지하고 있습니다.
- visibility:hidden으로 숨긴 요소 아래에 있는 요소사용이 가능합니다.
- transition 속성에 반응합니다.

#### opacity: 0

- 화면에 보이지 않지만 영역을 차지하고 있습니다.
- opacity: 0 으로 숨긴 요소 아래에 있는 요소의 제어(클릭,이벤트 등)를 방해하여 클릭되지 않습니다.
- transition 속성에 반응하고, fadein과 fadeout효과를 처리할 수 있습니다.
