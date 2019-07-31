---
title: "Simplehan lab 로컬에서 구동하기"
comments: true
categories:
  - jekyll
tags:
  - jekyll
  - ruby
date: "2019-07-30 11:00"
---
github pages를 이용하여 simplehan lab 블로그(현재 보고있는 블로그)를 개설하였다. jekyll 기반으로 만들어진 [minimal-mistakes] 테마를 이용하였다. 이를 이용하여 simplehan lab github에 올라와있는 git pages 소스를 내려받아서 수정 후 push를 하면 현재 호스트되고 있는 simplehan lab 블로그가 자동으로 갱신이 된다.

[minimal-mistakes]: https://github.com/mmistakes/minimal-mistakes	"minimal-mistakes github"

그러나 push를 한다고 해서 반영 및 배포가 바로바로 되지는 않기 때문에 지금 보고 있는 페이지가 수정이 완료가 된건지, 아직 배포가 안된건지 알기가 힘들다. 그리고 잘못 수정된 파일이 올라갈수도 있다. 이러한 문제점들을 방지하기 위해 local 환경에서 사이트를 띄워볼 수 있는데 여기에서 그 방법을 설명하려 한다.



## 소스코드 내려받기

일단 simplehanlab github에서 소스를 clone 한다.

```terminal
git clone https://github.com/simplehanlab/simplehanlab.github.io.git
```

그러면 아래와 같은 구조의 소스를 clone 받게 된다.

```bash
simplehanlab.github.io
├── _data                      # 테마 커스터마이징 데이터
|  ├── navigation.yml          # 네비게이션(메뉴) 데이터
|  └── ui-text.yml             # UI의 텍스트 (언어별)
├── _includes
|  ├── analytics-providers     # snippets for analytics (Google and custom)
|  ├── comments-providers      # snippets for comments
|  ├── footer                  # custom snippets to add to site footer
|  ├── head                    # custom snippets to add to site head
|  ├── feature_row             # feature row helper
|  ├── gallery                 # image gallery helper
|  ├── group-by-array          # group by array helper for archives
|  ├── nav_list                # navigation list helper
|  ├── toc                     # table of contents helper
|  └── ...
├── _layouts
|  ├── archive-taxonomy.html   # tag/category archive for Jekyll Archives plugin
|  ├── archive.html            # 아카이브
|  ├── categories.html         # 카테고리로 묶은 아카이브
|  ├── category.html           # 특정 카테고리로 묶은 아카이브
|  ├── collection.html         # 특정 콜렉션으로 묶은 아카이브
|  ├── compress.html           # compresses HTML in pure Liquid
|  ├── default.html            # 기본
|  ├── home.html               # 홈
|  ├── posts.html              # 년도별로 묶은 아카이브
|  ├── search.html             # 검색
|  ├── single.html             # single document (post/page/etc)
|  ├── tag.html                # 특정 태그로 묶은 아카이브
|  ├── tags.html               # 태그로 묶은 아카이브
|  └── splash.html             # 스플래시 페이지
├── _sass                      # SCSS
├── assets
|  ├── css
|  |  └── main.scss            # main stylesheet, loads SCSS partials from _sass
|  ├── images                  # image assets for posts/pages/collections/etc.
|  ├── js
|  |  ├── plugins              # jQuery plugins
|  |  ├── vendor               # vendor scripts
|  |  ├── _main.js             # plugin settings and other scripts to load after jQuery
|  |  └── main.min.js          # optimized and concatenated script file loaded before </body>
├── _config.yml                # 설정파일
├── Gemfile                    # gem file dependencies
├── index.html                 # 인덱스
└── package.json               # NPM build scripts
```



## Ruby 설치

이 블로그는 위에서도 언급했다시피 jekyll 기반으로 만들어져 있다. 그러므로 이 소스를 로컬 환경에서 구동하기 위해선 jekyll이 필요한데, jekyll은 Gem이라는 Ruby 기반의 패키지 매니저를 통해서 받을 수 있다. 이를 위해 먼저 Ruby를 설치해야 한다. [RubyInstaller]에 접속해서 Installer를 다운받도록 하자.

[RubyInstaller]: https://rubyinstaller.org/downloads/	"RubyInstaller Site"

![](\assets\images\rubyinstaller.jpg)

설치 중에 MSYS2 development toolchain도 설치할건지 체크박스가 나오는데 같이 설치해주자. (후에 선택지가 나오면 base installation 선택)

Devkit이 포함된 Ruby를 설치하면 Gem도 사용이 가능해진다. 이제 gem을 이용해서 jekyll과 gem 관리를 위한 bundler를 설치해보자.

```terminal
gem install jekyll bundler
```

이제 소스코드 폴더로 들어가서 의존성들을 다운로드 받아준다.

```terminal
bundle install
```

의존성 다운로드가 끝나면 구동을 해보자.

```bash
bundle exec jekyll serve	# --port xxxx 옵션을 통하여 포트 지정 가능
```

포트를 지정했다면 해당 포트로, 포트를 지정하지 않았다면 4000 포트로 접속 시 블로그가 뜨면 성공.



## 에러 case

로컬에서 구동 시 아래와 같은 에러가 출력될 수 있다.

```terminal
Conversion error: Jekyll::Converters::Scss encountered an error while converting 'assets/css/main.scss':
                    Invalid CP949 character "\xE2" on line 54
jekyll 3.8.5 | Error:  Invalid CP949 character "\xE2" on line 54
```

jekyll 홈페이지에서 windows 환경에서 구동 시 인코딩에 문제가 있다고 소개하고 있는데 다음과 같이 설명하고 있다.

> 만약 UTF-8 인코딩을 사용한다면, 문서 안에 `BOM` 헤더를 사용하지 않아야 합니다. 그렇지 않으면 Jekyll 에 아주, 아주 안 좋은 일이 벌어집니다. 이는 특히, 윈도우즈에서 Jekyll 을 사용하는 것에 연관된 문제입니다.
>
> 그리고, 사이트 생성 단계에서 “Liquid Exception: Incompatible character encoding” 에러가 발생하는 경우엔, 콘솔창의 코드 페이지를 UTF-8 로 바꿔야 할 수도 있습니다.

이를 위한 해결법 역시 적혀있는데 terminal에서 다음과 같이 입력을 해주면 된다.

```terminal
chcp 65001
```

그러면 terminal에서 Active code page: 65001 이라는 텍스트가 출력이 되는데 이제 아래의 실행코드를 실행하면 서버가 구동되는 모습을 확인할 수 있다.

```
bundle exec jekyll serve	// --port xxxx 옵션을 통하여 포트 지정 가능
```

[http://jekyllrb-ko.github.io/docs/windows/#%EC%9D%B8%EC%BD%94%EB%94%A9](http://jekyllrb-ko.github.io/docs/windows/#%EC%9D%B8%EC%BD%94%EB%94%A9)