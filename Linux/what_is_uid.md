# What is uid?

## 개요

uid == User ID. Unix 계열의 사용자 식별 번호.

uid는 양의 정수로 0부터 32767까지 사용한다.

슈퍼유저, 즉 root의 uid는 0 이다.



## uid 조회하기

`/etc/passwd`에서 확인할 수 있다.

``` shell
$ cat /etc/passwd
(이름):(패스워드):(uid):(gid):(홈디렉토리):(쉘환경)
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
...

```

