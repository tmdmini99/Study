##### Android Plugin 다운

![[Pasted image 20240808172111.png]]


##### SDK, Kotlin 설치

Create New Project를 클릭하고, Android 탭을 눌러서 Install SDK 버튼 클릭

![[Pasted image 20240808172207.png]]



##### Phone and Tablet 선택

![[Pasted image 20240808172253.png]]



하지만 이 상태로 Build를 하게되면 **"License for package Android SDK Build-Tools 30.0.2 not accepted."** 라는 에러를 확인할 수 있는데 이에 대한 해결 방법은 아래와 같습니다.

**File -> Setting -> System Settings -> Android SDK -> SDK Tools -> Google Play Licensing Library 설치**


![[Pasted image 20240808173037.png]]

#### **가상 애뮬레이터에서 앱 실행하기**

가상 애뮬레이터를 실행하기 위해 Tools -> Android -> SDK Manager -> Intel x86 Emulator ~를 설치합니다.









---
출처 - https://hacksms.tistory.com/155

https://blog.naver.com/ifthe1201/221412112020
