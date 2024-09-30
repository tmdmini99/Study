자바에서 JSON 사용


```java
Request request = new Request.Builder()  
  .url("https://tmdals23222.cafe24api.com/api/v2/admin/products")  
  .addHeader("Authorization", "Bearer npoXJR08JJoieKJimPJBRC")  
  .addHeader("Content-Type", "application/json")  
  .addHeader("X-Cafe24-Api-Version", "2024-06-01")  
  .get()  
  .build();
```


```json
{
    "products": [
        {
            "shop_no": 1,
            "product_no": 24,
            "product_code": "P000000X",
            "custom_product_code": "",
            "product_name": "iPhone X",
            "eng_product_name": "iPhone Ten",
            "supply_product_name": "iphone A1865 fdd lte",
            "internal_product_name": "Sample Internal product name",
            "model_name": "A1865",
            "price_excluding_tax": "1000.00",
            "price": "1100.00",
            "retail_price": "0.00",
            "supply_price": "9000.00",
            "display": "T",
            "selling": "F",
            "product_condition": "U",
            "product_used_month": 2,
            "summary_description": "This is Product Summary.",
            "margin_rate": "10.00",
            "tax_calculation": "M",
            "tax_type": "A",
            "tax_rate": 10,
            "price_content": null,
            "buy_limit_by_product": "T",
            "buy_limit_type": "F",
            "buy_group_list": [
                8,
                9
            ],
            "buy_member_id_list": [
                "sampleid",
                "testid"
            ],
            "repurchase_restriction": "F",
            "single_purchase_restriction": "F",
            "buy_unit_type": "O",
            "buy_unit": 1,
            "order_quantity_limit_type": "O",
            "minimum_quantity": 1,
            "maximum_quantity": 0,
            "points_by_product": "T",
            "points_setting_by_payment": "C",
            "points_amount": [
                {
                    "payment_method": "cash",
                    "points_rate": "0.00%"
                },
                {
                    "payment_method": "mileage",
                    "points_rate": "0.00%"
                }
            ],
            "except_member_points": "F",
            "product_volume": {
                "use_product_volume": "T",
                "product_width": "3cm",
                "product_height": "5.5cm",
                "product_length": "7cm"
            },
            "adult_certification": "F",
            "detail_image": "https://{domain}/web/product/big/201711/20_shop1_750339.png",
            "list_image": "https://{domain}/web/product/medium/201711/20_shop1_750339.png",
            "tiny_image": "https://{domain}/web/product/tiny/201711/20_shop1_750339.png",
            "small_image": "https://{domain}/web/product/small/201711/20_shop1_750339.png",
            "use_naverpay": "T",
            "naverpay_type": "C",
            "use_kakaopay": "T",
            "manufacturer_code": "M0000000",
            "trend_code": "T0000000",
            "brand_code": "B0000000",
            "supplier_code": "S0000000",
            "made_date": "",
            "release_date": "",
            "expiration_date": {
                "start_date": "2017-09-08",
                "end_date": "2017-09-14"
            },
            "origin_classification": "F",
            "origin_place_no": 1798,
            "origin_place_value": "",
            "made_in_code": "KR",
            "icon_show_period": {
                "start_date": "2017-10-30T09:00:00+09:00",
                "end_date": "2017-11-02T16:00:00+09:00"
            },
            "icon": [
                "icon_01_01",
                "icon_02_01"
            ],
            "hscode": "4303101990",
            "product_weight": "1.00",
            "product_material": "",
            "created_date": "2018-01-18T11:19:27+09:00",
            "updated_date": "2018-01-19T11:19:27+09:00",
            "english_product_material": null,
            "cloth_fabric": null,
            "list_icon": {
                "soldout_icon": true,
                "recommend_icon": false,
                "new_icon": false
            },
            "approve_status": "",
            "classification_code": "C000000A",
            "sold_out": "F",
            "additional_price": "0.00",
            "clearance_category_eng": "Necklaces",
            "clearance_category_kor": "주얼리 > 목걸이",
            "clearance_category_code": "ACAB0000",
            "exposure_limit_type": "M",
            "exposure_group_list": [
                8,
                9
            ],
            "set_product_type": null,
            "shipping_fee_by_product": "T",
            "shipping_fee_type": "W",
            "main": [
                3,
                2
            ],
            "market_sync": "F"
        },
        {
            "shop_no": 1,
            "product_no": 23,
            "product_code": "P000000W",
            "custom_product_code": "",
            "product_name": "iPhone X",
            "eng_product_name": "iPhone Ten",
            "supply_product_name": "iphone A1865 fdd lte",
            "internal_product_name": "Sample Internal product name",
            "model_name": "A1865",
            "price": 1000,
            "retail_price": 0,
            "supply_price": 0,
            "display": "T",
            "selling": "F",
            "product_condition": "U",
            "product_used_month": 2,
            "summary_description": "This is Product Summary.",
            "margin_rate": "10.00",
            "tax_calculation": "M",
            "tax_type": "A",
            "tax_rate": 10,
            "price_content": null,
            "buy_limit_by_product": "T",
            "buy_limit_type": "F",
            "buy_group_list": [
                8,
                9
            ],
            "buy_member_id_list": [
                "sampleid",
                "testid"
            ],
            "repurchase_restriction": "F",
            "single_purchase_restriction": "F",
            "buy_unit_type": "O",
            "buy_unit": 1,
            "order_quantity_limit_type": "O",
            "minimum_quantity": 1,
            "maximum_quantity": 0,
            "points_by_product": "T",
            "points_setting_by_payment": "C",
            "points_amount": [
                {
                    "payment_method": "cash",
                    "points_rate": "0.00%"
                },
                {
                    "payment_method": "mileage",
                    "points_rate": "0.00%"
                }
            ],
            "except_member_points": "F",
            "product_volume": {
                "use_product_volume": "T",
                "product_width": "3cm",
                "product_height": "5.5cm",
                "product_length": "7cm"
            },
            "adult_certification": "F",
            "detail_image": "https://{domain}/web/product/big/201711/20_shop1_750339.png",
            "list_image": "https://{domain}/web/product/medium/201711/20_shop1_750339.png",
            "tiny_image": "https://{domain}/web/product/tiny/201711/20_shop1_750339.png",
            "small_image": "https://{domain}/web/product/small/201711/20_shop1_750339.png",
            "use_naverpay": "T",
            "naverpay_type": "C",
            "use_kakaopay": "T",
            "manufacturer_code": "M0000000",
            "trend_code": "T0000000",
            "brand_code": "B0000000",
            "supplier_code": "S0000000",
            "made_date": "",
            "release_date": "",
            "expiration_date": {
                "start_date": "2017-09-08",
                "end_date": "2017-09-14"
            },
            "origin_classification": "F",
            "origin_place_no": 1798,
            "origin_place_value": "",
            "made_in_code": "KR",
            "icon_show_period": {
                "start_date": "2017-10-30T09:00:00+09:00",
                "end_date": "2017-11-02T16:00:00+09:00"
            },
            "icon": [
                "icon_01_01",
                "icon_02_01"
            ],
            "hscode": "4303101990",
            "product_weight": "1.00",
            "product_material": "",
            "created_date": "2018-01-18T11:19:27+09:00",
            "updated_date": "2018-01-19T11:19:27+09:00",
            "english_product_material": null,
            "cloth_fabric": null,
            "list_icon": {
                "soldout_icon": true,
                "recommend_icon": false,
                "new_icon": false
            },
            "approve_status": "C",
            "classification_code": "C000000A",
            "sold_out": "F",
            "additional_price": "0.00",
            "clearance_category_eng": null,
            "clearance_category_kor": null,
            "clearance_category_code": null,
            "exposure_limit_type": "M",
            "exposure_group_list": [
                8,
                9
            ],
            "set_product_type": null,
            "shipping_fee_by_product": "F",
            "shipping_fee_type": "T",
            "main": [
                3,
                2
            ],
            "market_sync": "F"
        }
    ]
}
```
이런식으로 떠야 하는데  

 이런식으로 나올때 해결법
```json
Response{protocol=h2, code=200, message=, url=https://tmdals23222.cafe24api.com/api/v2/admin/products}
```


```java
// Execute the request
Response response = client.newCall(request).execute();

// Check if the response is successful
if (response.isSuccessful()) {
    // Read the response body as a string
    String responseBody = response.body().string();
    
    // Print the response body
    System.out.println(responseBody);
} else {
    // Handle unsuccessful response
    System.out.println("Request failed: " + response.message());
}

```

gson 사용

```java
import com.google.gson.Gson;
import com.google.gson.JsonObject;
import com.google.gson.JsonArray;

// Initialize Gson
Gson gson = new Gson();

// Parse the JSON response body
JsonObject jsonResponse = gson.fromJson(responseBody, JsonObject.class);

// Extract data from the JSON response
JsonArray products = jsonResponse.getAsJsonArray("products");
for (int i = 0; i < products.size(); i++) {
    JsonObject product = products.get(i).getAsJsonObject();
    System.out.println("Product Name: " + product.get("product_name").getAsString());
    // Extract other fields as needed
}

```

