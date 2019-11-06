---
title: "Google Map API React"
comments: true
categories:
  - react
tags:
  - javascript
  - react
  - node
  - google
  - map
date: "2019-11-06 17:25"
---

### 1. 시작하기

  이번엔 React를 활용하여 Google Map Api를 사용하는 방법을 

  작성 하겠습니다.

  먼저 API를 사용하기 위해 KEY값이 필요하므로 

  [Google Maps Plat form](https://cloud.google.com/maps-platform/?hl=ko&utm_source=google&utm_medium=cpc&utm_campaign=FY18-Q2-global-demandgen-paidsearchonnetworkhouseads-cs-maps_contactsal_saf&utm_content=text-ad-none-none-DEV_c-CRE_267279866547-ADGP_Hybrid+%7C+AW+SEM+%7C+BKWS+~+%5B1:1%5D+%7C+KR+%7C+KO+%7C+BK+%7C+EXA+%7C+Google+Maps+Api-KWID_43700014363279245-kwd-335425467-userloc_1009846&utm_term=KW_google%20maps%20api-ST_google+maps+api&gclid=Cj0KCQiA-4nuBRCnARIsAHwyuPpvRXFCGZ_ipmxPbCFooAm3KI-YSkiBYlPz32VXkjdfN4cQE9eNTfwaAvn2EALw_wcB&authuser=2){:target="_blank"} 사이트에 접속하여 KEY값을 생성합니다.
  

  사이트에 접속 하여 다음과 같이 진행하여 키를 생성합니다.

  (설정이 따로 되어있지않으면, 결제 페이지가 나옵니다)
  ![img](\assets\images\google-maps-react\2019-11-06 173724.png)

  ![img](\assets\images\google-maps-react\2019-11-06 173728.png)

  ![img](\assets\images\google-maps-react\2019-11-06 173729.png)

  ![img](\assets\images\google-maps-react\2019-11-06 173739.png)

  ![img](\assets\images\google-maps-react\2019-11-06 174526.png)

  그 후 터미널에 다음과 같이 입력하여 

  React로 프로젝트를 생성후 Google Map API를 설치합니다.
  ```terminal
  npm init react-app AppName
  npm install --save google-maps-react
  yarn install
  ```

  그 후 컨테이너에서 google-maps-react를 import합니다.
  ```javascript
  import { Map, GoogleApiWrapper, Marker } from 'google-maps-react';
  ```

  렌더링할 내용은 다음과 같습니다.
  ```javascript
  render() {
    const mapStyles = {
      width: '100%',
      height: '100%',
    };
    return (
      <Fragment>
        <Map
          google={this.props.google}
          zoom={18}
          style={mapStyles}
          initialCenter={{lat: 37.44028337653064, lng: 126.67026155694566}}
        >
        </Map>
      </Fragment>
    );
  }
  ```

  또한 API를 사용하기 위한 KEY값을 설정하기 위해 
  
  export에 받아온 값을 저장합니다.
  ```javascript
  export default GoogleApiWrapper({
    apiKey: 'YOUR GOOGLE MAPS API KEY 입력'
  })(YourCntName);
  ```

### 2. 사용 방법
  1. HandlerEvnet

  기본적으로 핸들러들의 형태는

  on(HandlerEventType) 의 형태로 되어있으며,

  다음과 같이 이벤트를 추가 할수 있습니다.
  ```javascript
  <Map
    google={this.props.google}
    onClick ={this.onEventChecker}
  />
  ```

  파라미터는 총 3개입니다.

  e는 html element

  aug는 Map에 설정된 데이터 값

  geo는 좌표값을 받을수 있는 함수를 넘겨줍니다.

  ```javascript
  onEventChecker = (e, aug, geo) => {
    console.log(e);
    console.log(aug);
    console.log(geo);
  }
  ```

  이러한 핸들러 이벤트를 활용하여 
  
  지도에 마커를 추가하는 함수를 생성해 봅시다.

  ```javascript
  addMarkers = async (e, aug, geoData) => {
    // console.log(aug);
    const {stores} = this.state;
    let stateData = stores;
    let latLng;
    latLng = {latitude : geoData.latLng.lat(), longitude : geoData.latLng.lng()};
    stateData.push(latLng);
    await this.setState({
      stores: stateData
    });
  }
  ```

  그리고 배열을 그려주는 함수를 생성하여

  state의 배열이 추가될때마다 마커를 추가합니다.

  ```javascript
  displayMarkers = () => {
    return this.state.stores.map((store, index) => {
      return <Marker key={index} id={index} position={{
       lat: store.latitude,
       lng: store.longitude
      }} label={store.title}
     onClick={() => this.removeMarkers(index)} />
    })
  }
  ```
  ```javascript
  render() {
    const mapStyles = {
      width: '100%',
      height: '100%',
    };
    return (
      <Fragment>
        <Map
          google={this.props.google}
          zoom={18}
          style={mapStyles}
          initialCenter={{lat: 37.44028337653064, lng: 126.67026155694566}}
          onClick={this.addMarkers}
        >
          {this.displayMarkers()}
        </Map>
      </Fragment>
    );
  }
  ```

  마커를 그려주는 함수를 잘 보시면,
  
  마커를 클릭시 해당 마커를 지울수 있도록 생성된 걸

  확인 할 수 있습니다.

  해당 함수의 코드는 다음과 같습니다.

  ```javascript
  removeMarkers = async (i) => {
    const {stores} = this.state;
    let stateData = stores;
    stateData.splice(i,1);
    await this.setState({
      stores: stateData
    })
  }
  ```

  여기까지 작성한 예제 코드를 실행 한 후

  화면은 아래와 같습니다.
  ![img](\assets\images\google-maps-react\img1.png)

  해당 화면에서 클릭 이벤트를 실행시..
  ![img](\assets\images\google-maps-react\img2.png)

  ![img](\assets\images\google-maps-react\img3.png)

  2. 주소 검색

  먼저 검색을 할 input과 button을 렌더링합니다.
  ```javascript
  주소/건물 : <input value={this.state.searchText} onChange={(e) => this.onChangeInput(e)} />
  <button onClick={()=>this.onSearchEvent()}>검색</button>
  ```
  그 후 state에 검색할 데이터를

  가지고 있는 searchText를 만들어주며, 
  
  검색 함수를 생성합니다.
  ```javascript
  onChangeInput = (e) => {
    this.setState({
      searchText: e.target.value
    })
  }

  onSearchEvent = () => {
    const {google} = this.props;
    const {searchText} = this.state
    let geocoder = new google.maps.Geocoder();
    geocoder.geocode({
      address: searchText,
      region: 'ko'
    }, function (results, status) {
      if (status === google.maps.GeocoderStatus.OK) {
        console.log("RESULT OK::통신 성공");
        console.log(results);        
      } else if (status === google.maps.GeocoderStatus.ERROR) {
        console.log("Error::server 통신 에러");
      } else if (status === google.maps.GeocoderStatus.INVALID_REQUEST) {
        console.log("Invalid request::검색어없음");
      } else if (status === google.maps.GeocoderStatus.OVER_QUERY_LIMIT) {
        console.log("Over query limit::너무 잦은 통신");
      } else if (status === google.maps.GeocoderStatus.REQUEST_DENIED) {
        console.log("Geo code denied::geoCode 이용불가 API KEY 확인");
      } else if (status === google.maps.GeocoderStatus.UNKNOWN_ERROR) {
        console.log("Unknown error::알수없는 에러 발생");
      } else if (status === google.maps.GeocoderStatus.ZERO_RESULTS) {
        console.log("Zero result::찾는 데이터와 일치하는 데이터 존재하지않음.");
      } else {
        console.log("unknown status error..");
      }
    })
  }
  ```

  다음은 해당 함수를 통해

  부평구를 검색했을때 콜백받은 result의 값입니다.
  ![img](\assets\images\google-maps-react\img4.png)

  formatted_address 값이 주소명이며,

  좌표를 가져올 수 있는 함수 또한 포함 되어 있음을

  확인 할 수 있습니다.

  3. InfoWindow

  좌표에 설명하기 위한 라벨이아닌,

  메모같은 정보창을 표시할 수 있는

  InfoWindow가 존재합니다.

  먼저 InfoWindow를 import 합니다.
  ```javascript
  import { Map, GoogleApiWrapper, Marker, InfoWindow } from 'google-maps-react';
  ```

  렌더링 될 때

  엘리먼트로 넣어주면 사용이 가능합니다.
  ```javascript
  <InfoWindow
    visible= {Boolean}
    position={{
      lat: int,
      lng: int
    }} 
    content= {String}
    onClose= {function}
  >
  </InfoWindow> 
  // 예제에 사용되는 매개변수와 타입입니다.
  ```

  onClose 같은 경우에는 기본 함수가 있으며

  visible의 값을 false 로 바꿔줍니다.

  이번에 사용할 방법은 Marker를 클릭시 

  infoWindow를 활성화 시키며,

  close시 비활성화 시킬수 있도록 만들어 보겠습니다.

  먼저 Marker의 onClick과 

  InfoWindow의 onClose에 들어갈 함수를 생성합니다.
  ```javascript
  visibleInfoWindow = async (i) => {
    const {stores} = this.state;
    let stateData = stores;
    stateData[i].bool = !stateData[i].bool 
    await this.setState({
      stores: stateData
    })
  }
  ```

  기존에 state의 저장 했던

  stores의 키값에는 visible에 들어갈 
  
  boolean 값이 없으므로 추가해줍니다.
  ```javascript
  const latLng = {latitude:lat, longitude:lng, title: address, bool: false};
  ```

  Marker를 클릭시 InfoWindow가 보여야하므로

  onClick을 바꾸며,

  InfoWindow의 경우

  Marker 를 그리는 함수와 동일 한 방식으로

  출력할 수 있도록 그리는 함수를 생성합니다.
  ```javascript
  displayMarkers = () => {
    return this.state.stores.map((store, index) => {
      return (
        <Marker 
          key={index}
          position={{
            lat: store.latitude,
            lng: store.longitude
          }} 
          onClick={() => this.visibleInfoWindow(index)} 
        />
      );
    })
  }
  displayInfoWindows = () => {
    return this.state.stores.map((store, index) => {
      return (
        <InfoWindow
          key={index}
          visible={store.bool}
          position={{
            lat: store.latitude,
            lng: store.longitude
          }} 
          content={store.title}
          onClose={()=>this.visibleInfoWindow(index)}
        >
        </InfoWindow>
      );
    })
  }
  ```

  그 후 렌더링 되는 Map 안에

  다음과 같이 추가합니다
  ```javascript
  {this.displayInfoWindows()}
  ```

  예제 코드 실행시 아래와 같은 그림으로 실행됩니다.
  ![img](\assets\images\google-maps-react\img5.png)

  해당 마커를 클릭하면..
  ![img](\assets\images\google-maps-react\img6.png)

  이렇게 InfoWindow가 출력되는 것을 확인할수 있습니다.