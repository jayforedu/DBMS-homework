
## 1. Customer（買家）

| 欄位名稱           | 資料型別         | 說明           |
| -------------- | ------------ | ------------ |
| customer\_id   | INT (PK)     | 買家主鍵，自動編號    |
| name           | VARCHAR(100) | 買家姓名         |
| email          | VARCHAR(255) | 電子郵件，唯一值     |
| phone          | VARCHAR(20)  | 電話號碼         |
| birthdate | DATETIME     | 買家生日，不可為空。格式須為`YYYY-MM-DD`，必須為有效歷史日期，不得晚於系統當前日期。     |


### SQL
```sql
CREATE TABLE Customer (
    customer_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(20) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    phone_number VARCHAR(10) NOT NULL,
    gender ENUM('男', '女', '其他') NOT NULL,
    birthdate DATE NOT NULL,
    CONSTRAINT chk_name_format CHECK (name REGEXP '^[A-Za-z0-9]{3,20}$'),
    CONSTRAINT chk_email_format CHECK (email REGEXP '^[\\w\\.-]+@[\\w\\.-]+\\.\\w{2,}$'),
    CONSTRAINT chk_phone_format CHECK (phone_number REGEXP '^09\\d{8}$')
);
```

### 範例
```sql
INSERT INTO Customer (name, email, phone_number, gender, birthdate) VALUES
('Alice123', 'alice123@example.com', '0912345678', '女', '1990-05-10'),
('BobChen01', 'bobchen01@example.com', '0987654321', '男', '1985-12-01'),
('CathyWang9', 'cathyw9@example.com', '0911122233', '其他', '1992-07-23'),
('DavidLiu88', 'davidliu88@example.com', '0922334455', '男', '1988-03-14'),
('EmmaWu77', 'emmawu77@example.com', '0933445566', '女', '1995-10-30'),
('FrankHuang', 'frankh@example.com', '0944556677', '男', '1979-08-22'),
('GraceTsai1', 'gracetsai1@example.com', '0955667788', '女', '1983-01-18'),
('HenryYeh22', 'henryyeh22@example.com', '0966778899', '男', '1991-06-05'),
('IreneKuo3', 'irenekuo3@example.com', '0977889900', '女', '1996-11-11'),
('JackyHsu44', 'jackyhsu44@example.com', '0988990011', '男', '1980-07-27');
```

---

## 2. Seller（賣家）

| 欄位名稱           | 資料型別         | 完整性限制說明                                                                                       |
| -------------- | ------------ | --------------------------------------------------------------------------------------------- |
| `seller_id`    | INT (PK)     | 主鍵，唯一且不可為空。系統自動生成或由系統驗證唯一性。                                                                   |
| `name`         | VARCHAR(100)    | 僅可包含英文大小寫與數字（A-Z, a-z, 0-9），不得有空格或特殊字元，長度 3–20 字元。正規表示式：`^[A-Za-z0-9]{3,20}$`。                |
| `email`        | VARCHAR(255)     | 唯一且不可為空，需符合有效 Email 格式，例如 `user@example.com`，不可含空格。可使用正規表示式 `^[\w\.-]+@[\w\.-]+\.\w{2,}$` 驗證。 |
| `phone_number` | VARCHAR(20)     | 不可為空。必須為 10 位數字且開頭為 `09`。正規表示式：`^09\d{8}$`。                                                   |
| `gender`       | ENUM('男', '女', '其他') / VARCHAR(10)     | 不可為空。僅允許「男」、「女」、「其他」三種值。建議使用 ENUM 或 CHECK 限制。                                                 |
| `birthdate`    | DATE     | 賣家生日， 不可為空。格式須為`YYYY-MM-DD`，必須為有效歷史日期，不得晚於系統當前日期。                                                  |


### SQL
```sql
CREATE TABLE Seller (
    seller_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(20) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    phone_number VARCHAR(10) NOT NULL,
    gender ENUM('男', '女', '其他') NOT NULL,
    birthdate DATE NOT NULL,
    CONSTRAINT chk_name_format_seller CHECK (name REGEXP '^[A-Za-z0-9]{3,20}$'),
    CONSTRAINT chk_email_format_seller CHECK (email REGEXP '^[\\w\\.-]+@[\\w\\.-]+\\.\\w{2,}$'),
    CONSTRAINT chk_phone_format_seller CHECK (phone_number REGEXP '^09\\d{8}$')
);

```

### 範例
```sql
INSERT INTO Seller (name, email, phone_number, gender, birthdate) VALUES
('ShopA123', 'shopa123@example.com', '0912345678', '男', '1980-05-10'),
('ShopB456', 'shopb456@example.com', '0922334455', '女', '1985-12-01'),
('ShopC789', 'shopc789@example.com', '0933445566', '其他', '1990-07-23'),
('GadgetWorld', 'gadgetworld@example.com', '0944556677', '男', '1975-03-14'),
('BookHaven1', 'bookhaven1@example.com', '0955667788', '女', '1988-10-30'),
('FashionZone', 'fashionzone@example.com', '0966778899', '男', '1995-08-22'),
('TechHub123', 'techhub123@example.com', '0977889900', '女', '1992-01-18'),
('KitchenPro', 'kitchenpro@example.com', '0988990011', '其他', '1983-06-05'),
('ToyPlanetX', 'toyplanetx@example.com', '0911222333', '男', '1991-11-11'),
('GreenGarden', 'greengarden@example.com', '0922333444', '女', '1979-07-27');


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
    username VARCHAR(20) NOT NULL UNIQUE,
    CONSTRAINT chk_username_format_admin CHECK (username REGEXP '^[A-Za-z0-9]{3,20}$')
);
```

### 範例
```sql
INSERT INTO Admin (username) VALUES
('admin1'),
('admin2'),
('juliaadmin'),
('superadmin'),
('systemmgr'),
('useradmin'),
('johndoe'),
('susanadmin'),
('admintest'),
('webmaster');

```

---

## 4. Product（商品）

| 欄位名稱        | 資料型別          | 說明                                   |
| ----------- | ------------- | ------------------------------------ |
| product\_id | INT (PK)      | 商品主鍵，自動編號，唯一且不可為空                    |
| name        | VARCHAR(50)   | 商品名稱，不可為空，長度 3-50，允許中英文、數字及空格，禁止特殊符號 |
| price       | DECIMAL(10,2) | 商品價格，不可為空，必須大於 0，最多兩位小數              |
| stock       | INT           | 商品庫存數量，不可為空，整數且不可為負                  |
| category    | VARCHAR(50)   | 商品分類，可空，長度不超過 50，允許中英文、數字及空格         |
| seller\_id  | INT (FK)      | 賣家外鍵，連結 Seller.seller\_id，不可為空       |


### SQL
```sql
CREATE TABLE Product (
    product_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL CHECK (name REGEXP '^[A-Za-z0-9 ]{3,50}$'),
    price DECIMAL(10, 2) NOT NULL CHECK (price > 0),
    stock INT NOT NULL CHECK (stock >= 0),
    category VARCHAR(50) CHECK (name REGEXP '^[A-Za-z0-9 ]{3,50}$'),
    seller_id INT NOT NULL,
    FOREIGN KEY (seller_id) REFERENCES Seller(seller_id)
);

```

### 範例
```sql
INSERT INTO Product (name, price, stock, category, seller_id) VALUES
('Wireless Mouse', 599.00, 100, 'Electronics', 1),
('Bluetooth Keyboard', 899.00, 80, 'Electronics', 1),
('USBC Charger', 299.00, 150, 'Accessories', 2),
('Laptop Stand', 799.00, 50, 'Office', 3),
('Notebook', 59.00, 200, 'Stationery', 5),
('TShirt', 399.00, 120, 'Clothing', 6),
('Coffee Maker', 1299.00, 30, 'Kitchen', 8),
('Action Figure', 499.00, 75, 'Toys', 9),
('Garden Shovel', 199.00, 60, 'Garden', 10),
('LED Desk Lamp', 699.00, 40, 'Lighting', 4);


```

---

## 5. Order（訂單）

| 欄位名稱          | 資料型別     | 說明                                            |
| ------------- | -------- | --------------------------------------------- |
| `order_id`    | INT      | 主鍵，自動編號，唯一且不可為空                               |
| `order_date`  | DATETIME | 建立日期，不可為空，格式為 `YYYY-MM-DD HH:mm:ss`，預設為系統當前時間 |
| `status`      | ENUM     | 訂單狀態，不可為空，限定值：`未付款`、`處理中`、`已出貨`、`已完成`         |
| `customer_id` | INT      | 買家外鍵，不可為空，參照 `Customer.customer_id`           |


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
INSERT INTO `Order` (status, customer_id) VALUES
('處理中', 1),
('已完成', 2),
('已出貨', 3),
('未付款', 4),
('處理中', 5),
('已完成', 6),
('已出貨', 7),
('處理中', 8),
('未付款', 9),
('處理中', 10);

```

---

## 6. OrderItem（訂單項目）

| 欄位名稱                | 資料型別          | 說明                                     |
| ------------------- | ------------- | -------------------------------------- |
| order\_id           | INT (PK, FK)  | 訂單編號，複合主鍵，不可為空，參照 `Order.order_id`     |
| product\_id         | INT (PK, FK)  | 商品編號，複合主鍵，不可為空，參照 `Product.product_id` |
| quantity            | INT           | 購買數量，不可為空，必須大於 0                       |
| price\_at\_purchase | DECIMAL(10,2) | 購買時的單價，不可為空，價格不得為負                     |


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
INSERT INTO OrderItem (order_id, product_id, quantity, price_at_purchase) VALUES
(1, 1, 2, 599.00),
(1, 2, 1, 899.00),
(2, 3, 3, 299.00),
(3, 4, 1, 799.00),
(4, 5, 5, 59.00),
(5, 6, 2, 399.00),
(6, 7, 1, 1299.00),
(7, 8, 4, 499.00),
(8, 9, 2, 199.00),
(9, 10, 1, 699.00);

```

---

## 7. Review（商品評價）

| 欄位名稱         | 資料型別     | 說明                                  |
| ------------ | -------- | ----------------------------------- |
| review\_id   | INT (PK) | 評價主鍵，自動編號，不可為空                      |
| product\_id  | INT (FK) | 商品外鍵，不可為空，參照 `Product.product_id`   |
| customer\_id | INT (FK) | 買家外鍵，不可為空，參照 `Customer.customer_id` |
| rating       | INT      | 評分，不可為空，限制為 1 至 5 之間的整數             |
| comment      | TEXT     | 留言內容，可為空                            |
| review\_date | DATETIME | 評價時間，預設為目前時間，不可為空                   |


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
INSERT INTO Review (product_id, customer_id, rating, comment) VALUES
(1, 1, 5, '非常好用，品質也很棒'),
(2, 2, 4, '打字手感不錯，就是有點重'),
(3, 3, 3, '充電頭普通，不過夠用'),
(4, 4, 5, '很穩定的支架，推薦'),
(5, 5, 4, '紙質不錯，適合書寫'),
(6, 6, 2, '材質一般，容易皺'),
(7, 7, 5, '煮咖啡很方便，滿意'),
(8, 8, 4, '小朋友很喜歡'),
(9, 9, 3, '有點小貴，但好用'),
(10, 10, 5, '燈光柔和，很適合夜讀');
```

<div align=center> <img src="DB_SCHEMA.drawio.svg" alt="資料庫綱要圖" width="800"/> </div>


---

