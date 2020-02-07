---
title: "React Router, Code Splitting, Server Side Rendering"
comments: true
categories:
  - react
tags:
  - javascript
  - react
  - node
  - redux
  - router
  - code splitting
  - server side rendering
date: "2020-02-06 23:35"
---

### 1. React-router

a. React-router 란?

공식은 아니지만 가장 많이 사용되고 있는 라이브러리이다.
이 라이브러리는 클라이언트 사이드에서 이루어지는 라우팅을 간단하게 해주며,
화면이 여러개인 웹 어플리케이션을 만든다면 React-router는 필수 라이브러리이다.

전통적인 웹어플리케이션의 구조는, 여러 페이지로 구성되어 있다.
유저가 요청할 때마다 페이지가 새로고침되며, 페이지를 로딩 할 때마다 서버로부터
리소스를 전달받아 해석 후 렌더링을 한다.

요즘은 웹에서 제공되는 정보가 정말 많기 때문에 속도적인 측면에서 문제가 있었고,
이를 해소하기 위해 SPA(Single Page Application) 방식이 생겨났다.
리액트 같은 라이브러리 혹은 프레임워크를 사용해서 뷰 렌더링을 유저의 브라우저가
담당하도록 하고, 어플리케이션을 브라우저에 로드 한 다음에 정말 필요한 데이터만
전달받아 보여주는 것이다.

※ '웹페이지 구동방식에 대한 내용은 하단에 후술하겠다.'


하지만 싱글페이지앱이라고 해서 화면의 종류가 하나인 것은 아니다.

각 화면 페이지는 저마다 주소가 있고, 주소에 따라 다른 뷰를 보여주는것을
바로 라우팅이라고 한다.
리액트 자체에는 이 기능이 내장되어있지 않기 때문에 React-router를 사용하는 것이다.


b. React-router 사용하기

기본적인 라우터를 만들어보자

아무런 path가 주어지지 않았을 때 기본적으로 표시되는 라우트이다.

src/pages/Home.js
```java
import React from 'react';

const Home = () => {
    return (
        <div>
            <h2>
                홈
            </h2>
        </div>
    );
};

export default Home;
```

특정 경로로 들어왔을 때 표시할 페이지도 같은 형식으로 만들어준다. (ex - /about 경로)

src/pages/About.js
```java
import React from 'react';

const About = () => {
    return (
        <div>
            <h2>About</h2>
        </div>
    );
};

export default About;

```

다음 이 컴포넌트를 불러와서 파일로 보낼 수 있도록 인덱스를 만들어준다.

src/pages/index.js
```java
export { default as Home } from './Home';
export { default as About } from './About';
```

이제 라우트에 맞춰서 컴포넌트를 구분할 수 있도록 App 컴포넌트를 작성한다.

src/App.js
```java
import React, { Component } from 'react';
import { Route } from 'react-router-dom';
import { Home, About } from 'pages';

class App extends Component {
    render() {
        return (
            <div>
                <Route exact path="/" component={Home}/>
                <Route path="/about" component={About}/>
            </div>
        );
    }
}

export default App;
```

라우트를 설정 할 때에는 Route 컴포넌트를 사용하고, 경로는 path 값으로 설정한다.

첫번째 라우트 / 는 Home 컴포넌트를, 두번째 라우트 /about 는 About 컴포넌트가 보여진다.



### 2. 코드 스플리팅(Code Splitting)

앞서 언급한 싱글페이지 어플리케이션(SPA)의 단점은 앱을 처음 시작할 때 자바스크립트
번들 파일에 어플리케이션에 대한 모든 로직을 불러오는 방식이기 때문에 규모가 커지면
용량이 커져서 초기 로딩속도가 지연된다는 것이다.

이에 대한 솔루션이 바로 필요에 따라 자바스크립트 번들 파일을 여러개의 파일로 분리시키는
코드 스플리팅이다.

간단히 말해서 한개의 파일에서 처음부터 모든 것을 불러오는 것이 아니라, 설정해 놓은대로
라이브러리나 컴포넌트가 실제로 필요해질 때 불러오게하여 페이지 로딩 속도를 개선하는 것이다.


a. 기본적인 코드 스플리팅

컴포넌트를 스플리팅 하는 간단한 예제이다.

src/SplitMe.js
```java
import React from 'react';

const SplitMe = () => {
  return <div>필요할때만 불러</div>;
};

export default SplitMe;
```

src/App.js
```java
import React, { Component } from 'react';

class App extends Component {
  state = {
    SplitMe: null
  };
  handleClick = () => {
    import('./SplitMe').then(({ default: SplitMe }) => {
      this.setState({
        SplitMe
      });
    });
  };
  render() {
    const { SplitMe } = this.state;
    return (
      <div>
        <button onClick={this.handleClick}>Click Me</button>
        {SplitMe && <SplitMe />}
      </div>
    );
  }
}

export default App;
```

App 컴포넌트에 버튼 하나를 만들고 위의 컴포넌트를 호출하도록 해보자
버튼을 누르면 비동기적으로 SplitMe를 불러와서 state 에 담는다
그리고 render 함수에서는 state 안에 있는 SplitMe 가 유효 할 때만 렌더링을 해준다.
사실 저 버튼을 클릭하기 전까지 필요가 없는 코드이므로 필요할 때
불러와서 사용할 수 있게 되는것이다.

※ '이렇게 분리된 파일을 청크 파일이라고 부른다.'



b. 리액트 라우터와 함께 사용하기

```java
'src/pages/About.js'
import React from 'react';

const About = () => {
  return <div>About</div>;
};

export default About;


'src/pages/Home.js'
import React from 'react';

const Home = () => {
  return <div>Home</div>;
};

export default Home;
```

index.js 파일을 pages 디렉토리에 만들어준다.
이 index.js 파일에서 Abuot을 withSplitting 으로 감싸서 다시 보낸다.

pages/index.js
```java
import withSplitting from '../withSplitting';

export const Home = withSplitting(() => import('./Home'));
export const About = withSplitting(() => import('./About'));
```

이제 App.js 에서 방금 내보낸 페이지 컴포넌트들을 불러와서 라우터 설정을 한다.

src/App.js
```java
import React, { Component } from 'react';
import { Route, Link } from 'react-router-dom';
import { About, Home } from './pages';

class App extends Component {
  render() {
    return (
      <div>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/about">About</Link>
          </li>
        </ul>
        <hr />
        <Route exact path="/" component={Home} />
        <Route path="/about" component={About} />
      </div>
    );
  }
}

export default App;
```

브라우저에서 페이지를 이동해보면 이동할 때 파일을 불러오는것을 볼수 있다.
처음부터 파일을 불러오지 않고, 해당 파일이 필요할 때, 비동기적으로 불러와
사용하는 것이 바로 '코드 스플리팅'이다.


### 3. 서버 사이드 렌더링

a. 렌더링이란?

어떠한 웹 페이지 접속 시, 그 페이지를 화면에 그려주는 것을 렌더링이라 한다.

위에서도 언급한 것처럼 전통적인 방식에서의 웹페이지 구동 방식은
요청 시마다 새로고침이 일어나며 서버에 새로운 페이지에 대한 요청을 하는 방식이다.
이때, View 가 어떻게 보여질지 또한 서버에서 해석하여 보내는데
이러한 방식을 서버 사이드 렌더링이라고 한다.

이후 기술의 발전으로 웹에서 제공되는 정보량이 많아지고, 속도 등에서 문제점이 발견되면서
서버 사이드 렌더링과 다른 SPA(Single Page Application) 기법이 등장하였다.

말 그대로 하나의 페이지만 서버에서 제공하고, View 에 대해서는 클라이언트에서
자바스크립트를 통해 렌더링 하는 방식이다
이를 클라이언트 사이드 렌더링이라고 한다.

간단하게 말해서 보여지는 페이지를 그려주는 주체가 서버인지 혹은 클라이언트 인지에 따라
방식이 나뉘는 것이다.


b. 서버 사이드 렌더링과 클라이언트 사이드 렌더링 방식의 차이점

서버 사이드 렌더링의 경우 View를 서버에서 이미 렌더링하여 가져오기 때문에
첫 로딩 속도가 매우 빠르다. 또한 서버따로, 클라이언트 따로 작성하던 코드가 하나로 합쳐져
일관성 있는 코드를 작성할 수 있다.
하지만 View 가 바뀌게 되면 또다시 서버에 요청을 해야 한다는 단점이 있다.

클라이언트 사이드 렌더링의 경우 페이지를 읽어들이는 시간, 자바스크립트를 읽어들이는 시간,
자바스크립트가 화면을 그리는 시간까지 모두 마쳐야 콘텐츠가 사용자에게 보여진다.
여기에 웹 서버에서 데이터라도 가져와야 한다면 그 시간은 더욱 길어진다.
즉 초기 구동 속도가 느리다는 단점이 존재한다. 하지만 초기 로딩 후에는 서버에 다시 요청할
필요없이 클라이언트 내에서 작업이 이루어지므로 더 빠르다.


둘 다 완벽하지 않고 장단점이 존재한다 제공할 서비스가 어느 부분에 초점을 맞춰서
진행되느냐에 따라 어떤 방식을 사용해야 할지를 결정해야 할 것이다.
최근에는 이 두가지 방법을 적절하게 융합한 방법들도 나오는 것으로 보인다.


