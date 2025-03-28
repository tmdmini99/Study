


# Aws.java

`com.kwm.web.common.util.Aws`

AWS S3 Presigned URL을 생성하고 S3 클라이언트 객체를 관리하는 Spring 기반 유틸리티 클래스입니다.

---

## 📦 클래스 개요

```java
@Component
public class Aws {
    public final Region REGION = Region.AP_NORTHEAST_2; // 서울 리전
    private final AwsBasicCredentials awsCreds;
    ...
}
```

- AWS SDK v2 기반 S3 유틸리티 클래스
- `@Component`로 등록되어 Bean으로 주입 가능
- `PUT`, `GET` 방식의 Presigned URL을 생성

---

## 🧩 주요 필드 및 생성자

```java
public final Region REGION = Region.AP_NORTHEAST_2; // 서울 리전
private final AwsBasicCredentials awsCreds;
```

```java
public Aws(@Value("#{setting['aws.access.key']}") String accessKey,
           @Value("#{setting['aws.secret.key']}") String secretKey) {
    this.awsCreds = AwsBasicCredentials.create(accessKey, secretKey);
}
```

- Spring Expression Language를 사용하여 `application.properties` 또는 설정 파일에서 AWS 키 주입

---

## ☁️ S3 Client 생성

```java
public S3Client s3Client() {
    return S3Client.builder()
            .region(REGION)
            .credentialsProvider(StaticCredentialsProvider.create(awsCreds))
            .build();
}
```

- 일반적인 S3 API (deleteObject, listObjects 등)에 사용

---

## 🔐 S3 Presigner 생성

```java
public S3Presigner s3Presigner() {
    return S3Presigner.builder()
            .region(REGION)
            .credentialsProvider(StaticCredentialsProvider.create(awsCreds))
            .build();
}
```

- Presigned URL 생성 전용 객체

---

## 📤 업로드 Presigned URL 생성

```java
public String generatePresignedUrl(String bucketName, String key, Duration duration)
```

### 🔧 내부 구현
```java
PutObjectRequest putObjectRequest = PutObjectRequest.builder()
    .bucket(bucketName)
    .key(key)
    .build();

PutObjectPresignRequest presignRequest = PutObjectPresignRequest.builder()
    .putObjectRequest(putObjectRequest)
    .signatureDuration(duration)
    .build();

return s3Presigner().presignPutObject(presignRequest).url().toString();
```

### 📌 사용 예시
```java
String url = aws.generatePresignedUrl("my-bucket", "images/image1.jpg", Duration.ofMinutes(10));
```

---

## 📥 다운로드 Presigned URL 생성

```java
public String generateDownloadPresignedUrl(String bucketName, String key, Duration duration)
```

### 🔧 내부 구현
```java
GetObjectRequest getObjectRequest = GetObjectRequest.builder()
    .bucket(bucketName)
    .key(key)
    .build();

GetObjectPresignRequest presignRequest = GetObjectPresignRequest.builder()
    .getObjectRequest(getObjectRequest)
    .signatureDuration(duration)
    .build();

return s3Presigner().presignGetObject(presignRequest).url().toString();
```

### 📌 사용 예시
```java
String downloadUrl = aws.generateDownloadPresignedUrl("my-bucket", "downloads/file1.pdf", Duration.ofMinutes(5));
```

---

## ✅ 장점 및 확장 포인트

| 항목 | 설명 |
|------|------|
| ✅ Bean 등록 | Spring에서 주입 가능 (@Component) |
| 🔐 키 보안 | 설정 파일에서 관리 (권장: 환경 변수 or Vault 사용) |
| 📄 가독성 | `PUT`, `GET` 방식 메서드로 분리 |
| 🧩 확장성 | Content-Type, Metadata, ACL 추가 가능 |

---

## 💡 확장 아이디어

- 여러 Presigned URL 일괄 생성 메서드
- 썸네일/원본 구분 Presigned URL 헬퍼 추가
- `deleteObject()`, `copyObject()` 등 유틸 추가
- 만료 시간 기본값 상수화

---

## 📚 의존성 (pom.xml)
```xml
<dependency>
    <groupId>software.amazon.awssdk</groupId>
    <artifactId>s3</artifactId>
    <version>2.20.43</version>
</dependency>
```

---


