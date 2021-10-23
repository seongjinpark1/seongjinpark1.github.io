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
last_modified_at: 2021-10-23
---

자바스크립트는 weektype이며 코드가 실행되어야 실수한지 안한지 알수 있습니다. 이를 보완해주는 것이 타입스크립트 입니다.

```js
// 타입스크립트, 타입스크립트 노드 설치
npm install typescript ts-node

// 타입스크립트 config.js설치
tsc --init

// 컴파일 후 js로 변환
npx tsc
```

# 타입스크립트 기본 타입

```js
// 그냥 숫자로 선언
let count = 0;
count += 1;
count = 2;

// 문자열
const message: string = 'hello world';

// 불리언
const done: boolean = false;

// 숫자형인 배열
const numbers: number[] = [1, 2, 3];

// 문자형인 배열
const messages: string[] = ['hello', 'world'];
messages.push('!');

// 문자열 또는 undefined
let mightBeUndefined: string | undefined = undefined;

// 숫자형 또는 null
let nullableNumber: number | null = null;

// 컬러라는 변수에 레드,오렌지,옐로우만 허용
let color: 'red' | 'orange' | 'yellow' = 'red';

// 함수의 인자타입을 숫자형, 결과형도 숫자형
function sum(x: number, y: number): number {
	return x + y;
}
const result = sum(1, 2);

// 함수의 인자타입을 숫자인배열형, 결과형은 숫자형
function sumArray(numbers: number[]): number {
	return numbers.reduce((acc, cur) => acc + cur, 0);
}

const total = sumArray([1, 2, 3, 4, 5]);
```

# 타입스크립트 interface, Type Alias

#### interface

클래스 또는 객체타입을 지정할떄 사용되는 문법

```js
// 클래스 타입지정
interface Shape {
    getArea(): number;
}

class Circle implements Shape {
    radius: number;

    constructor(radius: number) {
        this.radius = radius;
    }
    getArea() {
        return this.radius * this.radius * Math.PI
    }
}


class Rectangle implements Shape {
    width: number;
    height: number;
    constructor(width: number, height: number) {
        this.width = width;
        this.height = height;
    }
    getArea() {
        return this.width * this.height;
    }
}

const circle: Circle = new Circle(5)
const rectangle: Rectangle = new Rectangle(2, 5)
const shapes: Shape[] = [circle, rectangle]

------------------------------------------------
// 객체 타입 지정(interface사용)
interface Person {
    name: string;
// ? 는 해당변수가 존재할수도 존재하지 않을수도있다는 뜻
    age?: number;
}

interface Developer extends Person {
// extends로 상속받아 name, age사용가능
    skills: string[]
}

const person:Person = {
    name: 'as'
}

const expert: Developer = {
    name: '김개발',
    skills: ['javascript', 'react']
}
  _____________________________________________    // 객체 타입 지정(type사용)
  type Person =  {
    name: string;
// ? 는 해당변수가 존재할수도 존재하지 않을수도있다는 뜻
    age?: number;
}

// interface대신 type사용가능하고 상속은 extends대신 &로 대체가능
type Developer = Person  & {
// extends로 상속받아 name, age사용가능
    skills: string[]
}


type People = Person[]
const people: People = [person, expert]

type Color = 'red' | 'orange' | 'yellow'
const color: Color = 'orange'

```

# Generics

Generics는 타입스크립트에서 함수, 클래스, interface, type alias를 사용하게 될 때 여러 종류의 타입애 대하여 호환을 맞춰야 하는 상황에서 사용하는 문법입니다.

### 함수에서 Geneerics 사용하기

만약 객체 A와 객체 B를 합쳐주는 merge라는 함수를 만든다고 가정할때 A와 B에 어떤 타입이 올 지 모르기 때문에 any라는 타입을 쓸 수도 있습니다.

```js
function merge(a: any, b: any): any {
	return {
		...a,
		...b,
	};
}

const merged = merge({ foo: 1 }, { bar: 1 });
```

그런데, 이렇게 하면 타입 유추가 모두 깨진거나 다름이 없습니다. 결과가 any라는 것은 즉 merged안에 무엇이 있는지 알 수 없다는 것입니다.
이런 상황에 Generics를 사용하면 됩니다.

```js
function merge<T1, T2>(a: T1, b: T2) {
	return {
		...a,
		...b,
	};
}

const merged = merge({ foo: 1 }, { bar: 2 });
```

이렇게 함수에서 Generics를 사용하면 any타입처럼 다양한 타입을 넣을 수 있고,
any는 어떤 타입이 들어오는지 정확히 알 수 없지만 Generics는 어떤 타입이 들어오는지 확인이 가능합니다.

### interface, type에서 Generics사용하기

- **interface**

```js
// list에는 배열의 아무타입이나 들어가게
interface Items<T> {
	list: T[];
}

// 배열이면서 string타입을 사용하도록
const items: Items<string> = {
	list: ['a', 'b', 'c'],
};
```

**- type**

```js
// list에는 배열의 아무타입이나 들어가게
type Items<T> = {
	list: T[],
};

// 배열이면서 string타입을 사용하도록
const items: Items<string> = {
	list: ['a', 'b', 'c'],
};
```
