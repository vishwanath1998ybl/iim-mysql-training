# Categorical Encoding

1. **One Hot Encoding** : Motive is to convert the categorical / groups of values into integer value, and each value having its own column
2. **One Hot Encoding (Top Categories)** : Motive is to convert Top n category  categorical / groups of values into integer value, and each value having its own column
3. **Label Encoding** : Map the categorical / group data into numeric value in a single column.
4. **Count Frequency Encoding** : Map the categorical / group data with the frequency of values
5. **Ordered Ordinal Encoding** : Sort the data in ascending / descending order and then perform label encoding



```sql
USE imputation;
```

```sql
CREATE TABLE product_sales (
    id INT AUTO_INCREMENT PRIMARY KEY,
    product_name VARCHAR(50),
    category VARCHAR(50),
    sales INT
);

INSERT INTO product_sales(product_name,category,sales) VALUES
('Product_A','Electronics',100),
('Product_B','Clothing',150),
('Product_C','Electronics',200),
('Product_D','Furniture',120),
('Product_E','Furniture',80),
('Product_F','Clothing',140);
```

```sql
-- One Hot Encoding
Select id, product_name, sales,
CASE WHEN category = 'Electronics' THEN 1 ELSE 0 END as category_electronics,
CASE WHEN category = 'Clothing' THEN 1 ELSE 0 END as category_clothing,
CASE WHEN category = 'Furniture' THEN 1 ELSE 0 END as category_furniture
FROM product_sales;
```

```sql
-- One hot encoding (Top n category)
Select id, product_name, sales,
CASE WHEN category = 'Electronics' THEN 1 ELSE 0 END as category_electronics,
CASE WHEN category = 'Furniture' THEN 1 ELSE 0 END as category_furniture,
CASE WHEN category NOT IN ('Electronics','Furniture') THEN 1 ELSE 0 END as category_other
FROM product_sales;
```

```sql
-- Label Encoding
Select id, product_name, sales,category,
CASE
WHEN category = 'Electronics' THEN 1
WHEN category = 'Furniture' THEN 2
WHEN category = 'Clothing' THEN 3
ELSE 0 
END AS label_encoding
FROM product_sales;
```

```sql
-- Count Frequence Encoding
SELECT ps.id, ps.product_name, ps.sales,ps.category,
cc.freq AS count_freq
FROM product_sales ps
JOIN (
    Select category, COUNT(*) as freq
    FROM product_sales
    GROUP BY category
) as cc ON ps.category = cc.category;
```

