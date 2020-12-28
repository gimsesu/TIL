# [Linux] rename으로 파일 이름 일괄 변경

리눅스에서 한 개의 파일 이름을 변경할 땐, 일반적으로 `mv` 명령을 사용한다. 그러나 복수의 파일 이름을 일괄적으로 변경하고 싶다면 `rename`을 사용하는 방법이 있다.

</br>

## 설치

rename이 없다면 다음 명령어로 설치해준다. (**Ubuntu** 기준)

```shell
$ sudo apt install rename
```

## 예시

파일 이름의 접두사를 일괄적으로 변경해보자.

```shell
# 예시 파일

$ ls -l
total 14752416 
-rw-r--r-- 1 root root   88633430 Dec 23 15:10 ucid_20201101.log 
-rw-r--r-- 1 root root  304198571 Dec 23 12:50 ucid_20201102.log 
-rw-r--r-- 1 root root  324424897 Dec 23 12:52 ucid_20201103.log 
-rw-r--r-- 1 root root 1319666218 Dec 23 12:55 ucid_20201104.log 
-rw-r--r-- 1 root root  336508148 Dec 23 12:57 ucid_20201105.log 
[...]
```

</br>

rename의 사용법은 다음과 같다.

```shell
rename 's/<기존문자열>/<바꿀문자열>/' <변경할 파일의 패턴>
```

</br>

파일 이름에서 `ucid_`문자열을  `ucis_`로 변경한다. 변경할 대상은 `ucid_`라는 문자열로 시작하는 모든 파일이다. 

```shell
$ rename 's/ucid_/ucis-/' ucid_*
```

</br>

다음과 같이 변경된 것을 확인할 수 있다.

```shell
$ ls -l 
total 14752416 
-rw-r--r-- 1 root root   88633430 Dec 23 15:10 ucis-20201101.log 
-rw-r--r-- 1 root root  304198571 Dec 23 12:50 ucis-20201102.log 
-rw-r--r-- 1 root root  324424897 Dec 23 12:52 ucis-20201103.log 
-rw-r--r-- 1 root root 1319666218 Dec 23 12:55 ucis-20201104.log 
-rw-r--r-- 1 root root  336508148 Dec 23 12:57 ucis-20201105.log 
[...]
```



## 📜참고

- [How to Use the rename Command on Linux [Website]. (2020.12.28)](https://www.howtogeek.com/423214/how-to-use-the-rename-command-on-linux)

