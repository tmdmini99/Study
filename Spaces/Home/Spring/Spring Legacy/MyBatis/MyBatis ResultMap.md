
result map 사용법
```xml
<select id="selectList" resultMap="giftCardsResultMap" parameterType="com.kpop.merch.common.vo.BasicParamVo">
    SELECT *
    FROM GIFT_CARDS
    LEFT JOIN CUSTOMERS
    ON GIFT_CARDS.customer_id = CUSTOMERS.id
    <include refid="common.postfix"/>
</select>

<resultMap id="giftCardsResultMap" type="com.kpop.merch.products.vo.GiftCardsVo">
    <!-- GIFT_CARDS 테이블 매핑 -->
    <id property="id" column="id"/>
    <result property="balance" column="balance"/>
    <result property="createdAt" column="created_at"/>
    <result property="updatedAt" column="updated_at"/>
    <result property="currency" column="currency"/>
    <result property="initialValue" column="initial_value"/>
    <result property="disabledAt" column="disabled_at"/>
    <result property="lineItemId" column="line_item_id"/>
    <result property="apiClientId" column="api_client_id"/>
    <result property="userId" column="user_id"/>
    <result property="customerId" column="customer_id"/>
    <result property="note" column="note"/>
    <result property="expiresOn" column="expires_on"/>
    <result property="templateSuffix" column="template_suffix"/>
    <result property="notify" column="notify"/>
    <result property="lastCharacters" column="last_characters"/>
    <result property="orderId" column="order_id"/>

    <!-- CUSTOMERS 테이블 매핑 (CustomersVo) -->
    <association property="customersVo" javaType="com.kpop.merch.customers.vo.CustomersVo">
        <id property="id" column="customers.id"/> <!-- CUSTOMERS 테이블의 id -->
        <result property="name" column="customers.name"/> <!-- 예시: 고객 이름 -->
        <result property="email" column="customers.email"/> <!-- 예시: 고객 이메일 -->
        <!-- 다른 CUSTOMERS 테이블 컬럼들에 대한 매핑 -->
    </association>
</resultMap>

```