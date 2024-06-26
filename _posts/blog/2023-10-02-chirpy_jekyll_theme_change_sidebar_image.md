---
title: "[Blog] Chirpy jekyll theme - Change sidebar image"
date: 2023-10-02 01:00:00 +0900
categories: [Blog]
tags: [blog, jekyll, chirpy, image, profile, background]
---

## Intro
---
sidebar에 기본으로 chirpy 테마 사진이 등록되어 있는데 다른 이미지로 블로그를 꾸미고 싶다.

**내가 사용하고 싶은 프로필과 배경 사진을 바꿔보자!**

### Modify file to profile
1. `_config.yml` 수정
    - <span style="color: violet;">img_cdn은 주석처리</span>한다.
    - `/asserts/img` 디렉터리에 원하는 <span style="color: violet;">프로필 이미지를 업로드하고 그 경로를 avatar에 입력</span>한다.

   ``` yml
   # e.g. 'https://cdn.com'
   # img_cdn: "https://chirpy-img.netlify.app"
   
   # the avatar on sidebar, support local or CORS resources
   avatar: "/assets/img/sidebar/profile.jpg"
   ```

### Modify file to background
1. _sass > addon > `commons.scss` 수정
   - 696줄 정도에 있는 #sidebar 스타일에 <span style="color: violet;">원래 있던 background는 주석처리</span>한다.
   - 주석처리한 줄 아래에 <span style="color: violet;">background, background-size, background-position을 추가</span>한다.
   - `/asserts/img` 디렉터리에 <span style="color: violet;">원하는 배경 이미지를 업로드하고 그 경로를 background에 입력</span>한다.
   
   ``` scss
   #sidebar {
       /* background: var(--sidebar-bg); */
       background: url('/assets/img/sidebar/background.jpg');
       background-size: 100% 100%;
       background-position: center;
   }
   ```
   