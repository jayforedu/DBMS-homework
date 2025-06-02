
## CustomerPublicInfo View

顯示顧客基本資料:

```sql
CREATE VIEW CustomerPublicInfo AS
SELECT 
    customer_id,
    name
FROM Customer;
```

### 範例

```sql
SELECT * FROM CustomerPublicInfo;
```

### 結果範例

| customer_id | name       |
|-------------|------------|
| 1           | Alice Lin  |
| 2           | Bob Lee    |


---

## SellerStats View

統計每位賣家商品數量與平均評價：

```sql
CREATE VIEW SellerStats AS
SELECT 
    s.seller_id,
    s.name AS seller_name,
    COUNT(p.product_id) AS total_products,
    ROUND(AVG(r.rating), 2) AS average_rating
FROM Seller s
LEFT JOIN Product p ON s.seller_id = p.seller_id
LEFT JOIN Review r ON p.product_id = r.product_id
GROUP BY s.seller_id, s.name;
```

### 範例

```sql
SELECT * FROM SellerStats WHERE average_rating >= 4.5;
```

### 結果範例

| seller_id | seller_name | total_products | average_rating |
|-----------|--------------|----------------|----------------|
| 1         | Jane Chen    | 12             | 4.67           |


---

## HighPricedProducts View

列出目前庫存中之商品資訊：

```sql
CREATE VIEW HighPricedProducts AS
SELECT 
    product_id,
    name,
    price,
    stock,
    category,
    seller_id
FROM Product
WHERE stock > 0
ORDER BY price DESC;
```

### 範例

```sql
SELECT * FROM HighPricedProducts LIMIT 2;
```

### 結果範例

| product_id | name         | price | stock | category | seller_id |
|------------|--------------|-------|-------|----------|------------|
| 101        | Luxury Watch | 8999  | 5     | Watches  | 2          |
| 102        | Diamond Ring | 7999  | 3     | Jewelry  | 3          |


---

## OrderSummary View

呈現訂單基本資料與顧客資訊：

```sql
CREATE VIEW OrderSummary AS
SELECT 
    o.order_id,
    o.order_date,
    o.status,
    o.customer_id,
    c.name AS customer_name
FROM `Order` o
JOIN Customer c ON o.customer_id = c.customer_id;
```

### 範例

```sql
SELECT * FROM OrderSummary WHERE status = '已出貨';
```

### 結果範例

| order_id | order_date          | status | customer_name | customer_email       |
|----------|----------------------|--------|----------------|-----------------------|
| 1        | 2024-06-01 12:00:00 | 已出貨 | Alice Lin      | alice@example.com     |


---

## OrderItemDetails View

查詢每筆訂單中商品詳細資訊：

```sql
CREATE VIEW OrderItemDetails AS
SELECT 
    oi.order_id,
    oi.product_id,
    p.name AS product_name,
    oi.quantity,
    oi.price_at_purchase
FROM OrderItem oi
JOIN Product p ON oi.product_id = p.product_id;
```

### 範例

```sql
SELECT * FROM OrderItemDetails;
```

### 結果範例
| order\_id | product\_id | product\_name | quantity | price\_at\_purchase |
| --------- | ----------- | ------------- | -------- | ------------------- |
| 401       | 302         | iPhone 15 Pro | 1        | 45900               |
| 401       | 303         | AirPods Pro   | 2        | 7490                |



---

## ProductReviewStats View

呈現商品之平均評價與最近評價時間：

```sql
CREATE VIEW ProductReviewStats AS
SELECT 
    p.product_id,
    p.name AS product_name,
    ROUND(AVG(r.rating), 2) AS average_rating,
    MAX(r.review_date) AS latest_review_time
FROM Product p
JOIN Review r ON p.product_id = r.product_id
GROUP BY p.product_id, p.name;
```

### 範例

```sql
SELECT * FROM ProductReviewStats;
```

### 結果範例
| product\_id | product\_name | average\_rating | latest\_review\_time |
| ----------- | ------------- | --------------- | -------------------- |
| 302         | iPhone 15 Pro | 4.75            | 2024-05-30 14:25:00  |
| 303         | AirPods Pro   | 4.60            | 2024-05-29 18:00:00  |



---

## LatestProductReview View

查詢每個商品的最新一筆評價：

```sql
CREATE VIEW LatestProductReview AS
SELECT r.*
FROM Review r
JOIN (
    SELECT product_id, MAX(review_date) AS latest_date
    FROM Review
    GROUP BY product_id
) latest ON r.product_id = latest.product_id AND r.review_date = latest.latest_date;
```

### 範例

```sql
SELECT * FROM LatestProductReview;
```

### 結果範例
| review\_id | product\_id | rating | review\_text             | review\_date        |
| ---------- | ----------- | ------ | ------------------------ | ------------------- |
| 601        | 302         | 5      | Excellent sound quality! | 2024-05-30 14:25:00 |
| 602        | 303         | 4      | Great value for price.   | 2024-05-29 18:00:00 |


---

