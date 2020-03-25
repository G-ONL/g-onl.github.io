---
layout: post
title: "GitHub 블로그 만들기"
comments: true
category: project
description: >
  GitHub 블로그를 같이 만들어봐요
---

## GitHub 블로그 만들기

원래 티스토리에서 블로그를 운영을 하고 있었는데 블로그를 한 번 옮기고 싶어서 GitHub 블로그로 옮기게 되었습니다.

자료가 찾아보니 많았지만 이상한 곳에서 삽질을 많이해서 간단하고도 쉽게 공유를 해드리려 합니다.

지금 환경상황에서 git을 설치해서 command단에서 설치와 clone으로 작업하기가 어려움이 있어서 오직 홈페이지로만 설정을 해도 만들 수 있는 방법을 알려드리겠습니다.


## 1. 원하는 포맷을 찾기

저는 jekyll을 대부분 많이 사용하셔서 [jekyll theme](https://github.com/topics/jekyll-theme) 이 곳에서 원하는 포맷을 찾으시면 됩니다.

## 2. 포크(Fork) 뜨기

![fork](https://g-onl.github.io/assets/images/20200107/fork.png)
저는 많이들 사용하는 minimal mistakes 테마를 사용하기로 했습니다.

## 3. 필수사항은 아니지만 Fork를 뜬 프로젝트의 프로젝트명은 username.github.io로 만들기

다른 이름을 사용해도 되지만 설정을 몇 가지 더 해줘야하며 URL주소가 깔끔하지 않습니다.

## 3. 우선 지워야할 파일 지우기


- .editorconfig
- .gitattributes
- .github
- /docs
- /test
- CHANGELOG.md
- minimal-mistakes-jekyll.gemspec
- README.md
- screenshot-layouts.png
- screenshot.png

이렇게 있는데 /docs와 /test 폴더는 놔두도록 합니다. 홈페이지에서는 폴더 삭제가 안되서 일일이 파일을 하나씩 삭제해야하는데 너무 큰 노가다 입니다.

그래도 나중에 삭제 할 수 있으면 삭제하시는거를 추천드립니다.

## 4. _config 설정 파일 입맛에 맞게 바꾸기

저의` _config` 파일을 모두 가져와 봤습니다. 

```
# Welcome to Jekyll!
#
# This config file is meant for settings that affect your entire site, values
# which you are expected to set up once and rarely need to edit after that.
# For technical reasons, this file is *NOT* reloaded automatically when you use
# `jekyll serve`. If you change this file, please restart the server process.

# Theme Settings
#
# Review documentation to determine if you should use `theme` or `remote_theme`
# https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/#installing-the-theme

# theme                  : "minimal-mistakes-jekyll"
# remote_theme           : "mmistakes/minimal-mistakes"
minimal_mistakes_skin    : "default" # "air", "aqua", "contrast", "dark", "dirt", "neon", "mint", "plum", "sunrise"
                            # 다양한 스킨을 지원합니다. default 말고 위에  여러가지를 적용시켜보세여.
# Site Settings
locale                   : "ko"  # 지역은 한국
title                    : "제목" # 블로그의 제목을 넣는칸 " " 안에 잘 넣어주세요.
title_separator          : "-"
subtitle                 : # site tagline that appears below site title in masthead
name                     : "별명 혹은 이름"
description              : "블로그 설명을 넣어주세요"
url                      : "https://g-onl.github.io" # 본인의 url주소를 넣어주세요.
baseurl                  : "" # "" 빈값으로 설정해주세요
repository               : "G-ONL/g-onl.github.io"# GitHub의 username/repo-name 순서로 적어주세요. 
teaser                   : # path of fallback teaser image, e.g. "/assets/images/500x300.png"
logo                     : # path of logo image to display in the masthead, e.g. "/assets/images/88x88.png"
masthead_title           : # overrides the website title displayed in the masthead, use " " for no title
# breadcrumbs            : false # true, false (default)
words_per_minute         : 200
comments:                 # comments 댓글 관련되어서는 따로 적어놓겠습니다. 저의 경우는 많이들 사용하는 disqus를 이용했습니다.
  provider               : "disqus" # false (default), "disqus", "discourse", "facebook", "staticman", "staticman_v2", "utterances", "custom"
  disqus:
    shortname            : "woopaloopa" # disqus에서는 url에 shortname을 붙일 수 있는데 그 값 입니다.
  discourse:
    server               : # https://meta.discourse.org/t/embedding-discourse-comments-via-javascript/31963 , e.g.: meta.discourse.org
  facebook:
    # https://developers.facebook.com/docs/plugins/comments
    appid                :
    num_posts            : # 5 (default)
    colorscheme          : # "light" (default), "dark"
  utterances:
    theme                : # "github-light" (default), "github-dark"
    issue_term           : # "pathname" (default)
staticman:
  allowedFields          : # ['name', 'email', 'url', 'message']
  branch                 : # "master"
  commitMessage          : # "New comment by {fields.name}"
  filename               : # comment-{@timestamp}
  format                 : # "yml"
  moderation             : # true
  path                   : # "/_data/comments/{options.slug}" (default)
  requiredFields         : # ['name', 'email', 'message']
  transforms:
    email                : # "md5"
  generatedFields:
    date:
      type               : # "date"
      options:
        format           : # "iso8601" (default), "timestamp-seconds", "timestamp-milliseconds"
  endpoint               : # URL of your own deployment with trailing slash, will fallback to the public instance
reCaptcha:
  siteKey                :
  secret                 :
atom_feed:
  path                   : # blank (default) uses feed.xml
search                   : true # 제목 관련해서 search할 수 있게 하려면 true해주시면 됩니다.
search_full_content      : # true, false (default) 이것은 content의 글까지 검색 할 수 있는 기 능입니다.
search_provider          : # lunr (default), algolia, google
algolia:
  application_id         : # YOUR_APPLICATION_ID
  index_name             : # YOUR_INDEX_NAME
  search_only_api_key    : # YOUR_SEARCH_ONLY_API_KEY
  powered_by             : # true (default), false
google:
  search_engine_id       : # YOUR_SEARCH_ENGINE_ID
  instant_search         : # false (default), true
# SEO Related
google_site_verification :
bing_site_verification   :
yandex_site_verification :
naver_site_verification  :

# Social Sharing
twitter:
  username               :
facebook:
  username               :
  app_id                 :
  publisher              :
og_image                 : # Open Graph/Twitter default site image
# For specifying social profiles
# - https://developers.google.com/structured-data/customize/social-profiles
social:
  type                   : # Person or Organization (defaults to Person)
  name                   : # If the user or organization name differs from the site's name
  links: # An array of links to social media profiles

# Analytics
analytics:                #이곳에서 구글 Analytics를 사용할 수 있습니다.
  provider               : false # false (default), "google", "google-universal", "custom"
  google:
    tracking_id          :
    anonymize_ip         : # true, false (default)


# Site Author
author:
  name             : "이름 혹은 별명"
  avatar           : # path of avatar image, e.g. "/assets/images/bio-photo.jpg" 이미지 url을 적어주면 됩니다.
  bio              : "Gooood!! :)" 
  location         : "Seoul, Korea" # 
  email            :
  links:
    - label: "Email"
      icon: "fas fa-fw fa-envelope-square"
      # url: mailto:your.name@email.com
    - label: "Website"
      icon: "fas fa-fw fa-link"
      # url: "https://your-website.com"
    - label: "Twitter"
      icon: "fab fa-fw fa-twitter-square"
      # url: "https://twitter.com/"
    - label: "Facebook"
      icon: "fab fa-fw fa-facebook-square"
      # url: "https://facebook.com/"
    - label: "GitHub"
      icon: "fab fa-fw fa-github"
      # url: "https://github.com/"
    - label: "Instagram"
      icon: "fab fa-fw fa-instagram"
      # url: "https://instagram.com/"

# Site Footer
footer:
  links:
    - label: "Twitter"
      icon: "fab fa-fw fa-twitter-square"
      # url:
    - label: "Facebook"
      icon: "fab fa-fw fa-facebook-square"
      # url:
    - label: "GitHub"
      icon: "fab fa-fw fa-github"
      # url:
    - label: "GitLab"
      icon: "fab fa-fw fa-gitlab"
      # url:
    - label: "Bitbucket"
      icon: "fab fa-fw fa-bitbucket"
      # url:
    - label: "Instagram"
      icon: "fab fa-fw fa-instagram"
      # url:


# Reading Files
include:
  - .htaccess
  - _pages
exclude:
  - "*.sublime-project"
  - "*.sublime-workspace"
  - vendor
  - .asset-cache
  - .bundle
  - .jekyll-assets-cache
  - .sass-cache
  - assets/js/plugins
  - assets/js/_main.js
  - assets/js/vendor
  - Capfile
  - CHANGELOG
  - config
  - Gemfile
  - Gruntfile.js
  - gulpfile.js
  - LICENSE
  - log
  - node_modules
  - package.json
  - Rakefile
  - README
  - tmp
  - /docs # ignore Minimal Mistakes /docs
  - /test # ignore Minimal Mistakes /test
keep_files:
  - .git
  - .svn
encoding: "utf-8"
markdown_ext: "markdown,mkdown,mkdn,mkd,md"


# Conversion
markdown: kramdown
highlighter: rouge
lsi: false
excerpt_separator: "\n\n"
incremental: false


# Markdown Processing
kramdown:
  input: GFM
  hard_wrap: false
  auto_ids: true
  footnote_nr: 1
  entity_output: as_char
  toc_levels: 1..6
  smart_quotes: lsquo,rsquo,ldquo,rdquo
  enable_coderay: false


# Sass/SCSS
sass:
  sass_dir: _sass
  style: compressed # https://sass-lang.com/documentation/file.SASS_REFERENCE.html#output_style


# Outputting
permalink: /:categories/:title/
paginate: 5 # amount of posts to show
paginate_path: /page:num/
timezone: # https://en.wikipedia.org/wiki/List_of_tz_database_time_zones


# Plugins (previously gems:)
plugins:
  - jekyll-paginate
  - jekyll-sitemap
  - jekyll-gist
  - jekyll-feed
  - jekyll-include-cache

# mimic GitHub Pages with --safe
whitelist:
  - jekyll-paginate
  - jekyll-sitemap
  - jekyll-gist
  - jekyll-feed
  - jekyll-include-cache


# Archives
#  Type
#  - GitHub Pages compatible archive pages built with Liquid ~> type: liquid (default)
#  - Jekyll Archives plugin archive pages ~> type: jekyll-archives
#  Path (examples)
#  - Archive page should exist at path when using Liquid method or you can
#    expect broken links (especially with breadcrumbs enabled)
#  - <base_path>/tags/my-awesome-tag/index.html ~> path: /tags/
#  - <base_path>/categories/my-awesome-category/index.html ~> path: /categories/
#  - <base_path>/my-awesome-category/index.html ~> path: /
category_archive:
  type: liquid
  path: /categories/
tag_archive:
  type: liquid
  path: /tags/
# https://github.com/jekyll/jekyll-archives
jekyll-archives:
  enabled:
    - categories
    # - tags
  layouts:
    category: archive-taxonomy
    # tag: archive-taxonomy
    permalinks:
      category: /categories/:name/
      # tag: /tags/:name/


# HTML Compression
# - https://jch.penibelst.de/
compress_html:
  clippings: all
  ignore:
    envs: development


# Defaults
defaults:
  # _posts
  - scope:
      path: ""
      type: posts
    values:
      layout: single
      author_profile: false # profile을 노출 시키고 싶으면 true
      read_time: true 
      comments: # true # 댓글 관련 true로 바뀌어 놓으시면 default로 됩니다.
      share: true
      related: true

```


## 5. 폴더 구조 알아보기

![folder](https://g-onl.github.io/assets/images/20200107/folder.PNG)

하나씩 살펴보면

`_data` 폴더에서 중요한 파일은 <strong>navigation.yml</strong>이다. 상단에서 네비게이션 바 역할을 한다.

안의 구조가 매우 간단하다.

```
main:
  - title: "Flutter"  => 메뉴의 명칭
    url: /flutter/    => 이어지는 url
```

이게 어디서 매칭이 되냐면

`_pages` 폴더로 가서 category-flutter.md 이러한 파일명을 만들었습니다.

그 안의 내용으로는 
```
---
title: "Flutter" => 제목
layout: category-flutter => 이 부분은_layouts 폴더안의 파일과 매칭됩니다.
permalink: /flutter/ => 연결되는 링크
author_profile: false => 옵션인데 flutter 메뉴 안에서는 profile은 공개를 안한다는 뜻입니다.
---
```

그러면 이어서 `_layouts` 폴더 안의 category-flutter.html 파일에서는 루비와 html 파일들로 구성을 하면 됩니다.

마지막으로 'Flutter'가 이어지는 부분은 `_posts` 폴더안에 있는 파일들 입니다.

`_posts`는 중요한 파트라 다음 6단계에서 설명을 하겠습니다.

## 6. 첫 글을 작성하자.

처음에 Fork를 뜨고 나면 `_posts`폴더는 존재하지 않습니다.

이 부분은 직접 만들어줘야 하는데 Create new file 버튼을 누르면 상단에 파일 제목을 적는 부분이 있습니다.

![create](https://g-onl.github.io/assets/images/20200107/create.PNG)

저 빈칸에 `_posts/YYYY-MM-dd-파일명.md` 를 적어주시게 되면 폴더는 자동으로 생성이 됩니다.

파일의 이름 형식은 꼭 맞춰주어야 합니다.

그 다음 파일의 제일 상단에 다음의 내용이 들어가야 합니다.
```
---
layout: single => 어떤 레이아웃을 사용할 것인지 적습니다.
title: "Flutter TabBar" => 글 제목
comments: true => 댓글 사용 여부
category: Flutter => 5번 마지막 이어지는 부분이 바로 이부분 입니다. 이부분에서 지정을 해줘야 이 글이 어떤 카테고리에 들어갈지 정해집니다. 
---

이 아래 부분은 마크다운 형식에 맞추어 글을 작성해주면 됩니다.

```
위에 적은 설정값들 말고도 tags , categories, toc(목차), gallery, ... 다양한 값이 있으니 공식문서를 확인하고 적용해보세요.


이렇게 간단하고 빠르게 github을 이용한 블로그 만들기를 해보았습니다.

[구경가기](https://g-onl.github.io)

제 홈페이지랑 비교해보며 만들어보세요.



