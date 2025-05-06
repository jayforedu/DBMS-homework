CREATE TABLE Customer (
    customer_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL
);

-- 範例
INSERT INTO Customer (name, email, password_hash)
VALUES ('Alice Lin', 'alice@example.com', 'hashed_password1');

CREATE TABLE Seller (
    seller_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL
);

-- 範例
INSERT INTO Seller (name, email, password_hash)
VALUES ('Shop A', 'shopa@example.com', 'hashed_password2');

