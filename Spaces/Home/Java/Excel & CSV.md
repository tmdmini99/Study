


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