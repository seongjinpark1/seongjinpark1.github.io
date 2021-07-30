---
title: 'Asynchronous'
excerpt: 'callback, promise, async&await'

categories:
    - TIL
tags:
    - TIL
    - JavaScript
    - Callback
    - Promise
    -Async & Await
toc: true
toc_sticky: true
last_modified_at: 2021-07-29
---

# 동기? 비동기?

![](https://images.velog.io/images/blackdavil01/post/1f843498-d724-4a86-bbdf-943d7c81ea30/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202021-07-28%2021-38-45.png)

#### 동기(Synchronous)란?

요청을 보낸 후 해당응답을 받아야지 다음 동작을 실행하는 것을 동기적이라고 말합니다.

#### 비동기(Asynchronous)란?

특정 코드의 연산이 끝날 때까지 코드의 실행을 멈추지 않고 다음 코드를 먼저 실행하는 것을 비동기라고 말합니다.

## 자바스크립트에서 왜 비동기 처리가 필요한가?

클라이언트에서 서버로 데이터를 요청 했을 때, 서버가 그 요청에 대한 응답을 언제 줄지도 모르는데 마냥 기다릴 수 없기 때문입니다.

## 비동기를 사용하면서 순서를 제어하고 싶다면?

```js
const printString = string => {
    setTimeout(() => {
        console.log(string);
    }, Math.floor(Math.random() * 100) + 1);
};
const printAll = () => {
    printString('A');
    printString('B');
    printString('C');
};
printAll();
```

> setTimeout()함수는 비동기 처리 모델로 작동하는 함수입니다.
> 따라서 다음의 함수는 비동기적으로 동작하지만 Math.random()을 이용해서 ABC순으로 작동할지 BCA순으로 작동할지 알 수가 없습니다. 이렇게 순서를 제어하고 싶을때 callback함수, promise, async/await 등을 사용해서 순서를 제어할 수 있습니다.

# Callback함수

## 콜백함수란?

콜백함수란 자바스크립트에서 함수는 object이다. 이 때문에 함수는 다른 함수의 인자로 쓰일 수도, 어떤 함수에 의해 리턴될 수도 있다. 이러한 함수를 고차함수라 부르고 인자로 넘겨지는 함수를 콜백함수라고 부른다.
위에 사용되었던 순서를 제어하지 못했던 코드를 callback을 사용하여 작성하면 비동기적이면서 순서를 제어할 수 있게 됩니다.

```js
const printString = (string, callback) => {
    setTimeout(() => {
        console.log(string);
        callback();
    }, Math.floor(Math.random() * 100) + 1);
};
const printAll = () => {
    printString('A', () => {
        printString('B', () => {
            printString('C', () => {});
        });
    });
};
printAll();
```

## 콜백지옥🤮

![](https://images.velog.io/images/blackdavil01/post/a2401954-f841-49de-9147-2dbafcd7fb28/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202021-07-28%2021-58-18.png)
콜백 지옥은 비동기 처리 로직을 위해 콜백 함수를 연속해서 사용할 때 발생하는 문제입니다. 만약 1000개, 10000개의 코드를 모두 비동기로 처리해야 한다고 한다면 콜백 안에 콜백을 계속무는 형식으로 코딩을 하게 됩니다. 콜백함수를 남용하게 되면 어디서 어떤식으로 연결되어 있는지 한눈에 가늠하기 어려워 가독성이 떨어지고, 에러가 발생하거나 디버깅을 해야할 때 굉장히 어렵고 유지보수도 어렵습니다.

# Promise

## Promise란?

> -   자바스크립트에서 제공하는 비동기를 간편하게 처리할 수 있도록 도와주는 오브젝트
> -   프로미스는 정해진 장시간의 기능을 수행하고나서 정상적으로 기능이 수행되어졌다면 성공의 메세지와 함께 처리된 결과값을 리턴해주고, 실패를 하게 되면 에러 메세지를 전달해주게 됩니다.
> -   프로미스는 콜백 패턴이 가진 단점인 콜백지옥을 보완하여 비동기 처리 시점을 명확하게 표현할 수 있습니다.

## Promise의 생성

프로미스는 Promise 생성자 함수를 통해 인스턴스화합니다. Promise생성자 함수는 비동기 작업을 수행할 콜백 함수를 인자로 전달받는데 이 콜백 함수는 resolve와 reject함수를 인자로 전달받습니다.
**new Promise를 생성할때, executor(resolve,reject)라는 콜백함수가 자동적으로 실행하게 됩니다**

```js
// Promise 객체 생성
const promise = new Promise((resolve, reject) => {
  // 비동기 작업 수행

  if(/*비동기 작업 성공*/) {
   resolve('result')
 }
 /*비동기 작업 실패*/
  else {
   reject('error')
 }
});
```

## Promise의 3가지 상태(States)

여기서 상태란 프로미스의 처리과정을 의미합니다. new Promise()로 프로미스를 생성하고 종료될 때 까지 3가지 상태를 갖습니다.

**1. Pending(대기):** new Promise()메서드를 호출하면 대기 상태가 됩니다. 즉, 비동기 처리 로직이 아직 완료되지 않은 상태입니다.

**2. Fulfilled(이행):** 비동기 작업이 성공적으로 수행되어 resolve를 실행하게 되면 Fulfilled상태가 됩니다. 즉, 비동기 처리가 완료되어 프로미스가 결과 값을 반환해준 상태입니다.

**3. Rejected(실패):** 비동기 작업이 실패 하였을 때, reject를 실행하게 되면서 Rejected상태가 됩니다.
즉, 비동기 처리가 실패하거나 오류가 발생한 상태입니다.
![](https://images.velog.io/images/blackdavil01/post/9446cfdc-870b-49b7-bba2-65256ce7a746/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202021-07-28%2022-16-37.png)

## Promise 후속 처리 메소드

1. catch(): reject()를 호출 했을 때 실패 이유를 catch()로 받습니다.
2. then(): resolve()를 성공적으로 호출 했을 때 then()을 이용해 결과 값을 받을 수 있습니다.
3. finally(): finally()는 수신 거부 여부에 관계없이 무조건 실행 됩니다.

```js
const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('success');
        // reject(new Error('no network'));
    }, 2000);
});

promise
    .then(value => {
        console.log(value); // 'success'
    })
    .catch(err => console.log(err))
    .finally(() => {
        console.log('finally');
    });
```

## Promise Chaining

비동기 함수의 처리 결과를 가지고 다른 비동기 함수를 호출해야 하는 경우, 함수의 호출이 중첩이 되어 복잡도가 높아지는 콜백 지옥이 발생한다. 프로미스는 후속 처리 메소드를 체이닝하여 여러 개의 프로미스를 연결하여 사용할 수 있다. 이로써 콜백 헬을 해결할 수 있습니다.

Promise 객체를 반환한 비동기 함수는 프로미스 후속 처리 메소드인 then이나 catch메소드를 사용할 수 있다. 따라서 then메소드가 Promise 객체를 반환하도록 하면**(then 메소드는 기본적으로 Promise를 반환한다.)** 여러 개의 프로미스를 연결하여 사용할 수 있습니다.

```js
const fetchNumber = new Promise((resolve, reject) => {
    setTimeout(() => resolve(1), 1000);
});

fetchNumber
    .then(num => num * 2)
    .then(num => num * 3)
    .then(num => {
        return new Promise((resolve, reject) => {
            setTimeout(() => resolve(num - 1), 1000);
        });
    })
    .then(num => console.log(num));
```

# async & await

## async란?

async는 function 앞에 위치합니다.
function앞에 async를 붙이면 **해당 함수는 항상 프로미스를 반환합니다**. 프로미스가 아닌 값을 반환하더라도 이행 상태의 프로미스로 값을 감싸 이행된 프로미스가 반환되도록 합니다.

```js
async function test() {
    return 1;
}
test().then(console.log); // 1
```

## await란?

await는 async 함수 안에서만 동작합니다.
자바스크립트는 await 키워드를 만나게 되면 프로미스가 처리될때까지 기다립니다. 결과는 그 이후에 반환됩니다.
프로미스가 처리되길 기다리는 동안엔 엔진이 다른 일(다른 스크립트를 실행, 이벤트 처리 등)을 할 수 있기 때문에 CPU리소스가 낭비되지 않습니다.

```js
async function test() {
let promise = new Promise((resolve,reject) => {
  setTimeout(() => resolve('완료'), 1000)
});

  let result = await promise;  // 여기서 1초를 기다리게된다.
  console.log(result) // 완료

  test();
```

## async & await를 사용하는 이유?

-   프로미스를 조금 더 간결하고 간편하고 동기적으로 실행되는 것처럼 보이게 만들어줍니다.
-   프로미스체이닝을 계속하게 되면 코드가 난잡해지기 때문에 이런거 위에 조금 더 간편한 api로 async와 await를 사용하면 동기식으로 코드를 작성하는것처럼 간편하게 작성할 수 있게 도와줍니다.

## try, catch

프로미스에서 에러 처리를 위해 `.catch()`를 사용했던 것처럼 async에서는 `try, catch`를 사용할 수 있습니다.

```js
function delay(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}

async function getApple() {
    await delay(3000);
    return '사과';
}

async function getBanana() {
    await delay(3000);
    return '바나나';
}

async function pickFruits() {
    try {
        const apple = await getApple();
        const banana = await getBanana();
    } catch (err) {
        console.log(err);
    }
}
```

## await 병렬처리

```js
function delay(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}

async function getApple() {
    await delay(3000);
    return '🍎';
}

async function getBanana() {
    await delay(3000);
    return '🍌';
}

async function pickFruits() {
    const apple = await getApple(); // 3초걸리고
    const banana = await getBanana(); // 3초 걸리고
    return `${apple} + ${banana}`; // 총 6초가 소모됨
}

pickFruits().then(console.log);
```

위의 코드에서 값을 받으려면 `await getApple()` 에서 3초가 걸리고 `await getBanana()`에서 3초가 걸리면서 총 6초가 소모되어 비효율적인 코드가됩니다. 이 코드를 개선하면 다음 코드처럼 나타낼 수 있습니다.

```js
async function pickFruits() {
    // 프로미스를 만들때 프로미스안에 있는 코드가 실행되게 됩니다.
    const applePromise = getApple();
    const bananaPromise = getBanana();
    const apple = await applePromise;
    const banana = await bananaPromise;
    return `${apple} + ${banana}`;
}
```

이렇게 실행하게 되면 3만에 결과값을 출력할 수 있습니다. 하지만 위의 코드를 Promise.all()함수를 사용하면 더 깔끔하게 작성할 수 있습니다.
**Promise.all()** 은 프로미스 배열을 전달하게 되면 모든 프로미스들이 병렬적으로 다 받을때까지 모아주는 함수입니다.
Promise.all의 첫 번째 프라미스는 가장 늦게 이행되더라도 처리 결과는 배열의 첫 번째 요소에 저장됩니다.
주어진 프로미스 중 하나가 거부하는 경우, 첫 번째로 거절한 프로미스의 이유를 사용해 자신도 거부합니다. Promise.all()을 사용하여 병렬처리를 하게 되면 다음 코드처럼 나타낼 수 있습니다.

```js
function pickAllFruits() {
    return Promise.all([getApple(), getBanana()]).then(fruits => fruits.join(' + '));
}
pickAllFruits().then(console.log);
```
