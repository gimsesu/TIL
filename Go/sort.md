# Go에서 정렬하기

정렬은 실무에서 자주 쓰기 때문에 `Go`에서 정렬을 구현하는 방법을 확실하게 알고 싶어 정리한다. 함수가 필요할 때면 복붙으로 쓰기 바빴다. 그래서 어떤 식으로 구현이 되는 건지 이해하지 못한 채 쓰는 것이 영 찜찜했다.



## `sort.Sort()` 

`Go`에서는 `sort`라는 자체 라이브러리를 제공한다. 기본적으로 `sort.Sort()`를 사용할 수 있다. 자바스크립트를 공부 중인데 정렬 메커니즘이 비슷해서 신기했다.



### 삼박자

`sort.Sort()`의 인자는 '정렬할 슬라이스를 타입으로 갖는 변수'다. 해당 변수는 `Len()`, `Swap()`, `Less()` 세 가지의 [메소드](https://tour.golang.org/methods/1)를 구현해야 한다. `Go`에서 `메소드(Methods)`는 수신자를 갖는 함수를 말한다.

```go
type Interface interface {
	Len() int
	Less(i, j int) bool
	Swap(i, j int)
}
```

### `Len()`

말 그대로 해당 슬라이스의 길이를 반환한다.

### `Less()`

정렬 순서를 결정짓는 함수다. `Less()`는 정렬할 슬라이스의 인덱스 값 두 개를 받아 값을 비교한다. 

  `return a < b` 혹은 `return a > b`로 반환값을 표시한다. 이 의미는 `a < b`는 `true` 혹은 `false`을 갖는다.

  `a < b` 가 `false`면 `a`가 `b`보다 크다는 뜻이므로 `a`와 `b`의 자리를 바꾼다. 오름차순 정렬이다.

  내림차순 정렬을 원한다면 `a > b`를 사용하면 된다.

### `Swap()`

 `swap()`은 정렬할 슬라이스의 인덱스 값 두 개를 서로의 위치를 바꾼다.



## 예시

구조체의 특정 필드 값을 기준으로 정렬하는 방법이다.

```go
// 슬라이스 요소
type Person struct {
	Name string
	Age  int
}

// 슬라이스를 타입으로 갖는 변수
type ByAge []Person

// sort.Sort()에서 요구하는 메소드. 각 메소드는 ByAge를 수신자로 갖는다.
func (a ByAge) Len() int           { return len(a) }
func (a ByAge) Swap(i, j int)      { a[i], a[j] = a[j], a[i] }
func (a ByAge) Less(i, j int) bool { return a[i].Age < a[j].Age } // 나잇값을 기준으로. 오름차순.

func main() {
	people := []Person{
		{"두둠칫", 10},
		{"비피엠", 150},
		{"노림수", 80},
		{"김성수", 31},
	}

	fmt.Println(people)
	// 정렬 전 - [두둠칫: 10 비피엠: 150 노림수: 80 김성수: 31]

	sort.Sort(ByAge(people))
	// 정렬 후 - [두둠칫: 10 김성수: 31 노림수: 80 비피엠: 150]
	fmt.Println(people)
}
```



## ⭐다른 방법 - `sort.Slice()`

위 방법처럼 `sort.Sort()`에 들어갈 인자들을 구현하는 게 번거롭다면, `sort.Slice()`를 이용해 좀 더 간편하게 작성할 방법도 있다. `Slice()`의 인자값은 두 개가 요구되는데,  `(1) 정렬할 슬라이스`와 `(2) Less()` 함수가 들어간다. `Less()`함수는 [클로저](https://www.geeksforgeeks.org/closures-in-golang/#:~:text=Go%20language%20provides%20a%20special,outside%20of%20the%20function%20itself.)를 사용하여 구현할 수 있다.

```go
sort.Slice(people, func(i, j int) bool {
		return people[i].Age > people[j].Age // 나잇값을 기준으로. 내림차순
	})

fmt.Println(people)
// [{비피엠 150} {노림수 80} {김성수 31} {두둠칫 10}]
```

위와 같이 간단하게 구현할 수 있지만, `Go`에서 정렬이 어떻게 구현되는지 알고 싶다면 위의 방법부터 사용해보는 것을 추천한다.



## 📜참고

- [sort - The Go Programming Language[Website]. (2020.12.04)](https://golang.org/pkg/sort/)
- [A Tour of Go[Website]. (2020.12.04)](https://tour.golang.org/methods/1)
- [Closures in Golang - GeeksforGeeks[Website]. (2020.12.04)](https://www.geeksforgeeks.org/closures-in-golang/#:~:text=Go%20language%20provides%20a%20special,outside%20of%20the%20function%20itself.)

