

```json
[
    { "productId": "123", "quantity": 2 },
    { "productId": "456", "quantity": 5 },
    { "productId": "789", "quantity": 1 }
]
```

데이터 삽입시 for문 돌려서  insert 돌리기
```java
@Data  
public class KWM1200CUDParamVo extends BasicCUDParamVo {  
    private String regId;  
    private List<Map<String, Object>> cartRow;  
}
```


```java
ResponseBody  
@RequestMapping("/1200Cart")  
public Map<String, Object> modalCud(@RequestBody List<Map<String, Object>> rows, HttpServletRequest request, HttpServletResponse response) throws Exception {  
    log.info("Received Map: {}", rows);  
    KWM1200CUDParamVo paramVo = new KWM1200CUDParamVo();  
    paramVo.setCartRow(rows);  
    paramVo.setRegId(LoginUtil.getCurrentUser().getUserId());  
    Map<String, Object> result = service.cartInsert(paramVo);  
    return result;  
}
```


```xml
<insert id="cartInsert" parameterType="map">
    <foreach collection="cartRow" item="row" separator=";">
        INSERT INTO cart_items (
        user_id,
        product_id,
        quantity
        )
        VALUES (
        #{regId},
        #{row.productId} :: numeric,
        #{row.quantity}:: numeric
        )
        ON CONFLICT (user_id, product_id)
        DO UPDATE SET
        quantity = cart_items.quantity + #{row.quantity} :: numeric,
        upt_dt = now()
    </foreach>
</insert>
```