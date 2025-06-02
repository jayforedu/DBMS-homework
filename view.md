

## View：OrderSummary（訂單摘要檢視表）

| 欄位名稱         | 資料型別 | 說明                     |
|------------------|----------|--------------------------|
| order_id         | INT      | 訂單編號（來源：Order）   |
| order_date       | DATETIME | 訂單建立時間             |
| status           | ENUM     | 訂單狀態                 |
| customer_name    | VARCHAR  | 買家名稱（來源：Customer）|
| customer_email   | VARCHAR  | 買家電子郵件（來源：Customer）|

### SQL
```sql
CREATE VIEW OrderSummary AS
SELECT 
    o.order_id,
    o.order_date,
    o.status,
    c.name AS customer_name,
    c.email AS customer_email
FROM `Order` o
JOIN Customer c ON o.customer_id = c.customer_id;
```

### 查詢範例
```sql
SELECT * FROM OrderSummary WHERE status = '已出貨';
```
### 查詢結果範例
| order_id | order_date          | status   | customer_name | customer_email       |
|----------|---------------------|----------|----------------|----------------------|
| 1        | 2024-06-01 12:00:00 | 已出貨   | Alice Lin      | alice@example.com    |
| 2        | 2024-06-02 09:30:00 | 處理中   | Bob Lee        | bob@example.com      |

---

## View：CustomerOrderHistory（買家訂單紀錄）

| 欄位名稱         | 資料型別 | 說明                       |
|------------------|----------|----------------------------|
| customer_id      | INT      | 買家編號（來源：Customer）   |
| order_id         | INT      | 訂單編號（來源：Order）     |
| order_date       | DATETIME | 訂單日期                   |
| status           | ENUM     | 訂單狀態                   |

### SQL
```sql
CREATE VIEW CustomerOrderHistory AS
SELECT 
    c.customer_id,
    o.order_id,
    o.order_date,
    o.status
FROM Customer c
JOIN `Order` o ON c.customer_id = o.customer_id;
```

### 查詢範例
```sql
SELECT * FROM CustomerOrderHistory WHERE customer_id = 1;
```
### 查詢結果範例
| customer_id | order_id | order_date          | status   |
|-------------|----------|---------------------|----------|
| 1           | 1        | 2024-06-01 12:00:00 | 已出貨   |
| 1           | 3        | 2024-06-05 15:45:00 | 未付款   |
---

## View：SellerProductList（賣家商品清單）

| 欄位名稱       | 資料型別       | 說明                         |
|----------------|----------------|------------------------------|
| seller_id      | INT            | 賣家編號（來源：Seller）       |
| seller_name    | VARCHAR(100)   | 商店名稱                     |
| product_id     | INT            | 商品編號（來源：Product）      |
| product_name   | VARCHAR(100)   | 商品名稱                     |
| price          | DECIMAL(10,2)  | 商品價格                     |
| stock          | INT            | 商品庫存                     |

### SQL
```sql
CREATE VIEW SellerProductList AS
SELECT 
    s.seller_id,
    s.name AS seller_name,
    p.product_id,
    p.name AS product_name,
    p.price,
    p.stock
FROM Seller s
JOIN Product p ON s.seller_id = p.seller_id;
```

### 查詢範例
```sql
SELECT * FROM SellerProductList WHERE seller_id = 1;
```
### 查詢結果範例
| seller_id | seller_name | product_id | product_name    | price   | stock |
|-----------|-------------|------------|------------------|---------|--------|
| 1         | Shop A      | 101        | Wireless Mouse   | 599.00  | 100    |
| 1         | Shop A      | 102        | USB Keyboard     | 299.00  | 50     |
