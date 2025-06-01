# 1. Customer（買家）

|  欄位名稱   | 資料型別  | 說明 |
|  ----  | ----  | ----  |
| customer_id  | INT (PK) | 買家主鍵，自動編號 |
| name  | VARCHAR(100) | 姓名 |
| email  | VARCHAR(255) | 電子郵件，唯一值 |
| password_hash  | VARCHAR(255) | 密碼加密值 |

## SQL<br>
```
CREATE TABLE Customer (
    customer_id INT PRIMARY KEY AUTO_INCREMENT, 
    name VARCHAR(100) NOT NULL, 
    email VARCHAR(255) UNIQUE NOT NULL, 
    password_hash VARCHAR(255) NOT NULL
);
```

## 範例<br>
```
INSERT INTO Customer (name, email, password_hash)<br>
VALUES ('Alice Lin', 'alice@example.com', 'hashed_password1');<br>
```
<br>

# 2. Seller（賣家）

|  欄位名稱   | 資料型別  | 說明 |
|  ----  | ----  | ----  |
| seller_id  | INT (PK) | 賣家主鍵，自動編號 |
| name  | VARCHAR(100) | 商店名稱 |
| email  | VARCHAR(255) | 電子郵件，唯一值 |
| password_hash  | VARCHAR(255) | 密碼加密值 |

## SQL<br>
```
CREATE TABLE Seller (
    seller_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL
);
```

## 範例
```
INSERT INTO Seller (name, email, password_hash)
VALUES ('Shop A', 'shopa@example.com', 'hashed_password2');
```
<br>

# 3. Admin（管理員）

|  欄位名稱   | 資料型別  | 說明 |
|  ----  | ----  | ----  |
| admin_id  | INT (PK) | 管理員主鍵，自動編號 |
| username  | 	VARCHAR(50) | 使用者名稱，唯一 |
| password_hash  | VARCHAR(255) | 密碼加密值 |

## SQL<br>
```
CREATE TABLE Admin (
    admin_id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) NOT NULL UNIQUE,
    password_hash VARCHAR(255) NOT NULL
);
```

## 範例
```
INSERT INTO Admin (username, password_hash)
VALUES ('admin1', 'hashed_admin_password');
```
<br>

# 4. Product（商品）

|  欄位名稱   | 資料型別  | 說明 |
|  ----  | ----  | ----  |
| product_id  | INT (PK) | 商品主鍵，自動編號 |
| name  | 	VARCHAR(100) | 商品名稱 |
| price  | DECIMAL(10,2) | 商品價格 |
| stock  | INT | 商品庫存數量 |
| category  | VARCHAR(100) | 商品分類 |
| passwseller_idord_hash  | NT (FK) | 賣家外鍵 → Seller.seller_id |

## SQL<br>
```
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

## 範例
```
INSERT INTO Product (name, price, stock, category, seller_id)
VALUES ('Wireless Mouse', 599.00, 100, 'Electronics', 1);
```
<br>

# 5. Order（訂單）

|  欄位名稱   | 資料型別  | 說明 |
|  ----  | ----  | ----  |
| order_id  | INT (PK) | 訂單主鍵，自動編號 |
| order_date  | DATETIME | 建立日期，預設為目前時間 |
| status  | ENUM('未付款', '處理中', '已出貨', '已完成') | 訂單狀態 |
| customer_id  | INT (FK) | 買家外鍵 → Customer.customer_id |

## SQL<br>
```
CREATE TABLE `Order` (
    order_id INT PRIMARY KEY AUTO_INCREMENT,
    order_date DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    status ENUM('未付款', '處理中', '已出貨', '已完成') NOT NULL,
    customer_id INT NOT NULL,
    FOREIGN KEY (customer_id) REFERENCES Customer(customer_id)
);
```

## 範例
```
INSERT INTO `Order` (status, customer_id)
VALUES ('處理中', 1);
```
<br>

# 6. OrderItem（訂單項目）

|  欄位名稱   | 資料型別  | 說明 |
|  ----  | ----  | ----  |
| order_id  | INT (PK, FK) | 訂單編號，複合主鍵 |
| product_id  | INT (PK, FK) | 商品編號，複合主鍵 |
| quantity  | INT | 購買數量 |
| price_at_purchase  | DECIMAL(10,2) | 當時購買單價（保留歷史價格） |

## SQL<br>
```
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

## 範例
```
INSERT INTO OrderItem (order_id, product_id, quantity, price_at_purchase)
VALUES (1, 1, 2, 599.00);
```
<br>

# 7. Review（商品評價）

|  欄位名稱   | 資料型別  | 說明 |
|  ----  | ----  | ----  |
| review_id  | 	INT (PK) | 評價主鍵，自動編號 |
| product_id  | INT (FK) | 商品外鍵 → Product.product_id |
| customer_id  | INT (FK) | 買家外鍵 → Customer.customer_id |
| rating  | INT | 評分，限制 1～5 |
| comment  | TEXT | 留言內容 |
| review_date  | DATETIME | 評價時間，預設為目前時間 |

## SQL<br>
```
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

## 範例
```
INSERT INTO Review (product_id, customer_id, rating, comment)
VALUES (1, 1, 5, '非常好用，品質也很棒');
```
