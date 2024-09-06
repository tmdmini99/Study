비동기 insert
```js
axios(options)

  .then(async function (response) {

    const products = response.data.products;

  

    try {

      for (const product of products) {

        const normalizedProduct = normalizeProductData(product);

        const keys = Object.keys(normalizedProduct);

        const values = keys.map(key => normalizedProduct[key]);

        const columns = keys.join(', ');

        const placeholders = keys.map((_, i) => `$${i + 1}`).join(', ');

  

        const query = `INSERT INTO products (${columns}) VALUES (${placeholders})

                       ON CONFLICT (product_code) DO NOTHING`;

  

        await pool.query(query, values);

      }

      console.log('Products inserted successfully.');

    } catch (err) {

      console.error('Error inserting products:', err.stack);

    }

  })

  .catch(function (error) {

    console.error('Error making API request:', error);

  });
```