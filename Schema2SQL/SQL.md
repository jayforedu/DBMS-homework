# 1. Customer（買家）

|  欄位名稱   | 資料型別  | 說明 |
|  ----  | ----  | ----  |
| customer_id  | INT (PK) | 買家主鍵，自動編號 |
| name  | VARCHAR(100) | 姓名 |
| email  | VARCHAR(255) | 電子郵件，唯一值 |
| password_hash  | VARCHAR(255) | 密碼加密值 |

# SQL<br>
```
CREATE TABLE Customer (<br>
    customer_id INT PRIMARY KEY AUTO_INCREMENT,<br>
    name VARCHAR(100) NOT NULL,<br>
    email VARCHAR(255) UNIQUE NOT NULL,<br>
    password_hash VARCHAR(255) NOT NULL<br>
);<br>
```

# 範例<br>
INSERT INTO Customer (name, email, password_hash)<br>
VALUES ('Alice Lin', 'alice@example.com', 'hashed_password1');<br>
