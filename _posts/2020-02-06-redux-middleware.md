---
title: "Redux-middleware"
comments: true
categories:
  - react
tags:
  - javascript
  - react
  - node
  - css
  - redux
date: "2020-02-06 20:35"
---

### 1. Redux-middleware 란?

  리덕스 미들웨어(middleware)는 액션을 디스패치했을 때 리듀서에서 이를 처리하기 전에 
  사전에 지정된 작업들을 실행한다. 미들웨어는 액션과 리듀서 사이의 중간자라고 볼 수 있다.

  ![img](\assets\images\react\middleware.png)


### 2. Redux-middleware를 사용하는 이유

  미들웨어는 객체 대신 함수를 생성하는 액션 생성함수를 작성할 수 있도록 도와주는 것이다.
  일반 액셩생성자는 파라미터를 가지고 액션 객체를 생성하는 작업만을 한다.

  ```java
   const actionCreator = (parms) => ({action: 'ACTION', parms});
  ```

  만약 특정 액션을 현재 상태에 따라 무시하도록 만드려면 일반 액션생성자로는 할수 없다.
  미들웨어는 이를 가능하게 한다.

  ```java
  const INCREMENT_COUNTER = 'INCREMENT_COUNTER';

  function increment() {
    return {
      type: INCREMENT_COUNTER
    };
  }

  function incrementIfOdd() {
    return (dispatch, getState) => {
      const {counter} = getstate();

      if(counter % 2 === 0) {
        return;
      }

      dispatch(increment());
    };
  }
  ```

  액션이 리듀서로 이동되는 그 사이를 커스텀 할수 있는 것이다.
  리듀서가 액션을 처리하기 전에 미들웨어가 전달받은 액션을 콘솔에 기록하거나, 액션을 취소하거나,
  다른 종류의 액션을 추가로 디스패치 할 수 있다.
  

### 3. 미들웨어 만들기

  로거 미들웨어 만들기

  src/lib/loggerMiddleware.js
  ```java
  const loggerMiddleware = store => next => action => {
      /* 미들웨어 내용 */
  }
  ```
  여기서 next는 store.dispatch 와 비슷한 역할을 한다. 차이점은 next(action) 을 했을 때 바로 리듀서로 넘기거나,
  혹은 미들웨어가 더 있다면 다음 미들웨어 처리가 되도록 진행한다는 것이다.
  store.dispatch 의 경우에는 처음부터 다시 액션이 디스패치 되는 것이기 때문에 현재 동작한 미들웨어를 다시 한번 처리하게 된다.


### 4. 미들웨어 적용하기

  미들웨어는 store 를 생성 할 때 설정한다. redux 모듈 안에 들어있는 applyMiddleware 를 사용하여 설정한다.

  src/store.js
  ```java
  import { createStore, applyMiddleware } from 'redux';
  import modules from './modules';
  import loggerMiddleware from './lib/loggerMiddleware';

  // 미들웨어가 여러개인경우에는 파라미터로 여러개를 전달해주면 됩니다. 예: applyMiddleware(a,b,c)
  // 미들웨어의 순서는 여기서 전달한 파라미터의 순서대로 지정됩니다.
  const store = createStore(modules, applyMiddleware(loggerMiddleware))

  export default store;
  ```

### 5. 비동기작업 처리

  ※ Promise 란?
  Promise는 비동기 처리를 다루기 위해 사용되는 객체이다.

  비동기적으로 해야 할 작업이 많아져 코드의 구조는 자연스럽게 깊어지다보면 코드를 읽기 힘들어질 것이다.
  기존의 자바스크립트 콜백 지옥 문재에서 구제해주는것이 바로 Promise 이다.

  ```java
  function printLater(number) {
    return new Promise( // 새 Promise 를 만들어서 리턴함
        (resolve, reject) => { // resolve 와 reject 를 파라미터로 받습니다
            setTimeout( // 1초뒤 실행하도록 설정
                () => {
                    if(number > 5) { return reject('number is greater than 5'); } // reject 는 에러를 발생시킵니다
                    resolve(number+1); // 현재 숫자에 1을 더한 값을 반환합니다
                    console.log(number);
                },
                1000
            )
        }
    )
  }

  printLater(1)
  .then(num => printLater(num))
  .then(num => printLater(num))
  .then(num => printLater(num))
  .then(num => printLater(num))
  .then(num => printLater(num))
  .then(num => printLater(num))
  .then(num => printLater(num))
  .catch(e => console.log(e));
  ```

  몇번을 하던 코드의 깊이는 일정하다.
  Promise 에서는 값을 리턴하거나, 에러를 발생 시킬 수도 있다.

  결과
  ```java
  1
  2
  3
  4
  5
  number is greater than 5
  ```

  Promise를 사용하면 콜백지옥에 빠질일이 없어진다.
