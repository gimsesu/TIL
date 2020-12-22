# [Go] MySQL에서 여러 ID를 한 번에 조회하기

조회하려는 데이터들의 ID 값이 담긴 슬라이스가 있다고 가정해보자. 그동안 나는 ID 슬라이스를 `for`문으로 순회하여 하나씩 값을 조회했다. 이 방법은 ID당 쿼리 한 번이므로 ID 개수에 비례하여 쿼리 개수가 늘어난다. 한 번의 쿼리로 가져올 수 있다면 성능 면에서 더 이롭다. 한 번에 가져오는 방법을 알아보자.



## WHERE ~ IN

`WHERE 컬럼 IN (조건1, 조건2, 조건3, ...)` 문장으로 여러 ID에 해당하는 데이터를 한 번에 조회할 수 있다. `IN` 조건은 `IN` 뒤에 나열한 조건들 중 일치하는 모든 `row`를 가져온다. 



### 예제

```go
// 저장 데이터
// 	id	|	fruit
//	1	|	"사과"
//	2 	|	"배"
//	3	|	"두리안"
//	4 	|	"한라봉"
//	5	|	"토마토"

ids := []string{"1", "3", "5"}
args := make([]interface{}, len(ids))
for i, id := range ids {
    args[i] = id
}

query := `SELECT id, fruit FROM table WHERE id in (?`+strings.Repeat(",?", len(ids)-1)+`)`

rows, err := db.Query(query, args...)
if err != nil {
    log.Println(err)
    return
}

for rows.Next() {
    var id int
    var fruit string
    
    err = rows.Scan(&id, &fruit)
    if err != nil {
        log.Println(err)
        return
    }

    fmt.Printf("id: %d, fruit: %s\n", id, fruit)
    // id: 1, fruit: 사과
    // id: 3, fruit: 배
    // id: 5, fruit: 토마토
}
```



## 📜참고

- [go - golang slice in mysql query with where in clause - Stack Overflow[Website]. (2020.12.22)](https://stackoverflow.com/questions/45351644/golang-slice-in-mysql-query-with-where-in-clause)
- [WHERE 절의 IN 사용법![Website]](https://ojava.tistory.com/12)



