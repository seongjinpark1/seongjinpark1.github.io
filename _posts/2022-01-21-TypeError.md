---
title: '[Error]EventTarget' 형식에 'id' 속성이 없습니다.'
tags: [TIL, Error]
style: fill
color: warning
description: '타입스크립트를 사용하면서 event target에 속성을 사용할 일이 있어 위와 같이 event의 타입을 지정해주었고 id값을 이용하려 하는데...
'
---

```js
const checkOnlyOne = (e: React.MouseEvent<HTMLInputElement>) => {};
```

타입스크립트를 사용하면서 event target에 속성을 사용할 일이 있어 위와 같이 event의 타입을 지정해주었고 id값을 이용하려 하는데

![](https://images.velog.io/images/blackdavil01/post/f96dc0c3-305b-4f99-a6ef-eca41e80a56c/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202022-01-21%2012-32-46.png)
위와 같이 EventTarget 형식에 id속성이 없다는 에러를 만나게 되었다.
처음에는 `console.log(e.target)`1를 해보며 `e.target`이 console창에 찍히는 걸 확인했고 id도 존재하는 것을 확인 하였다.
그래서 구글링을 해서 찾아보니...

### ❓왜 EventTarget이 id를 인지할 수 없을까??

event target은 기본적으로 타입스크립트의 EventTarget타입을 가지고 있는데 EventTarget이 Element로 부터 상속을 받는 것이 아니기 때문에 타입스크립트 입장에선 Element의 id나 value등을 찾을 수 없다는 것이 이유였다.

### ❓그렇다면 왜 EventTarget타입은 Element로부터 상속을 받지 않을까??

왜냐하면 모든 EventTarget이 Element라는 보장이 되지 않기 때문이다. (EventTarget이 FileReader, AudioNode 등이 될 수도 있기 때문이다.)

### ❓그래서 해결법은??

**_첫번째 해결방안(레퍼런스에서 나온 방법)_**
레퍼런스에는 문제를 해결하기 위해서 다음과 같이 타입을 지정해 주라고 하였지만 나의 경우에는
`js const target = e.target as Element; `
다음과 같이 더 명확하게 타입을 지정해 주어야 해결이 되었다.
`js const target = e.target as HTMLInputElement; `

**_두번째 해결방안_**
두번째 방법으로는 currentTarget을 사용하는 것이다.
currentTarget은 이벤트 핸들러가 부착된 것을 가리키기 때문에 id나 value등을 인지할 수 있는 것 같았다.
하여

```js
e.currentTarget.id;
```

위와 같이 사용하면 event.target의 id를 사용하려해도 인지하지 못한다는 에러를 발견하지 않았다.

#### Reference

[레퍼런스](https://www.designcise.com/web/tutorial/how-to-fix-property-does-not-exist-on-type-eventtarget-typescript-error)
