# 문자열 포맷팅

## 개요

Go언어의 문자열 포맷팅에 대해서 일부 정리했다. 자주 사용하지 않는 동사(`verb`)는 까먹기 때문에 예시와 함께 정리하여 참고할 생각이다.

fmt 패키지는 C 언어의 `printf` 및 `scanf`와 유사한 포맷팅 기능을 구현한다.
표현할 값을 담는 변수의 일종인 동사(`verb`)를 이용한다.

---

- [일반](#일반)
  - [`%v`](#v) &nbsp; [`%+v`](#v-1) &nbsp; [`%#v`](#v-2) &nbsp; [`%T`](#t) &nbsp; [`%%`](#----출력)
- [불리언](#불리언)
  - [`%t`](#t-1)
- [정수형](#정수형)
  - [`%b`](#b) &nbsp; [`%c`](#c) &nbsp; [`%d`](#d) &nbsp; [`%o`](#o) &nbsp; [`%O`](#o-1) &nbsp; [`%q`](#q) &nbsp; [`%x`](#x) &nbsp; [`%X`](#x-1)
- [부동소수점 및 복소수 구성 요소](#부동소수점-및-복소수-구성-요소)
  - [`%b`](#b-1) &nbsp; [`%e`, `%E`](#e-e) &nbsp; [`%f`, `%F`](#f-f) &nbsp; [`%g`, `%G`](#g-g) &nbsp; [`%x`](#x-2) &nbsp; [`%X`](#x-3)
- [문자열 및 바이트 슬라이스](#문자열-및-바이트-슬라이스)
  - [`%s`](#s) &nbsp; [`%q`](#q-1) &nbsp; [`%x`](#x-4) &nbsp; [`%X`](#x-5)
- [슬라이스](#슬라이스)
  - [`%p`](#p)
- [포인터](#포인터)
  - [`%p`](#p-1)
- [복합 객체](#복합-객체)
  - [`struct`](#struct) &nbsp; [`array`, `slice`](#array-slice) &nbsp; [`maps`](#maps) &nbsp; [`pointer`](#pointer)

---



## 형식

### 일반

#### `%v`

```go
// 기본적인 포맷으로 대부분의 값을 출력한다.

type point struct {
	x int
	y float64
	z string
}

fmt.Printf("%v", p)
	// {1 3.1 Hello}

```

#### `%+v`

```go
// 구조체 출력 시 필드 이름을 같이 출력한다.

fmt.Printf("%+v", p)
// {x:1 y:3.1 z:Hello}
```

#### `%#v`

```go
// 값을 다루는 Go문법을 출력한다.

fmt.Printf("%#v", p)
// main.point{x:1, y:3.1, z:"Hello"}
```

#### `%T`

```go
// 값 타입을 다루는 Go문법을 출력한다.

fmt.Printf("%T", p.x)
// int
```

#### `%%` - '%' 출력

```go
fmt.Printf("%%")
// %
```

---

### 불리언

#### `%t`

```go
// 불리언 값을 출력한다.

var imGenius bool
fmt.Printf("%t", imGenius)
// false
```

---

### 정수형

#### `%b`

```go
// 2진수(binary)를 출력한다.

fmt.Printf("%b", 10)
// 1010
```



#### `%c`

```go
// 해당 정수(10진수, 8진수)가 가리키는 유니코드 문자를 출력한다.

fmt.Printf("%c = %c", 65, 0101)
// A = A
```



#### `%d`

```go
// 10진수(decimal)를 출력한다.

fmt.Printf("%d", p.x)
// 1
```



#### `%o`

```go
// 8진수(octal)를 출력한다.

fmt.Printf("%o", 23)
// 27
```



#### `%O`

```go
// 8진수 출력. 맨앞에 '0o'(숫자 0, 소문자 o)가 온다.

fmt.Printf("%O", 23)
// 0o27
```



#### `%q`

```go
// 이스케이프 문자를 작은따옴표(' ')로 감싸서 출력한다.

fmt.Printf("%q", 2)
// '\x02'
```



#### `%x`

```go
// 16진수(hexadecimal)를 출력한다. 알파벳은 소문자

fmt.Printf("%x", 231)
// e7
```



#### `%X`

```go
// 16진수 출력. 알파벳은 대문자

fmt.Printf("%X", 231)
// E7
```

---

### 부동소수점 및 복소수 구성 요소

#### `%b`

```go
// strconv.FormatFloat 함수의 방식을 따라, 지수가 2의 거듭제곱인 십진수가 없는 과학적 표기법으로 출력한다.

var f float64 = 1073741824.1 // 1073741824 = 2^30

fmt.Printf("%b", f)
// 4503599627789926p-22
```



#### `%e`, `%E`

```go
// 과학적 표기법으로 출력한다.

fmt.Printf("%e", f)
// 1.073742e+09

fmt.Printf("%E", f)
// 1.073742E+09
```



#### `%f`, `%F`

```go
// 지수가 없는 소수점을 출력한다.

fmt.Printf("%f", f)
// 1073741824.100000

fmt.Printf("%F", f)
// 1073741824.100000
```



#### `%g`, `%G`

```go
// 지수가 클 경우 %e 로, 그렇지 않으면 %f 로 출력한다.

fmt.Printf("%g", f)
// 1.0737418241e+09

fmt.Printf("%g", 164.1)
// 164.1

fmt.Printf("%G", f)
// 1.0737418241E+09

fmt.Printf("%G", 164.1)
// 164.1
```



#### `%x`

```go
// 16진수를 출력(두 지수의 10진수)

fmt.Printf("%x", f)
// 0x1.0000000066666p+30
```



#### `%X`

```go
// 16진수 출력. 알파벳은 대문자

fmt.Printf("%X", f)
// 0X1.0000000066666P+30
```

---

### 문자열 및 바이트 슬라이스

#### `%s`

```go
// 문자열 또는 슬라이스의 해석되지 않은 바이트를 출력한다.

var soon string = "soon"
bSoon := []byte(soon)

fmt.Printf("%s", soon)
// soon

fmt.Printf("%s", bSoon)
// soon

fmt.Println(bSoon)
// [115 111 111 110]
```

#### `%q`

```go
// 문자열을 큰따옴표(" ")로 감싸서 출력한다.

fmt.Printf("%q", "soon")
// "soon"
```

#### `%x`

```go
// 16진수를 출력한다. 알파벳은 소문자. 바이트 당 2자

fmt.Printf("%x", "Hello World")
// 48656c6c6f20576f726c64
```

#### `%X`

```go
// 16진수를 출력한다. 알파벳은 대문자. 바이트 당 2자

fmt.Printf("%", "Hello World")
// 48656C6C6F20576F726C64
```

---

### 슬라이스

#### `%p`

```go
// 슬라이스 0번째 요소의 주소를 출력한다. 앞에 '0x'가 붙는다.

var sl1 []string
fmt.Printf("%p", sl1)
// 0x0

sl2 := []string{}
fmt.Printf("%p", sl2)
// 0x1396dc8

sl1 = append(sl1, "smile")
sl2 = append(sl2, "smile")

fmt.Printf("%p, %p", sl1, sl2)
// 0xc0001050b0, 0xc0001050c0

fmt.Printf("%p", &sl1)
// 0xc000004b40
```

---

### 포인터

#### `%p`

```go
// 16진수를 출력한다. 알파벳은 소문자. 바이트 당 2자
// %b, %d, %o, %x 그리고 %X 또한 포인터 출력을 지원한다.

sl3 := []int{23}
	fmt.Printf("%b, %d, %o, %x, %X", &sl3, &sl3, &sl3, &sl3, &sl3)
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

#### `struct`

```go
// {field0 field1 ...}

p = point{
	x: 1,
	y: 0.2,
	z: "삼",
}
fmt.Println(p)
// {1 0.2 삼}
```

#### `array`, `slice`

```go
// [elem0 elem1 ...]

sl5 := []string{"일", "영쩜이", "삼"}
fmt.Println(sl5)
// [일 영쩜이 삼]
```

#### `maps`

```go
// map[key1:value1 key2:value2 ..]

m := map[string]interface{}{
	"일":   1,
	"영쩜이": 0.2,
	"삼":   3,
}
fmt.Println(m)
// map[삼:3 영쩜이:0.2 일:1]
```

#### `pointer`

```go
// &{}, &[], &map[]

fmt.Println(&p, &sl5, &m)
// &{1 0.2 삼} &[일 영쩜이 삼] &map[삼:3 영쩜이:0.2 일:1]
```



---

## 결론

소수점 표기 부분은 부족한 영어, 수학 실력으로 해석에 다수 오류가 있을 수도 있습니다.

오류, 오타 환영합니다.

---

## 📜참고

- [fmt - The Go Programming Language[Website]. (2020.11.13)](https://golang.org/pkg/fmt/)
- [Go by Example: 문자열 포맷팅[Website]. (2020.11.13)](https://mingrammer.com/gobyexample/string-formatting/)
- [과학적 기수법 - 위키백과, 우리 모두의 백과사전[Website]. (2020.11.13)](https://ko.wikipedia.org/wiki/과학적_기수법)
- [[컴퓨터 공학 계열] 컴퓨터에서 소수점을 표현하는 방법, 지수 표기  방식에 대해서 : 네이버 블로그[Website]. (2020.11.13)](https://m.blog.naver.com/PostView.nhn?blogId=yun4794&logNo=220989670770&proxyReferer=https:%2F%2Fwww.google.com%2F)
- [소년코딩 - C++ 02.06 - 부동 소수점 숫자 (floating point numbers)[Website]. (2020.11.13)](https://boycoding.tistory.com/152)

