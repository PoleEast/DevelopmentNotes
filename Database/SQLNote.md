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

## 進階SQL語法

### *以下許多功能會放在SEVER做了，使用上請注意*

## VIEW檢視表

`虛擬的將實體資料表結構隱藏起來`

```sql
CREATE VIEW schema_name.view_name[(column_name...)]
WITH view_attribute
AS
SELECT column_name1
WHERE condition
WITH CHECK OPTION

--ALTER VIEW:修改VIEW的屬性OR 先DROP後再新增

--注意事項:
-- 加密後的VIEW無法檢視其設計只能刪除OR整個修改，所以務必寫好文件
-- 使用WITH CHECK OPTION後新增或修改不符條件將無法執行
```

## VARIABLE變數

```SQL
DECLARE @variable_name data_type
SET @variable_name = value

--@為區域變數
--@@為痊癒變數
```

## 流程控制

```SQL
WHILE boolean_expression
    IF
        BEGIN
            statement1
            BREAK
            statement2
        END
    ELSE
        BEGIN
            statement3
            CONTINUE
            statement4
        END
END

--有多句陳述式藥用BEGIN...END包覆
```

## TRANSACTION交易

`又稱事務，確保資料的一致性與完整性`

### ACID

* Atomicity(原子性)
* Consistency(一致性)
* Isolation(隔離性)
* Durability(永久性)

```sql
--明顯交易模式
EXPLICIT TRANSACTION
BEGIN
    statement1
    COMMIT
END
--隱含交易模式
SET IMPILICIT_TRANSACTIONS ON
    statement1
    COMMIT
SET IMPILICIT_TRANSACTIONS OFF

--注意事項:隱含交易模式如果沒關閉則永為隱含交易模式
```

## PROCEDURE預存程序

`SQL版函式`

```SQL
CREATE PROCEDURE procedure_name
@variable_name1 data_type, @variable_name2 data_type OUT 
WITH procedure_option
AS
BEGIN
    statement
END

DECLARE @variable_name3 data_type
exec type_value, @variable_name3 out;

--注意事項:
--別使用sp開頭作為名稱，那是系統預設的預存程序
```

## TRIGGERS觸發

`由系統自動執行沒有參數也沒有回傳值，此為過氣作法，現在主流是將其寫在SEVER中`

* **DML TRIGGERS**:最常使用，資料表操作指令時觸發
* **DDL TRIGGERS**:稽核或管理資料庫作業
* **LOGON TRIGGERS**:登入或工作階段
  
```SQL
CREATE TRIGGER trigger_name
ON table_name
WITH dml_trigger_option
NOT FOR REPLICATION
{FOR|AFTER|INSTEAD OF} INSERT, UPDATE, DELETE 
AS
BEGIN
    statement
END

--刪除trigger
DROP TRIGGER + trigger_name

--注意事項:
--再說一次此為過氣作法
--使用太多邏輯複雜管理不易
--務必寫清楚技術文件
```

## IN運算子

`搭配WHERE可以用資料表來做比較`

```SQL
SELECT *
FROM table_name
WHERE column_name NOT IN condition
```

## CASE關鍵字

`邏輯判斷用`

```SQL
CASE 
    WHEN condition1 THEN statement1
    WHEN condition2 THEN statement2
ELSE
    statement3
END
```

## 集合運算

`使用方式上與JOIN差不多，只是她是直的JOIN是橫的`

* UNION:有重複只會顯示一筆，要全印請用UNION
* INRERSECT:只印相同的資料
* EXCEPT:自己剪掉對象

## SEQUENCE順序

`跨資料表運作，可以共用這個變數`

```SQL
CREATE SEQUENCE sequence_name
AS data_type
START WITH value
INCREMENT BY value
MINVALUE value
MAXVALUE value

--重設SEQUENCE值
ALTER SEQUENCE sequence_name RESTART WITH value

--注意事項:
-- 部餐與交易
```

## INDEX索引

* 索引可以減少搜尋的時間，類似於二元搜尋樹
* 叢集索引(Clustered Indexes):設定Primary key時會自動建立，主要用來定位資料的位置
* 非叢集索引(NonClustered Indexes):定位叢集索引位置用，只用Unique條件約束欄位
