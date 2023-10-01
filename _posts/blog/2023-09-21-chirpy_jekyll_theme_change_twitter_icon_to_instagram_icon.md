---
title: "[Blog] Chirpy Jekyll Theme - Change Twitter Icon To Instagram Icon"
date: 2023-09-21 19:00:00 +0900
categories: [Blog]
tags: [blog, jekyll, chirpy, icon, twitter, instagram]
---

## Intro
---
블로그에 Chirpy Jekyll Theme를 입혔더니 아래 사용하지 않는 트위터 아이콘을 발견했다.<br>
트위터 아이콘을 지워보니 5칸 짜리가 4칸이 되니까 뭔가 이상해 보이니까<br>
**트위터 아이콘 대신 인스타그램 아이콘으로 바꿔보자!**

### Modify files to change icon
1. `_config.yml` 수정<br>
  - <mark style='background-color: #ffdce0'>트위터 관련 항목인 twitter</mark>와 <mark style='background-color: #ffdce0'>social에서 links에 twiter 링크를 주석처리</mark>한다.
  - <mark style='background-color: #ffdce0'>인스타그램 관련 항목인 instagram</mark>과 <mark style='background-color: #ffdce0'>social에서 links에 자신의 instagrame 주소를 추가</mark>한다.
  
	``` yml
	# twitter:
	#   username: twitter_username # change to your twitter username

	instagram:
    username: yn3__3x.h

	social:
    # Change to your full name.
    # It will be displayed as the default author of the posts and the copyright owner in the Footer
    name: Yn3
    email: tong13579@naver.com # change to your email address
    links:
      # The first element serves as the copyright owner's link
      # https://twitter.com/username # change to your twitter homepage
      - https://github.com/yn3-3xh # change to your github homepage
      - https://www.instagram.com/yn3__3x.h~~~~
      # Uncomment below to add more social links
      # - https://www.facebook.com/username
      # - https://www.linkedin.com/in/username
	```
  
2. _data > `contact.yml` 수정<br>
  - <mark style='background-color: #ffdce0'>트위터 관련 항목을 주석처리</mark>한다.
  - <mark style='background-color: #ffdce0'>인스타그램 관련 항목을 추가</mark>한다.

    ``` yml
      # - type: twitter
      #   icon: "fa-brands fa-x-twitter"
    
      - type: instagram
        icon: "fab fa-instagram"
    ```

3. _includes > `sidebar.html` 수정<br>
  - <mark style='background-color: #ffdce0'>62번째 줄</mark> 이후에 있는 부분에서
	
    ``` html
      {% raw %}{% case entry.type %}
        {% when 'github', 'twitter' %}
          {%- capture url -%}
            https://{{ entry.type }}.com/{{ site[entry.type].username }}
          {%- endcapture -%}
          ...
      {% endcase %}{% endraw %}
    ```
    
    - <mark style='background-color: #ffdce0'>twitter를 instagram으로 수정</mark>한다.

    ``` html
      {% raw %}{% case entry.type %}
        {% when 'github', 'instagram' %}
          {%- capture url -%}
            https://{{ entry.type }}.com/{{ site[entry.type].username }}
          {%- endcapture -%}
          ...
      {% endcase %}{% endraw %}
    ```

## Addition
---
추가적으로 다 하고 보니까 footer에 있는 테마 설명이 없으면 더 깔끔해질거 같다.
**테마 설명을 지워보자!**

### Modify file to theme description
1. _includes > `footer.html` 수정<br>
  - <mark style='background-color: #ffdce0'>테마 설명 관련 코드를 주석처리</mark>한다.

    ``` html
      <!-- The Footer -->
  
      <footer
        aria-label="Site Info"
        class="
          d-flex flex-column justify-content-center text-muted
          flex-lg-row justify-content-lg-between align-items-lg-center pb-lg-3
        "
      >
        <p>{% raw %}
          {{ '©' }}
          <time>{{ 'now' | date: '%Y' }}</time>
          <a href="{{ site.social.links[0] }}">{{ site.social.name }}</a>.
          {% if site.data.locales[include.lang].copyright.brief %}
            <span
              data-bs-toggle="tooltip"
              data-bs-placement="top"
              title="{{ site.data.locales[include.lang].copyright.verbose }}"
            >
              {{- site.data.locales[include.lang].copyright.brief -}}
            </span>
          {% endif %}
        {% endraw %}</p> 
   
        <!-- <p>{% raw %}
          {%- capture _platform -%}
            <a href="https://jekyllrb.com" target="_blank" rel="noopener">Jekyll</a>
          {%- endcapture -%}
  
          {%- capture _theme -%}
            <a href="https://github.com/cotes2020/jekyll-theme-chirpy" target="_blank" rel="noopener">Chirpy</a>
          {%- endcapture -%}
  
          {{ site.data.locales[include.lang].meta | replace: ':PLATFORM', _platform | replace: ':THEME', _theme }}
        {% endraw %}</p> -->
      </footer>
    ```
