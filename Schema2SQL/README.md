
## 1. Customer（買家）

| 欄位名稱       | 資料型別       | 說明           |
|----------------|----------------|----------------|
| customer_id    | INT (PK)       | 買家主鍵，自動編號 |
| name           | VARCHAR(100)   | 姓名           |
| email          | VARCHAR(255)   | 電子郵件，唯一值 |

### SQL
```sql
CREATE TABLE Customer (
    customer_id INT PRIMARY KEY AUTO_INCREMENT, 
    name VARCHAR(100) NOT NULL, 
    email VARCHAR(255) UNIQUE NOT NULL
);
```

### 範例
```sql
INSERT INTO Customer (name, email)
VALUES ('Alice Lin', 'alice@example.com');
```

---

## 2. Seller（賣家）

| 欄位名稱    | 資料型別       | 說明           |
|-------------|----------------|----------------|
| seller_id   | INT (PK)       | 賣家主鍵，自動編號 |
| name        | VARCHAR(100)   | 商店名稱        |
| email       | VARCHAR(255)   | 電子郵件，唯一值 |

### SQL
```sql
CREATE TABLE Seller (
    seller_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL
);
```

### 範例
```sql
INSERT INTO Seller (name, email)
VALUES ('Shop A', 'shopa@example.com');
```

---

## 3. Admin（管理員）

| 欄位名稱   | 資料型別       | 說明           |
|------------|----------------|----------------|
| admin_id   | INT (PK)       | 管理員主鍵，自動編號 |
| username   | VARCHAR(50)    | 使用者名稱，唯一 |

### SQL
```sql
CREATE TABLE Admin (
    admin_id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) NOT NULL UNIQUE
);
```

### 範例
```sql
INSERT INTO Admin (username)
VALUES ('admin1');
```

---

## 4. Product（商品）

| 欄位名稱      | 資料型別       | 說明           |
|---------------|----------------|----------------|
| product_id     | INT (PK)       | 商品主鍵，自動編號 |
| name           | VARCHAR(100)   | 商品名稱        |
| price          | DECIMAL(10,2)  | 商品價格        |
| stock          | INT            | 商品庫存數量      |
| category       | VARCHAR(100)   | 商品分類        |
| seller_id      | INT (FK)       | 賣家外鍵 → Seller.seller_id |

### SQL
```sql
CREATE TABLE Product (
    product_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    stock INT NOT NULL,
    category VARCHAR(100),
    seller_id INT NOT NULL,
    FOREIGN KEY (seller_id) REFERENCES Seller(seller_id)
);
```

### 範例
```sql
INSERT INTO Product (name, price, stock, category, seller_id)
VALUES ('Wireless Mouse', 599.00, 100, 'Electronics', 1);
```

---

## 5. Order（訂單）

| 欄位名稱       | 資料型別       | 說明           |
|----------------|----------------|----------------|
| order_id       | INT (PK)       | 訂單主鍵，自動編號 |
| order_date     | DATETIME       | 建立日期，預設為目前時間 |
| status         | ENUM           | 訂單狀態 |
| customer_id    | INT (FK)       | 買家外鍵 → Customer.customer_id |

### SQL
```sql
CREATE TABLE `Order` (
    order_id INT PRIMARY KEY AUTO_INCREMENT,
    order_date DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    status ENUM('未付款', '處理中', '已出貨', '已完成') NOT NULL,
    customer_id INT NOT NULL,
    FOREIGN KEY (customer_id) REFERENCES Customer(customer_id)
);
```

### 範例
```sql
INSERT INTO `Order` (status, customer_id)
VALUES ('處理中', 1);
```

---

## 6. OrderItem（訂單項目）

| 欄位名稱           | 資料型別       | 說明           |
|--------------------|----------------|----------------|
| order_id           | INT (PK, FK)   | 訂單編號，複合主鍵 |
| product_id         | INT (PK, FK)   | 商品編號，複合主鍵 |
| quantity           | INT            | 購買數量        |
| price_at_purchase  | DECIMAL(10,2)  | 購買時的單價     |

### SQL
```sql
CREATE TABLE OrderItem (
    order_id INT,
    product_id INT,
    quantity INT NOT NULL,
    price_at_purchase DECIMAL(10, 2) NOT NULL,
    PRIMARY KEY (order_id, product_id),
    FOREIGN KEY (order_id) REFERENCES `Order`(order_id),
    FOREIGN KEY (product_id) REFERENCES Product(product_id)
);
```

### 範例
```sql
INSERT INTO OrderItem (order_id, product_id, quantity, price_at_purchase)
VALUES (1, 1, 2, 599.00);
```

---

## 7. Review（商品評價）

| 欄位名稱       | 資料型別       | 說明           |
|----------------|----------------|----------------|
| review_id      | INT (PK)       | 評價主鍵，自動編號 |
| product_id     | INT (FK)       | 商品外鍵 → Product.product_id |
| customer_id    | INT (FK)       | 買家外鍵 → Customer.customer_id |
| rating         | INT            | 評分，限制 1～5 |
| comment        | TEXT           | 留言內容        |
| review_date    | DATETIME       | 評價時間，預設為目前時間 |

### SQL
```sql
CREATE TABLE Review (
    review_id INT PRIMARY KEY AUTO_INCREMENT,
    product_id INT NOT NULL,
    customer_id INT NOT NULL,
    rating INT CHECK (rating BETWEEN 1 AND 5),
    comment TEXT,
    review_date DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (product_id) REFERENCES Product(product_id),
    FOREIGN KEY (customer_id) REFERENCES Customer(customer_id)
);
```

### 範例
```sql
INSERT INTO Review (product_id, customer_id, rating, comment)
VALUES (1, 1, 5, '非常好用，品質也很棒');
```

<div align=center> <img src="C:\Users\user\Downloads\DB_SCHEMA.drawio.svg"/> </div>


---

