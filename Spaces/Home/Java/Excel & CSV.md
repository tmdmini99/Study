


```java
package com.kpop.merch.common.util;  
  
import com.kpop.merch.common.vo.BasicVo;  
import org.apache.ibatis.session.SqlSession;  
import org.apache.ibatis.session.SqlSessionFactory;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.http.HttpHeaders;  
import org.springframework.http.HttpStatus;  
import org.springframework.http.ResponseEntity;  
import org.springframework.web.bind.annotation.PostMapping;  
import org.springframework.web.bind.annotation.RequestParam;  
import org.springframework.web.bind.annotation.RestController;  
  
import java.io.ByteArrayOutputStream;  
import java.io.IOException;  
import java.lang.reflect.Field;  
import java.nio.charset.StandardCharsets;  
import java.util.ArrayList;  
import java.util.List;  
import java.util.Map;  
  
@RestController  
public class ExcelUtil {  
  
    @Autowired  
    private SqlSessionFactory sqlSessionFactory;  
  
    @PostMapping("/excel/excel")  
    public ResponseEntity<byte[]> generateCsv(@RequestParam String table) {  
        if (table == null || table.isEmpty()) {  
            return ResponseEntity.badRequest().body("Table name is required.".getBytes(StandardCharsets.UTF_8));  
        }  
  
        try (SqlSession sqlSession = sqlSessionFactory.openSession()) {  
            // MyBatis를 사용하여 데이터 조회  
            List<? extends BasicVo> rows = sqlSession.selectList(table + ".selectList");  
  
            if (rows == null || rows.isEmpty()) {  
                return ResponseEntity.status(HttpStatus.NO_CONTENT)  
                    .body("No data found in the specified table.".getBytes(StandardCharsets.UTF_8));  
            }  
  
            // CSV 콘텐츠 생성  
            StringBuilder csvContent = new StringBuilder();  
            List<String> headers = getHeaders(rows.get(0));  
  
            // 헤더 작성  
            csvContent.append(String.join(",", headers)).append("\n");  
  
            // 데이터 행 작성  
            for (BasicVo row : rows) {  
                List<String> rowValues = new ArrayList<>();  
                for (String header : headers) {  
                    String value = getFieldValue(row, header);  
                    rowValues.add(value.replace(",", " ")); // 쉼표 제거  
                }  
                csvContent.append(String.join(",", rowValues)).append("\n");  
            }  
  
            // ResponseEntity로 CSV 파일 반환  
            HttpHeaders headersResponse = new HttpHeaders();  
            headersResponse.add(HttpHeaders.CONTENT_DISPOSITION, "attachment; filename=\"" + table + ".csv\"");  
            headersResponse.add(HttpHeaders.CONTENT_TYPE, "text/csv");  
  
            return new ResponseEntity<>(csvContent.toString().getBytes(StandardCharsets.UTF_8), headersResponse, HttpStatus.OK);  
        } catch (Exception e) {  
            e.printStackTrace();  
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)  
                .body("Error while generating CSV file.".getBytes(StandardCharsets.UTF_8));  
        }  
    }  
  
    private List<String> getHeaders(BasicVo vo) {  
        List<String> headers = new ArrayList<>();  
        Field[] fields = vo.getClass().getDeclaredFields();  
        for (Field field : fields) {  
            headers.add(toSnakeCase(field.getName()));  
        }  
        return headers;  
    }  
  
    private String getFieldValue(BasicVo vo, String fieldName) {  
        try {  
            String camelCaseFieldName = toCamelCase(fieldName);  
            Object value = vo.getClass().getMethod("get" + capitalize(camelCaseFieldName)).invoke(vo);  
            return (value == null) ? "" : value.toString();  
        } catch (Exception e) {  
            e.printStackTrace();  
            return "";  
        }  
    }  
  
    private String toCamelCase(String snakeCase) {  
        StringBuilder result = new StringBuilder();  
        String[] parts = snakeCase.split("_");  
        for (String part : parts) {  
            if (result.length() == 0) {  
                result.append(part.toLowerCase());  
            } else {  
                result.append(part.substring(0, 1).toUpperCase()).append(part.substring(1).toLowerCase());  
            }  
        }  
        return result.toString();  
    }  
  
    private String toSnakeCase(String fieldName) {  
        StringBuilder result = new StringBuilder();  
        for (char c : fieldName.toCharArray()) {  
            if (Character.isUpperCase(c)) {  
                if (result.length() > 0) {  
                    result.append("_");  
                }  
                result.append(Character.toLowerCase(c));  
            } else {  
                result.append(c);  
            }  
        }  
        return result.toString();  
    }  
  
    private String capitalize(String str) {  
        if (str == null || str.isEmpty()) {  
            return str;  
        }  
        return str.substring(0, 1).toUpperCase() + str.substring(1);  
    }  
}
```




CSV
```java
package com.kpop.merch.common.util;  
  
import com.kpop.merch.common.vo.BasicVo;  
import org.apache.ibatis.session.SqlSession;  
import org.apache.ibatis.session.SqlSessionFactory;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.http.HttpHeaders;  
import org.springframework.http.HttpStatus;  
import org.springframework.http.ResponseEntity;  
import org.springframework.web.bind.annotation.PostMapping;  
import org.springframework.web.bind.annotation.RequestParam;  
import org.springframework.web.bind.annotation.RestController;  
  
import java.lang.reflect.Field;  
import java.nio.charset.StandardCharsets;  
import java.util.ArrayList;  
import java.util.List;  
  
@RestController  
public class CSVUtil {  
  
    @Autowired  
    private SqlSessionFactory sqlSessionFactory;  
  
    @PostMapping("/csv/csv")  
    public ResponseEntity<byte[]> generateCsv(@RequestParam String table) {  
        System.out.println("들어옴");  
        if (table == null || table.isEmpty()) {  
            return ResponseEntity.badRequest()  
                    .header(HttpHeaders.CONTENT_TYPE, "text/plain")  
                    .body("Table name is required.".getBytes(StandardCharsets.UTF_8));  
        }  
  
        try (SqlSession sqlSession = sqlSessionFactory.openSession()) {  
            List<? extends BasicVo> rows = sqlSession.selectList(table + ".selectList");  
  
            if (rows == null || rows.isEmpty()) {  
                return ResponseEntity.status(HttpStatus.NO_CONTENT)  
                        .header(HttpHeaders.CONTENT_TYPE, "text/plain")  
                        .body("No data found in the specified table.".getBytes(StandardCharsets.UTF_8));  
            }  
  
            StringBuilder csvContent = new StringBuilder();  
            List<String> headers = getHeaders(rows.get(0));  
  
            // 헤더 작성  
            csvContent.append(String.join(",", headers)).append("\n");  
  
            // 데이터 행 작성  
            for (BasicVo row : rows) {  
                List<String> rowValues = new ArrayList<>();  
                for (String header : headers) {  
                    String value = getFieldValue(row, header);  
                    // 쉼표나 줄바꿈이 포함된 경우 큰따옴표로 감싸기  
                    rowValues.add("\"" + value.replace("\"", "\"\"") + "\"");  
                }  
                csvContent.append(String.join(",", rowValues)).append("\n");  
            }  
  
            HttpHeaders headersResponse = new HttpHeaders();  
            headersResponse.add(HttpHeaders.CONTENT_DISPOSITION, "attachment; filename=\"" + table + ".csv\"");  
            headersResponse.add(HttpHeaders.CONTENT_TYPE, "text/csv");  
  
            return new ResponseEntity<>(csvContent.toString().getBytes(StandardCharsets.UTF_8), headersResponse, HttpStatus.OK);  
        } catch (Exception e) {  
            e.printStackTrace();  
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)  
                    .header(HttpHeaders.CONTENT_TYPE, "text/plain")  
                    .body("Error while generating CSV file.".getBytes(StandardCharsets.UTF_8));  
        }  
    }  
  
  
    // 모든 필드 이름을 가져오는 메서드  
    private List<String> getHeaders(BasicVo vo) {  
        List<String> headers = new ArrayList<>();  
        Field[] fields = vo.getClass().getDeclaredFields();  
        for (Field field : fields) {  
            headers.add(field.getName()); // 모든 필드 이름 추가  
        }  
        return headers;  
    }  
  
    // 필드 값을 가져오는 메서드  
    private String getFieldValue(BasicVo vo, String fieldName) {  
        try {  
            Field field = vo.getClass().getDeclaredField(fieldName);  
            field.setAccessible(true);  
            Object value = field.get(vo);  
  
            // 값이 null이 아닐 경우 공백과 줄바꿈 제거  
            return (value == null) ? "" : value.toString().replaceAll("\\s+", "").replaceAll("[\\n\\r]", "");  
        } catch (Exception e) {  
            e.printStackTrace();  
            return "";  
        }  
    }  
}
```