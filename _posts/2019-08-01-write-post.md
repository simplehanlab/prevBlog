---
title: "VSCode, Typora를 이용하여 jekyll 포스트 작성하기 (with minimal-mistakes)"
comments: true
categories:
  - jekyll
  - minimal-mistakes
tags:
  - github pages
  - jekyll
  - minimal-mistakes
  - VSCode
  - Typora
date: "2019-08-01 17:00"
---
jekyll 포스트를 작성하는 방법에 대해 작성하려 한다. 나는 공개된 테마 중 minimal-mistakes 테마를 이용하고 있으므로 해당 테마 기반으로 글을 작성한다. 그리고 jekyll 사용 편의성을 높히기 위해 VSCode와 Typora를 같이 이용하기로 하였다.

jekyll, minimal-mistakes 등에 관한 설명은 지난 포스트를 참조하자.

[minimal-mistakes 테마 로컬에서 구동하기]

[minimal-mistakes 테마 로컬에서 구동하기]: http://localhost:5000/jekyll/minimal-mistakes/run-on-local/

포스트 작성 전에 VSCode와 Typora를 설치해보자.



## VSCode 설치

VSCode는 Visual Studio Code의 약칭으로 마이크로소프트가 제작한 소스 코드 편집기이다. 이를 이용해서 jekyll 프로젝트의 편집을 할 계획이다.

VSCode를 받기 위해서 [VSCode 사이트]에 접속한다. 사이트에서 Installer를 다운로드 받자.

[VSCode 사이트]: https://code.visualstudio.com/

![](\assets\images\write-post\vscode-site.jpg)

VSCode 설치가 끝난 뒤 실행을 해보면 아래와 같은 화면이 나온다.

![](\assets\images\write-post\vscode-exec.jpg)

VSCode의 폴더 열기를 이용하여 자신의 소스 코드 폴더를 열면 아래처럼 화면이 변한다. 이제부터 소스 코드 편집을 진행할 수 있다.

![](\assets\images\write-post\vscode-load-folder.jpg)



## Typora 설치

VSCode를 이용하여 파일을 빠르게 작성하는 것은 좋으나, jekyll 포스트는 기본적으로 파일 형식이 md이다. md파일은 마크다운 포맷으로 구성되기 때문에 작성과 출력에 차이가 있다. 이 포맷을 작성하는 도중에도 확인할 수 있다면 더욱 편하게 포스트 작성이 가능하기 때문에 마크다운 에디터인 Typora를 이용하기로 했다.

Typora를 설치하기 위해 [Typora 사이트]에 접속한다. Download 탭에서 자신의 OS에 맞는 버전을 선택하여 다운받는다. (나는 Windows로 진행하였다.)

[Typora 사이트]: https://typora.io/

![](\assets\images\write-post\typora-site.jpg)

Typora를 다운받은 뒤 실행해보면 아래와 같은 화면이 나온다.

![](\assets\images\write-post\typora-exec.jpg)

이제부터 Typora를 이용하여 마크다운 문서를 작성할 수 있다. Typora의 다양한 기능을 이용하여 마크다운 문서를 작성해보자.

Typora 상단 메뉴 중 본문, 서식 등을 이용하여 문서를 작성할 수 있는데 문서를 작성하면 실시간으로 마크다운 문서가 어떻게 해석되서 나타나는 지 볼 수 있다. 아래 이미지처럼 말이다.

![](\assets\images\write-post\typora-p.jpg)

![](\assets\images\write-post\typora-basic.jpg)

물론 소스 코드 상태로도 볼 수 있는데 Typora 아래쪽의 **</>** 아이콘을 클릭하면 소스 코드 모드로 변환된다. 이 상태에서 위의 문서를 보게 되면 아래와 같이 변한다.

![](\assets\images\write-post\typora-source.jpg)

현재 작성한 이 포스트도 Typora에서 작성한 포스트이다. 앞으로 포스트들은 이를 이용해서 작성될 예정이다.

이제 준비가 끝났으니 VSCode를 이용하여 첫 포스트를 작성해보자.



## 포스트 작성하기

포스트를 작성하기 위해선 일단 **_posts** 폴더가 필요하다. jekyll은 _posts 폴더에 특정한 이름의 마크다운 파일을 자동으로 포스트로 인식한다. 이 폴더가 있다면 상관없지만 없다면 다음과 같이 폴더를 생성해주자.

![](\assets\images\write-post\mkdir-posts.jpg)

폴더를 생성했다면 다음으로 포스트를 작성해볼 차례인데, 주의점이 있다. 파일의 이름 형식을 꼭 맞춰줘야 하는데, 파일 형식은 다음과 같다.

> YEAR-MONTH-DAY-title.MARKUP
>> YEAR : 4자리의 숫자  
>> MONTH, DAY : 2자리의 숫자  
>> title : 포스트 제목 (파일 이름 정도로 인식하면 된다.)  
>> MARKUP : 마크다운 포맷이 인식되는 확장자

이를 토대로 만들어진 올바른 포스트 파일명은 아래와 같다.

> 2011-12-31-new-years-eve-is-awesome.md  
> 2012-09-12-how-to-write-a-blog.md

이제 첫 포스트를 작성해보자. 파일 이름은 **2019-08-01-first-post.md** 로 지정했다.

![](\assets\images\write-post\first-post.jpg)

이제 간단히 글을 작성해보자. jekyll 문서들은 첫 문단에 머리글(Front Matter)을 작성할 수 있는데 문서의 각종 설정, 메타 데이터라고 인식하면 된다. 첫 포스트의 머리글은 아래와 같이 작성할 수 있는데 이미지 아래에서 설명하겠다.

![](\assets\images\write-post\front-matter.jpg)

| 이름              | 기능                      |
| ---------------- | ------------------------- |
| **title**        | 포스트 제목                |
| **categories**   | 포스트 카테고리 정의        |
| **tags**         | 포스트 태그 정의            |
| **date**         | 작성일                     |

이 외에도 정말 다양한 설정값이 존재하나 추후 포스트에서 설명하겠다.

머리글 작성이 끝나면 그 아래에 작성하는 내용이 포스트의 본문이 된다. 테스트용으로 간단하게 작성해보자.

![](\assets\images\write-post\write-first-post.jpg)

작성이 다 끝났으면 자신의 로컬서버, 혹은 github pages 서버에서 포스트가 잘 작성되었는지 확인해보자. *(github pages를 이용하는 방법은 추후 포스팅 예정이다.)*

![](\assets\images\write-post\check-first-post.jpg)

첫 포스트가 잘 올라간 모습을 볼 수 있다. *(나는 minimal-mistakes 테마의 다른 설정들도 건드린 상태이기 때문에 순정 상태와 모습이 다르다.)*