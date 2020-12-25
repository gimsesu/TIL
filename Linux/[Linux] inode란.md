# [Linux] inode란?

Photorec를 이용해 파일을 복구하고 나니 서버의 inode가 가득 찼다. 분명 서버 디스크 스토리지는 부족하지 않은데, 임시 파일 생성이 되지 않길래 무슨 일이지 하고 있는데 팀장님이 알려 주셨다 ^^;. Phtorec는 텍스트 파일의 경우, 하나의 파일을 이전 형태로 온전히 복구하지 않고 텍스트들이 분리되어 조각조각이 난 파일 모음으로 복구한다. 그래서 매우 많은 수의 파일이 떨어진다.

</br>

**에러**

```shell
$ cd /va-bash: cannot create temp file for here-document: No space left on device
```

</br>

**inode 사용량 조회**

```shell
$ df -i 
Filesystem       Inodes    IUsed    IFree IUse% Mounted on 
udev           16488783      470 16488313    1% /dev 
tmpfs          16491586      580 16491006    1% /run 
/dev/dm-0      20840448 20840448        0  100% / 
none           16491586        2 16491584    1% /sys/fs/cgroup 
none           16491586        3 16491583    1% /run/lock 
none           16491586        1 16491585    1% /run/shm 
none           16491586        2 16491584    1% /run/user 
/dev/sda1         62248      302    61946    1% /boot
```

---

##  indoe가 뭡니까..?🧐

inode(아이노드)는 ***index node*** 의 약자로 유닉스/리눅스 운영체제에서 사용하는 일종의 자료구조다.

</br>

리눅스에서 파일/디렉토리는 실제 데이터와 파일의 메타데이터로 이루어져 있다. 아이노드는 메타데이터를 가리킨다(파일을 읽는 데 필요한 모든 관리 데이터를 저장한다.). 모든 파일/디렉토리는 아이노드를 1개씩 가지고 있다.

</br>

## 구성 요소

- inode number(아이노드 번호)
- 파일 크기, 파일 주소
- 파일이 저장된 모든 *블록* 목록
- 소유자 정보(사용자 ID, 그룹 ID)
- 파일 모드(권한)
- 파일 유형: 일반 파일, 디렉토리, 파이프 등
- 링크 수: indoe에 상대적인 하드링크 수

</br>

💡블록: 파일 시스템에서 데이터를 저장하는 단위. 메모리에서 I/O 작업을 한 번 거칠 때 읽거나 쓰는 단위

</br>

## 구조

`ext3` 또는 `ext4`와 같은 리눅스 확장 파일 시스템은 ***indoe table*** 이라고 하는 아이노드 배열을 유지한다. 테이블에는 해당 파일 시스템의 모든 파일 목록이 들어있다. 아이노드 테이블 안의 각 아이노드들은 ***inode number*** 라는 고유 식별자가 있다. 

</br>

## inode number

아이노드 번호는 ***인덱스 번호*** 라고도 한다. 파일/디렉토리가 생성될 때 고유하게 부여되는 아이노드들 간의 식별자다.

</br>

## 아이노드 확인

### stat

`stat ` 명령으로 파일/디렉토리의 아이노드 데이터를 확인할 수 있다.

```shell
$ stat test
  File: ‘test’ 
  Size: 12              Blocks: 8          IO Block: 4096   regular file 
Device: fc00h/64512d    Inode: 18613173    Links: 1 
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root) 
Access: 2020-12-24 09:02:29.119373414 +0900 
Modify: 2020-12-24 09:02:29.119373414 +0900 
Change: 2020-12-24 09:02:29.119373414 +0900 
 Birth: -
```

</br>

`--format` 옵션으로 아이노드 번호만 조회할 수도 있다.

```shell
$ stat --format=%i test
18613173
```

### ls

`ls` 명령으로 파일/디렉토리의 정보 목록을 조회할 수 있다. `-i`  옵션을 이용해 아이노드번호를 표시할 수 있다. 

```shell
$ ls -i 
18613151 nohup.out  19740925 recover_ucid_logs.sh  18613173 test
```

</br>

`-l` 옵션을 조합하여 세부 정보를 같이 나열할 수 있다.

```shell
$ ls -il 
total 12 
18613151 -rw------- 1 root root 1817 Dec 23 15:10 nohup.out 
19740925 -rwxr-xr-x 1 root root  416 Dec 23 15:08 recover_ucid_logs.sh 
18613173 -rw-r--r-- 1 root root   12 Dec 24 09:02 test
```

💡`total`: 조회한 파일/디렉토리의 총 크기(KB)

</br>

## 파일 시스템별 inode 공간 조회

서두에서도 보았다시피 `df` 명령으로 파일 시스템에서 inode의 할당 공간과 사용량을 조회할 수 있다. `df` 명령은 일반적으로 디스크의 사용 정보를 표시하지만, `-i` , `--inodes` 옵션을 이용해 아이노드의 사용 정보를 확인할 수 있다.

```shell
$ df -i 
Filesystem       Inodes IUsed    IFree IUse% Mounted on 
udev           16488783   470 16488313    1% /dev 
tmpfs          16491586   580 16491006    1% /run 
/dev/dm-0      20840448 89099 20751349    1% / 
none           16491586     2 16491584    1% /sys/fs/cgroup 
none           16491586     3 16491583    1% /run/lock 
none           16491586     1 16491585    1% /run/shm 
none           16491586     2 16491584    1% /run/user 
/dev/sda1         62248   302    61946    1% /boot
```

</br>

`-h` 옵션을 조합하여 사람이 보기 편한 단위로 표시해준다.

```shell
$ df -ih
Filesystem     Inodes IUsed IFree IUse% Mounted on 
udev              16M   470   16M    1% /dev 
tmpfs             16M   580   16M    1% /run 
/dev/dm-0         20M   88K   20M    1% / 
none              16M     2   16M    1% /sys/fs/cgroup 
none              16M     3   16M    1% /run/lock 
none              16M     1   16M    1% /run/shm 
none              16M     2   16M    1% /run/user 
/dev/sda1         61K   302   61K    1% /boot
```

</br>

## 📜참고
- [반달가면 : 리눅스에서 디스크 용량이 충분한데 더 이상 쓰기가 안 되는 경우 발생[Website]. (2020.12.25)](http://bahndal.egloos.com/602209)
- [Detailed Understanding of Linux Inodes with Example[Website]. (2020.12.25)](https://linoxide.com/linux-command/linux-inode)
- [아이노드 ( i-node ) : 네이버 블로그[Website]. (2020.12.25)](https://m.blog.naver.com/PostView.nhn?blogId=s2kiess&logNo=220124665335&proxyReferer=https:%2F%2Fwww.google.com%2F)




