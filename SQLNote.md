# SQL 超濃縮語法筆記

`資料庫管理系統(DBMS) = Database Management System`

* **SQL**：MYSQL, MariaDB (MYSQL 分支), Oracle, MSSQL

* **NoSQL**：MongoDB (Document)

## **DataBase基本動作**

```sql
--顯示系統內全部資料夾
SELECT name,database_id,create_date
FROM sys.databases;

--創建資料庫
CREATE DATABASE database_name;

--移除資料庫
DROP DATABASE database_name;

-- 注意事項:
-- MSSQL不分大小寫
-- 名稱盡量不要有空格，請用駝峰or底線
```

## **Table基本動作**

`欄位(Columns or Headers)，列、第n筆資料(Row)`

```sql
--創建資料表
CREATE TABLE table_name(
    column_name1 data_type NOT NULL primary key identity(1,1),
    --or設定UUID
    column_name2 data_type NOT NULL primary key DEFAULT newid(),
    column_name3 data_type NOT NULL DEFAULT 'unnamed',
    --資料不可重複
    column_name4 data_type NOT NULL unique,
    );

--查詢database中所有Table
SELECT * FROM INFORMATION_SCHEMA.TABLES 
WHERE TABLE_TYPE='BASETABLE'

--查詢表格設計
EXEC sp_columns table_name

--刪除表格
DROP TABLE table_name

--注意事項
--SSMS預設鎖住修改欄位的功能，通常會重建整個Table
--設計欄位時DEFAULT通常會設計成NOT NULL
--主鍵必為NOT NULL系統會自動補上
```

### 新增資料(INSERT)

```sql
--新增資料
INSERT INTO table_name(column_name1,column_name2,...)
VALUES(value1,value2,...),
(value3,value4,...),
...;

-- 注意事項:
--也可以不寫欄位但值必須按照順序和所有欄位
```
