---
title: '타입스크립트 기초'
excerpt: '타입스크립트 기초'

categories:
  - TIL
tags:
  - TIL
  - JavaScript
  - Typescript
toc: true
toc_sticky: true
last_modified_at: 2021-10-24
---

# 설치 및 명령어

`npm install typescript`
타입스크립트를 사용하기위해 설치

`npm install ts-node`
노드환경에서 타입스크립트 실행하기위해 설치

`npx ts-node ./src/practice.ts`
위 명령어로 js로 컴파일하지 않고도 노드환경에서 실행가능

`tsc --init`
프로젝트 타입스크립트 설정이 만들어진다.
직접 작성해서 만들어도 되지만 명령어로 하면 쉽게 바로 만들 수 있다.

`tsc`
ts파일을 js파일로 컴파일 시켜주는데
tsconfig.json의 outDir에 경로를 설정하면 해당 경로에 js파일이 컴파일된다.

# 기초문법

- 문자열 타입으로 선언

```js
const message: string = 'hello world';
```

- 숫자형 타입으로 선언

```js
const count: number = '1';
```

- 불리언 타입으로 선언

```js
const done: boolean = false;
```

- 숫자형 요소를 가진 배열 선언

```js
const: numbers: number[] = [1,2,3]
```

- 문자형 요소를 가진 배열 선언

```js
const messages: string[] = ['hello', 'world'];
```

- 문자형 또는 undefined형을 받아야 할때

```js
let mightBeUndefined: string | undefined = 'asdf';
```

- 미리 값을 정해 놓을 때

```js
let color: 'red' | 'orange' | 'yellow';
```

> 타입스크립트에서 타입선언을 빼놓게 되면 기본적으로 any로 설정되며, 빨간줄이 그어진다.
> 빨간줄만 없애고 싶다면 any라고 직접적으로 타입선언을 해주어야한다.

- 함수에서 타입을 선언할 때

```js
// 받아오는 인자에 다음처럼 타입을 선언해줄 수 있다.
// 함수가 리턴하는 값의 타입을 선언할때는 인자가 끝나는 괄호다음 타입을 선언해줄 수 있다.
function sum(x: number, y: number): number {
return x + y;
```

- 함수안에 리턴하는 값이 없을 때

```js
// 결과 값을 void로 선언하고 return 값을 넣게 된다면 오류가 발생한다.
function returnNothing(): void {
	console.log('어쩌고저쩌고');
}
returnNothing();
```

# Interface

interface는 클래스 또는 객체를 위한 타입을 지정할 때 사용하는 문법이다.

- 클래스에서 타입정의

```js
// interface를 사용해서 Shape의 타입을 선언해주고 implements Shape를 사용했기 때문에 Circle클래스 내부에는 Shape의 타입을 충족해주어야한다.
interface Shape {
	getArea(): number;
}

class Circle implements Shape {
	radius: number;

	constructor(radius: number) {
		this.radius = radius;
	}
	getArea() {
		return this.radius * this.radius * Math.PI;
	}
}
```

- 객체에서 타입정의

```js
interface Person {
	name: string;
	// age옆에 ?를 넣게되면 age값이 있을 수도 없을 수도 있다는 뜻이된다.
	age?: number;
}

const person: Person = {
	name: 'asdf',
	age: 5,
};
```

- 타입이 중첩일 때

```js
interface Person {
	name: string;
	age?: number;
}

interface Developer {
	name: string;
	age?: number;
	skills: string[];
}

const person: Person = {
	name: '김사람',
	age: 20,
};

const expert: Developer = {
	name: '김개발',
	skills: ['javascript', 'react', 'typescript'],
};
```

위 코드와 같이 name과 age가 중첩일 때 extends를 사용해서 interface를 상속 받을 수 있다.

```js
interface Developer extends Person {
	skills: string[];
}
```

# Type alias

type 은 특정 타입에 별칭을 붙이는 용도로 사용한다. 이를 사용하여 객체를 위한 타입을 설정할 수도 있고, 배열, 또는 그 어떤 타입이던 별칭을 지어줄 수 있다.
interface에서 사용했던 코드를 type alias로 전환해보면 다음과 같이 사용할 수 있다.

```js
type Person = {
	name: string,
	age?: number,
};
// 중첩되는 타입이 있어 상속 받고 싶을 때는 &연산자를 사용할 수 있다.
type Developer = Person & {
	skills: string[],
};

const person: Person = {
	name: '김사람',
	age: 20,
};

const expert: Developer = {
	name: '김개발',
	skills: ['javascript', 'react', 'typescript'],
};
```

- type과 interface 어떤 용도로 사용을 해야 할까?
  클래스와 관련된 타입의 경우엔 interface를 사용하는게 좋고, 일반 객체의 타입의 경우엔 그냥 type을 사용해도 무방하다.
  객체를 위한 타입을 정의할때 무엇이든 써도 상관없지만 일관성 있게 사용하는 것을 권한다고 한다.

# Generics

제너릭(Generics)은 타입스크립트에서 함수, 클래스, interface, type alias를 사용하게 될 때 여러 종류의 타입에 대하여 호환을 맞춰야 하는 상황에서 사용하는 문법이다.

- 함수에서 Generics 사용하기

예를 들어 객체 A와 객체 B를 합쳐주는 merge라는 함수를 만든다고 가정했을때, A와 B에 어떤 타입이 올 지 모르기 떄문에 이런 상황에서는 any라는 타입을 쓸 수도 있다.

```js
function merge(a: any, b: any): any {
	return {
		...a,
		...b,
	};
}

const merged = merge({ foo: 1 }, { bar: 1 });
```

그런데, 이렇게 하면 타입 유추가 모두 깨진거나 다름이 없다. 결과가 any라는 것은 merged안에 무엇이 있는지 알 수 없는 것이 되어버리기 떄문이다.
물론 타입을 object로 선언해서 사용할 수도 있지만 merged가 객체형태라는 것만 알뿐 merged안에 뭐가 들어있는지 확인 할 수가 없다.
이런 상황에 Generics를 사용할 수 있다.

```js
function merge<A, B>(a: A, b: B): A & B {
	return {
		...a,
		...b,
	};
}

const merged = merge({ foo: 1 }, { bar: 1 });
```

<T> 형식으로 사용하며, 위와 같이 사용하게 되면 파라미터의 타입이 유추가 되게 되고, merged안에 뭐가 들어있는지 확인할 수 있습니다.
즉, 이렇게 함수에서 Generics를 사용하면 파라미터로 다양한 타입을 넣을 수도있고, 타입 지원을 지켜낼 수도 있다.

- interface에서 Generics 사용하기

```js
interface Items<T> {
	list: T[];
}
// 아래코드의 string를 number로 바꾸게 되면 list의 요소들을 숫자형으로 바꾸어 주어야 합니다.
const items: Items<string> = {
	list: ['a', 'b', 'c'],
};
```

- type에서 Generics 사용하기

```js
type Items<T> = {
	list: T[],
};

const items: Items<string> = {
	list: ['a', 'b', 'c'],
};
```
