## 특정 IP 주소 도메인 적용 법



### **1. 관리자 권한으로 메모장 실행**

1. **Windows + S** 키를 눌러 검색창을 엽니다.
2. **메모장** 또는 **Notepad**를 검색합니다.
3. 메모장을 **우클릭**하고 **관리자 권한으로 실행**을 선택합니다.

### **2. `hosts` 파일 열기**

1. 관리자 권한으로 실행된 메모장에서 **파일 > 열기**를 선택합니다.
2. 파일 선택 창에서 아래 경로로 이동합니다

```
C:\Windows\System32\drivers\etc\
```


1. 파일 형식이 기본적으로 **텍스트 파일(.txt)**로 되어 있어서 `hosts` 파일이 보이지 않을 수 있습니다. 파일 형식을 **모든 파일**로 변경합니다.
2. `hosts` 파일을 선택하여 엽니다.

### **3. `hosts` 파일 편집**

1. 파일 맨 아래에 다음 내용을 추가합니다

```
10.0.2.2    local.example.com

```


### 전체 파일 예시

```
# Copyright (c) 1993-2009 Microsoft Corp.
#
# This is a sample HOSTS file used by Microsoft TCP/IP for Windows.
#
# This file contains the mappings of IP addresses to host names. Each
# entry should be kept on an individual line. The IP address should
# be placed in the first column followed by the corresponding host name.
# The IP address and the host name should be separated by at least one
# space.
#
# Additionally, comments (such as these) may be inserted on individual
# lines or following the machine name denoted by a '#' symbol.
#
# For example:
#
#      102.54.94.97     rhino.acme.com          # source server
#       38.25.63.10     x.acme.com              # x client host

# localhost name resolution is handled within DNS itself.
#	127.0.0.1       localhost
#	::1             localhost
# Added by Docker Desktop
10.33.150.85 host.docker.internal
10.33.150.85 gateway.docker.internal
# To allow the same kube context to work on the host and the container:
127.0.0.1 kubernetes.docker.internal
# End of section
10.0.2.2    local.example.com

```