---
title: "Ruby on Rails 세줄 요약"
date: 2021-07-18
permalink: /posts/2021/07/blog-post-2/
tags:
  - Ruby
  - Rails
  - MVC
---

공모전에 스마트 TV 앱을 만들어 제출해야해서 이번에 새로 앱 개발 스터디를 시작하게 되었다. 이번 포스트에서는 수많은 앱 개발 플랫폼이 있지만 왜 우리 스터디가 RoR를 스터디 프레임워크로 정했는지에 대한 설명과 RoR의 철학, 그리고 주요 작동구조를 기록하고자 한다. 그리고 이것이 어떻게 어플리케이션이 되는지에 대해서도 이해한 바를 기록하고자 한다.

### - 앱 개발 플랫폼의 종류

<br>
앱 개발은 크게 세가지로 나눌 수 있다 : 네이티브, 하이브리드, 웹

- 네이티브 : 자바, 코틀린 등 -> 빠르다
- 하이브리드 : 리액트 네이티브 등 -> 네이티브 + 웹
- 웹 : Webview, Recyclerview 등 -> 말 그대로 웹

우리 스터디에서는 고성능의 앱이 목표가 아니였고, 하이브리드 앱 같은 경우 진입장벽이 있다고 판단, 웹 앱을 개발하는 것을 목표로 하기로 했다. 결국 이것은 웹을 만드는 것과 마찬가지인데, 빠르게 웹 개발에 입문할 수 있는 RoR이라는 프레임워크가 있다고 하여 스터디 공통 언어로 선정하게 되었다.

### - RoR과 철학

~~별거 없는 거 같다~~
<br>
RoR은 Ruby on Rails의 줄임말이며, Ruby는 프로그래밍 언어, Rails는 Ruby 언어 기반 프레임워크이다. RoR은 두가지 철학이 있다:

1. DRY(Don't Repeat Yourself) : <br>
   partial에서 이 철학을 많이 느낄 수 있었는데 그냥 모든 프로그래밍 언어가 그렇듯이 코드의 재사용성을 보장한다는 것이다.
2. CoC(Convention over Configuration) : <br>
   프레임워크라면 이 철학은 기본으로 내장되어 있다고 생각한다. 실제로 RoR이 현재의 메이저 웹 프레임워크에 많은 영향을 주었다고 한다.

### **-RoR의 주요 작동구조**

<br>
사실 이번 포스트는 이 부분이 핵심이다.
RoR은 MVC(Model-View-Controller) 패턴을 이용한다.

![MVC](https://upload.wikimedia.org/wikipedia/commons/thumb/a/a0/MVC-Process.svg/300px-MVC-Process.svg.png)
<br>출처 - 위키피디아

그림만 봐서는 이해가 안될 것이다. RoR 공식 가이드 문서에서는 RoR로 blog를 만들고 CRUD를 하는 가이드를 제공하는데 그 중 C(Create)을 직접 해봄으로써 MVC 패턴에 대하여 알아보도록 하자. 공식 가이드 v6.1.4이다.

---

우선 위에서 언급한 MVC 말고도 route라는 것이 있다. route는 사용자의 request를 controller의 action으로 연결시켜준다.

일단 모든 게시글을 보여주는 웹페이지가 있는데,(View) 모든 게시글 중 하나를 클릭하면 그 게시글을 보여주는 View를 만들어보자. 우선 route를 추가하자.(맨 밑줄)
<br>
config/routes.rb

```Ruby
Rails.application.routes.draw do
  root "articles#index"

  get "/articles", to: "articles#index"
  get "/articles/:id", to: "articles#show"
end
```

:id는 parameter로, 예를들어 사용자가 <code>GET http://localhost:3000/articles/1</code>를 request하면 1은 id가 되어 controller action에 있는 params[:id]에 접근 가능하다.
_즉 route를 통해 controller의 action에 접근 가능하다는 것이다._ params : hash라고 생각하고 넘어가자.
<br>
apps/controllers/articles_controller.rb

```Ruby
class ArticlesController < ApplicationController
  def index
    @articles = Article.all
  end

  def show
    @article = Article.find(params[:id])
  end
end
```

이런 식으로 id를 통하여 게시글을 찾고 반환하여 @article instance variable에 저장하여, view를 통해 접근 할 수 있다.<br>
app/views/articles/shows.html.erb :

```Ruby
<h1><%= @article.title %></h1>

<p><%= @article.body %></p>
```

그리고 마지막으로 모든 게시글을 보는 view에서 link를 해주면 끝
<br>
app/views/articels/index.html.erb

```Ruby
<h1>Articles</h1>

<ul>
  <% @articles.each do |article| %>
    <li>
      <a href="/articles/<%= article.id %>">
        <%= article.title %>
      </a>
    </li>
  <% end %>
</ul>
```

이런 식으로 MVC패턴이 이뤄지고, 나머지 CRUD도 약간씩을 다르지만 이러한 MVC패턴으로 기능들을 수행한다.

**따라서 RoR의 주요 작동구조인 MVC 패턴을 정리하자면,**<br>

**1. route로 사용자의 request를 controller의 mapping**<br>
**2. mapping된 controller에서 action 수행, model/view에 반영**<br>
**3. action은 model/view에 반영되어 사용자가 request한 것의 결괏값을 받을 수 있음(erb -> html)**<br>

model에 관한 설명이 빈약한 것 같긴 한데, 이런 식으로 MVC패턴이 작동한다. 후에 스터디가 진행됨에 따라 점차 MVC패턴에 대한 글을 업데이트 하겠다.

그리고 RoR를 통하여 Web app을 빌드했다면 두 가지 방법으로 모바일 앱으로 변환할 수 있다: Native or Hybrid

Native 경우에는 RoR로 만든 Web app을 API로 변환시킨 후 front-end단을 mobile 환경으로 바꿔주면 된다. 즉 다시 처음부터 싹 다 갈아 엎는거다.

Hybrid 경우에는 webview 쓰면 된다. mobile 적인 UI 요소는 native로 개발하면 되므로, only native 보다는 노력의 양이 적다. 그러나 느리다.

## <br>

이번 글을 통해 크게 모바일 앱의 세가지 종류를 알게 되었고 왜 우리 스터디가 RoR를 스터디 주제로 채택했는지, 그리고 RoR의 철학과 주요원리인 MVC 패턴에 대하여 알고 RoR로 빌드한 웹 앱을 어떻게 모바일 앱으로 변경하는지 간략하게 알아보았다. 오랜만에 포스트를 써서 번잡할 수 있는데, 스터디가 진행되고 시간이 나면 업데이트 해보겠다. 끝
