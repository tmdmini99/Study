

String type split 후 forEach문 돌리기

```xml
<delete id="deleteSelectedProducts" parameterType="map">
    DELETE FROM products
    WHERE id IN
    <foreach collection="ids" item="id" separator="," open="(" close=")">
        #{id}
    </foreach>
</delete>
```


#### **📌 2️⃣ MySQL / PostgreSQL에서 `STRING_TO_ARRAY()` 사용**

**PostgreSQL**:


```xml
<delete>
  DELETE FROM products
  WHERE id = ANY(STRING_TO_ARRAY(#{id}, ',')::INT[])
</delete>
```


```xml
<delete>
  DELETE FROM products
  WHERE FIND_IN_SET(id, #{id})
</delete>
```
