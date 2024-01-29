## **WSL이란?**

 - WSL (Windows Subsystem for Linux)은 마이크로소프트에서 제공하는 Windows 운영 체제에서 Linux 운영 체제를 실행할 수 있는 기술임.  
 - WSL은 Windows 10에서 제공되고 있으며, Windows 사용자들이 Linux 환경에서 제공되는 명령줄 도구, 스크립트, 언어 등을 사용할 수 있는 기회를 제공함  
- 가상 머신이나 부팅 가상 머신을 사용하는 것과 달리, WSL은 Windows 운영 체제의 커널에서 동작함  
- WSL을 사용하면 Windows 운영 체제에서 Linux 패키지를 설치하고 실행할 수 있으며, Windows 파일 시스템에 접근할 수 있어 편의성이 높음

## WSL 사용하는 이유

---

- 리눅스 커맨드라인을 윈도우에서 사용가능
- bash shell에서 윈도우 파일에 쉽게 접근하고 실행시킬수 있음
- bash script를 윈도우 드라이브에서 실행할 수 있음
- 윈도우에서 vscode로 작업하면서 리눅스에서 돌아가는 백엔드 앱을 디버깅할 수 있음
- AF unit socket을 사용하면 윈도우 프로세스와 리눅스 프로세스 사이의 연계가 가능
- 맥북, 아이맥 구입시 램 하나 추가할때도 수십만원의 돈을 써야 했던것에 비하면 5950x, 보드, 파워, 저렴한 그래픽카드 이렇게 구성하면 200만원 이하로 맥프로급의 성능을 낼수 있다.



![[WSL1.png]]

WSL2는 기존과 다른 VM 환경을 가지고 있습니다. **WSL 1에서 Linux의 System Call을 Windows API로 변환하는 구조였다고 하면, WSL 2에서는 윈도우즈에 리눅스 커널을 아얘 올려버렸다고 합니다.**

WSL1이 윈도우의 api를 이용하기 위하여 변환과정을 거쳤기 때문에, 속도적인 측면에서 불리하였고 일부 api는 변환이 불가능하였습니다. 하지만, WSL2는 linux 커널을 포함하고 있기 때문에, linux의 모든 api를 지원합니다.

WSL2는 1과 다르게 Hyper-V를 사용해서 경량 VM기술을 사용합니다.
따라서 기존 가상머신처럼 100 % 리눅스 커널과 호환됩니다.  
커널은 마이크로소프트에서 직접 리눅스 4.19 버전의 커널을 제공합니다.


그리고 가상머신처럼 메모리가 할당되고 WSL2부터는 가상 IP도 부여됩니다.  
말 그대로 Windows 에서 Hyper-V 기능을 WSL에 적용했다고 보시면 됩니다.


## WSL 설치


PowerShell 관리자 권한으로 실행
![[Docker-Compose 설치2.png]]

그후 
```PowerShell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

입력



WSL2를 사용하고 싶으면

```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
입력

### WSL 설치

[https://learn.microsoft.com/ko-kr/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package](https://learn.microsoft.com/ko-kr/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package) 

접속 후 설치


---
참조 - https://seven-rain.tistory.com/22

https://sac4686.tistory.com/56