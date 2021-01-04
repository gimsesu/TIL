# [Go] MySQL - Null 값 확인과 생성

Go에서도 MySQL의 Null 값을 확인하거나 생성할 수 있다.



## 준비물

- `database/sql` 패키지



## Go와 NULL

Go에서 `NULL` 값은 필드의 타입에 따라 `sql.Null*`과 같은 구조체 타입으로 지원한다. `sql.NullString`을 예로 들어보자.

</br>

MySQL의 `VARCHAR`나 `TEXT` 타입은 Go에서 `sql.NullString`으로 `NULL` 여부를 확인할 수 있다.

```go
type NullString struct {
	String string
	Valid  bool // Valid is true if String is not NULL
}
```

`sql.NullString.Valid == true` 이면 `String` 값은 `NULL`이 아니다.

## 🥝예시 데이터

- 테이블 `fruits`

| id   | fruit  | taste  |
| ---- | ------ | ------ |
| 1    | 바나나 | 달콤   |
| 2    | 두리안 | (NULL) |
| 3    | 키위   | 새콤   |



## NULL 확인하기

DB에서 특정 필드가 `NULL` 값일 경우, 단순 `string` 타입으로 받으면 에러가 난다. `sql.NullString`으로 받은 다음, `NULL` 여부를 확인해주어야 한다.

```go
query := "SELECT id, fruit, taste FROM fruits"

rows, err := db.Query(query)
if err != nil {
    log.Fatal(err)
}
defer rows.Close()

for rows.Next() {
    var id int
    var fruit string
    var tasteRaw sql.NullString
    
    err = rows.Scan(&id, &fruit, &tasteRaw)
    if err != nil {
        log.Fatal(err)
    }
    
    // Null 값을 확인
    var taste = tasteRaw.String
    if !tasteRaw.Valid {
        taste := "맛이없어"
    }
    taste := CheckNullStr(tasteRaw)
    
    fmt.Printf("id:%d, 과일:%s, 맛:%s\n", id, fruit, taste)
    // id:1, 과일:바나나, 맛:달콤
    // id:2, 과일:두리안, 맛:맛이없어
    // id:3, 과일:키위, 맛:새콤
}
```



## NULL 값 생성하기

Go에서 `string` 타입의 값을 DB에 저장할 때,  `NULL` 값으로 저장하고 싶다면 비어 있는 `sql.NullString` 구조체를 생성하여 쿼리 인자로 넣을 수 있다.

```go
fruit := "방울토마토"
taste := sql.NullString{} // 비어 있는 NullString 구조체

query := "INSERT INTO fruits (fruit, taste) VALUES (?,?)"
_, err := db.Exec(query, fruit, taste)
if err != nil{
	log.Fatal(err)
}
```

</br>

다음과 같이 저장된 데이터를 확인할 수 있다.

| id   | fruit      | taste  |
| ---- | ---------- | ------ |
| 1    | 바나나     | 달콤   |
| 2    | 두리안     | (NULL) |
| 3    | 키위       | 새콤   |
| 4    | 방울토마토 | (NULL) |



## 🏄‍♂️편하게 함수화

`NULL` 값을 사용하는 테이블이라면 수시로 사용할 기능이다. 함수로 빼서 코드를 재사용해보자.

### 1) NULL 확인

```go
func CheckNullStr(s sql.NullString) string {
	if !s.Valid {
		return ""
	}
	return s.String
}	
```

### 2) NULL 생성

```go
func NewNullString(s string) sql.NullString {
	if s == "" {
		return sql.NullString{}
	}
	return sql.NullString{
		String: s,
		Valid:  true,
	}
}
```

