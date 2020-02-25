---
title: "JavaScript 프린트 제어"
comments: true
categories:
  - JavaScript
tags:
  - JavaScript
  - print
date: "2020-02-25 11:56"
---

### 1. JavaScript Print

javascript만을 사용하여 print기능을 제어하는 방법을 정리했습니다.

먼저 현재창에서 Class가 printWrap인 HTML 엘리먼트를 A4 크기로 설정 후,

프린트 하는 방법입니다.

```javascript
  const _tempHTML = document.getElementsByClassName("printWrap")[0].innerHTML;
  window.onbeforeprint = function(){
    document.body.innerHTML = "<div class='printWrap' style='width:21cm; height: 29.7cm;'>" + _tempHTML +"/div>";        
  }
  window.onafterprint = function(){
    //새로고침하여 페이지를 다시 출력합니다.
    this.window.location.reload();
  }  
  window.print();
```

또는 새 창을 출력하여 해당 창의 내용을 print 할 수도 있습니다.
```javascript
  const _tempHTML = document.getElementsByClassName("printWrap")[0].innerHTML;
  const innerHTMLs = "<div class='printWrap' style='width:21cm; height: 29.7cm;'>" + _tempHTML + "</div>"; 
  const popupWindow = window.open("", "_blank", "width=700,height=800");
  popupWindow.document.write("<!DOCTYPE html>"+
			      "<html>"+
			        "<head>"+
			        	"<style type='text/css'>"+
			        		".endline{page-break-before:always}"+
			        	"</style>"+
			        	"<style type='text/css' media='print'>"+
				    		"@page {"+
				    		    "size: auto;  /* auto is the initial value */"+
				    		    "margin: 0px 100px 0px 100px;  /* this affects the margin in the printer settings */"+
				    		"}"+
				    	"</style>"+
			        "</head>"+
			        "<body>"+innerHTMLs+"</body>"+
			      "</html>");
  popupWindow.document.close();
	popupWindow.focus();
  //1초후 새 창 프린트
  window.setTimeout(function () {
	 	popupWindow.print();
	 	popupWindow.close();
	}, 1000);
```

위와 같이 css media 에 print 속성을 주면 프린트 시 css를 설정 할 수 있습니다.

해당 css에서는 margin의 top, bottom을 0으로 주어 상단 날짜와

하단의 페이지 꼬리말을 제거후 가운데로 정렬한 예제입니다.

이러한 방법을 통해 새창 또는 현재창에서 a4용지에 맞게

javascript로 제어하여, 프린트 할 수 있습니다.