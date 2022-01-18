---
name: Test111
tools: [nothing, important]
image: https://www.sketchappsources.com/resources/source-image/project-neon-groove-music-ui.png
description: Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
---

## Float란?

CSS에서 float속성은 레이아웃을 잡는데 사용하는 속성이며 붕 뜬다는 의미이다.
개인적으로 flex를 사용하여 레이아웃을 잡는 게 훨씬 편하긴 하지만 float에 대한 글을 작성하는 이유는 강의를 들을때나 여러 블로그에서 한번씩 보이는 float에 대해서도 알아 두기 위함이다.

## ❓ What happen?(float속성을 사용하면 무슨일이 일어나는가?)

![](https://images.velog.io/images/blackdavil01/post/d44b90db-2a5e-4337-8b7b-95d3abf07533/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202022-01-11%2012-58-07.png)

```html
<!-- html  -->
<div class="parent">
	<div class="child red">Child</div>
	<div class="child orange">Child</div>
	<div class="child blue">Child</div>
</div>
```

```css
/* css */
.parent {
	width: 200px;
	margin: 0 auto;
	background-color: #eff2f7;
}
```

위 코드처럼 부모요소에게는 height를 주지 않았기 때문에 부모요소의 height는 자식요소의 height의 합으로 정해지게 된다.

#### 1. 자식요소에게 float속성을 부여하게 되면 말 그대로 붕 뜨기 때문에 부모요소,형제요소가 해당 요소(float속성을 적용한 요소)를 인식하지 못하게 되고 빈영역으로 인식하게 된다.

![](https://images.velog.io/images/blackdavil01/post/17030536-e8b2-4aa8-a927-212df00efa3e/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202022-01-11%2013-11-56.png)

```css
.red {
	background: red;
	float: left;
}
```

빨간박스에게 `float:left`속성을 부여하게 되면 다른 요소가 인식을 못하게 되어 오렌지박스와 블루박스가 빨간박스를 빈 영역으로 인식하게 되어 기존의 빨간박스가 있던 자리부터 배치가 시작되고 부모요소도 빨간박스를 인식할 수 없기 때문에 height가 기존에 600px에서 400px로 변경되는 것을 확인할 수 있다.

#### 2. 요소에게 float속성을 부여하게 되면 display속성이 (하자있는)block으로 변경된다.

![](https://images.velog.io/images/blackdavil01/post/2cfdea6d-f4b0-4a90-b63e-54b967ecb836/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202022-01-11%2013-29-34.png)

float속성을 부여하게 되면 `display:block`이 되고 width, height, margin, padding 또한 부여할 수 있다.

하지만 기존의 block요소와는 다른 점이 있다.

- 기존의 block요소는 width를 설정하지 않은 경우에 width = 부모의 content-box 의 100%를 차지하게 된지만 float속성을 적용하여 변경된 block은 위 사진과 같이 컨텐츠의 길이만큼 width값을 가지는 것을 볼 수 있다.

- 기존의 block요소는 부모width = 1000px이고 자식요소width = 400px 이라면 남은 600px은 margin이 자동으로 채워주게 되지만 float속성을 적용하여 변경된 block은 자동으로 margin을 부여하지 않는다.
  (여기서 주의할 점은 남은 여백이 margin으로 채워지지 않는다는 점이지 margin을 부여할 수 없다는 뜻은 아니다.)

#### 3. float속성을 부여했을 때 레이아웃의 붕괴

![](https://images.velog.io/images/blackdavil01/post/d98bc26b-2c40-4f29-a20b-d7099b43c8a3/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202022-01-11%2013-45-49.png)

모든 자식요소에게 float속성을 부여하게 되면 부모요소는 모든 자식을 인식할 수 없기 때문에 height의 값이 0이 되게 된다.
이 때 부모요소의 형제요소를 하나 만들어주고 임의의 텍스트를 작성하면 위와 같이 레이아웃이 개판이 되는걸 확인할 수 있다.

#### 4. block요소는 float적용된 요소를 인식할 수 없지만 inline요소는 인식할 수 있다.

![](https://images.velog.io/images/blackdavil01/post/943ca6a2-233a-4360-8c2b-484fc90070cd/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202022-01-11%2013-53-29.png)

```css
.blue {
	background: blue;
	margin-right: 300px;
}
```

block요소는 float적용된 요소를 없는 것 처럼 취급하지만 텍스트나 이미지 같은 인라인의 성격을 가지고 있는 것들은 float가 적용된 요소를 인식할 수 있게 되기 때문에 3번과 같은 레이아웃의 붕괴가 일어나는 것이다.

위 그림처럼 파란박스에 `margin-right = 300px`적용해주게되면 텍스트가 float적용된 요소를 인식하고 피해서 레이아웃이 적용된 모습을 화인할 수 있다.

## 🔧 How to fix(float을 사용해서 망가진 레이아웃을 어떻게 잡을까)

#### 1. 부모요소에 overflow: hidden 속성을 적용

![](https://images.velog.io/images/blackdavil01/post/83a77d14-c9f5-47aa-b144-bab65aa5489f/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202022-01-11%2013-59-16.png)

float을 사용하여 레이아웃이 망가진 경우에 부모요소에 `overflow:hidden` 속성을 부여해주게 되면 위 그림과 같이 부모요소가 자식요소를 인식할 수 있게 되어 0이었던 height값이 자식요소의 height값의 합인 `height = 400px`가 된 것을 확인할 수 있고 부모요소의 형제였던 텍스트는 형제요소가 있다는 것을 인식하게 되고 레이아웃이 정렬된 것을 확인할 수 있다.

#### 2. clear속성을 적용

![](https://images.velog.io/images/blackdavil01/post/e482d527-4224-482b-b7f1-8d67a04eb4da/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202022-01-11%2014-05-07.png)

- clearfix는 float으로 망가진 레이아웃을 해결하는데 정석같은 방법이다.
- clear속성은 오로지 float으로 인해 망가진 레이아웃을 해결하기 위한 속성이다.
- **clear속성은 block요소에만 적용이 된다.**

위 그림처럼 자식1에게는 `float:left`속성을 주고 자식2에게는 `clear:left`를 적용하게 되면 부모는 float을 적용한 자식요소를 포함한 height값을 가질 수 있게 된다.

#### 3. Pseudo-Element(가상요소)에 clear속성을 부여

![](https://images.velog.io/images/blackdavil01/post/9a04e446-1f09-4d35-965b-b4a188613c65/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202022-01-11%2014-12-00.png)

만약 자식요소 모두가 float속성을 적용하게 되면 부모는 자식을 인식할 수 없기 때문에 아무 의미 없는 div태그 하나를 추가로 만들어서 clear속성을 부여해야 하는데 스타일적 이슈로 인해 아무런 의미도 없는 엘리먼트 요소를 만들어서 마크업을 복잡하게 만들게 된다.

해당이슈를 해결하기 위해 Pseudo-element(가상요소)를 사용할 수 있다.
Pseudo-element(가상요소)는 HTML에 존재하지 않는 가상 요소이며 css를 이용하여 생성할 수 있으며 요소당 2개씩(`::before`, `::after`) 만 만들 수 있다.
![](https://images.velog.io/images/blackdavil01/post/2334ea2c-c095-4fad-85a3-ab4ad1b2e1c7/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202022-01-11%2013-59-16.png)

```css
.parent::after {
	display: block;
	content: '';
	clear: left;
}
```

부모요소에 clear속성을 부여해주면 레이아웃이 정렬되는 것을 볼 수 있다.
Pseudo-element(가상요소)를 사용할 때 주의 할 점은 **`content` 속성이 반드시** 들어가야하고 가상요소에 쓸 내용이 없다면 `''`로 처리할 수 있다.

> 💡 float요소를 많이 사용하게 되면 clear속성또한 많이 사용하게 되는데 이 때 다음과 같이 clearfix라는 클래스에 가상요소를 만들어두면 float를 적용한 부모요소에 .clearfix 클래스만 추가해주게 되면 레이아웃 붕괴 문제를 해결 할 수 있다.

```html
<div class="parent clearfix">
	<div class="child red">Child</div>
	<div class="child orange">Child</div>
	<div class="child blue">Child</div>
</div>
```

```css
.clearfix::after {
	content: '';
	display: block;
	clear: both;
}
```
