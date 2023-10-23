---
title: "[Study - Front-end basic] JavaScript"
date : 2023-03-11 19:00:00 +09:00
categories : [Study - Front-end basic]
tags : [study, JavaScript]
---

# 객체 활용

---

함수도 객체의 속성으로 쓸 수 있다.

```javascript
var Links = {
    setColor:function(color){
        var alist = document.querySelectorAll('a');
        var i = 0;
        while(i < alist.length){
            alist[i].style.color = color;
            i = i + 1;
        }
    }
}

// Links.setColor('blue') 처럼 객체 사용
```

# Reference

---

[WEB2 - JavaScript - 생활코딩](https://opentutorials.org/course/3085)
