

http 허용
```xml
<?xml version="1.0" encoding="utf-8"?>  
<network-security-config xmlns:android="http://schemas.android.com/apk/res/android">  
    <domain-config cleartextTrafficPermitted="true">  
        <domain includeSubdomains="true">10.0.2.2</domain>  
    </domain-config>  
    <!-- 다른 도메인 설정이 필요한 경우 여기에 추가 -->  
</network-security-config>
```

http 금지 https 허용
```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config xmlns:android="http://schemas.android.com/apk/res/android">
    <domain-config cleartextTrafficPermitted="false">
        <domain includeSubdomains="true">example.com</domain>
    </domain-config>
</network-security-config>

```