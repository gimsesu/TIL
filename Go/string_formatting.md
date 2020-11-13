# 문자열 포맷팅

## 개요

Go언어의 문자열 포맷팅에 대해서 일부 정리했다. 자주 사용하지 않는 동사(verb)는 까먹기 때문에 예시와 함께 정리하여 참조할 생각이다.

fmt 패키지는 C 언어의 `printf` 및 `scanf`와 유사한 포맷팅 기능을 구현한다.
표현할 값을 담는 변수의 일종인 동사(verb)를 이용한다.

## 형식

### 일반

##### %v

기본적인 포맷으로 대부분의 값을 출력한다.

```go
type point struct {
	x int
	y float64
	z string
}

fmt.Printf("%v\n", p)
	// {1 3.1 Hello}

```

##### %+v

구조체 출력 시 필드 이름을 같이 출력한다.

```go
fmt.Printf("%+v\n", p)
// {x:1 y:3.1 z:Hello}
```

%#v

값을 다루는 Go-문법을 출력한다.

```go
fmt.Printf("%#v\n", p)
// main.point{x:1, y:3.1, z:"Hello"}
```

##### %T

값 타입의 Go-문법을 출력한다.

```go
fmt.Printf("%T\n", p.x)
// int
```

##### %%

'%'을 출력한다

```go
fmt.Printf("%%\n")
// %
```

---

### 불리언

##### %t :불리언 값을 출력한다.

```go
var imGenius bool
fmt.Printf("%t\n", imGenius)
// false
```

---

### 정수형

#### %b : 2진수(binary)를 출력한다.

```go
fmt.Printf("%b\n", 10)
// 1010
```



#### %c

해당 정수(10진수, 8진수)가 가리키는 유니코드 문자를 출력한다.

```go
mt.Printf("%c = %c\n", 65, 0101)
// A = A
```



#### %d : 10진수(decimal)를 출력한다.

```go
fmt.Printf("%d\n", p.x)
// 1
```



#### %o : 8진수(octal)를 출력한다.

```go
fmt.Printf("%o\n", 23)
// 27
```



#### %O : 8진수 출력. 맨앞에 '0o'(숫자 0, 소문자 o)가 온다.

```go
fmt.Printf("%O\n", 23)
// 0o27
```



#### %q : 이스케이프 문자를 작은따옴표(' ')로 감싸서 출력한다.

```go
fmt.Printf("%q\n", 2)
// '\x02'
```



#### %x : 16진수(hexadecimal)를 출력한다. 알파벳은 소문자.

```go
fmt.Printf("%x\n", 231)
// e7
```



#### %X : 16진수 출력. 알파벳은 대문자.

```go
fmt.Printf("%X\n", 231)
// E7
```

---

### 부동소수점 및 복소수 구성 요소

#### %b : strconv.FormatFloat 함수의 방식을 따라, 지수가 2의 거듭제곱인 십진수가 없는 [과학적 표기법](https://ko.wikipedia.org/wiki/과학적_기수법)으로 출력한다.

```go
var f float64 = 1073741824.1 // 1073741824 = 2^30

fmt.Printf("%b\n", f)
// 4503599627789926p-22
```



#### %e, %E : 과학적 표기법으로 출력한다.

```go
fmt.Printf("%e\n", f)
// 1.073742e+09

fmt.Printf("%E\n", f)
// 1.073742E+09
```



##### %f, %F : 지수가 없는 소수점을 출력한다.

```go
fmt.Printf("%f\n", f)
// 1073741824.100000

fmt.Printf("%F\n", f)
// 1073741824.100000
```



##### %g, %G : 지수가 클 경우 `%e` 처럼, 그렇지 않으면 `%f` 처럼 출력한다.

```
fmt.Printf("%g\n", f)
// 1.0737418241e+09

fmt.Printf("%g\n", 164.1)
// 164.1

fmt.Printf("%G\n", f)
// 1.0737418241E+09

fmt.Printf("%G\n", 164.1)
// 164.1
```



#### %x : 16진수를 출력(두 지수의 10진수)

```go
fmt.Printf("%x\n", f)
// 0x1.0000000066666p+30
```



#### %X : 16진수 출력. 알파벳은 대문자

```go
fmt.Printf("%X\n", f)
// 0X1.0000000066666P+30
```

---

### 문자열 및 바이트 슬라이스

##### %s

문자열 또는 슬라이스의 해석되지 않은 바이트를 출력한다.

```go
fmt.Printf("%s\n", "soon")
// soon
```

##### %q

문자열을 큰따옴표(" ")로 감싸서 출력한다.

```go
fmt.Printf("%q\n", "soon")
// "soon"
```

##### %x

16진수를 출력한다. 알파벳은 소문자. 바이트 당 2자

```go
fmt.Printf("%x\n", "Hello World")
// 48656c6c6f20576f726c64
```

%X

16진수를 출력한다. 알파벳은 대문자. 바이트 당 2자

```go
fmt.Printf("%X\n", "Hello World")
// 48656C6C6F20576F726C64
```

---

### 슬라이스

##### %p

슬라이스 0번째 요소의 주소를 출력한다. 앞에 '0x'가 붙는다.

```go
var sl1 []string
fmt.Printf("%p\n", sl1)
// 0x0

sl2 := []string{}
fmt.Printf("%p\n", sl2)
// 0x1396dc8

sl1 = append(sl1, "smile")
sl2 = append(sl2, "smile")

fmt.Printf("%p, %p\n", sl1, sl2)
// 0xc0001050b0, 0xc0001050c0

fmt.Printf("%p\n", &sl1)
// 0xc000004b40
```

---

### 포인터

#### %p : 16진수를 출력한다. 알파벳은 소문자. 바이트 당 2자

%b, %d, %o, %x 그리고 %X 또한 포인터 출력을 지원한다.

```go
sl3 := []int{23}
	fmt.Printf("%b, %d, %o, %x, %X\n", &sl3, &sl3, &sl3, &sl3, &sl3)
	// &[10111], &[23], &[27], &[17], &[17]

sl4 := []float64{1073741824.1}
fmt.Printf("%b\n %x\n %X\n", &sl4, &sl4, &sl4)
// &[4503599627789926p-22]
// &[0x1.0000000066666p+30]
// &[0X1.0000000066666P+30]
```

---

### 복합 객체

복합 객체 요소들은 아래와 같은 규칙을 통해 재귀적으로 출력한다.

##### struct

{field0 field1 ...}

```go
p = point{
	x: 1,
	y: 0.2,
	z: "삼",
}
fmt.Println(p)
// {1 0.2 삼}
```

##### array, slice

[elem0 elem1 ...]

```go
sl5 := []string{"일", "영쩜이", "삼"}
fmt.Println(sl5)
// [일 영쩜이 삼]
```

##### maps

map[key1:value1 key2:value2 ..]

```
m := map[string]interface{}{
	"일":   1,
	"영쩜이": 0.2,
	"삼":   3,
}
fmt.Println(m)
// map[삼:3 영쩜이:0.2 일:1]
```

pointer

&{}, &[], &map[]

```go
fmt.Println(&p, &sl5, &m)
// &{1 0.2 삼} &[일 영쩜이 삼] &map[삼:3 영쩜이:0.2 일:1]
```



---

### 결론

소수점 표기 부분은 부족한 영어, 수학 실력으로 해석에 다수 오류가 있을 수도 있습니다.

오류, 오타에 대해서는 두 팔 벌려 환영합니다.

---

### 참조

- [Package fmt](https://golang.org/pkg/fmt/)
- [Go by Example: 문자열 포맷팅](https://mingrammer.com/gobyexample/string-formatting/)
- [컴퓨터에서 소수점을 표현하는 방법, 지수 표기 방식에 대해서](https://m.blog.naver.com/PostView.nhn?blogId=yun4794&logNo=220989670770&proxyReferer=https:%2F%2Fwww.google.com%2F)
- [소수점 정밀도](https://boycoding.tistory.com/152)