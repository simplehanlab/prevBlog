---
title: "Google Map API React에서 사용하기 (3)"
comments: true
categories:
  - react
tags:
  - javascript
  - react
  - node
  - google
  - map
date: "2020-01-02 16:00"
---

### 1. 추가 응용 

  기존 사용 했던 Map 에서 initialCenter 에는 기본적으로 출력해줄 중앙 좌표값을 설정할수 있습니다.

  기본적으로 {lat : number, lng: number} 형태의 데이터가 필요하며,

  추후 center 포지션을 바꾸고자 할때는 setCenter() 함수의 사용시 유동적으로 변경 가능합니다.

  위 설명으로 간단한 예제를 만들어보면..

  ```javascript
    onChangePosition = (lat, lng) => {
      let map = this.mapRef.map;
      let position = new google.maps.LatLng(lat, lng);
      map.setCenter(position);
    }

    initDefaultPosition = () => {
      let defaultPosition = {lat:0 lng:0};
      this.setState({
        initPos = defaultPosition
      });
    }
  ```

  이런식으로 간단한 함수를 만들어서 기본 중앙 좌표값 설정 및

  특정 좌표로 중앙값 이동이 가능합니다.

  추가적으로 좌표와 좌표간의 거리를 알고자한다면

  ((ACOS(
    SIN( data.x * PI() / 180) * SIN(data2.x * PI() / 180) + 
    COS( data.x * PI() / 180) * COS(data2.x * PI() / 180) * 
    COS(( data.y - data2.y) * PI() / 180)) * 180 / PI()) * 60 * 1.1515)

  을 통해 data와 data2의 거리를 mile로 표현 할 수 있습니다.

