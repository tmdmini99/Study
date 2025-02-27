# 쉘 스크립트

서버 작업 자동화 및 운영(DevOps)을 위해 기본적으로 익혀둘 필요가 있다.

쉘 명령어를 기본으로 하되, 몇 가지 문법이 추가된 형태

시스템 프로그래밍에서 꼭 익히는 내용 중 하나이다.

최근 perl 이나 python이 사용되고 있다.

- 쉘 스크립트를 사용하는 경우

서버가 다운되어서 확인해보니 서버 저장공간이 하나도 남아 있지 않았다.

이유는 로그 파일이 많이 쌓였기 때문이었다.

로그 파일이 업데이트가 안되어 관련 프로그램이 비정상적으로 종료되었다.

어떻게 하면 자동으로 오래된, 혹은 일정 시간 경과한 로그 파일을 삭제할 수 있을까??

이런 문제를 간단한 쉘 스크립트 생성 및 주기적 실행으로 해결할 수 있다.

 로그 파일 : 서버에 어떤 일이 있었고 어떤 동작을 했었는지 기록해놓는 파일

 'crontab' : 주기적으로 실행하는 프로그램

# 쉘 스크립트 기본 문법

1. 쉘 스크립트는 파일로 작성 후, 파일을 실행 한다.

2. 파일의 가장 위 첫 라인은 #!/bin/bash로 시작한다.

3. 쉘 스크립트 파일은 코드를 작성한 후에는 실행 권한을 부여해야한다.

4. 일반적으로 '파일이름.sh' 와 같은 형태로 파일 이름을 작성한다.

5. 주석은 `#`내용 으로 처리한다.

> 'vi 파일이름.sh' 로 파일을 만들고 insert 를 눌러 작성한 다음 다시 insert 를 누르고 esc 를 누른다음 :wq로 저장한다.
> 다음의 여러 예제들에서는 chmod 777 로 파워권한을 주어 실행(./ 파일이름.sh) 하도록 한다.

예제 1.

bash 쉘에서 제공하는 echo 함수를 이용하여

화면에 "Hello bash!"를 출력할 수 있도록 스크립트를 작성해보자

echo는 화면에 출력해주는 쉘 명령어다.

vi hello.sh

```bash
#!/bin/bash                     // 쉘 스크립트의 시작
  
echo "Hello Bash!"            // echo 함수 사용
```


ls -al 로 확인해보면 실행 권한이 없어서

chmod 777 hello.sh 를 입력해서 전체 권한을 준다.

아직 path로 잡혀 있지 않아서

./ 로 실행하면

Hell Bash! 가 출력되는 것을 확인할 수 있다.

> 변수, 조건문, 반복문 - 여러 쉘 명령어를 조합해서 프로그래밍한다.

---

 # 변수

- 선언

변수명=데이터

변수명=데이터 사이에 띄어쓰기는 허용되지 않는다.

- 사용

$변수명 (echo $변수명 $변수명 .... )

코드 예는 다음과 같다.

```bash
#!/bin/bash  
  
mysql_id='root'
mysql_directiory='/etc/mysql'
  
echo $mysql_id  
echo $mysql_directory
```

예제 2.

아이디 관련 정보 변수를 만들어보자

실제 이름

나이

직업

vi id.sh

```bash
#!/bin/bash
  
myname='probecoding'  
myage=30                            // 숫자는 따옴표 처리 안해도 된다.  
mycarrer='IT'
  
echo $myname $myage $ mycarrer
```

결과적으로 이와 같이 출력된다.

probecoding 30 IT

---

# 리스트 변수(배열)

- 선언

변수명=(데이터1데이터2데이터3...)

- 사용

${변수명\[인덱스번호]} : 인덱스번호는 0이 시작이다.

코드 예는 다음과 같다.

```bash
#!/bin/bash  
  
daemons=("httpd" "mysqld" "vsftpd")      // 변수 선언  
echo ${daemons[1]}                     // $daemons 배열의 두 번째 인덱스에 해당하는 myspld 출력  
echo ${daemons[@]}                    // $daemons 배열의 모든 데이터 출력  
echo ${daemons[*]}                     // $daemons 배열의 모든 데이터 출력  
echo ${#daemons[@]}                  // $daemons 배열의 배열 크기 출력  
  
filelist=( $(ls) )                           // 해당 쉘 스크립트 실행 디렉토리의 파일 리스트를 배열로 변수 선언  
echo ${filelist[*]}                        // $filelist 모든 데이터 출력
```



결과적으로 이와 같이 출력된다.

mysqld  
httpd mysqld vsftpd 
httpd mysqld vsftpd  
3

array.sh ~.sh ~.sh ...(모든 파일 리스트가 나열됨)

* 특수 상황

$(daemons[1]) 이 아니라 $daemons[1] 라고 입력할 경우   
쉘 스크립트는 daemons 까지를 읽고 데이터로 변환시켜고 [1] 이거는 일반적인 스트링이구나  
라고 인식해서 httpd[1] 이라고 출력된다. 

예제 3.

아이디 관련 정보 리스트 변수로 만들고, 각 정보를 출력해보자

실제 이름(0번 인덱스)

나이(1번 인덱스)

직업(2번 인덱스)

```bash
#!/bin/bash 
  
mine=("probecoding" "30" "IT")  
echo ${mine[*]}                       // $mine 모든 데이터 출력
```

결과적으로 이와 같이 출력된다.

probecoding 30 IT

> 쉘 스크립트의 기본 문법은 까다롭다. 가변적으로 문법을 바꿔도 동작을 하긴하지만 환경에 따라 동작을 달리한다.

---

# 사전에 정의된 지역 변수

\$$ : 쉘의 프로세스 번호 (pid)
$0 : 쉘 스크립트 이름  
$1~$9 : 명령줄 인수  
$* : 모든 명령줄 인수 리스트  
$# : 인수의 개수  
$? : 최근 실행한 명령어의 종료 값  
- 0 성공, 1~125 에러, 126 파일이 실행가능하지 않음, 128~255 시그널 발생  
  
ls -al -z 의 경우

ls 가 \$$

-al 이 $1

-z 가 $2  
$#은 2개

예제 4.

쉘 프로세스 번호, 쉘 스크립트 이름, 명령줄 인수, 모든 명령줄 인수 리스트, 인수 개수 출력해보기

vi shell.sh

```bash
#!/bin/bash
  
echo $$ $0 $1 $* $#
```

$0 은 이름

$1 은 첫번째 인자

$* 는 이름을 뺀 나머지 인자 리스트

$# 는 이름을 뺀 나머지 인자 개수

이렇게 저장한 다음

파일이름이 shell.sh 이라서 ./shell.sh 로 실행해보면

15242 ./shell.sh 0 이렇게 출력된다.

./shell sh kkk 이렇게 실행해보면

15243 ./shell.sh kkk kkk 1 이렇게 출력된다.

./shell.sh kkk 222 이렇게 실행해보면

15249 ./shell.sh kkk kkk 222 2 이렇게 출력된다.

---

# 연산자

expr : 숫자 계산  
1. expr 를 사용하는 경우 역 작은 따옴표 (`) 를 사용해야한다. ` ` ` ` ` ` `   
2. 연산자 * 와 괄호 ( ) 앞에는 역 슬래시를 같이 사용해야 한다. \ \ \ \ \ \ (붙여 쓴다.)  
3. 연산자와 숫자, 변수, 기호 사이에는 space를 넣어야한다.
~~

코드 예는 다음과 같다.

```bash
#!/bin/bash  
  
num=`expr \( 3 \* 5 \) / 4 + 7`    ( 터미널에서는 \가 역슬래시로 나온다.)
```


예제 5.

expr 명령으로 (10+20)/8 - 8 계산해보기

```bash
#!/bin/bash  
  
num=`expr \( 10 + 20 \) / 8 - 8`
```


-5 가 출력된다.

---

# 조건문 문법

* 기본 if 문법

then 과 fi 안에만 들어가 있으면 되기 때문에

명령문을 꼭 탭으로 띄워야 하는 것은 아니다.

if \[ 조건 ] 

then

     명령문

fi


예제 6.

두 인자값을 받아서 두 인자값이 다르면 'different values' 를 출력해보자


```bash
#!/bin/bash
  
if [ $1 != $2 ]                             //  != 둘이 다르면 이라는 의미  
then  
      echo "different values"  
      exit  
fi
```


!=

둘이 다르면 이라는 의미

echo $ 를 사용하지 않고

echo "문장" 이라고 사용한다.

결과적으로 이와 같이 출력된다.

different values

조건

조건 작성이 다른 프로그래밍 언어와 달리 가독성이 현저히 떨어진다. 필요할 때마다 참조하면 됨!

문자1 == 문자2    // 문자 1과 문자 2가 일치

문자1 !=  문자2    // 문자 1과 문자 2가 일치하지 않는다.

-z 문자               // 문자가 null 이면 참(값이 없으면 true)

-n 문자               // 문자가 null 이 아니면 참

수치 비교

<, > 는 if 조건 시 [[]]를 넣는 경우 정상 작동하기는 하지만, 기본적으로 다음 문법을 사용한다.  
  
값1 -eq 값2       //값이 같음(equal)  
값1 -ne 값2       //값이 같지 않음(not equal)  
값1 -lt 값2        //값1이 값2보다 작음(less than)  
값1 -le 값2        //값1이 값2보다 작거나 같음(less or equal)  
값1 -gt 값2       //값1이 값2보다 큼(greater than)  
값1 -ge 값2       //값1이 값2보다 크거나 같음(greater or equal)

파일 검사  
-e 파일명   //파일이 존재하면 참  
-d 파일명   //파일이 디렉토리면 참  
-h 파일명   //파일이 심볼릭 링크 파일이면 참  
-f 파일명   //파일이 일반파일이면 참  
-r 파일명   //파일이 읽기 가능하면 참  
-s 파일명   //파일크기가 0이 아니면 참  
-u 파일명   //파일이 set-user-id가 설정되면 참  
-w 파일명   //파일이 쓰기 가능이면 참  
-x 파일명   //파일이 실행 가능이면 참

예제 7.

쉘 스크립트로 해당 파일이 있는지 없는지 확인해보자

```bash
#!/bin/bash  
  
if [ -e $1 ] 
then 
   **echo "file exist"
fi
```

라고 코드를 작성하고

./if3.sh (파일명) 으로 현재 찾는 파일이 디렉토리에 있다면

file exitst 가 출력되는 것을 확인할 수 있다.

  
논리 연산 (참고)  
조건1 -a 조건2     //AND  
조건1 -o 조건2     //OR  
조건1 && 조건2    //양쪽 다 성립  
조건1 || 조건2       //한쪽 또는 양쪽 다 성립  
!조건                  // 조건이 성립하지 않음  
true                   // 조건이 언제나 성립  
false                  // 조건이 언제나 성립하지 않음  
  
  

  
* 기본 if/else 구문
if [ 조건 ]   
then  
     명령문      // 이 명령문에는 참일 때  
else    
     명령문      // 이 명령문에는 거짓일 때  
fi

예제 8.

두 인자값을 받아서 두 인자값이 같으면 'same value', 다르면 'different value'가 출력되도록 해보자

```bash
#!/bin/bash  
  
if [ $1 -eq $2 ]  
then   
    echo "same values"  
else  
    echo "different values"  
fi
```

라고 작성

하고 인자를 같게 넣으면 same values,

다르게 넣으면 different values 가 출력되는 것을 확인할 수 있다.

---

ping

서버에서 여러 가지 컴퓨터가 연결되어 있을 때  
연결된 특정 컴퓨터가 정상적으로 동작하는 지,꺼져 있는지, 비정상적으로 동작하는지 확인하는 명령어  
해당 컴퓨터의 ip 주소로 ping 명령어를 실행한다.  
그 주소에 확인 요청을 한다.  
정상적인 컴퓨터는 응답을 한다.  
응답이 오지 않으면 정상적으로 동작하지 않는다고 판단한다.

  
ping -c 1 192.168.0.1 1>/dev/null  
 0 : 표준입력, 1 : 표준출력, 2 : 표준에러  
 1>/dev/null : 표준 출력 내용은 버려라 (출력하지 말아라)  
-c 1 : 1번만 체크해봐라 라는 의미

코드 예제는 다음과 같다.





```bash
#!/bin/bash**  
ping -c 1 192.168.0.1 1> /dev/null  
if [ $? == 0 ]   
then   
     echo "Gateway ping success!"      // 0 일 경우 응답이 온 것이라 성공
else  
     echo "Gateway ping failed!"        // 응답이 없을 때 나타남  
fi
```

$? : 가장 최근에 쉘 스크립트에서 실행한 명령의 결과값을 가져온다. 



조건문 한줄에 작성하기 (if 구문)

  
```bash
if [ 조건 ]; then 명령문; fi


if [ -z $1 ]; then echo 'Insert arguments"; fi
```

  
  

if \[ 뒤와 ] 앞에는 반드시 공백이 있어야함 [ ] 에서 &&, ||, <, > 연산자들이 에러가 나는 경우는 \[ [ ] ] 를 사용하면 정상 작동하는 경우가 있음  
  
  
  
  
  
반복문 문법 - 기본 for 구분

for 변수 in 변수값1 변수값2 ....  
do   
    명령문  
done  
  

예제 8.

현재 디렉토리에 있는 파일과 디렉토리를 출력해보자

```bash
#!/bin/bash   
for database in $(ls)   
do    
     echo $database   
done
```

이렇게 쓰거나


```bash
#!/bin/bash   
for database in $(ls); do   
     echo $database   
done
```


이렇게 쓰거나

```bash
#!/bin/bash
for database in $(ls); do echo $database; done
```

또는

```bash
#!/bin/bash  
lists=$(ls)  
num=${#lists[@]}  
index=0  
while [ $num -ge 0 ]  
do**  
     echo ${lists[$index]}  
     index=`expr $index + 1`  
     num=`expr $num - 1`  
done
```

로 작성할 수 있다.

  
  
반복문 문법 - 기본 while 구문   
  
while [ 조건문 ]    
do   
     명령문   
done   
  

---

# 쉘 스크립트 실제 예제

1. 백업하기

코드 예제는 다음과 같다.

```bash
#!/bin/bash
  
if [ -z $1 ] | | [ -z $2 ]; then                                 // | | or 두 가지 하나라도 없으면  
       echo usage : $0 sourcedir targetidir  
else  
   SRCDIR=$1  
   DSTDIR=$2  
   BACKUPFILE=backup.$(date +%y%m%d%H%M%S).tar.gz  
   if [ -d $DSTDIR ]; then  
       tar -cvzf $DSTDIR/$BACKUPFILE $SRCDIR  
   else  
       mkdir $DSTDIR  
       tar -cvzf $DSTDIR/$BACKUPFILE $SRCDIR  
   fi  
fi
```


코드에서 

| | 는 or 이라는 뜻으로 두 가지 중 하나라도 없으면 이라는 의미

BACKUPFILE=backup.$(date +%y%m%d%H%M%S).tar.gz  이 문장은 백업할 파일이름을 back~gz 로 하겠다는 뜻

코드를 풀어쓰자면

2개의 인자를 받아서 두 가지 중 하나라도 없으면   
$0 쉘이름  
sourcedir 압축할 디렉토리명  
targetidr  압축된 파일을 넣을 디렉토리명  
백업할 파일이름을 back~gz 로 해주는데  
date 라는 쉘 명령어를 사용해서 년, 월, 일, 시간, 분, 초 를 알수있게 해준다. (backup.현재시각.tar.gz)

mkdir 디렉토리 생성하는 명령어  
tar 압축 명령(확장자) 묶었다.  
gz 압축까지 되었다는 의미

tar

압축 명령

tar 주요 옵션  
x 묶음을 해체  
c 파일을 묶음  
v 묶음/해제 과정을 화면에 표시  
z gunzip을 사용  
f 파일 이름을 지정  
  
압축 시 주로 사용하는 옵션  
tar -cvzf [압축된 파일 이름] [압축할 파일이나 폴더명]  
  
압축을 풀 때 주로 사용하는 옵션  
tar -xvzf [압축 해제할 압축 아카이브 이름]

2. 로그 파일 정리하기

정책 예시 ** 
1. 로그 파일 중에 2일 이상 지난 파일들은 압축을 해서 보관해라  
2. 압축된 로그 파일 중에 3일 이상 경과한 것들은 삭제해라  
  
  
find . -type f -name '파일명검색어' -exec bash -c "명령어1; 명령어2; 명령어3;" \;  
  
// find . 현재 디렉토리  
// -type f : 일반 파일 중에  
// -type f : 파일 타입 지정해서 검색(f는 일반 파일)  
// '파일명검색어' 정규표 형식으로  
// 명령어는 쉘 명령어 실행**

코드 예제는 다음과 같다.

```bash
#!/bin/bash  
  
LOGDIR=/var/log  
GZIPDAY=1  
DELDAY=2  
cd $LOGDIR  
echo "cd $LOGDIR"  
  
sudo find . -type f -name '*log.?' -mtime +$GZIPDAY -exec bash -c "gzip {}" \; 2>  
sudo find . -type f -name '*.gz' -mtime +$DELDAY -exec bash -c "rm -f {}" \; 2>
```










---
출처 - https://probe29.tistory.com/47