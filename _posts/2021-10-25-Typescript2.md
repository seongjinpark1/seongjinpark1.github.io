---
title: 'TypeScript(with React)'
tags: [TIL, React, Typescript]
style: fill
color: secondary
description: 'TypeScript(with React)'
---

리액트 타입스크립트 설치

```
npx create-react-app ts-react-tutorial --template typescript
```

이미 존재하는 프로젝트에 타입스크립트를 설치

```

npm install --save typescript @types/node @types/react @types/react-dom @types/jest

# or

yarn add typescript @types/node @types/react @types/react-dom @types/jest
```

# React.FC

```ts
import React from 'react';

type GreetingsProps = {
	name: string;
};

const Greetings: React.FC<GreetingsProps> = ({ name }) => <div>Hello, {name}</div>;

export default Greetings;
```

위 코드처럼 화살표함수를 사용하여 컴포넌트를 선언하였고, React.FC라는 것을 사용하여 컴포넌트의 타입을 지정했습니다.

React.FC를 사용할 때는 props의 타입을 Generics로 넣어서 사용합니다. 이렇게 React.FC를 사용해서 얻을 수 있는 이점은 두가지가 있습니다.

#### 첫번째는 props에 기본적으로 children이 들어가있다는 것입니다.

children이 기본적으로 들어가있기 때문에 다음처럼 children타입을 지정해주지 않아도 됩니다.

```ts
type GreetingsProps = {
	name: string;
	children: React.ReactNode;
};
```

하지만 children이 옵션 형태로 들어가있다보니 props의 타입이 명백하지 않습니다.
어떤 컴포넌트는 children이 필요할 수도, 필요없을 수도 있는데 React.FC를 사용하면 기본적으로는 이에 대한 처리를 못하게 되어 만약 하고 싶다면 결국 위 코드와 같이 Props타입 안에 children을 설정해주어야 합니다.

#### 두번째는 컴포넌트의 defaultProps, propTypes, contextTypes 를 설정 할 때 자동완성이 될 수 있다는 것 입니다.

하지만 React.FC는 defaultProps가 제대로 작동하지 않습니다.

**src/Greetings.tsx**

```ts
import React from 'react';

type GreetingsProps = {
	name: string;
	mark: string;
};

const Greetings: React.FC<GreetingsProps> = ({ name, mark }) => (
	<div>
		Hello, {name} {mark}
	</div>
);

Greetings.defaultProps = {
	mark: '!',
};

export default Greetings;
```

**src/App.tsx**

```ts
import React from 'react';
import Greetings from './Greetings';

const App: React.FC = () => {
	return <Greetings name="Hello" />;
};

export default App;
```

만약 위와 같은 코드가 있을 때 App에서는 mark값을 전달해 주지 않았지만 mark의 값을 defaultProps로 설정해 주었음에도 불구하고 App에서 mark값이 없다면서 제대로 작동하지 않습니다.
물론 type Greetings에 mark? 라고 표시해주면 string또는 undefined를 받게 되어 당장의 문제는 해결하게 되지만 이것은 또다른 문제를 일으키게 됩니다.

```ts
type GreetingsProps = {
  name: string;
  mark: string;
  array?: string[]
};

const Greetings: React.FC<GreetingsProps> = ({ name, mark, array }) => (
  array.map
  <div>
    Hello, {name} {mark}
  </div>
);

Greetings.defaultProps = {
  mark: '!'
  array: ['a', 'b', 'c']
};
```

App에서는 array라는 props를 전달해주지 않고 위 코드 처럼 작성되었을 때 array는 string요소를 가진 배열 혹은 undefined가 되기 때문에 배열메소드인 map이 작동하지 않게 됩니다.
만약 사용하고 싶다면 array가 존재한다면이라는 조건문을 두어야 동작하게 할 수 있습니다.

따라서 React.FC를 사용하면 defaultProps를 사용할 수 없다는 걸 알게 되었고, defaultProps를 설정하고 싶다면 props를 인자로 받는곳에 default값을 직접 입력해주어야 합니다.

# function 키워드 사용

React.FC를 사용하지 않고 function 키워드를 사용하여 나타낸다면 defaultProps가 동작하게 하면서 작성할 수 있습니다.

```ts
import React from 'react';

type GreetingsProps = {
	name: string;
	mark: string;
};

const Greetings = ({ name, mark }: GreetingsProps) => (
	<div>
		Hello, {name} {mark}
	</div>
);

Greetings.defaultProps = {
	mark: '!',
};

export default Greetings;
```

### 컴포넌트에 특정 함수를 props로 받아오기

**src/Greetings.tsx**

```ts
import React from 'react';

type GreetingsProps = {
	name: string;
	mark: string;
	// void는 아무것도 리턴하지 않는다는 함수를 의미합니다.
	onClick: (name: string) => void;
};

function Greetings({ name, mark, onClick }: GreetingsProps) {
	const handleClick = () => onClick(name);
	return (
		<div>
			Hello, {name} {mark}
			<div>
				<button onClick={handleClick}>Click Me</button>
			</div>
		</div>
	);
}

Greetings.defaultProps = {
	mark: '!',
};

export default Greetings;
```

**App.tsx**

```ts
import React from 'react';
import Greetings from './Greetings';

const App: React.FC = () => {
	const onClick = (name: string) => {
		console.log(`${name} says hello`);
	};
	return <Greetings name="Hello" onClick={onClick} />;
};

export default App;
```

# 타입스크립트로 리액트 상태관리

```ts
import React, { useState } from 'react';

function Counter() {
	// 제너릭을 사용해서 타입을 선언할수 있지만 생략가능하다.
	const [count, setCount] = useState<number>(0);
	const onIncrease = () => setCount(count + 1);
	const onDecrease = () => setCount(count - 1);
	return (
		<div>
			<h1>{count}</h1>
			<div>
				<button onClick={onIncrease}>+1</button>
				<button onClick={onDecrease}>-1</button>
			</div>
		</div>
	);
}

export default Counter;
```

useState를 사용하여 간단한 카운터를 타입스크립트를 이용하여 만들 때 위 코드처럼 Generics를 사용하여 해당 상태가 어떤 타입을 가지고 있을지 설정해 줄수 있습니다.
하지만 useState를 사용 할 때 Generics를 사용하지 않아도 알아서 타입을 유추하기 때문에 생략해도 상관없습니다.

> 💡그럼 useState를 사용 할 때 어떤 상황에 Generics를 사용하는게 좋을까요?
> 상태가 null일 수도 있고 아닐수도 있을때 Generics를 활용하면 좋습니다.

## 이벤트 타입 지정하기

event객체의 타입이 무엇인지 궁금할 때는 onChange, onSubmit등에 마우스 커서를 올려보면 어떤 타입을 해야하는지 알려주기 때문에 마우스로 드래그해서 복사해서 사용하면 됩니다.
예를 들어 onChange의 event객체의 타입에는 `React.ChangeEventHandler<HTMLInputElement>` 해당 타입이 나오는데 Handler 부분만 지우고 사용 할 수 있습니다.
