# UTF-8과 EUC-KR의 차이

## UTF-8 

- **Unicode Transformation Format**
- 가장 많이 사용되는 가변 길이 **유니코드** 인코딩
- HTML 파일을 유니코드로 작성할 때는 반드시 UTF-8 이어야 한다
- 한글&한자 1자 = 3 Byte



## EUC-KR

- **Extended Unix Code - Korean** 
- 한글 및 '한국에서 통용되는 한자어' 그리고 영문을 표현
- 일본식 한자/ 중국어의 간체자는 표현할 수 없는 등, 언어의 표현에 제한이 있다.
- 한글 1자 = 2 Byte



## 유니코드?

- 전 세계의 모든 문자를 컴퓨터에서 일관되게 표현하고 다룰 수 있도록 설계된 산업 표준

- 기존의 인코딩들은 그 규모나 표현하는 언어의 범위에서 한계가 있고, 환경에 따라 호환성 문제가 있다.

- 유니코드는 기존 방법들의 한계점을 파악하여 현존하는 모든 문자 인코딩 방법을 교체하고 통합한다.

- 'U+'로 시작한다.

  

## 예시

Go언어의 기본 인코딩은 UTF-8이다. 때문에 문자열의 유니코드 코드 포인트와 UTF-8 인코딩을 쉽게 확인할 수 있다. 

- 유니코드 및 UTF-8 에서 한글은 1글자당 **3byte**, 영문은 **1byte**를 차지한다.
- EUC-KR 에서 한글은 1글자당 **2byte**를 차지한다. 

```go
package main

import (
	"bytes"
	"fmt"

	"golang.org/x/text/encoding/korean"
	"golang.org/x/text/transform"
)

func main() {
	const kor = "안녕"
	const eng = "Hi"

	/* UTF-8 인코딩 */
	fmt.Printf("[% x]\n", []byte(kor))
	// [ec 95 88 eb 85 95]

	fmt.Printf("[% x]\n", []byte(eng))
	// [48 69]

	/* 유니코드 코드 포인트 */
	for i, runeValue := range kor {
		fmt.Printf("%#U starts at byte position %d\n", runeValue, i)
		// U+C548 '안' starts at byte position 0
		// U+B155 '녕' starts at byte position 3
	}

	for i, runeValue := range eng {
		fmt.Printf("%#U starts at byte position %d\n", runeValue, i)
		// U+0048 'H' starts at byte position 0
		// U+0069 'i' starts at byte position 1
	}
    
	/* EUC-KR 인코딩 */
	var buf bytes.Buffer
	w := transform.NewWriter(&buf, korean.EUCKR.NewEncoder())
	w.Write([]byte(kor))
	w.Close()

	euckr := buf.String()

	fmt.Printf("[% x]\n", []byte(euckr))
	// [be c8 b3 e7]
}
```



## 📜참고

- [utf-8 / euc-kr 의 차이  : 네이버 블로그[Website]. (2020.12.18)](https://m.blog.naver.com/PostView.nhn?blogId=junhwen&logNo=130080223604&proxyReferer=https:%2F%2Fwww.google.com%2F)
- [유니코드 - 위키백과, 우리 모두의 백과사전[Website]. (2020.12.18)](https://ko.wikipedia.org/wiki/%EC%9C%A0%EB%8B%88%EC%BD%94%EB%93%9C)
- [List of Unicode characters - Wikipedia[Website]. (2020.12.18)](https://en.wikipedia.org/wiki/List_of_Unicode_characters)
- [Go의 문자열, 바이트, 룬 및 문자-The Go Blog[Website]. (2020.12.18)](https://blog.golang.org/strings)
- [Go 에서 EUC-KR 변환 하기[Website]. (2020.12.18)](https://m.blog.naver.com/PostView.nhn?blogId=nersion&logNo=220884742148&proxyReferer=https:%2F%2Fwww.google.com%2F)

