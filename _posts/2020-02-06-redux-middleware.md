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
  

