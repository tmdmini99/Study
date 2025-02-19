

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



update set forEach

```xml
<update id="updateModalTranslates" parameterType="map" keyProperty="translates_id" useGeneratedKeys="true">  
    WITH updated AS (  
        UPDATE ${tableNm}_translates        <set>  
            <foreach collection="columnsTranslates" item="column" index="index" separator=",">  
                ${column} = #{valuesTranslates[${index}]}  
            </foreach>  
        </set>  
        WHERE id = #{translates_id}::numeric  
        RETURNING id    )    INSERT INTO ${tableNm}_translates (        id,        <foreach collection="columnsTranslates" item="column" separator=",">  
            ${column}  
        </foreach>  
        ,reg_id  
    )    SELECT #{id}::numeric,        <foreach collection="valuesTranslates" item="value" separator=",">  
            #{value}  
        </foreach>  
    WHERE NOT EXISTS (SELECT 1 FROM updated)  
    RETURNING id as translates_id</update>
```
