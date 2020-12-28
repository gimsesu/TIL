# HTML Template에서 문자열 합치기

Go의 `html/template` 패키지는 웹앱에서 서비스하는 `.html` 파일에 안전하게 데이터를 표시하기 위해 HTML Template 문법을 지원한다. Template 문법으로 문자열을 합치는 방법에 대해 알아본다.



## 예시 1

```go
{{ $1 := "Hello" }}{{ $2 := "World" }}
{{ $result := (print $1 $2) }} 
// result = "Hello World"
```



## 예시 2

```go
{{ $url := (print "https://www.google.com/search?q=" "www.google.com") }}
<a href={{ $url }} target="_blank">
// <a href="https://www.google.com/search?q=www.google.com" target="_blank">
```







