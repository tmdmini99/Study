

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