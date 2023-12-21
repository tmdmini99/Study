  **1> File 객체의 정의** 

                 * File객체는 하드디스크에 존재하는 실제 파일이나 디렉토리가 아니고 그것에 대한 경

                    로(Pathname) 또는 참조(reFerence)를 추상화한 객체이다. 파일 객체는 새 파일에 

                    대한 경로나 만들고자하는 디렉토리를 캡슐화한 것이다.
  **2> File 객체의 용도** 

                 * 물리적 파일 시스템에 대해 캡슐화한 경로명을 확인하고 실제의 파일이나 디렉토리 

                   와 대응되는지 알아볼 때.

                 * 파일 스트림객체를 생성하고자 할 때.

## file 메소드

#### **createNewFile - 파일 생성**

```java
String rootPath = "C:/DevLog/";
String sFilePath = rootPath + "test.txt";

File file = new File(sFilePath);
file.createNewFile();
```

실무에 적용할 때 rootPath는 설정값을 읽어서 사용하도록 한다.

properties파일 또는 yml 파일의 값은 서버와 분리하여 사용하기 때문이다.

(Build시 해당 파일을 제외한다던가 설정을 통해 Local, 개발서버, 운영서버 설정 파일을 분리

#### **mkdir - 디렉토리 생성**

```java
String rootPath = "C:/DevLog/";
String sNewDirectory = rootPath + "study";

File file = new File(sNewDirectory);
file.mkdir();
```

#### **exists - 파일 존재여부 체크**

```java
String rootPath = "C:/DevLog/";

//생성일자, 파일명
Calendar cal = Calendar.getInstance()  ;
SimpleDateFormat dateFormat = new SimpleDateFormat("yyyyMMdd");
SimpleDateFormat timeForamt = new SimpleDateFormat("yyyyMMddhhssSSS");
String date = dateFormat.format(cal.getTime());
String serial = timeForamt.format(cal.getTime());
		
String sDirectoryPath = String.format("%s%s", rootPath, date);
String sFilePath = String.format("%s/devlog_%s.txt", sDirectoryPath, serial);
File directory = new File(sDirectoryPath);//디렉토리 경로
File file = new File(sFilePath);		//파일 경로
		
// 디렉토리 존재여부 체크
if(!directory.exists()) {
	directory.mkdir();	// 디렉토리 생성
	System.out.println("디렉토리 생성");
}else {
	System.out.println("디렉토리 존재");
}
		
file.createNewFile();	// 파일 생성
```

일자별 폴더에 파일을 생성한다고 가정했다. 해당 일자에 최초 한 번만 폴더를 생성하면 된다. exists 메소드를 사용하여 존재여부를 체크하고 일자 폴더를 생성하였다.

파일명 또한 시간값을 이용하여 뒤에 suffix를 생성하여 만들었다. 파일 업로드, 다운로드 모두 어떤 파일명이 올지 모른다. 사용자가 요청한 파일명을 그대로 사용하여 서버에 저장한다면 이미 생성되어 있는 파일이 존재할 수 있다.

그렇기 때문에 업로드 할 경우 실제 파일명에 일련번호(시간값)를 붙여서 저장하거나 일련번호로 바꿔서 저장하는 것이 보통이다.

DB에는 실제 파일명과 물리적 파일명을 함께 저장하기 때문에 다운로드 할때 물리적 파일명을 찾아 다운로드한다.

#### **rename - 파일명 변경**

```java
File file = new File("C:/DevLog/devlog.txt");		//original File
File renameFile = new File("C:/DevLog/rename.txt");	//new File
file.renameTo(renameFile);
```

#### **delete - 파일 삭제**

```java
File file = new File("C:/DevLog/devlog.txt");		//original File
file.delete();
```

### **FileCopyUtils**

파일 복사를 위해 사용하는 객체이다.

```java
String rootPath = "C:/DevLog/";
String fileName = "devlog";

//생성일자
Calendar cal = Calendar.getInstance();
SimpleDateFormat timeForamt = new SimpleDateFormat("yyyyMMddhhssSSS");
String serial = timeForamt.format(cal.getTime());
		
File file = new File(String.format("%s%s.txt", rootPath, fileName));	//download target file
File newFile = new File(String.format("%s%s_%s.txt", rootPath, fileName, serial));	//download file
		
FileCopyUtils.copy(file, newFile);
```


[[[제어자|생성자]]

- new File(File parent, String child) : 상위 주소와 파일 이름(또는 디렉토리)
- new File(String pathname) : 상위 주소
- new File(String parent, String child) : 상위 주소와 파일 이름(또는 디렉토리)
- new File(URI uri) : 파일의 uri 주소

---

| return type  |  method   | 설명  |
|---|---|---|
|boolean|exists()|- 파일이 실제 존재하는지 판단|
|boolean|isDirectory()|- 디렉토리인지 판단|
|boolean|isFile()|- 파일인지 판단|
|boolean|canRead()|- 파일이 읽기 가능한지 판단|
|boolean|canWrite()|- 파일이 쓰기 가능한지 판단|
|boolean|canExecute()|- 파일이 실행 가능한지 판단|
|boolean|isHidden()|- 파일이 숨김파일인지 판단|
|int|length()|- 파일의 길이(byte) 반환|

```java
package study.first;

import java.io.File;

public class Main {
	public static void main(String[] args) {

		// 파일의 주소 및 파일명을 가진 file 객체 생성
		// input.txt : "Hello World..!!"
		File file = new File("C:\\JAVA\\FirstStudy\\FisrtStudy\\input.txt");

		System.out.println(file.exists());      // true
		System.out.println(file.isDirectory()); // false
		System.out.println(file.isFile());      // true
		System.out.println(file.canRead());     // true
		System.out.println(file.canWrite());    // true
		System.out.println(file.canExecute());  // true
		System.out.println(file.isHidden());    // false
		System.out.println(file.length());      // 15
	}
}
```

---

| return type  |  method   | 설명  |
|---|---|---|
|boolean|renameTo(File dest)|- 경로가 같으면 이름 변경, 경로가 다르면 이름 바뀌면서 해당 경로로 이동됨|
|boolean|delete()|- 파일 삭제|
|boolean|mkdir()|- 생성자에 넣은 경로에 맞게 폴더 생성|
|boolean|createNewFile()|- 생성자에 넣은 경로 및 파일명에 맞게 파일 생성|

```java
package study.first;

import java.io.File;

public class Main {
	public static void main(String[] args) {

		try {
			File dir = new File("C:\\JAVA\\FirstStudy\\FisrtStudy\\newforder");
			dir.mkdir(); // 폴더 생성

			File file = new File(dir, "input2.txt");
			file.createNewFile(); // 파일 생성

			File file2 = new File("input3.txt");
			file.renameTo(file2); // 경로가 다르므로 파일 이름 변경 및 이동 수행

		} catch (Exception e) {
			System.out.println("파일 및 폴더 생성에 실패했습니다.");
		}
	}
}
```

---

| return type  |  method   | 설명  |
|---|---|---|
|boolean|setReadable(treu/false)|- 읽기 권한 설정|
|boolean|setWritable(true/false)|- 쓰기 권한 설정|
|boolean|setExecutable(true/false)|- 실행 권한 설정|

```java
package study.first;

import java.io.File;

public class Main {
	public static void main(String[] args) {

		try {
			
			File file = new File("input3.txt");
			file.setReadable(false);
			file.setWritable(false);
			file.setExecutable(false);

		} catch (Exception e) {
			System.out.println("파일 및 폴더 생성에 실패했습니다.");
		}
	}
}
```

---


| return type  |  method   | 설명  |
|---|---|---|
|String|getPath()|- 파일 또는 디렉토리의 상대 경로 추출 (생성자로 준 경로 반환)|
|String|getAbsolutePath()|- 파일 또는 디렉토리의 절대 경로 추출 (생성자 관계없이 절대 경로 반환)|
|File|getAbsoluteFile()|- 절대 경로를 가지는 File 객체 생성|
|String|getParent()|- 현재 파일의 상위 경로 추출 (생성자에서 제공했을 경우)|
|File|getParentFile()|- 현재 파일의 상위 경로를 가진 File 객체 생성 (생성자에서 제공했을 경우)|

```java
package study.first;

import java.io.File;

public class Main {
	public static void main(String[] args) {

		try {
			
			File file = new File("input3.txt");
			
			System.out.println(file.getPath()); // "input3.txt"
			System.out.println(file.getAbsolutePath()); // 전체 주소 출력
			System.out.println(file.getAbsoluteFile()); // 전체 주소 출력(File 객체 생성)
			System.out.println(file.getParent()); // 생성자에  Parent주소 없으므로 null
			System.out.println(file.getParentFile()); // 생성자에 Parent주소 없으므로 null

		} catch (Exception e) {
			System.out.println("파일 및 폴더 생성에 실패했습니다.");
		}
	}
}
```

---

| return type  |  method   | 설명  |
|---|---|---|
|String[]|list()|- 현재 경로의 파일 또는 디렉토리 목록 추출|
|File[]|listFiles()|- 현재 경로의 파일 또는 디렉토리를 가지는 File 타입 배열 추출|

File 인스턴스가 파일이 아닌 경로를 가지고 있을 때 해당 경로에 존재하는 파일 및 폴더의 정보를 배열에 담아줍니다. 파일 정보까지 가지고 있을 때는 예외가 발생됩니다.

```java
package study.first;

import java.io.File;

public class Main {
	public static void main(String[] args) {

		try {
			
			File file = new File("C:\\JAVA\\FirstStudy\\FisrtStudy");

			String[] arr = file.list();
			
			for(String a : arr)
				System.out.println(a);	// 폴더 내 모든 파일과 디렉토리 정보	

		} catch (Exception e) {
			System.out.println("파일 및 폴더 생성에 실패했습니다.");
		}
	}
}
```

예외 발생을 피하고 싶으면 isDiretory() 메소드를 이용해 조건문으로 써주면 됩니다.

![](https://blog.kakaocdn.net/dn/PxsT8/btqAwIQPpVA/PwBcHbydirTOeARvPCajZK/img.png)

```java
package study.first;

import java.io.File;

public class Main {
	public static void main(String[] args) {

		File file = new File("C:\\JAVA\\FirstStudy\\FisrtStudy\\input.txt");

		// "디렉토리 정보라면"
		if (file.isDirectory()) {
			String[] arr = file.list();

			for (String a : arr)
				System.out.println(a); // 폴더 내 모든 파일과 디렉토리 정보
			
		} else {
			
			System.out.println("File 객체가 폴더가 아닙니다.");
		}
	}
}
```


---
출처 - https://searcher.tistory.com/373

https://codevang.tistory.com/156