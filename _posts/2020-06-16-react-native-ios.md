---

title: "Mac 에서 React Native 환경설정"
comments: true
categories:
  - react-native
  - ios
tags:
  - react-native
  - ios
date: "2020-06-16 18:12"
---

# Mac 에서 React Native 개발 환경 세팅


## Homebrew 설치


※ Homebrew
[링크](https://brew.sh/)

Homebrew는 맥(Mac)에서 패키지를 설치하고 관리하는 맥(Mac) 패키지 관리자입니다.

우선 아래 명령어를 통해 맥(Mac)에 Homebrew가 설치되었는지 확인해봅시다.

```terminal
brew --version
# 만약 설치가 되었다면, 아래와 같이 버전을 확인 할 수 있습니다.
homebrew x.x.x
homebrew/homebrew-core (git revision xxxx; last commit 2020-xx-xx)
```

만약, 버전이 표시되지 않는다면 다음과 같이 명령어를 통해 Homebrew를 설치 합니다.

```terminal
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

설치가 정상적으로 완료되었다면, 이전에 실행한 버전확인 명령어를 통해 버전을 확인할 수 있습니다.


## Nodejs 설치


※ Nodejs
[링크](https://nodejs.org/)

React native는 Javascript이므로, Javascript의 런타임인 Nodejs가 필요합니다.

이전에 설치한 Homebrew 명령어를 통해 Nodejs를 설치합시다.

```terminal
brew install node
```

Nodejs를 설치하게되면, 기본적으로 Nodejs의 패키지 매니저인 NPM(Node Package Manager)도 같이 설치됩니다.

설치가 완료되었다면 다음의 명령어를 통해 버전을 확인 할 수 있습니다.

```terminal
node -version

vx.x.x

npm --version
x.x.x
```


## Watchman 설치


※ Watchman
[링크](https://facebook.github.io/watchman/)

Watchman은 특정 폴더나 파일을 감시하다가 변화가 생기면, 특정 동작을 실행하도록 설정하는 역할을 합니다. 

react-native에서는 소스코드의 추가, 변경이 발생하면 다시 빌드하기 위해 Watchman을 사용하고 있습니다.

Watchman을 설치 하기 위하여 아래의 Homebrew 명령어를 통해 설치합니다.

```terminal
brew install watchman
```

설치가 정상적으로 완료되었다면, 다음의 명령어를 통해 버전을 확인할 수 있습니다.


```terminal
watchman –version
```


## React Native CLI 설치


※ React Native CLI
[링크](https://facebook.github.io/watchman/)

react-native로 앱을 개발하기 위해 필요한 React Native CLI를 설치합시다.

아래의 npm 명령어를 통해 React Native CLI를 설치 할 수 있습니다.

```terminal
npm install -g react-native-cli
```

설치가 정상적으로 완료되었다면, 다음의 명령어를 통해 버전을 확인할 수 있습니다.


```terminal
react-native --version
```


## XCode 설치


※ Xcode
[다운로드 링크](https://apps.apple.com/us/app/xcode/id497799835?mt=12)

iOS 개발을 하기 위해서는 Xcode가 필요합니다.

위의 다운로드 링크를 통해 Xcode를 다운로드 합니다.

그 후 상단 메뉴에서 Xcode > Preferences... > Locations로 이동하여 아래와 같이 Command Line Tools가 잘 설정되었는지 확인합니다.

![img](\assets\images\react-native-ios\r1.png)

만약 사진과 같이 설정되어 있지 않다면,

가장 최신의 Command Line Tools을 선택해 줍시다.



## Cocoapods 설치


※ Cocoapods
[링크](https://cocoapods.org/)

Cocoapods는 iOS 개발에 사용되는 의존성 관리자입니다.

react-native로 iOS 앱을 개발하려면 꼭 필요하므로 아래에 명령어를 사용하여 Cocoapods를 설치합니다.

```terminal
sudo gem install cocoapods
```

설치가 완료되면, 다음 명령어를 통해 정상적으로 되었는지 확인합니다.

```terminal
pod --version
```


## React Native 프로젝트 생성 및 확인


react-native는 버전에 따라 잘 동작하던 앱이 에러를 발생하는 등 문제를 일으킬 가능성이 높습니다.

따라서 아래 npm 명령어를 통해 버전을 고정 시켜 사용하는 것을 추천합니다.


```terminal
npm config set save-exact=true
```

이제 React Native CLI 명령어를 통해 프로젝트를 생성합니다

```terminal
# AppName 에 원하는 이름을 넣어 프로젝트를 생성 할 수 있습니다.
react-native init AppName
```

생성이 완료되면 아래의 명령어를 통해 구동 할 수 있습니다

```terminal
cd AppName

npm run ios
```

실행이 잘 되지 않는 경우, ios/AppName.xcworkspace 파일을 실행하고 

왼쪽 상단의 시뮬레이터를 설정하고 화살표 버튼을 눌러 시뮬레이터를 실행합니다.

잘 실행이 되었다면, 아래와 같은 화면을 확인할 수 있습니다.

![img](\assets\images\react-native-ios\r2.png)
![img](\assets\images\react-native-ios\r3.png)