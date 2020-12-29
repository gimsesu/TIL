# [Git] 파일 이름 변경 - 소문자에서 대문자로

Git 환경 안에서 파일 혹은 디렉토리 이름을 변경하려면 `git mv` 명령어를 사용한다. 

그러나 바꾸려는 파일 이름이 대/소문자만 다른 경우, Git은 이름을 변경하는 것으로 인식하지 않는다. 

```shell
$ git mv web wEb
Rename from 'web' to 'wEb/web' failed. Should I try again? (y/n)
```



## 👏우회

이름을 변경하려면 조금 돌아가야 한다.

```shell
$ git mv web webs
```

```shell
$ git mv webs wEb
```

그리고 변경 사항은 반드시 커밋을 해준다.



## 📜참고

- [Issue with renaming a directory in git to lowercase while ignoreLowercase=True - Stack Overflow[Website]. (2020.12.29)](https://stackoverflow.com/questions/13201906/issue-with-renaming-a-directory-in-git-to-lowercase-while-ignorelowercase-true)