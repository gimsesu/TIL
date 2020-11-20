# 크롤링 가능한 경로 확인하기

웹 사이트 운영자는 `robots.txt`에 웹 크롤링을 금지하는 경로들을 기술해 놓을 수 있다.  웹 크롤러를 이용하여 특정 웹사이트를 크롤링하고자 할 때, `robots.txt`를 조회하는 방법에 대해 간략히 정리한다.



## 규칙

규칙은 매우 간단하다.

```
User-agent: *  (제어할 특정 사용자 에이전트. *: 모든 에이전트)

Allow: /a/b    (특정 디렉토리 접근 허가)

Allow: /       (모든 디렉토리 접근 허가)

Disallow: /c/d (특정 디렉토리 접근 금지)

Disallow: /    (모든 디렉토리 접근 금지)

Sitemap: http://www.example.com/sitemap.xml (이 사이트의 사이트맵 파일 위치)
```



## 조회 예시

`robots.txt`는 일반적으로 웹사이트 주소의 최상위 경로에 존재한다.

```shell
https://www.google.com/robots.txt
```



```shell
$ curl -X GET "https://google.com/robots.txt"

User-agent: *
Disallow: /search
Allow: /search/about
Allow: /search/static
Allow: /search/howsearchworks
Disallow: /sdch
Disallow: /groups

...

Sitemap: https://www.google.com/sitemap.xml
```



## 📜참고

- [Create a robots.txt file  | Google 검색 센터  | Google Developers[Website]. (2020.11.19)](https://developers.google.com/search/docs/advanced/robots/create-robots-txt?hl=ko)

