---
title: "코르도바 활용 블루투스 프로젝트 생성"
comments: true
categories:
  - iOS
tags:
  - xcode
  - cordova
  - ble
  - ios
date: "2019-08-23 14:06"
---

### 1. 코르도바 개발환경 설정

  Cordova 개발환경 설정시 기본적으로 필요한 프로그램 목록입니다.
  -[node.js 다운로드](https://nodejs.org/en/){:target="_blank"} node는 특별한 사유가없다면 LTS버전으로 설치합니다.
	-cordova
  npm 설치 후 터미널을 열고

  ```terminal
  sudo npm -g install cordova
  ```

  설치 완료 후, cordova -v를 통해 코르도바 버전을 확인합니다. 
  (8.1.2 버전 기준으로 작성하였으므로 구동되지 않을시 해당 버전의 코르도바로 설치합니다)
  그 후 ios 시뮬레이터를 호출할 수 있는 ios-sim을 설치합니다.

  ```terminal
  sudo npm -g install ios-sim
  ```

  이후 새로운 코르도바 프로젝트를 만들고자 할 경우 프로젝트를 생성하고자하는 경로에서 ios플랫폼이 있는 코르도바 프로젝트를 생성할 수 있습니다.

  ```terminal
  cordova create example com.example.test “TestExample”
  cd example
  cordova platform add ios
  cordova prepare  // or cordova build
  ```

  필요한 plugin의 설치 및 삭제는 다음 명령어로 가능합니다.
  ```terminal
  cordova plugin add (삭제는 remove) [Plugin Name]
  ```

### 2. WebView와 내부 메소드 연동방법 (javascript 호출)

  내부 plugines 폴더 내부에 new file을 통해 objectve-c 파일을 생성합니다.
  (Subclass of는 CDVPlugin 을 참조하도록 설정후 원하는 class name을 설정후 objectve-c를 선택합니다.)
  그렇다면 설정한 classname.h 와 classname.m 파일이 생성 될 것이며, 
  우선 h 파일을 열어 코드를 수정합니다. (MGPluginEcho 으로 생성됨)

  ```c
  #Import <Cordova/CDV.h>
  @Interface MGPluginEcho: CDVPlugin
  -(void)echo:(CDVInvokedUrlCommand *)command;
  @end
  ```

  다음은 m파일입니다.
  
  ```c
  #Import “MGPluginEcho.h”
  @Implementation MGPluginEcho
  -(void)echo: (CDVInvokedUrlCommand *)command {
    NSString* message = [command.arguments objectAtindex:0];
    NSLog(@”message : %@”, messaage);
    [[[UIAlertView alloc]
      InitWithTitle:@”네이티브팝업창”
      message:message
      delegate: nil
      CancelButtonTitle:@”취소”
      OtherButtonTitles:@”확인”,nil]show];
    }
  @End
  ```

  여기까지가 native에서 생성가능 한 코드이며, 이후 javascript에서 사용 가능하도록 연동해야합니다. 우선 config.xml 내부에 추가하여 연동하는 방법입니다.

  ```xml
  <feature name="MGPluginEcho">
	  <param name="ios-package" value="MGPluginEcho" />
  </feature>
  ```

  다음은 javascript 입니다.

  ```javascript
  function MGPluginEcho () {}
  MGPlugin.prototype.echo = function (msg) {
    Cordova.exec(null, null, 'MGPluginEcho', 'echo', [msg]);
  }
  var runEcho = function () {
    var pluginEcho = new MGPluginEcho();
    pluginEcho.echo('javascript/native 연동 html에서 보낸 내용 나타내기');
  }
  ```

### 3. 블루투스 연결 방법

  간편한 방법으로 cordova-plugin-ble-central 하여 사용할 수 있으며, 작동원리는 위의 Native / Javascript 연동과 같습니다. 터미널에서 설치시 plugin에 ble-cental-plugin.m /h 파일이 생성됩니다. 콜백함수는 다음과 같습니다.

  ```c
  - (void)scan:(CDVInvokedUrlCommand *)command;
  - (void)startScan:(CDVInvokedUrlCommand *)command;
  - (void)startScanWithOptions:(CDVInvokedUrlCommand *)command;
  - (void)stopScan:(CDVInvokedUrlCommand *)command;
  - (void)connect:(CDVInvokedUrlCommand *)command;
  - (void)disconnect:(CDVInvokedUrlCommand *)command;
  - (void)read:(CDVInvokedUrlCommand *)command;
  - (void)write:(CDVInvokedUrlCommand *)command;
  - (void)writeWithoutResponse:(CDVInvokedUrlCommand *)command;
  - (void)startNotification:(CDVInvokedUrlCommand *)command;
  - (void)stopNotification:(CDVInvokedUrlCommand *)command;
  - (void)isEnabled:(CDVInvokedUrlCommand *)command;
  - (void)isConnected:(CDVInvokedUrlCommand *)command;
  - (void)startStateNotifications:(CDVInvokedUrlCommand *)command;
  - (void)stopStateNotifications:(CDVInvokedUrlCommand *)command;
  - (void)onReset;
  - (void)readRSSI:(CDVInvokedUrlCommand *)command;
  ```

  좀더 자세한 사항은 <a href="https://github.com/don/cordova-plugin-ble-central" target="_blank">git-hub</a> 를 통해 확인 가능합니다.

### 4. iOS백그라운드 서비스

  iOS 백그라운드 서비스를 코르도바에서 사용하기위해 우선, 다음과 같은 플러그인을 설치합니다.
  ```terminal
  cordova plugin add cordova-custom-config
  ```

  ```xml
  <platform name="ios"> 
    <config-file parent="UIBackgroundModes" target="*-Info.plist"> 
      <array> <string>bluetooth-central</string> </array> 
    </config-file> 
  </platform>
  ```

  또한 info.plist 에서 UIBackgroundModes가 활성화후, ble 통신 및 backgoround fetch를 활성화 하여 백그라운드 서비스를 정상적으로 작동 할 수 있습니다.
  구동 원리의 코드는 appDelegate 이며 코드를 통해 background 패치를 초기화 시켜 주므로 백그라운드 상태에서도 계속 사용 가능해집니다.

  ```c
  - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
  {
    self.viewController = [[MainViewController alloc] init];
    return [super application:application didFinishLaunchingWithOptions:launchOptions];
  }
  ```

### 5. PUSH 개발 방법 (FireBase 연동)

  우선 cordova fcm 플러그인을 설치합니다.
  ```terminal
  cordova plugin add cordova-plugin-fcm
  ```

  그후 firebase 홈페이지에 접속하여 다음과 같이 진행합니다.

  ![img](\assets\images\ios\firebase-1.png)
  ![img](\assets\images\ios\firebase-2.png)
  ![img](\assets\images\ios\firebase-3.png)
  <!-- ![img](\assets\images\ios\2017-09-17_22-33-53-1024x531.png) -->
  여기서 주의해야 할 부분은, 번들 ID는 프로젝트에서 생성한 번들 ID와 같아야합니다.
  ![img](\assets\images\ios\firebase-4.png)
  GoogleService-info.plist 파일을 다운로드 받은 후 프로젝트 내부 폴더로 옮겨줍니다.
  ![img](\assets\images\ios\firebase-5.png)
  <!-- ![img](\assets\images\ios\2017-09-17_22-35-31.png) -->
  전부 설정 후html에서 토큰 설정후 사용할 수 있습니다. (사진과 다르게 현재 프로젝트에서는 resources 폴더 내부에 저장하여 사용중입니다.) 또한 Push 사용을 위해 인증서 업로드가 필요한데 애플 개발자 사이트에서 인증서를 받은후 FCM에 업로드 하여 등록 가능합니다.
  ![img](\assets\images\ios\1_DXho7RVN7BVRBHnFY2GQDQ.png)
  ![img](\assets\images\ios\1_4z5mGIVWrAKLlS071Gxafw.png)
  ![img](\assets\images\ios\dev-ios-45.png)
  그후 네이티브 소스에서 해당 소스를 통해 기본 설정을 할 수 있다고 되어있으나 현재 프로젝트에서는 cordova-fcm 플러그인을 통해 사용중이며 javascript에서 호출되어 사용되고 있습니다. (plugin/FCMPlugin.m)
  그 후 Cloud Messaging을 통해 메시지를 전송할 수 있습니다 (특정 토큰 선택 또는 전체 가능)
  ![img](\assets\images\ios\dev-ios-63.png)

### 6. Splash 및 App icon 설정

  Images.xcassets 폴더 내부의 
  LaunchImage.launchimage 폴더 내부의 이미지 파일들이 Splash 이미지이며, AppIcon.appiconset 폴더의 내부 파일들이 App icon 이미지입니다. 이미지 파일 수정시 Splash 및 App icon 설정이 가능합니다. 또는 xcode 실행시 Resource 폴더 내부의 images.xcassets 내부로 접근시 해당 이미지를 좀더 편하게 수정 할 수 있도록 그림과 같은 화면이 출력됩니다.
  ![img](\assets\images\ios\2019-06-21-5.35.08.png)

### 7. 앱에서 필요한 퍼미션 권한 허용, 거부 출력

  ![img](\assets\images\ios\2019-06-21-5.37.12.png)

  대부분 권한은 이곳에서 설정 가능하며, 오류 발생시 resources 폴더 내부의 plist 파일의 key와 type value가 정상적으로 입력되었는지 확인합니다.
  
  퍼미션 종류는 다음 그림과 같습니다.

  ![img](\assets\images\ios\info.png)

  해당 퍼미션을 info.plist에 추가하면 퍼미션 확인 메시지를 출력할수 있습니다

  ![img](\assets\images\ios\onfo2.png)

이상으로 iOS - Cordova를 활용한 BLE 프로젝트 생성 및 필요한 환경 설정 세팅에 대해 포스트 했습니다.