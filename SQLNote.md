# MSSSQL 超濃縮語法筆記

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
    --建立外來鍵(FK)
    FOREIGN KEY (column_name3) 
    REFERENCES table_name2(column_name1) ON DELETE SET NULL
    );

--查詢database中所有Table
SELECT * FROM INFORMATION_SCHEMA.TABLES 
WHERE TABLE_TYPE='BASETABLE'

--查詢表格設計
EXEC sp_columns table_name

--刪除表格
DROP TABLE table_name

--FOREIGN KEY刪除、更新的約束模式:
--  NO ACTION00(預設)、CASCADE模式(級聯模式)、SET NULL模式

--注意事項
--SSMS預設鎖住修改欄位的功能，通常會重建整個Table
--設計欄位時DEFAULT通常會設計成NOT NULL
--主鍵必為NOT NULL系統會自動補上
--FK可為NULL
```

## 資料操作CRUD

* **CREATE**: 新增資料
* **READ**: 查詢資料
* **UPDATE**: 更新/修改資料
* **DELETE**: 刪除資料

### CREATE新增資料(INSERT)

```sql
--新增資料
INSERT INTO table_name(column_name1,column_name2,...)
VALUES(value1,value2,...),
(value3,value4,...),
...;

-- 注意事項:
--也可以不寫欄位但值必須按照順序和所有欄位
```

### READ資料查詢(SELECT)

```sql
--查詢欄位資料(不分大小寫)
SELECT column_name1 AS 'name1',column_name2 AS'name2'  
FROM table_name 
WHERE condition

--分大小寫查詢
SELECT column_name1,column_name2 
FROM table_name 
WHERE condition 
COLLATE SQL_Latin1_General_CP1_CS_AS

--COLLATE:用於指定排序規則，影響字串的比較和排序方式
--AS:別名
```

#### SUBQUERY子查詢

`查詢語法SELECT中再放入一個查詢`

```SQL
SELECT column_name1 
FROM table_name 
WHERE column_name2=
(
    SELECT column_name2 
    FROM table_name
    WHERE condition
)
```

#### LIKE模糊搜尋

`字串搜尋的方式之一`

```SQL
WHERE column_name1 LIKE '%string%'

--%為其他內容，如要指定字數請用_
```

### UPDATE更新資料(UPDATE,SET)

```SQL
--替換欄位名稱
UPDATE table_name 
SET column_name1 = value1
WHERE column_name1 = value2

--將資料的某欄位做更改
UPDATE table_name 
SET column_name1 = value1
WHERE column_name2 = value2

--注意事項:
--更新是無法做復原的，建議先找到資料後再做更新
```

### DELETE刪除資料(DELETE)

```SQL
--刪除資料
DELETE 
FROM table_name
WHERE condition 

--注意事項:
--刪除是無法做復原的，建議先找到資料後再做刪除
```

## JOIN連結

`從多個關聯資料表中取得合併資料(需要在同一個SQL)`

```sql
--合併檢視
SELECT NEWcolumn_name1,NEWcolumn_name2
FROM table_name1
LEFT JOIN table_name2
ON table_name1.column_name=table_name2.column_name2
JOIN table_name3
ON table_name1.column_name2=table_name3.column_name2

--常用的四種JOIN:LINNER JOIN,LEFT JOIN,RIGHT JOIN,FULL JOIN 

--注意事項；
--只是把資料選出來後合併檢視，並不會改變原始資料
--FROM和JOIN順序不同結果會不同
```

## AGGREGATION聚合、GROUP分組、HAVING過濾、DISTINCT去重複

* **AGGREGATION**:從表格中取得資料並算出代表值
* **GROUP BY**:依照規定分為一組
* **HAVING**:過濾分組後的資料
* **DISTINCT**:去除重複的資料
  
```SQL
--將資料分組
SELECT DISTINCT column_name1,COUNT(column_name2)
FROM table_name1
GROUP BY column_name1
HAVING condition

--DISTINCT為兩個時會過濾兩欄都一樣的資料

--注意事項:
--NULL不會計算在內，如藥包括在內請用COUNT(*)
--HAVING通常與AGGREGATION一起使用
--當DISTINCT複數欄位時無法搭配AGGREGATION
```

## SORTING資料排序

* **ORDER BY**:強制資料做排序
* **OFFSET**:跳過前n筆資料
* **FETCH NEXT**:再選擇多少筆

```SQL
--資料排序
SELECT * 
FROM table_name1
ORDER BY ASC column_name1
OFFSET n ROWS
FETCH NEXT ROWS m ONLY

--ORDER BY排序:ASC:小到大 DESC:大到小

--注意事項:FETCH NEXT一定要經過ORDER BY排序
```
