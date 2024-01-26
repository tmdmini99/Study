![](https://www.mireene.com/webimg/btn_h2.gif)**퍼미션(권한)이란?**

|   |   |   |   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|---|---|---|
|Owner|   |   |Group|   |   |Other|   |   |Owner와 Group은 파일소유자자신과 자신이 속한그룹. Other은 제3자, 웹사이트 방문객은 제3자로 nobody로 취급.|
|r|w|x|r|w|x|r|w|x|r은 파일 읽기(4), w는 파일 쓰기(2), x는 파일 실행(1)|
|7|   |   |5|   |   |5|   |   |파일소유자는 그것을 읽고 쓰고 실행시킬 수 있지만, 제3자는 읽고 실행만 시킬 수 있다.|
|7|   |   |7|   |   |7|   |   |제3자도 쓰기 권한이 주어진다.|

*.html  *.cgi, *.pl *.txt등의 파일은 업로드시 반드시 ascii로 하고 나머지 그림(*.gif *.jpg)이나 자바 애플릿(*.class), 실행파일(*.exe *.zip *.rar)등은 binary mode로 업로드 할 것.

   ![](https://www.mireene.com/webimg/btn_h2.gif)**리눅스 기본명령어**

|   |   |
|---|---|
|**명령어**|**사 용 법**|
|login|사용자 인증과정<br><br>리눅스 시스템은 기본적으로 multi-user 개념에서 시작하였기 때문에 시스템을 이용하기 위해서는 반드시 로그인을 하여야 합니 다. 로그인은 PC 통신에서도 많이 사용되어져 왔기 때문에 그 개 념  설정에 그다지 어려움이 없을 것입니다. 흔히 말하는 ID를 입력하는 과정입니다.|
|passwd|패스워드 변경<br><br>리눅스, 특히 인터넷의 세계에서는 일반 컴퓨팅 상황에 비하여 훨씬 해킹에 대한 위험이 높습니다. 패스워드는 완성된 단어 보다는 단어 중간에 숫자나 키보드의 ^, #, ' 등과 같은 쉽게 연상 할 수 없는 기호를 삽입하여 만들어 주는 것이 좋습니다|
|du|하드사용량 체크(chkdsk)<br><br>자신의 하드공간을 알려면  <br># du  <br>특정 디렉토리의 사용량을 알려면  <br># du -s diretory_name|
|ls|파일 리스트 보기(dir)<br><br>F : 파일 유형을 나타내는 기호를 파일명 끝에 표시  <br>    (디렉토리는 '/', 실행파일은 '*', 심볼릭 링크는 '@'가 나타남).  <br>l  : 파일에 관한 상세 정보를 나타냅니다.  <br>a : dot 파일(.access 등)을 포함한 모든 파일 표시.  <br>t  : 파일이 생성된 시간별로 표시  <br>C : 도스의 dir/w명령과 같 이 한줄에 여러개의 정보를 표시  <br>R : 도스의 dir/s 명령과 같이 서브디렉토리 내용까지.<br><br>(예)  <br># ls -al    <br># ls -aC  <br># ls -R|
|cd|디렉토리를 변경<br><br># cd cgi-bin     : 하부 디렉토리인 cgi-bin으로 들어감.  <br># cd  ..             : 상위디렉토리로 이동  <br># cd 또는 cd ~  : 어느곳에서든지 자기 홈디렉토리로 바로 이동  <br># cd /webker     : 현재 작업중인 디렉토리의 하위나 상위 디렉토리가  <br>                          아닌 다른 디렉토리(webker)로 이동하려면 /로  <br>                          시작해서 경로이름을 입력하면 된다.|
|cp|일 복사(copy)<br><br># cp index.html index.old  <br>     : index.html 화일을 index.old 란 이름으로 복사.  <br>  <br># cp /home/test/*.*  .  <br>     : test 디렉토리내의 모든 화일을 현 디렉토리로 복사. |
|mv|파일이름(rename) / 위치(move)변경<br><br># mv index.htm index.html  <br>     : index.htm 화일을 index.html 로 이름 변경<br><br>$ mv file  ../main/new_file  <br>     : 파일의 위치변경|
|mkdir|디렉토리 생성<br><br># mkdir download  : download 디렉토리 생성|
|rm|화일삭제<br><br># rm test.html : test.html 화일 삭제  <br># rm -r <디렉토리> : 디렉토리 전체를 삭제  <br># rm -i a.*  <br>     : a로 시작하는 모든 파일을 일일이 삭제할 것인지 확인하면서 삭제|
|rmdir|디렉토리 삭제<br><br># rmdir cgi-bin : cgi-bin 디렉토리 삭제|
|pwd|현재의 디렉토리 경로를 보여주기|
|pico|리눅스용 에디터|
|put|ftp 상태에서 화일 업로드<br><br>> put  guestbook.tar.gz|
|get|ftp 상태에서 화일 다운로드<br><br>> get  guestbook.tar.gz|
|mput 또는 mget|여러개의 화일을 올리고 내릴때 (put,get과 사용법동일)|
|chmod|파일 permission 변경<br><br>리눅스에서는 각 화일과 디렉토리에 사용권한을 부여.<br><br>예) -rwxr-xr-x   guestbookt.html  <br>rwx  :처음 3개 문자 = 사용자 자신의 사용 권한  <br>r-x  :그다음 3개 문자 = 그룹 사용자의 사용 권한  <br>r-x  :마지막 3개 문자 = 전체 사용자의 사용 권한<br><br>읽기(read)---------- 화일 읽기 권한  <br>쓰기(write)---------- 화일 쓰기 권한  <br>실행(execution)---------- 화일 실행 권한  <br>없음(-)---------- 사용권한 없음<br><br>명령어 사용법  <br>chmod [변경모드] [파일]<br><br># chmod 666  guestbook.html  <br>     : test.html 화일을 자신에게만 r,w,x 권한을 줌<br><br># chmod 766  guestbook.html  <br>     : 자신은 모든 권한을 그룹사용자와,전체사용자에게는  <br>       읽기와 쓰기 권한만 줌 |
|alias|" doskey alias" 와 비슷하게 이용할 수 있는 쉘 명령어 alias는 말그대로 별명입니다. 사용자는 alias를 이용하여 긴 유 닉스 명령어를 간단하게 줄여서 사용할 수도 있습니다.  <br>이들 앨리어스는 [alias ls 'ls -al'] 같이 사용하시면 되는데, 한 번 지정한 alias를 계속해서 이용하시려면, 자신의 홈디렉토리에 있는  <br>.cshrc(Hidden 속성)을 pico등의 에디터를 이용하여 변경시 키면 됩니다.|
|cat|파일의 내용을 화면에 출력하거나 파일을 만드는 명령( 도스의 TYPE명령)  <br>  <br># cat filename|
|more|cat 명령어는 실행을 시키면 한 화면을 넘기는 파일일 경우 그 내용을 모두 볼수가 없다. 하지만 more 명령어를 사용하면 한 화면 단위로 보여줄 수 있어 유용.<br><br># more <옵션>  <br>옵션은 다음과 같습니다.  <br>  <br>Space bar : 다음 페이지  <br>Return(enter) key : 다음 줄  <br>v : vi 편집기로 전환  <br>/str : str 문자를 찾음  <br>b : 이전 페이지  <br>q : more 상태를 빠져나감  <br>h : 도움말  <br>= : 현재 line number를 보여줌|
|who|현재 시스템에 login 하고 있는 사용자의 리스트를 보여줍니다.<br><br># who|
|whereis|소스, 실행파일, 메뉴얼 등의 위치를 알려줍니다<br><br># whereis perl : perl의 위치를 알려준다|
|vi,  <br>touch,  <br>cat|새로운 파일을 만드는 방법  <br><br># vi newfile :  vi 편집기 상태로 들어감  <br># touch newfile : 빈 파일만 생성됨  <br># cat > newfile  : vi 편집기 상태로 들어감, 문서 작성후 Ctrl+D로 빠져나옴|
|cat,  <br>head,  <br>tail|파일 내용만 보기  <br><br># cat filename         : 파일의 내용을 모두 보여줌  <br># head -n filename : n줄 만큼 위세서부터 보여줌  <br># tail -n filename     : n줄 만큼 아래에서부터 보여줌|

   ![](https://www.mireene.com/webimg/btn_h2.gif)**압축명령어 사용법**

|   |   |
|---|---|
|압축 명령어|사 용 법|
|tar|.tar, _tar로 된 파일을 묶거나 풀때 사용하는 명령어  <br>(압축파일이 아님)  <br>  <br># tar cvf [파일명(.tar, _tar)] 압축할 파일(또는 디렉토리): 묶을때  <br># tar xvf [파일명(.tar, _tar)]  :  풀 때  <br>   (cf) cvfp/xvfp 로 하면 퍼미션 부동|
|compress|확장자 .Z 형태의 압축파일 생성  <br>  <br># compress    [파일명]     : 압축시  <br># uncompress [파일명]    : 해제시|
|gzip|확장자  .gz, .z 형태의 압축파일 생성  <br>  <br>#  gzip     [파일명]    : 압축시  <br>#  gzip -d [파일명]   : 해제시|
|기타|.tar.Z  <br>이것은 tar로 묶은 후에 compress를 사용하여 압축한 것으로 uncompress를 사용해서 압축을 푼 다음,  <br>다시 tar를 사용해서 원래의 파일들을 만들어내면 됩니다.  <br>아니면 다음과 같이 한 번에 풀 수도 있다.  <br># zcat  [파일명].tar.Z  : 해제시  <br>  <br>.tar.gz또는 .tar.z  <br># gzip -cd [파일명]    : 해제시  <br>  <br>.tar.gz 또는 .tar.z .tgz  <br>gzip을 사용해서 푼 다음 다시 tar를 사용해서 원래 파일을 만들어 낼 수 있으나,  <br>하지만 다음과 같이 하면 한 번에 처리를 할 수 있다.  <br>  <br># gzip -cd 파일.tar.gz \| tar xvf -  또는  <br># tar xvzf 파일.tar.gz  <br># tar xvzf 파일.tgz|

   ![](https://www.mireene.com/webimg/btn_h2.gif)**리눅스 필수명령어**

|   |   |   |
|---|---|---|
|**Linux/Unix 명령어**|**설 명**|**MS-DOS 비교**|
|./_x_|x 프로그램 실행  <br>(현재 디렉토리에 있는 것)|x|
|↑/ ↓|이전에(↑) / 다음에(↓) 입력했던 명령어|doskey|
|cd _x_ (또는 cd /_x_)|디렉토리 X로 가기|cd|
|cd .. (또는 cd ../ 또는 cd /..)|한 디렉토리 위로 가기|cd..|
|x 다음 [tab] [tab]|x 로 시작하는 모든 명령어 보기|-|
|adduser|시스템에 사용자 추가|/|
|ls (또는 dir)|디렉토리 내부 보여주기|dir|
|cat|터미널 상의 텍스트 파일 보기|type|
|mv _x y_|파일 x를 파일 y로 바꾸거나 옮기기|move|
|cp _x y_|파일 x를 파일 y로 복사하기|copy|
|rm x|파일 지우기|del|
|mkdir _x_|디렉토리 만들기|md|
|rmdir _x_|디렉토리 지우기|rd|
|rm -r _x_|디렉토리 x를 지우고 하위도 다 지우기|deltree|
|rm p|패키지 지우기|-|
|df (또는 df _x_)|장치 x의 남은 공간 보여주기|chkdsk ?|
|top|메모리 상태 보여주기(q는 종료)|mem|
|man _x_|명령어 x에 관한 매뉴얼 페이지 얻기|/|
|less _x_|텍스트 파일 x 보기  <br>(리눅스에서는 더 많은 필터 적용 가능)|type x \| more|
|echo|어떤 것을  echo 화면에 인쇄한다.|echo|
|mc|UNIX를 위한 노턴 커맨더|nc|
|mount|장치 연결(예: CD-ROM, 연결을 해제하려면 umount)|-|
|halt|시스템 종료|-|
|reboot ([ctrl] + [alt] +[del])|시스템  다시 시작하기|[ctrl] + [del] + [del]|

    **![](https://www.mireene.com/webimg/btn_h2.gif)고급명령어**

|   |   |
|---|---|
|**고급 명령어**||
|chmod <권한> <파일>|파일 권한(permissions) 변경|
|ls -l _x_|파일 x의 자세한 상황을 보여줌|
|ln -s _x y_|x에서 y로 심볼릭 링크를 만들어 줌|
|find x -name y -print|디렉토리 x안에서 파일 y를 찾아서 화면에 그 결과를 보여줌|
|ps|지금 작동중인 모든 프로세스들을 보여줌|
|kill _x_|프로세스 x를 종료 (x는 ps 명령으로 알 게 된 PID)|
|[alt] + F1 - F7|터미널 1-7까지 바꾸기 (텍스트 터미널에서; F7은 X-윈도우(시작될때))|
|lilo|부트 디스크를 만듦|
|**용어**||
|symlink|다른 파일이나 디렉토리로 심볼릭 링크. 윈도유98의 바로가기 같은 것|
|shell script|여러 명령어들을 차례로 수행하게 한 것. MS-DOS의 배치 파일 같은 것|

     **![](https://www.mireene.com/webimg/btn_h2.gif)팁!!**

 **- 웹에서 생성한 노바디파일 삭제 하는방법..**

기본적으로 웹서버는 nobody 권한으로 동작이 되게 됩니다.  
고객님께서 FTP 로 접속하여 전송한 파일이 아니라 웹상에서 사용자들이 파일을 업로드 한 경우나 웹상에서 생성된 파일의 경우 삭제가 되지 않는 경우가 있을 수 있습니다.

웹서버의 동작 권한은 nobody 이고 웹상에서 생성된 파일이므로 해당 파일이 nobody 소유권으로 시스템에 생성이 되게 됩니다.

아래와 같이 웹상에서 실행시키면 됩니다.

1. 메모장을 열어 아래 소스를 붙여넣기 하신후..

<?

//폴더/파일 삭제시

$cmd = `rm -rf 노버디로된파일혹은폴더명`;

echo "$cmd";

echo "폴더가 삭제 되었습니다.";

?>

-- 위에까지..  
-- **위에서 수정할 사항은 "노버디로된파일혹은폴더명"을 삭제하시고자 하는 파일명으로 바꿔주세요..

2. 파일 -> 다른이름으로저장 -> 아래 탭에서 파일형식을 "모든파일"로 선택후

   -> "원하는파일명.php" 로 저장 (ex: del.php)

3. ftp를 통해 고객계정에 파일업로드를 하시고 웹에서 파일을 불러주시면 됩니다

   ex: html폴더안에/temp 안에 삭제하고자하는 파일이 있을경우 / html폴더/temp안에 del.php를 업로드하고..

       브라우저에서 http://고객도메인/temp/del.php 를 하면 됩니다

4. 실행하시면 삭제되고 nobody 권한의 폴더만 남습니다.(폴더안의화일들만 지워짐)

   그후 ftp 접속후 폴더를 삭제하시면 됩니다.

ex)

<?

퍼미션 변경시

$cmd = `chmod -R 777 노버디로된파일혹은폴더명`;

echo "$cmd";

echo "퍼미션 변경되었습니다.";

?>





### **01 pwd**

pwd는 print work directory의 약자로 작업 중인 디렉터리를 보여줍니다.

```plaintext
$ pwd
/Users/gyus
```



### **02 ls**

list segments의 약자로 현재 디렉터리의 파일과 디렉터리를 보여줍니다. 보통 단독으로 잘 사용하지 않고 a, l 등의 옵션을 함께 사용합니다.

- ls : 디렉토리 목록 확인
- ls -l : 파일들의 상세 정보를 보여줌
- ls -a : 숨김 파일 표시
- ls -t : 최신 파일부터 표시
- ls -rt : 오래된 파일부터 표시
- ls -F : 파일을 표시할 때 파일의 타입을 나타내는 문자열을 표시( / 디렉터리, * 실행 파일, @ 심볼릭 링크)
- ls -R : 하위 디렉터리의 내용까지 표시

보통은 위 옵션들을 조합해 ls -al, ls -alt, ls -altF 등으로 사용합니다.

* 심볼릭 링크(symbolic link): 원본 파일을 가리키도록 링크만 연결시켜둔 겁니다. 윈도우의 바로가 기 링크와 같은 개념입니다.

### **03 cd**

change directory의 약자로 말 그대로 디렉터리 이동 시 사용하는 명령입니다.

- cd ~ : 홈디렉터리로 이동
- cd .. : 상위 디렉터리로 이동. cd ../../ 같은 식으로 여러 단계를 한 번에 이동 가능
- cd /dir : 절대 경로를 지정해 이동 가능
- cd – : 바로 전의 디렉터리로 이동

### **04 mkdir**

make directory의 약자로 디렉터리를 만들 때 사용합니다.

```plaintext
# <이름>의 디렉터리를 현재 디렉터리에 만듭니다.
$ mkdir <이름>
```



\# -p 옵션으로 하위 디렉터리까지 한 번에 생성할 수 있습니다.  
$ mkdir -p <디렉터리명>/<하위 디렉터리명>

### **05 cp**

copy의 약자입니다. 파일 또는 디렉터리를 복사할 때 사용합니다.

```plaintext
# source를 target으로 복사하기
$ cp source target

# target 파일이 이미 있는 경우 덮어쓰기
$ cp -f source target

# 디렉터리를 복사할 때 사용. 하위 디렉터리도 모두 복사하기
$ cp -R sourceDir targetDir
```



### **06 mv**

move의 약자입니다. 파일 또는 디렉터리의 위치를 옮길 때 사용합니다. 혹은 이름을 변경할 때도 사용합니다.

```plaintext
# afile 이름을 bfile로 변경
$ mv afile bfile

# afile을 상위 디렉터리로 옮김
$ mv afile ../

# afile을 /opt 이하 디렉터리로 옮김
$ mv afile /opt/
```



### **07 rm**

remove의 약자입니다. 파일 또는 디렉터리를 삭제할 때 사용합니다.

```plaintext
# afile을 삭제
$ rm afile

# 디렉터리 adir을 삭제. 삭제 시 확인을 함
$ rm -r adir

# 디렉터리 adir을 삭제. 삭제 시 확인 안 함
$ rm -rf adir

# txt로 끝나는 모든 파일을 삭제할지 물어보면서 삭제
$ rm -i *.txt
```



### **08 cat**

catenate (잇다 연결하다)의 약자입니다. 파일의 내용을 확인할 때 사용합니다.

```plaintext
# test.txt 파일의 내용을 확인
$ cat test.txt
```


### **09 touch**

touch는 빈 파일을 생성합니다. 혹은 파일의 날짜와 시간을 수정할 때 사용합니다.

```plaintext
# afile을 생성
$ touch afile

# afile의 시간을 현재 시간으로 갱신
$ touch -c afile

# bfile의 날짜 정보를 afile의 정보와 동일하게 변경
$ touch -r afile bfile
```



### **10 echo**

echo는 어떤 문자열을 화면에 보여줄 때 사용합니다. echo와 리다이렉션을 사용해 파일을 생성, 추가하는 작업을 많이 합니다.

```plaintext
# helloworld 출력
$ echo 'helloworld'

# 패스로 지정된 문자열을 출력
$ echo $PATH

# 이스케이프 문자열을 해석
$ echo -e 문자열

# 개행을 표시할 수 있음
$ echo -e "안녕하세요\n이렇게 하면\n새 줄이생겨요"

# ls와 유사하게 현재 디렉터리의 파일과 폴더를 출력
$ echo *

# 리다이렉션 '>'을 사용해 hello.txt 파일 생성. 파일 내용에는 echo로 표시되는 내용이 들어감
$ echo hello redirection > hello.txt

# 추가 연산자 >>를 사용해 기존 파일에 문자열 추가
$ echo hello2 >> hello.txt
```



### **11 ip addr / ifconfig**

접속한 리눅스의 IP 정보를 알아낼 때 사용합니다.

```plaintext
$ ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default
qlen 1000
link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
inet 127.0.0.1/8 scope host lo
valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP
group default qlen 50000
link/ether fa:16:3e:5d:0b:d7 brd ff:ff:ff:ff:ff:ff
inet 10.201.1.10/16 brd 10.202.255.255 scope global eth0
valid_lft forever preferred_lft forever
```



ip addr이 설치되어 있지 않은 경우에는 ifconfig를 사용하면 됩니다.

```plaintext
$ ifconfig
eth0 Link encap:Ethernet HWaddr 06:4d:de:ae:a8:50
inet addr:172.31.27.212 Bcast:172.31.31.255 Mask:255.255.240.0
inet6 addr: fe80::44d:deff:feae:a850/64 Scope:Link
UP BROADCAST RUNNING MULTICAST MTU:9001 Metric:1
RX packets:68903966 errors:0 dropped:0 overruns:0 frame:0
TX packets:75295223 errors:0 dropped:0 overruns:0 carrier:0
collisions:0 txqueuelen:1000
RX bytes:15691124260 (15.6 GB) TX bytes:42265387295 (42.2 GB)

lo Link encap:Local Loopback
inet addr:127.0.0.1 Mask:255.0.0.0
inet6 addr: ::1/128 Scope:Host
UP LOOPBACK RUNNING MTU:65536 Metric:1
RX packets:6623596 errors:0 dropped:0 overruns:0 frame:0
TX packets:6623596 errors:0 dropped:0 overruns:0 carrier:0
collisions:0 txqueuelen:1
RX bytes:349206971 (349.2 MB) TX bytes:349206971 (349.2 MB)
```



### **12 ss**

socket statistics의 약자입니다. 네트워크 상태를 확인하는 데 사용합니다. 원래는 netstat를 사용했는데, 최근에는 ss를 주로 사용합니다. 옵션으로 a, t, u, l, p, n 등이 있습니다.

- ss -a : 모든 포트 확인
- ss -t : TCP 포트 확인
- ss -u : UDP 포트 확인
- ss -l : LISTEN 상태 포트 확인
- ss -p : 프로세스 표시
- ss -n : 호스트, 포트, 사용자명을 숫자로 표시

TCP 포트 중 LISTEN 상태인 포트의 번호를 알고 싶을 때 다음과 같이 합니다.

```plaintext
$ ss -tln
LISTEN 0 511 *:443 *:*
LISTEN 0 1 127.0.0.1:8006 *:*
LISTEN 0 511 *:80 *:*
```



### **13 nc**

netcat의 약자입니다. 예전에는 포트가 열렸는지 확인하는 데 telnet 명령어를 사용했지만 요즘은 주로 nc를 사용합니다.

```plaintext
# 포트가 오픈됐는지 확인
$ nc IP주소 포트

# 더 자세한 정보가 남음
$ nc -v IP주소 포트

# 현재 서버의 포트를 오픈(방화벽에 해당 포트 번호가 설정 함)
$ nc -l 포트
```



### **14 which, whereis, locate**

which는 특정 명령어의 위치를 찾아줍니다.

```plaintext
$ which git
/usr/local/bin/git

# which -a : 검색 가능한 모든 경로에서 명령어를 찾아줍니다.
$ which -a git
/usr/local/bin/git
/usr/bin/git

# where : which -a와 같습니다.
$ where git
/usr/local/bin/git
/usr/bin/git

# whereis는 실행 파일, 소스, man 페이지의 파일을 찾아줍니다.
$ whereis ssh
ssh: /usr/bin/ssh /usr/share/man/man1/ssh.1

# locate는 파일명을 패턴으로 빠르게 찾아줍니다.
# 아래 예제는 .java 파일을 찾아주는 명령입니다.
$ locate *.java
```



### **15 tail**

tail은 꼬리라는 의미처럼 파일의 마지막 부분을 보여줍니다. tail이 있으면 당연히 head도 있는데 사용법은 같습니다. 필자는 tail -f {파일}을 가장 많이 쓰는 편입니다. 서버의 로그를 실시간으로 보고 싶을 때 사용합니다.

```plaintext
# 파일의 마지막 라인부터 숫자만큼의 파일의 라인 수를 보여주기
$ tail -n {숫자} {파일경로}

# 숫자로 지정한 라인부터 보여주기
$ tail -n +{숫자} {파일경로}

# 파일의 마지막 라인부터 숫자로 지정한 바이트 수 만큼 보여주기
$ tail -c {숫자} {파일경로}

# Ctrl + C로 중단하기 전까지 지정한 파일의 마지막에 라인이 추가되면 계속 출력하기
$ tail -f {파일경로} :

# 파일의 마지막 라인부터 지정한 숫자만큼을
# {초}로 지정한 시간이 지날 때마다 리프레시해서 보여주기
$ tail -n {숫자} -s {초} -f {파일경로}
```


**head**
처음에 여러 파일을 연결하기 위해 고안된 cat 명령은 이후 다른 목적으로 사용된다. 해당 명령어는 **새 파일을 작성하고 터미널에서 파일 내용을 보고 출력을 다른 명령 도구나 파일로 리디렉션**하는 데 사용한다.

![[Linux28.png]]





### **16 find**

find는 명령어의 뜻 그대로 파일이나 디렉터리를 찾는 데 사용하는 명령어입니다. 굉장히 많은 옵션이 있으나 그중에 자주 사용되는 것만 소개합니다.

```plaintext
# 확장자 명으로 찾기
$ find {디렉터리} -name '*.bak'

# 디렉터리를 지정해 찾기
$ find {디렉터리} -path '**/검색 시 사용하는 디렉터리명/**.*.js'

# 파일명을 패턴으로 찾기
$ find {디렉터리} -name '*패턴*'

# 파일명을 패턴으로 찾되 특정 경로는 제외하기
$ find {디렉터리} -name '*.py' -not -path '*/site-packates/*'

# 파일을 찾은 다음 명령어 실행하기
$ find {디렉터리} -name '*.ext' -exec wc -l {} \;

# 최근 7일간 수정된 파일을 찾고 삭제하기
$ find {디렉터리} -daystart -mtime -7 -delete

# 0바이트인 파일을 찾고 삭제하기
$ find {디렉터리} -type f -empty -delete

# 확장자가 .txt인 파일만 찾아내고, txt 파일 안에 있는 ‘hi’ 라는 문자열을 ‘hello’로 바꾸기기
$ find ./ -name "*.txt" -exec sed -i 's/hi/hello/g' {} \;

```



### **17 ps**

**Process Status**의 약자로, **현재 시스템에서 실행 중인 프로세스를 시각화**해준다.
현재 실행 중인 프로세스 목록과 상태를 보여줍니다.

```plaintext
# 실행 중인 모든 프로세스를 보여주기
$ ps aux

# 실행 중인 모든 프로세스를 전체 커맨드를 포함해 보여주기
$ ps auxww

# 특정 문자열과 매칭되는 프로세스 찾기(grep은 바로 다음에 나옵니다)
$ ps aus | grep {패턴}

# 메모리 사용량에 따라 정렬하기
$ ps --sort size
```

- **ps** : 명령어를 입력한 순간의 프로세스 정보 출력. **현재의 Shell에 의해서 수행된 프로세서들을 조회**활 수 있다.
    - **ps -f** : Full Listing. **프로세스 정보에 대해 상세하게 출력**한다. (uid, pid, parent pid, CPU 사용량, 시작 시간 등등)
    - **ps -l** : Long Listing. **프로세스의 기본 정보 및 프로세스가 사용하고 있는 OS 자원(CPU, Memory)의 활용 규모, OS의 리소스 활성화 상태 등을 출력**한다.
    - **ps -o** : Optional Listing. **프로세스의 상태값 중 출력을 원하는 컬럼값을 지정하여 출력**한다
- **List에서 각 칼럼의 정보**
    - **UID** : **User ID**. 일반적으로 컴퓨터의 최초 사용자를 가리키는 UID 501을 출력함. (다를 수 있다.)
    - **PID** : **Process ID**. 동일한 프로그램이지만 다른 PID를 부여 받을 수 있다.
    - **PPID** : **Parent Process ID**. 해당 프로세스를 실행시킨 부모 프로세스의 PID
    - **TTY** : The Controlling Terminal For the Process, **터미널 번호**
    - **Time** : 시작 시간
    - **CPU** : 해당 프로세스가 사용한 CPU 시간의 양
    - **CMD** : 실행 중인 명령 커맨드
- 현재 Shell 뿐만 아니라 **모든 프로세스를 출력하고 싶을 때는 -e 옵션 또는 -aux 옵션을 사용**한다. **리스팅 수가 너무 많을 수 있으니 "| less" 명령을 포함**시키면 좋다.
    - 유닉스 옵션 : **-a**, **-e**, **-u**. **"man ps" 명령어로 내장 매뉴얼 확인**
    - BSD 옵션 : **a**, **u**, **x**.
        1. **a** : 터미널에서 실행한 프로세스의 정보 출력
        2. **u** : 프로세스 소유자 이름, CPU, 메모리 사용량 등 상세정보 출력
        3. **x** : 시스템에서 실행 중인 모든 프로세스의 정보 출력
- **특정 유저의 프로세스 출력** : **-u [UserName]**





### **18 grep**

grep은 입력에서 패턴에 매칭되는 내용을 찾는 명령어입니다. grep이라는 이름은 ed의 명령어인 g/re/p(내용 전체를 정규식으로 찾은 다음 프린트하라: globally search for a regular expression and print matching lines)에서 왔습니다. 보통 find, ps 등과 조합해 사용합니다.

```plaintext
# 파일에서 특정 패턴을 만족하는 부분 찾기
$ grep "패턴" 파일경로

# 파일명과 라인을 함께 표시하기
$ grep --with-filename --line-number "패턴" 파일경로

# 매칭하지 않는 부분 표시하기
$ grep --invert-match "패턴"

# cat과 함께 사용하기
$ cat 파일경로 | grep "패턴"
```



### **19 kill**

프로세스를 죽이는 명령어입니다. 프로세스를 죽인다고는 하지만 원리는 프로세스에 중지하라는 시그널을 보내는 겁니다. SIGKILL, SIGSTOP은 강제 종료이며 나머지는 정상적으로 종료시킵니다. 프로세스 아이디는 ps 명령어로 알아낼 수 있습니다 .

```plaintext
# kill에서 사용할 수 있는 시그널 표시하기
$ kill -l

# 프로세스 죽이기 SIGTERM(terminate)
$ kill 프로세스ID

# 백그라운드 잡 종료시키기
$ kill {잡ID}

# 프로세스 강제 종료
$ kill -9 | KILL 프로세스ID
```




### **20 alias**

자주 사용하는 명령어가 길면 타이핑하려면 귀찮습니다. 이때 alias를 사용하면 줄여서 사용할 수 있습니다.

```plaintext
# 모든 alias 표시하기
$ alias

# alias 만들기
# 예) alias ll="ls -al"
$ alias 단어="명령"

# cd ../..을 cd …으로 줄여 쓰기
# cd ../../../은 cd ….으로 가능
$ alias ...=../..
$ alias ....=../../..
$ alias .....=../../../..
$ alias ......=../../../../..

# alias 삭제하기
$ unalias 단어
```



### **21 vi / vim**

vi 혹은 vim은 대부분의 리눅스에 기본적으로 설치되어 있는 텍스트 에디터입니다. 백엔드 개발 환경에서는 적지 않게 사용할 기회를 만나게 됩니다. 최소한의 vim 사용법을 알려드리겠습니다.

vi {파일명 혹은 디렉터리명}을 사용해 진입할 수 있습니다.

```plaintext
$ vi test.txt
```



test.txt 파일이 있다면 text.txt 파일을 읽어서 화면에 보여주고 없다면 다음과 같이 빈 화면을 보여줍니다.

![[Linux24.png]]

여기서 i, 혹은 a를 눌러서 편집 모드로 들어갈 수 있습니다.

![[Linux25.png]]

편집 모드에서 “hello vim”을 적고 esc를 누르면 편집 모드가 종료되고 명령 모드로 변경됩니다.


![[Linux26.png]]
명령모드에서 :wq 또는 :x를 입력하고 enter를 누르면 저장하고 vim을 빠져나오게 됩니다.

![[Linux27.png]]

명령 모드에서 커서 이동은 h (왼쪽), j (아래), k (위), l (오른쪽) 키로 합니다. vim은 여기서 소개한 것보다 더 강력한 편집 도구입니다. 공들여 공부할 가치가 있으니 추가로 공부해두기 바랍니다.


### **22 uname**

**Unix Name**의 약자이며, **이름, 버전 및 기타 시스템 특정 세부 사항과 같은 시스템 정보를 얻기 위한 기본 Linux 명령어**다. 이 명령으로 OS 및 커널 버전을 빠르게 확인할 수 있으며, 시스템의 명령 길이를 확인할 수 있다.

macOS의 uname 기능들은 다음과 같다.

- **uname -a** : 시스템의 모든 정보를 출력
- **uname -m** : 시스템 하드웨어 타입 정보
- **uname -n** : 사용중인 네트워크 호스트 이름 확인
- **uname** **-p** : 프로세서 정보 확인
- **uname** **-r** : 커널 릴리즈 확인 (운영체제 배포 버전)
- **uname** **-s** : 커널명 확인
- **uname** **-v** : 커널 버전 확인


### **23 shutdown**

Linux 명령어 shutdown은 halt, init과 함께 **시스템을 종료하는 명령어** 중 하나다. shutdown은 **현재 접속 중인 모든 사용자에게 시스템이 종료된다는 메시지**를 보낼 수 있다.


### 24. cmp

**Compare**의 약자로, **두 파일을 비교하고 그 결과를 표준 출력 스트림에 인쇄**하는 명령어다. 해당 명령어는 comm과 함께 대량의 텍스트 파일을 정기적으로 처리하는 사용자들이 가장 많이 사용하는 Linux 명령어 중 하나다.


![[Linux32.png]]


### 25. dd

베테랑 사용자들이 **파일을 한 유형에서 다른 유형으로 복사 및 변환하기 위해 가장 많이 사용하는 Linux 명령 중 하나**다. 해당 명령어에 대한 흥미로운 점은 **부팅 가능한 라이브 USB 스틱을 만들 때 다른 터미널 명령 중에서 자주 사용**한다는 것이다.




### 26. less

가장 많이 사용되는 또 다른 Linux 명령어인 less 명령은 **파일의 내용을 볼 때 제공하는 편리성** 때문에 많이 사용된다. cat 과는 달리 less 명령을 사용하면 **터미널 세션을 방해하지 않으면서 파일 내에서 양방향으로 탐색**할 수 있다. 기본 탐색 키는 다음과 같다.

- **위, 아래 방향키** : 한줄 위, 아래 이동
- **스페이스바** : 한페이지 아래로 이동
- **/**[keyword] : 파일 내에서 [keyword] 찾기
- **q** : 나가기


### 27. comm

**두 개의 파일을 공통 행과 구별되는 행으로 비교**할 수 있다. 터미널에서 다량의 파일을 처리해야 하는 사람들에게 필수적인 명령어다.

![[Linux33.png]]


![[Linux34.png]]

### 28. sed

**지정된 부분을 교체하여 파일 또는 스트림의 각 줄을 조작하는데 가장 많이 사용되는 명령어** 중 하나이다. 많은 양의 텍스트 데이터를 다루고 이동 중에도 변경해야 하는 사용자들이 많이 사용한다.

## **# 네트워크 관리**

### 29. wget

네트워크 관리자가 **터미널에서 바로 웹에서 파일을 다운로드하는데 활용하는 명령어**. 해당 명령어는 스크립트나 크론 작업에 사용될 수 있는 편리한 작은 터미널 명령 중 하나이며, 사용자에게 **HTTP, HTTPS 및 FTP 인터넷 프로토콜을 사용할 수 있는 기능을 제공**한다.

### 30. iptables

**시스템 관리자가 특정 호스트 시스템에서 들어오고 나가는 인터넷 트래픽을 제어할 수 있는 터미널 유틸리티를 호출**하는 명령어. sysadmins는 정기 트래픽을 정의하고, 의심스럽거나 신뢰할 수 없는 네트워크 요청을 블랙리스트에 올리는 데 가장 많이 사용하는 명렁어이다.

### 31. traceroute

**네트워크 패킷이 한 시스템에서 다른 시스템으로 이동하는 경로를 결정하기 위해 사용하는 명령어**. 여러 가지 유해한 침입자로부터 컴퓨터를 보호할 수 있는 강력한 네트워크 명령어다.

### 32. cURL

**네트워크를 통해 파일을 전송하여 새로운 Linux 시스템 사용자도 사용할 수 있는 매우 강력한 네트워크 도구**. 사용자 개입 없이 작동하도록 설계된 Linux 명령어 중 하나이며, 일반적으로 네트워크 관련 쉘 스크립트에 사용된다.



### 33. cal

달력을 ASCII 텍스트 형식으로 표시하는 명령어.

![[Linux29.png]]


### 34. fortune

**터미널에 직접 입력하고 확인하시오.**  
**macOS** 에서는 기본적으로 지원하지 않는 Command이다.  
**`brew install fortune`을 통해 쉽게 설치할 수 있으니, 결과가 궁금하신 분은 GOGO**

### 35. history

**터미널 세션의 bash 기록을 출력**해주는 명령어

### 36. yes

해당 명령어는 **주어진 문자열을 Ctrl+C 키로 멈출 때까지 계속 반복해서 출력**한다. 시스템 성능 테스트 같은 것을 할 때 사용할 수 있다.

### 37. banner

**배너를 만들 수 있는 명령어**.


![[Linux30.png]]

### 38. rev

**입력 텍스트를 가져 와서 각 문자를 반대로 하여 표준 출력에 기록**하는 명령어.



![[Linux31.png]]



## **# I/O 및 소유권**

### 39. clear

**터미널 화면을 지우는 데 사용**하는 명령어.


### 40. sort

정렬 명령어. **사전 순 또는 역순으로 파일을 정렬해야 할 때 사용**한다.

### 41. sudo

sudo 명령어는 Linux 명령의 성배와 같다. 권한이 없는 사용자는 낮은 수준의 권한이 필요한 파일에만 액세스하고 수정할 수 있다. **해당 명령어를 사용하면 일반 사용자 계정에서 root에 액세스**할 수 있다.


### 42. chown

chown 명령은 chmod 명령과 매우 유사하다. 그러나 액세스 권한을 변경하는 것이 아닌, **파일 또는 디렉터리의 소유권을 변경하는 명령어**이다. **chmod와 chown 명령어는 모두 root 권한이 필요**하다



### 43. man

**Manual**의 약자로, man 명령어 다음에 다른 명령어의 이름을 같이 입력하면 그 **명령어의 매뉴얼이나 설명서 페이지를 볼 수 있다**. 특정 명령어의 사용 방법을 결정해야 할 때 자주 사용한다.



![[Linux35.png]]
**매뉴얼 페이지에서 나가고 싶다면 "q" 를 입력**하면 된다.

### 44. tar

**파일을 아카이브하고 추출하는데 사용하는 명령어**. 파일을 압축하는데 널리 사용되는 명령어로, 매우 효율적으로 처리할 수 있다.

### 45. whatis

사용자가 제공한 간단한 설명으로 데이터베이스 세트를 순회하며 **해당 데이터베이스 명령과 일치하는 시스템 명령을 인쇄**한다.

 manpath [명령어]

특정 명령어의 man 페이지가 존재하는 위치를 찾는 검색 경로를 확인

* whereis [옵션] [명령어]

명령어의 실행 파일 절대 경로, 소스코드, 설정 파일 및 매뉴얼 페이지의 위치를 찾아 출력

* apropos [문자열]

man 페이지 설명에서 지정한 키워드를 포함하고 있는 명령어

ex) apropos system | grep^system

#### **< 사용자 생성 명령어 >**

* useradd [옵션] [계정명]

계정을 생성하는 명령어 =adduser

* 파일 /etc/default/useradd

명령어 useradd로 사용자 계정을 추가할 때 사용되는 정보를 읽어오는 파일

- useradd –D

: /etc/default/useradd을 변경

* passwd [옵션] [계정명]

생성된 계정자의 패스워드를 입력 및 변경하는 명령어

* su [옵션] [사용자] [셸변수]

= switch user

현재 사용자 계정에서 로그아웃하지 않고 다른 사용자 계정으로 로그인하여 해당 사용자의 권한을 획득하는 명령어

* 파일 /etc/passwd

계정자의 정보를 가지고 있는 파일

* 파일 /etc/shadow

계정자의 패스워드 정보가 암호화되어 있는 파일

* 파일 /etc/login.defs

사용자 계정 설정과 관련된 기본값을 정의한 파일

#### **< 사용자 계정 관리 명령어 >**

* usermod [옵션] [계정명]

디렉터리 /home에 위치한 사용자들의 정보를 변경하는 명령어

* userdel [옵션] [계정명]

기존 계정 정보를 삭제하는 명령어

* chage [옵션] [계정명]

패스워드의 만료 정보를 변경하는 명령어

사용자의 패스워드에 대한 정보를 출력하고 /etc/shadow의 날짜 관련 필드에 모두 설정할 수 있는 명령어

- 그룹관리 명령어

* 파일 /etc/group

사용자 그룹에 대해 정의되어 있는 파일

* 파일 /etc/gshadow

그룹의 암호를 MD5로 하여 저장, 그룹의 소유주와 구성원 설정이 가능한 파일

* groupadd [옵션] [그룹명]

새로운 그룹을 생성하는 명령어

* groupdel [그룹명]

기존의 그룹을 삭제하는 명령어

* groupmod [옵션] [그룹명]

그룹의 설정을 변경하는 명령어

#### **< 사용자 조회 명령어 >**

* users [옵션]

시스템에 로그인한 사용자의 정보를 출력하는 명령어

* who [옵션]

현재 시스템에 접속해 있는 사용자들을 조회하는 명령어

관리자 root와 일반 사용자 모두 사용 가능

- who am i, whoami 명령어 : 자신의 정보 조회 가능

* w

현재 접속 중인 사용자들의 정보를 나타내는 명령어

* id [옵션] [계정명]

사용자 계정의 uid, gid, group을 확인하는 명령어

* groups [계정명]

사용자 계정이 속한 그룹 목록을 확인하는 명령어

#### **< 파일 비교 명령어 >**

* diff [옵션] [파일명1] [파일명2] _or_ diff [옵션] [디렉터리1] [디렉터리2]

두 개의 파일을 행 단위로 비교하여 다른 부분을 출력하는 명령어

#### **< 네트워크 관련 명령어 >**

* ping [옵션] [도메인명 or IP주소]

외부 호스트에 신호를 보내며 신호를 받은 호스트는 응답을 주면서 서로 네트워크가 연결되어 있음을 확인시켜 주는 명령어

* traceroute [도메인명 or IP주소]

목적지 호스트까지의 경로를 표시

그 구간의 정보를 기록하는 명령어

경로상의 어떤 장애가 있는 경우 위치 파악

*nslookup [옵션] [호스트명]

도메인명 ↔ IP주소

도메인명으로 IP주소 조회, IP주소로 도메인명 조회하는 명령어

* dig [서버명] [호스트명] [질의타입]

호스트명 ↔ IP주소

호스트명에 대한 IP주소 정보, IP주소에 대한 호스트명을 조회하는 명령어

* host [옵션] [도메인 또는 IP주소] [DNS 서버]

호스트명 ↔ IP주소

호스트명을 알고 있는데 IP주소를 모르거나 그 반대의 경우에 사용하는 명령어

호스트명을 이용하면 IP주소뿐만 아니라 하위 호스트명도 조회 가능

* hostname [옵션] [파일명]

시스템 이름을 확인하거나 변경할 때 사용하는 명령어

사용 중인 시스템의 도메인 네임을 출력하기 위해 사용하는 명령어

* route

호스트의 라우팅 테이블을 확인하는 명령어

#### **< 시스템 종료 명령어 >**

* shutdown [옵션] [시간] [경고메시지]

시스템을 종료하거나 재부팅하는 명령어

현재 수행 중인 프로세스들을 종료

sync를 수행하여 저장되지 않은 데이터를 디스크에 저장

모든 파일 시스템을 mount시킨 후 시스템 종료

root 사용자만이 권한을 가짐

* init [런레벨]

shutdown 명령어와 동일한 기능을 가진 명령어

*reboot [옵션]

시스템을 재부팅하는 명령어

* halt [옵션]

시스템을 종료하는 명령어

#### **< 기타 명령어 >**

* cal [옵션] [[month]year]

달력을 출력하는 명령어

옵션 없이 실행시킬 시 달 출력

* date [옵션] [MMDDhhmm]

or date [옵션] [+FORMAT]

시스템의 날짜와 시간을 표시하거나 변경하는 명령어

* clear = cls

터미널의 내용을 지우는 명령어

* tty

현재 사용하고 있는 단말기 장치의 경로명과 파일명을 나타내는 명령어

현재 사용 중인 가상 콘솔 창 정보를 확인하는 명령어

* time

프로그램이 수행되는 데 걸리는 시간을 측정하여 출력하는 명령어

- real: 총 수행시간

- user: CPU가 사용자 영역에서 보낸 시간

- sys: 시스템 호출 실행에 걸린 시간

* wall [메시지 내용]

터미널을 통해 메시지를 전달하는 명령어

* write [계정명] [ttyname]

해당 사용자에게 메시지를 전달하는 명령어

* mesg [y/n]

write을 사용해서 들어오는 메시지 수신 여부를 확인하고 제어하는 명령어

* rdate -s

지정된 서버와 현재 시스템의 날짜와 시간을 동기화하는 명령어

* hwclock

시스템의 하드웨어 시간 정보를 출력하는 명령어



![[리눅스 명령어.pdf]]





---
참조 - https://www.mireene.com/webimg/linux_tip1.htm

https://www.codestates.com/blog/content/%EB%A6%AC%EB%88%85%EC%8A%A4-%EA%B8%B0%EB%B3%B8-%EB%AA%85%EB%A0%B9%EC%96%B4


https://choboit.tistory.com/88