


```java
@InitBinder
public void initBinder(WebDataBinder binder) {
    binder.registerCustomEditor(List.class, "sc_ID_List", new PropertyEditorSupport() {
        @Override
        public void setAsText(String text) {
            List<Integer> list = Arrays.stream(text.replaceAll("\\[|\\]", "").split(","))
                    .map(String::trim)
                    .map(Integer::parseInt)
                    .collect(Collectors.toList());
            setValue(list);
        }
    });
}
```


## 적용 위치

Controller 클래스 안에 다음처럼 작성합니다:

```java
@Controller
@RequestMapping("/your-url")
public class YourController {

    @InitBinder
    public void initBinder(WebDataBinder binder) {
        binder.registerCustomEditor(List.class, "sc_ID_List", new PropertyEditorSupport() {
            @Override
            public void setAsText(String text) {
                List<Integer> list = Arrays.stream(text.replaceAll("\\[|\\]", "").split(","))
                        .map(String::trim)
                        .map(Integer::parseInt)
                        .collect(Collectors.toList());
                setValue(list);
            }
        });
    }

    @RequestMapping("/your-path")
    public String yourHandlerMethod(BasicParamVo paramVo) {
        // 여기서 paramVo.getSc_ID_List()가 List<Integer>로 자동 변환됨
        System.out.println("sc_ID_List = " + paramVo.getSc_ID_List());
        return "someView";
    }
}
```


## 커스텀 PropertyEditor 공통 클래스로 만들기

### 1. 공통 PropertyEditor 클래스 생성


```java
public class StringToIntegerListEditor extends PropertyEditorSupport {
    @Override
    public void setAsText(String text) {
        if (text == null || text.isEmpty()) {
            setValue(Collections.emptyList());
            return;
        }

        List<Integer> list = Arrays.stream(text.replaceAll("\\[|\\]", "").split(","))
                .map(String::trim)
                .map(Integer::parseInt)
                .collect(Collectors.toList());

        setValue(list);
    }
}
```


각 컨트롤러에서 재사용

```java
@InitBinder
public void initBinder(WebDataBinder binder) {
    binder.registerCustomEditor(List.class, "sc_ID_List", new StringToIntegerListEditor());
}
```

※ 여기서 `"sc_ID_List"`는 VO의 필드명입니다.


## 방법 2: WebMvcConfigurer + Converter로 전역 등록 (추천)

Spring Boot 2.0+ 이상에서는 `WebMvcConfigurer`에서 `Converter`로 등록하면,  
**모든 컨트롤러에 전역으로 적용**할 수 있습니다.


```java
@Component
public class StringToIntegerListConverter implements Converter<String, List<Integer>> {
    @Override
    public List<Integer> convert(String source) {
        return Arrays.stream(source.replaceAll("\\[|\\]", "").split(","))
                .map(String::trim)
                .map(Integer::parseInt)
                .collect(Collectors.toList());
    }
}
```


2. WebMvcConfigurer에서 등록 (Spring 설정 클래스에)

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addFormatters(FormatterRegistry registry) {
        registry.addConverter(new StringToIntegerListConverter());
    }
}
```


### 그럼 이렇게 사용할 수 있습니다:

VO에서 그냥 선언:

```java
private List<Integer> sc_ID_List;
```

HTML에서 넘길 땐:
```html
<input type="hidden" name="sc_ID_List" value="[46,45,43]">
```


### Legacy 프로젝트에서 공통 `PropertyEditor` 재사용 방법

#### 1. 공통 PropertyEditor 클래스 만들기


```java
package com.example.editor;

import java.beans.PropertyEditorSupport;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class StringToIntegerListEditor extends PropertyEditorSupport {

    @Override
    public void setAsText(String text) {
        if (text == null || text.trim().isEmpty()) {
            setValue(new ArrayList<>());
            return;
        }

        List<Integer> list = new ArrayList<>();
        String[] items = text.replaceAll("\\[|\\]", "").split(",");

        for (String item : items) {
            try {
                list.add(Integer.parseInt(item.trim()));
            } catch (NumberFormatException e) {
                // 필요시 로깅 또는 무시
            }
        }

        setValue(list);
    }
}
```

2. 각 컨트롤러에서 등록

```java
@InitBinder
public void initBinder(WebDataBinder binder) {
    binder.registerCustomEditor(List.class, "sc_ID_List", new StringToIntegerListEditor());
}
```


※ 여기서 `"sc_ID_List"`는 VO의 필드명입니다.

4. HTML 파라미터 예시

```html
<input type="hidden" name="sc_ID_List" value="1,2,3">
```

`value="[1,2,3]"` 형태도 처리 가능하게 위에서 `replaceAll("\\[|\\]", "")` 처리해뒀습니다.


