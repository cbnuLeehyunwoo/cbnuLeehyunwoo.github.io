---
title: Jekyll, GitHub Page로 개인 블로그 만들기-1(초기 설정 및 빌드)
date: 2025-07-22
categories: [블로그 만들기]
layout: single
tags: [jekyll, minimal-mistakes, git, github, ruby]
slug: jekyll-githubpage-setup
author_profile: true
comments: true

---

- 내가 깃허브 페이지로 개인 블로그를 만드는 과정, 겪었던 문제들을 정리하여 공유해보려고 한다.

## 1. 환경 세팅

### Ruby 설치

- **jekyll은 Ruby** 기반의 정적 사이트를 생성기이다.  따라서** Ruby**를 먼저 설치하고 시작해야 한다.
- 다음의 공식 다운로드 사이트[https://rubyinstaller.org/downloads/](https://rubyinstaller.org/downloads/) 
에서 하이라이트 되어있는 버전을 설치하여 사용하였다, 

![](/assets/스크린샷 2025-07-22 160316.png)

- **jekyll**이 **32비트** 기반이기 때문에 **32비트(x86)**을 설치해야 한다는 이야기를 봤는데. **64비트** 버전으로 다운받았는데 딱히 오류가 발생하지는 않았다.
- 만약 오류가 발생한다면 **32비트** 버전을 다운받아 보는 것을 추천한다.

### Jekyll 및 Bundler 설치

- Ruby 설치가 완료되었다면 Ruby의 의존성 관리도구인 bundler를 설치한다.
    
    ```bash
    gem install bundler
    ```
    
- 그 다음으로는 jekyll을 설치한다.
    
    ```bash
    gem install jekyll bundler
    ```
    
- 다음과 같은 명령어로 설치를 확인할 수 있다.
    
    ```bash
    ruby -v
    jekyll -v
    ```
    

## 2. 깃허브 레포지토리 만들기

- 자신의 깃허브에 자신의 **github_id.github.io**라는 이름의 레포지토리를 만든다. 레포지토리 초기 세팅은 따로 설정하지 않고 기본값으로 진행하면 된다.
![](/assets/스크린샷 2025-07-22 160134.png)

## 3. 블로그 테마 정하기

- **jekyll**에서는 다양한 테마를 제공하는데, 해당 사이트[http://jekyllthemes.org/](http://jekyllthemes.org/) 에 들어가면  여러 웹사이트 템플릿을 둘러볼 수 있다.
- 나는 **minimal-mistakes** 라는 테마를 사용하기로 하였다.
- 테마의 변화에 따라 세팅이 바뀌는지는 확실하지는 않지만, 해당 포스팅은 **minimal-mistakes** 테마를 기준으로 하고 있다.  **minimal-mistakes** 깃헙 링크[https://github.com/mmistakes/minimal-mistakes](https://github.com/mmistakes/minimal-mistakes)
- 깃헙 리드미의 가이드에 따르면 해당 테마는 다양한 색상을 정할 수 있게 되어있다, 난 **contrast** 테마를 사용하기로 했다
![](/assets/스크린샷 2025-07-22 134333.png
)

### 4. 블로그 디렉토리 설정

- 해당 테마를 베이스로 블로그 디렉토리를 설정하는 단계이다. 테마를 적용하는 방법은 공식 문서에 따르면 3가지(**1.  gem-based, 2. remote theme, 3. fork**)가 있는데 나는 2번을 적용하고, 필요한 부분만 따로 로컬에서 세팅하는 방법을 사용하였다.
- 일단 난 VS Code를 사용하여 디렉토리를 설정하였다.
- 먼저 로컬에 폴더를 생성한다. 폴더 이름은 상관 없지만 일단 깃허브 레포지토리명과 동일하게 설정했다.
- 폴더 내의 동일한 레벨에 **_layouts, _pages, _posts, assets, _config.yml, Gemfile, index.md** 파일 및 디렉토리를 생성한다. (**Gemfile.lock, _site**는 빌드 시 자동 생성된다)
![](/assets/스크린샷 2025-07-22 141030.png
)
- 이것저것 만들게 많긴 하지만, 포크하거나 다운로드 받아서 필요없는 부분을 잘라내는 것보다 해당 방법이 편하고 이해하기 용이할 것 같아서 이렇게 진행하기로 했다.
    - Gemfile:
    
    ```bash
    source "https://rubygems.org"
    
    gem "github-pages", group: :jekyll_plugins
    gem "jekyll-include-cache", group: :jekyll_plugins
    ```
    
    - _config.yml: 해당 설정을 바탕으로 jekyll이 사이트를 생성한다.  본인의 정보로 알맞게 수정하면 된다.
    
    ```bash
    # Site settings
    title: "블로그 명"
    description: "블로그 설명"
    url: "https://깃허브 아이디.github.io"
    baseurl: "" # 루트 디렉토리에서 사이트를 배포하겠다는 의미.
    repository: "깃허브 아이디/깃허브 아이디.github.io" # GitHub 저장소 
    
    # Theme settings
    remote_theme: "mmistakes/minimal-mistakes@4.27.1" # 사용할 원격 테마 버전 지정
    minimal_mistakes_skin: "contrast" # Minimal Mistakes 테마 스킨 설정
    
    # Plugins
    plugins:
      - jekyll-include-cache
      
    # Author information
    author:
      name: "이현우" # 블로그 작성자 이름
      bio: "개발자 지망생" # 작성자 소개
      location: "Chungbuk National University" # 작성자 위치
      avatar: "/assets/images/profile.jpg" # 작성자 프로필 이미지 경로
    
    # Site layout configuration
    header:
      navigation: # 상단 메뉴에 표시될 링크들 _pages 폴더에 해당 파일들이 있어야함!
        - title: "Home"
          url: /
        - title: "Categories"
          url: /categories/
        - title: "Tags"
          url: /tags/
        - title: "About"
          url: /about/
          
    # Sidebar configuration
    sidebar:
      nav: "main" # 사이드바에 표시할 네비게이션 메뉴 지정 (일반적으로 'main'을 사용)
    
    collections:
      pages:
        output: true
        permalink: /:path/ # pages 컬렉션의 각 항목에 대한 permalink 규칙 정의
    ```
    
    - index.md

  ```md
    ---
    layout: home
    author_profile: true # 해당 옵션을 키면 왼쪽에 프로필이 나온다.
    ---
  ```
    
    - assets 폴더에 images 폴더를 만들고 자신의 프로필 사진을 추가한다.
        
	![](/assets/스크린샷 2025-07-22 141520.png)        
    - _pages 폴더 내에 _config.yml 파일에 설정했던 상단 네비게이션 바에 링크될 페이지들을 만든다. 내용은 우선 빈칸으로 두었다.
    - 보다보면 자연스럽게 알겠지만, 페이지 파일들 상단의 프론트매터 설정에 따라 jekyll이 html 파일을 어떻게 빌드할지 설정할 수 있다.
    
    ![image.png](/assets/스크린샷 2025-07-22 141815.png)
    
    - about.md
    
	 ```markdown
	---
	 layout: single
	 title: "About"
	permalink: /about/
	author_profile: true
	---

		자기소개를 적어주세요.
	```
    
    - categories.md
    
    ```markdown
    ---
    layout: categories
    title: "Categories"
    permalink: /categories/
    author_profile: true
    ---
    ```
    
    - tags.md
    
    ```markdown
    ---
    layout: tags
    title: "Tags"
    permalink: /tags/
    author_profile: true
    ---
    
    ```
    
    - _posts 폴더 내에 **연도-이름-날짜-파일이름.md** 파일을 만든다.  jekyll이 글을 각각의 폴더로 관리하기 때문에 해당 형식을 지켜서 만들어야 하지만.  반드시 오늘 날짜로 하지 않는다고 해서 빌드가 안되지는 않는다,
    - 하지만 블로그 상에서 해당 글을 클릭할 때 **파일명으로 기반하여** 요청하는 반면,  jekyll이 빌드할 때는 파일 **상단의 프론트매터를 참고하여 분류**해놓는 것으로 보인다. 때문에 둘을 일치시키지 않으면 **Not Found** 오류가 발생한다.
		- 2025-07-22-hello-world.md

	```markdown
	---
	title: "Hello, World"
	date: 2025-07-22
	categories: [블로그]
	layout: single
	tags: [jekyll, minimal-mistakes, 시작]
	author_profile: true
	---
	hello world
	```
		
		![image.png](/assets/스크린샷 2025-07-22 162451.png)
        
				
    

    

## 5. 빌드 및 테스트

- 이정도 되었으면 대강 웹사이트를 빌드할 준비가 완료되었다.
- 우선 Gemfile에 기재한 의존성을 다음 명령어로 다운받는다,
    
    ```bash
    bundle install
    ```
    
    ![image.png](/assets/스크린샷 2025-07-22 153142.png)
    
- 설치가 완료되었으면 다음 명령어를 실행하여 로컬환경에서 웹사이트를 빌드한다.
    
    ```bash
    bundle exec jekyll serve
    ```
    
- 이것저것 길게 뜰텐데 마지막에 Server running…. 이 뜨면 성공이다
    
    ![image.png](/assets/스크린샷 2025-07-22 153352.png)
    
- 콘솔창에도 나오는 로컬호스트 주소로 접속하면 빌드가 잘 되어있는 모습을 볼 수 있다

![image.png](/assets/스크린샷 2025-07-22 155742.png)
