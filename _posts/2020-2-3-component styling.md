---
title: "안드로이드 BLE 통신"
comments: true
categories:
  - react
tags:
  - javascript
  - react
  - node
  - css
date: "2020-02-04 17:35"
---

### 1. CSS

  CSS는 HTML로 표현한 정보를 시각적으로 어떻게 보여지도록 하는지 기술하는 언어이다.
  이는 정보와 디자인을 분리하여 관리하기 때문에 매우 유용하다.
  그러나 프로젝트의 규모가 커진다면 일반적인 CSS로는 클래스명이 중첩될 가능성이 있다는 단점이 있다.

  이번 글에서는 CSS의 다른 솔루션을 알아본다.

  우선 CSS와 관련한 설정정보를 변경해 주어야한다 이는 프로젝트 생성하면 볼수 없도록 내부에 들어있다.
  프로젝트를 생성 후 콘솔에서 명령어를 입력하여 밖으로 빼주어야 한다.
    
    ◎ yarn eject 또는 npm eject
    
  적용이 완료되면 Webpack 설정을 변경해야 한다.
  
  프로젝트 디렉토리 내에서 아래 파일을 열어서 'css-loader'를 찾아서 설정을 바꾸어 준다.

  ```java
  'config/webpack.config.dev.js'

  {
    loader: require.resolve('css-lader'),
    options: {
      importLoaders: 1,
      modules: true,
      localIdentName: '[name]__[local]___[hash:base64:5]'
    },
  },
  ```

  ```java
  'config/webpack.config.prod.js'

  {
    loader: require.resolve('css-lader'),
    options: {
      importLoaders: 1,
      modules: true,
      localIdentName: '[name]__[local]___[hash:base64:5]',
      minimize: true,
      sourceMap: true,
    },
  },
  ```
  
### 2. CSS Module

  설정을 변경 후 yarn start 을 입력하면 이제 일반 CSS가 적용되지 않는다.
  
  CSS Module을 사용하면 CSS 파일을 불러온 컴포넌트 내부에서만 작동한다.
  적용할 파일에 CSS 파일을 불러올때 래퍼런스 안에 넣어주어 모듈처럼 사용하는 것이다.

  ```java
  'App.css'

  .excemple_bluebox {
    width: 100px;
    height: 100px;
    background-color: blue;
  }

  ```

  ```java
  import style from './App.css';

  class App extends Component {
    render() {
      return (
        <div className={style.excemple_bluebox}></div>
      );
    }
  }

  export default App;
  ```

※ CSS Module 의 사용법
```java
'글로벌 스타일 사용'
:global .excemple_bluebox {
  width: 100px;
  height: 100px;
  background-color: blue;
}

'여러 개의 클래스를 적용해야 할 때'
<div className={`${style.excemple_bluebox} ${style.excemple_redbox}`}></div>

'특정 css를 상속할 때'
.excemple_bluebox {
  composes: redbox;
  background-color: blue;
}

'다른 클래스에서 가져와야 할 때'
.excemple_bluebox {
  composes: className from "./style.css";
  background-color: blue;
}
```

### 3. CSS 전처리기

  CSS 전처리기는 CSS 문서의 작성에 도움을 주는 도구이다
  CSS를 작성할 때 반복적인 작업이 요구되고 클래스의 상속과 같은 사항으로
  문서는 양이 많아지고 이로 인해 유지관리에 많은 영향을 준다

  CSS 전처리기는 이런 문제점들을 변수, 함수, 상속 등 프로그래밍 개념을 사용해 해결한다.

  가장 많이 사용되는 전처리기는 SASS, LESS, Stylus 가 있으며, 서로의 특징에 맞게
  약간의 구문만 다를 뿐 개념 자체는 동일하다.

  CSS 전처리기의 장점

    1. 재사용성 - 공통 요소 또는 반복적인 항목을 변수 또는 함수로 대체
    2. 유지관리 - 중첩, 상속과 같은 요소로 인해 구조화된 코드로 유지 및 관리가 용이

  CSS 전처리기의 단점

    '실무에서의 사용'이다. 실무에서 대부분 CSS 작업은 퍼블리셔(Publisher) 또는 디자이너(Designer)
    또는 Fropnt-End 개발자가 진행하는 경우도 있는데 문제는 이들이 개발에 대한 역량과 요소를 알아야 한다는 것이다.
    CSS 전처리기는 프로그래밍 한 요소를 접목했기 때문에 분기문 처리, 변수의 이해, CSS함수의 이해 등
    개발적인 요소를 알아야 하기 때문이다.

  CSS 전처리기의 기능을 간단하게 알아보자.

  ex 1)
  우리가 특정 클래스에 color 속성을 주고 값을 지정할 때 hex code 또는 RGB 로 지정한다.
  그런데 특정 색상이 반복된다면 우리는 주기적으로 색상 코드를 찾고 반영을 해줘야 한다.
  하지만 CSS 전처리기에서는 이런 반복되는 부분을 변수로 처리할 수 있다.
  대부분 하나의 파일 안에 color-set 을 모두 정의하여 사용된다.

  ```java
  $primary-color: #fff

  .class-A {
    background-color: $primary-color
  }

  .class-B {
    background-color: $primary-color
  }

  ```
  ex 2)
  각 CSS 전처리기에는 내장함수(Built-in Functions)가 존재한다.
  이 내장 함수는 이미 전처리기 내에 포함된 함수로 우리는 필요한 값들만 전달하면
  이에 대한 결과를 얻을 수 있다. 예를 들어 darken(color, amount) 라는 내장 함수는
  색상과 퍼센티지를 지정해 주면 거기에 알맞는 값을 출력해 준다.

  ```java
  $color: #2ecc71
  $buttonDark: darken($buttonColor, 10%)

  .button {
    background-color: $color;
    box-shadow: 0px 5px 0px $buttonDark
  }
  ```

  ex 3)
  유지 관리는 가장 중요하고 눈에 띄는 장점 중 하나이다. 클래스를 정의하다 보면
  상속과 공통속성을 지정하기 위해 굉장히 길게 정의해야 할때가 있다.
  이런 이유로 CSS 문서가 가독성이 매우 떨어지는데 CSS 전처리기에서는
  중첩(Nesting) 과 상속(Extend) 을 통해 구조화 할 수 있다.

  ```java
  .class-A {
    width: 100%;

    .A-child-1 {
      color: red;
    }

    .A-child-2 {
      color: blue;
    }
  }

  .class-A {
      color: red
      padding: 10px
  }

  .class-B {
      @extend: .class-A
      border: 1px solid red
  }

  ```
  위 내용을 컴파일하면 아래와 같다.
  ```java
  .class-A {
    width:100%;
  }

  .class-A .A-child-1 {
    color: red;
  }

  .class-A .A-child-2 {
    color: blue;
  }

  .class-A, .class-B {
      color: red
      padding: 10px
  }

  .class-B {
      border: 1px solid red
  }
  ```
  
  이처럼 CSS 전처리기를 사용하면 많은 도움이 된다.
  하지만 그 전처리기를 사용하는 실무자가 개발자가 아닐수 있다.

  이상 CSS에 대해 알아보았다.