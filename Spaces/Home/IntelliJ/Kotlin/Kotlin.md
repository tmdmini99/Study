##### Android Plugin 다운

![[Pasted image 20240808172111.png]]


##### SDK, Kotlin 설치

Create New Project를 클릭하고, Android 탭을 눌러서 Install SDK 버튼 클릭

![[Pasted image 20240808172207.png]]



##### Phone and Tablet 선택

![[Pasted image 20240808172253.png]]


![[Pasted image 20240808175605.png]]


하지만 이 상태로 Build를 하게되면 **"License for package Android SDK Build-Tools 30.0.2 not accepted."** 라는 에러를 확인할 수 있는데 이에 대한 해결 방법은 아래와 같습니다.

**File -> Setting -> System Settings -> Android SDK -> SDK Tools -> Google Play Licensing Library 설치**

File -> Setting -> Languages & Frameworks -> Android SDK -> SDK Tools -> Google Play Licensing Library 설치


![[Pasted image 20240808174243.png]]



![[Pasted image 20240808173037.png]]

#### **가상 애뮬레이터에서 앱 실행하기**

가상 애뮬레이터를 실행하기 위해 Tools -> Android -> SDK Manager -> Intel x86 Emulator ~를 설치합니다.(인텔)



![[Pasted image 20240808174907.png]]



![[Pasted image 20240808173617.png]]


가상 애뮬레이터를 실행하기 위해 Tools -> Android -> SDK Manager -> AMD 하이퍼바이저 드라이버


![[Pasted image 20240808174844.png]]


![[Pasted image 20240808173718.png]]


빨간색 체크 3개는 필수 설치이고 (기본으로 설치되어있음) 파란색 체크 2개 중 AMD CPU라면 위, Intel CPU라면 아래를 설치하면 된다.



가상 디바이스를 생성하기 위해 Tools -> Android -> AVD Manger를 클릭한 뒤, 나머지 절차를 진행합니다.



![[Pasted image 20240808173804.png]]



가상 애뮬레이터 세팅이 마무리 되었다면, Run APP 버튼을 클릭합니다.


![[Pasted image 20240808173812.png]]

이후, 가상 애뮬레이터가 켜지면서 앱이 실행되는 것을 확인할 수 있습니다.




---
출처 - https://hacksms.tistory.com/155

https://blog.naver.com/ifthe1201/221412112020



https://velog.io/@jihyeon9975/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EC%BD%94%ED%8B%80%EB%A6%B0%EC%9C%BC%EB%A1%9C-%ED%95%98%EB%8A%94-%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EC%95%B1-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-1-



https://blog.seoft.co.kr/31 -  안드로이드 초기 설정

https://www.jetbrains.com/help/idea/create-your-first-android-application.html#Android-make-application-interactive  - jetbrains