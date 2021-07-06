---
title: '벤더 프리픽스란?'
excerpt: 'css속성중 animation에 대해 공부하던 중 @keyframes사용해야 한다는 건 알았는데..'

categories:
    - TIL
tags:
    - TIL
    - CSS
last_modified_at: 2021-07-06
---

# 벤더 프리픽스(Vendor Prefix)

css속성중 animation에 대해 공부하던 중 @keyframes사용해야 한다는 건 알았는데 앞에 -webkit- ,-moz- 등 알 수 없는 것들이 나와 벤더 프리픽스에 대해 알아보게 되었습니다.

벤터 프리픽스는 HTML과는 다르게 CSS는 아직 '웹 표준'이 정해지지 않았기 때문에 브라우저에 따라 다른 방식으로 지원이 되는데 이 때 CSS를 모든 브라우저에서 원활하게 사용하기 위해서, 스타일 속성 앞에 프리픽스를 붙여주어야 합니다.

# 사용법

프리픽스의 사용법은 스타일 속성 앞에 프리픽스를 넣어주면 브라우저 마다 적용되는 스타일 속성을 표현할 수 있습니다.

```css
@-webkit-keyframes {
}
@-moz-keyframes {
}
@-o-keyframes {
}
@-ms-keyframes {
}
@keyframes {
}
```

CSS 속성은 속성이 오는 순서에 따라 순차적으로 적용되기 때문에 벤더 프리픽스가 붙은 속성과 표준 속성이 모두 있을 경우 나중에 오는 속성이 적용됩니다.
따라서 위의 코드에서 표준 속성인 @keyframes가 적용됩니다.
**표준 속성의 적용을 우선해야 하므로, 벤더 프리픽스가 있는 속성은 표준 속성 앞쪽에 표시하는 것이 원칙입니다.**

# 브라우저별 프리픽스

| 벤더 프리픽스 | 웹 브라우저       |
| ------------- | ----------------- |
| -webkit-      | 사파리, 크롬      |
| -moz-         | 파이어폭스        |
| -ms-          | 인터넷 익스플로러 |
| -o-           | 오페라            |

# -Prefix-free

스타일 속성을 사용할 때마다 일일이 브라우저별 지원 스타일 속성을 확인하고 다 프리픽스를 붙인다면 많이 번거롭고 코드양도 길어지기 때문에 -Prefix-free라는 라이브러리를 사용하여 번거로움을 해결 할 수 있습니다.
사용법은 다음과 같습니다.

1. [Prefix](https://projects.verou.me/prefixfree/) 사이트에 들어가서 다음 그림을 다운 받는다.
   ![](https://images.velog.io/images/blackdavil01/post/ccf2a86e-f702-41ac-bc12-e7d8d486eb12/img.png)

2. HTML파일에 다음 코드를 삽입해준다.

```js
<script src="prefixfree.min.js"></script>
```
