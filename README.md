# SQL基礎教學
---
這裡會以MySQL語法為主，並且在MySQL Workbench上實作
>- [MySQL online](https://paiza.io/en/languages/mysql)
>- [MySQL online2](https://www.sql-practice.com/)
>- [KDnuggets Top Blog Winner
7 Best Platforms to Practice SQL](https://www.kdnuggets.com/2023/01/7-best-platforms-practice-sql.html)
>- [MySQL安裝教學](https://clay-atlas.com/blog/2019/11/16/mysql-mysqlworkbench-tutorial-download-install-steps/)
>- [參考資料1](https://jerrynest.io/mysql-tutorial/)
>- [參考教學](https://youtu.be/gvRXjsrpCHw?si=4JKqeI4Ya_EnMy39&t=1)
---

## 一、Database Management System(DBMS)

透過加工處理的資料集合體就稱為資料庫(Database)
管理資料庫的電腦資訊系統就稱為資料庫管理系統(Database Management System)
![](https://hackmd.io/_uploads/SJOabsIzp.png)
當使用者想要存取資料庫內容時，必須先通過帳戶驗證
![](https://hackmd.io/_uploads/HJ4-dqIMT.png)

### 為什麼需要DBMS，純文字或試算表的不足之處
- 較難讓多人共用相同的資料
- 無法擴展到處理大量的資料
- 想要自動讀寫資料需要另外撰寫程式
- 電腦發生狀況可能失去資料

DBMS具有解決上述問題的功能，可以同時讓多人安全且方便的取用資料
### DBMS類型
- 階層式資料庫(Hierachical Database)
- <font color=red>關聯式資料庫(Relational Database)</font>
- 物件導向式資料庫(Object Oriented Database)
- XML資料庫(XML Database)
- 鍵值式資料儲存(Key-Value Store)

---
## 二、SQL敘述與分類
```
SQL敘述做最後要加上(;)
關鍵字偏好使用全大寫(但大小寫不影響)
```
按照對於RDBMS所下達的指令，SQL敘述可分為三大類
### DDL(Data Definition Language)
能建立資料庫和資料表等用來儲存資料的物件
- CREATE:建立資料庫或資料表
- DROP:刪除資料庫或資料表
- ALTER:修改資料庫或資料表
### DML(Data manipulation Language)
能查詢或修改資料表內的紀錄
- SELECT:從資料表查詢紀錄
- INSERT:將新紀錄儲存至資料表中
- UPDATE:修改資料表的紀錄
- DELETE:刪除資料表的紀錄
### DCL(Data Control Language)
用來認可或取消對資料庫執行的變更
- COMMIT:認可對資料庫執行的變更動作
- ROLLBACK:取消對資料庫執行的變更動作
- GRANT:賦予使用者操作的權限
- REVOKE:撤銷使用者操作的權限
---
## Chap1
### 建立資料庫和資料表
>- [MySQL資料型別](https://www.mysqltutorial.org/mysql-data-types.aspx/)
```sql
-- 建立資料庫
CREATE DATABASE `database_name`; 
-- 建立資料表
CREATE TABLE `table_name`;       
(
  column_name1 data_type 條件約束,
  column_name2 data_type 條件約束,
  column_name3 data_type 條件約束,
  ···
  column_nameN data_type 條件約束);


-- data_type
INT          -- 整數
DECIMAL(m,n) -- 有小數點的整數
VARCHAR(n)   -- 字串
BLOB         -- (Binary Large Object) 圖片 影片 檔案...
DATE         -- 'YYYY-MM-DD'
TIMESTAMP    -- 'YYYY-MM-DD HH:MM:SS'紀錄時間


-- 查看
SHOW DATABASES;            -- 查看目前有哪些databases
DESCRIBE `table_name`;     -- 查看table
SELECT *FROM `table_name`; -- 查看table內項目

```
#### 範例
```SQL=
-- 建立shop資料庫
CREATE DATABASE `shop`;
-- 進入shop這個資料庫
USE `shop`;
-- 建立Shohin資料表
CREATE TABLE `Shohin`
(shohin_id       CHAR(4)      NOT NULL,
 shohin_name     VARCHAR(100) NOT NULL,
 shohin_catalg   VARCHAR(32)  NOT NULL,
 sell_price      INTEGER,
 buying_price    INTEGER,
 reg_date        DATE,
 PRIMARY KEY(shohin_id)); -- 使id具有唯一性
 


SHOW DATABASES;        -- 查看database
DESCRIBE `Shohin`;     -- 查看table
SELECT *FROM `Shohin`; -- 查看table內項目

```
### 刪除與修改資料表
- DROP資料表(庫)後便無法在取得其中儲存的資料，執行前請三思
- 資料庫中已建立的資料表，有可能在使用一段時間後才發現欄位不足，此時就可以使用ALTER TABLE敘述
- 如果只是想要清空資料表，可以使用DELETE
```sql
-- 刪除資料庫
DROP DATABASE `database_name`;
-- 刪除資料表
DROP TABLE `table_name`;
-- 新增資料表欄位
ALTER TABLE `table_name` ADD COLUMN column_name data_type;
-- 刪除資料表欄位
ALTER TABLE `table_name` DROP COLUMN column_name;
-- 新增資料到資料表中
INSERT INTO `table_name` VALUE(value1,value2,...,valueN); --注意資料必須按順序填入
```
#### 範例
```sql
-- 刪除shop資料庫
DROP DATABASE `shop`; 
-- 刪除資料表
DROP TABLE `table_name`;     
-- 新增資料表欄位
ALTER TABLE `Shohin` ADD COLUMN shohin_info VARCHAR(100);
-- 刪除資料表欄位
ALTER TABLE `Shohin` DROP COLUMN shohin_info;


 -- 在Shohin資料表中新增資料
START TRANSACTION;
INSERT INTO Shohin VALUES ('0001', 'T侐', '衣物', 1000, 500, '2009-09-20');
INSERT INTO Shohin VALUES ('0002', '打孔機', '辦公用品', 500, 320, '2009-09-11');
INSERT INTO Shohin VALUES ('0003', '襯衫', '衣物', 4000, 2800, NULL);
INSERT INTO Shohin VALUES ('0004', '菜刀', '廚房用品', 3000, 2800, '2009-09-20');
INSERT INTO Shohin VALUES ('0005', '壓力鍋', '廚房用品', 6800, 5000, '2009-01-15');
INSERT INTO Shohin VALUES ('0006', '叉子', '廚房用品', 500, NULL, '2009-09-20');
INSERT INTO Shohin VALUES ('0007', '刨絲器', '廚房用品', 880, 790, '2008-04-28');
INSERT INTO Shohin VALUES ('0008', '鋼珠筆', '辦公用品', 100, NULL, '2009-11-11');
COMMIT;
```

### SELECT用法
- 從資料表取出資料時需要用到SELECT敘述，利用SELECT搜尋和取得資料的動作也被稱為「查詢」。
```sql
-- 輸出特定欄位資料
SELECT column_name1,column_name3,...
    FROM `table_name`;
-- 輸出所有欄位資料
SELECT *
    FROM `table_name`;
-- 省略結果中重複的紀錄
SELECT DISTINCT column_name1
    FROM `table_name`;
```
#### 範例
```sql=
-- 輸出Shohin中的id,name,buying price
SELECT shohin_id,shohin_name,buying_price
    FROM `Shohin`;
-- 輸出所有欄位資料
SELECT *
    FROM `Shohin`;
-- 省略shohin_catalog中重複的紀錄
SELECT DISTINCT shohin_catalg
    FROM `Shohin`;
```

### WHERE用法
- SELECT可以搭配WHERE來指定條件，篩選出想要的紀錄，例如「某個欄位的數值大於此數值」或「某欄位資料等於此字串」。
- 注意子句有撰寫順序，例如WHERE子句要在FROM子句的後方。
```sql
-- SELECT敘述加上WHERE子句
SELECT column_name,...
    FROM table_name
    WHERE condition1;
```
#### 範例
```sql=
-- 選擇欄位='衣物'的紀錄
SELECT shohin_name, shohin_catalg
	FROM Shohin
    WHERE shohin_catalg='衣物';
-- 可以不輸出作為搜尋條件的欄位
SELECT shohin_name
	FROM Shohin
    WHERE shohin_catalg='衣物';
```
---
## Chap2
### 算數運算子/比較運算子
| 算術運算子 | 符號 |
| ----- | ---- |
| 加    | +    |
| 減    | -    |
| 乘    | *    |
| 除    | /    |


| 比較運算子 | 符號 |
| -------- | ---- | 
| 和~等於   | =    |
| 和~不等於  | <>   |
| 大於等於~  | >=   |
| 大於~     | >    |
| 小於等於~  | <=   |
| 小於~     | <    |


#### 範例
```sql=
-- 在SQL敘述中寫入計算式
SELECT shohin_name,sell_price,sell_price*2 AS "sell_price_X2"
    FROM Shohin;
-- 篩選sell_price為500的值
SELECT shohin_name, shohin_catalg
    FROM Shohin
    WHERE sell_price=500;
-- 篩選sell_price不為500的值
SELECT shohin_name, shohin_catalg
    FROM Shohin
    WHERE sell_price<>500;
-- 篩選登錄日期早於2009-09-27的紀錄
SELECT shohin_name, shohin_catalg,reg_date
    FROM Shohin
    WHERE reg_date<'2009-09-27';
```

#### 對字串使用不等號
```sql=
-- 建立字串資料表
CREATE TABLE Chars
(chr CHAR(3) NOT NULL,
 PRIMARY KEY (chr));
 
START TRANSACTION;
INSERT INTO Chars VALUES('1');
INSERT INTO Chars VALUES('2');
INSERT INTO Chars VALUES('3');
INSERT INTO Chars VALUES('10');
INSERT INTO Chars VALUES('11');
INSERT INTO Chars VALUES('222');
COMMIT;

-- 篩選出大於'2'
SELECT chr
    FROM Chars
    WHERE chr > '2';
```

<details>
    <summary>輸出結果如何，為什麼?</summary>
因為字串型別會按照「字典排序」
</details>

### 邏輯運算子
#### 範例
```sql=
-- NOT運算
SELECT shohin_name,shohin_catalg,sell_price
    FROM Shohin
    WHERE NOT sell_price >=1000;
-- AND運算
-- 分類為廚房用品'且'單價高於3000元的商品
SELECT shohin_name,shohin_catalg,sell_price
    FROM Shohin
    WHERE shohin_catalg='廚房用品'
    AND sell_price >=3000;
-- OR運算
-- 分類為廚房用品'或'單價高於3000元的商品
SELECT shohin_name,shohin_catalg,sell_price
    FROM Shohin
    WHERE shohin_catalg='廚房用品'
    OR sell_price >=3000;
```
---
## Chap3
### 彙總函數
- 彙總函數會排除NULL資料，除了COUNT(*)。
- MAX/MIN適用於大部分的資料型態，而SUM/AVG僅適用數值型態。
- 常用的五個彙總函數：COUNT、SUM、AVG、MAX、MIN。
#### 範例
```sql=
-- 計算所有紀錄筆數
SELECT COUNT(*)
    FROM Shohin;
-- 計算buying_price除了NULL之外的筆數
SELECT COUNT(buying_price)
    FROM Shohin;
-- SUM用法
SELECT SUM(sell_price),SUM(buying_price)
    FROM Shohin;
-- AVG用法
SELECT AVG(sell_price)
    FROM Shohin;
-- MAX,MIN用法
SELECT MAX(reg_date),MIN(reg_date)
    FROM Shohin;
    
```

### GROUP BY用法
- 在前面的彙總函數，都是以整個資料表為對象，這裡我們先將資料區分為幾個群組，例如「按照商品分類」或「按照登錄日期」來統計資料。
- 子句的撰寫順序：SELECT -> FROM -> WHERE -> GROUP BY。
```sql
-- GROUP BY
SELECT column1,column2,...
    FROM table_name
    GROUP BY column1,column2,...;
-- WHERE+GROUP BY
SELECT column1,column2,..
    FROM table_name
    WHERE condition
    GROUP BY column1,column2,...;
```
#### 範例
```sql=
-- 按照商品分類計算紀錄筆數
SELECT shohin_catalg,COUNT(*)
    FROM Shohin
    GROUP BY shohin_catalg;
-- WHERE+GROUP BY
SELECT buying_price,COUNT(*)
    FROM Shohin
    WHERE shohin_catalg='衣物'
    GROUP BY buying_price;
```
### HAVING 用法
- 使用COUNT等函數彙總資料表時，如果想要篩選結果指定條件，<font color=red>不能使用WHERE子句，必須改用HAVING子句</font>。
- WHERE用來「對記錄指定條件」，而HAVING用來「對群組指定條件」。
- 子句的撰寫順序：SELECT -> FROM -> WHERE -> GROUP BY -> HAVING。
![](https://hackmd.io/_uploads/ryGRE9wM6.png =50%x)

```sql
SELECT column1,column2,...
    FROM table_name
    GROUP BY column1,column2,...
    HAVING cond_for_group;
```

#### 範例
```sql=
-- 錯誤示範
SELECT shohin_catalg,COUNT(*)
    FROM Shohin
    WHERE COUNT(*)=2
    GROUP BY shohin_catalg;
-- 找出含兩筆資料的群組
SELECT shohin_catalg,COUNT(*)
    FROM Shohin
    GROUP BY shohin_catalg
    HAVING COUNT(*)=2;
```

### ORDER BY用法
- 想要排列查詢結果時，使用ORDER BY子句。
- 子句的撰寫順序：SELECT -> FROM -> WHERE -> GROUP BY -> HAVING -> ORDER BY。
- 因為NULL無法排序，所以會統一被放在最後或最前。
```sql
SELECT col1, col2,...
    FROM table_name
    ORDER BY 排序基準1,排序基準2,...;
```

#### 範例
```sql=
-- 按照售價低到高
SELECT shohin_id,shohin_name,sell_price, buying_price
    FROM Shohin
    ORDER BY sell_price;
-- 高到低
SELECT shohin_id,shohin_name,sell_price, buying_price
    FROM Shohin
    ORDER BY sell_price DESC;
-- 多個排序鍵
SELECT shohin_id,shohin_name,sell_price, buying_price
    FROM Shohin
    ORDER BY sell_price, shohin_id;
-- 彙總函數也可以用在ORDER BY
SELECT shohin_catalg,COUNT(*)
    FROM Shohin
    GROUP BY shohin_catalg
    ORDER BY COUNT(*);
```

---
## Chap4
### DELETE用法
- 在chap1有提到DROP可以把資料表整個刪除，但如果今天只是要清空資料表，可以使用DELETE。
```sql
-- 清空資料表
DELETE FROM table_name;
-- 有條件的清空資料表
DELETE FROM table_name
    WHERE cond;
```
#### 範例
```sql=
-- 清空資料表
DELETE FROM Shohin;
-- 有條件的清空資料表
DELETE FROM Shohin
    WHERE sell_price>=4000;
```

### UPDATE用法
- 修改資料表中的資料需要使用UPDATE。
- <font size=2>(Error Code: 1175. You are using safe update mode and you tried to update a table without a WHERE that uses a KEY column. To disable safe mode, toggle the option in Preferences -> SQL Editor and reconnect.)</font>出現此ERROR的其中一個解決方法：輸入<font color=red>SET SQL_SAFE_UPDATES=0;</font>
```sql
-- 修改全部
UPDATE table_name
    SET column_name=new_value
-- 有條件的修改
UPDATE table_name
    SET column_name=new_value
    WHERE condition;
```
#### 範例
```sql=
-- 更新日期
UPDATE Shohin
    SET reg_date='2009-10-10';
-- 把廚房用品的價格乘以10
UPDATE Shohin
    SET sell_price=sell_price*10
    WHERE shohin_catalg='廚房用品';
-- 修改多個資料
UPDATE Shohin
    SET sell_price=sell_price*10,
        buying_price=buying_price/2
    WHERE shohin_catalg='廚房用品';
```

### TRANSACTION用法
- 在同一次服務操作請求時，若有需要對 DB進行多筆資料異動，使用 transaction將逐筆操作包覆起來，確保更動是能同時成功或同時失敗的。
- START TRANSACTION：開啟交易，打開交易功能。
- COMMIT：確認交易，在交易結束時確認交易，在確認時資料才會真的寫入資料表。
- ROLLBACK：回復交易，執行這行時，會回復在交易內所有更動的內容。
```sql
-- 設定交易功能
START TRANSACTION; --交易開始
    DML敘述1;
    DML敘述2;
    ...
COMMIT;            --交易結束
-- 或是ROLLBACK
```
#### 範例
```sql=
START TRANSACTION;
    -- 襯衫單價降低1000
    UPDATE Shohin
        SET sell_price=sell_price-1000
        WHERE shohin_name='襯衫';
    -- 將T侐單價提高1000
    UPDATE Shohin
        SET sell_price=sell_price+1000
        WHERE shohin_name='T侐';
COMMIT; -- ROLLBACK
```
---
## Chap5 
### 檢視表(View)
- 檢視表和資料表的不同之處：是否儲存著實際的資料
- 檢視表中沒有儲存任何資料，它儲存的僅是「SELECT敘述」，利用檢視表取出資料，其實是先執行儲存的SELECT敘述，暫時建立出虛擬資料表，再從中提取資料。
- 使用View的好處：
    1. 節省空間，因為只有儲存查詢敘述
    2. 提高工作效率，因為只要呼叫使用就能簡單取得SELECT敘述的執行結果
    3. 確保資料的邏輯獨立性，由於操作僅有調整檢視表使用的SELECT敘述，對於基底資料表的敘述並不會去接觸到
```sql
-- 建立檢視表
/*
注意這裡的SELECT敘述內不能有ORDER BY子句，因為檢視表被設定成「各筆紀錄沒有先後順序」。
*/
CREATE VIEW 檢視表名稱 (col1,col2,...)
AS
SELECT敘述
```
#### 範例
```sql=
-- 建立ShohinSum檢視表
-- 注意這裡使用到的AS和取別名不同，絕對不能省略。
CREATE VIEW ShohinSum(shohin_catalg, cnt_shohin)
AS
SELECT shohin_catalg,COUNT(*)
    FROM Shohin
    GROUP BY shohin_catalg;

/* 使用檢視表
當使用FROM使用檢視表查詢時：
1.先執行建立檢視表時定義的SELECT敘述
2.將該結果當作FROM子句指定的資料來源
*/
SELECT shohin_catalg,cnt_shohin
    FROM ShohinSum;

-- 刪除
DROP VIEW Shohin_Sum;
```
### 子查詢
- 用過即丟棄的檢視表

#### 範例
```sql=
-- 在FROM子句中直接寫入建立檢視表的SELECT敘述
SELECT shohin_catalg,cnt_shohin
    FROM (SELECT shohin_catalg,COUNT(*) AS cnt_shohin
            FROM Shohin
            GROUP BY shohin_catalg) AS ShohinSum;
            
-- 純量子查詢：篩選出販售單價高於全部商品平均販售單價的商品
/*
平均販售單價
SELECT AVG(sell_price)
    FROM Shohin;
*/
SELECT shohin_id,shohin_name,sell_price
    FROM Shohin
    WHERE sell_price >(SELECT AVG(sell_price)
                       FROM Shohin);
                       
-- 關聯子查詢：在各商品分類中比較平均售價
-- 錯誤例子：利用純量子查詢，回傳值只能1筆
SELECT shohin_id,shohin_name,sell_price
    FROM Shohin
    WHERE sell_price >(SELECT AVG(sell_price)
                       FROM Shohin
                       GROUP BY shohin_catalg);
-- 正確例子
SELECT shohin_id,shohin_name,sell_price
    FROM Shohin AS S1
    WHERE sell_price >(SELECT AVG(sell_price)
                       FROM Shohin AS S2
                       -- key point
                       WHERE S1.shohin_catalg=S2.shohin_catalg
                       --
                       GROUP BY shohin_catalg);

```
### 集合運算
#### 範例
```sql=
-- 建立第二個資料表
CREATE TABLE Shohin2
(shohin_id     	 CHAR(4)      NOT NULL,
 shohin_name     VARCHAR(100) NOT NULL,
 shohin_catalg 	 VARCHAR(32)  NOT NULL,
 sell_price  	 INTEGER      ,
 buying_price  	 INTEGER      ,
 reg_date      	 DATE         ,
 PRIMARY KEY (shohin_id));
 
START TRANSACTION;
INSERT INTO Shohin2 VALUES ('0001', 'T侐', '衣物', 1000, 500, '2009-09-20');
INSERT INTO Shohin2 VALUES ('0002', '打孔機', '辦公用品', 500, 320, '2009-09-11');
INSERT INTO Shohin2 VALUES ('0003', '襯衫', '衣物', 4000, 2800, NULL);
INSERT INTO Shohin2 VALUES ('0009', '手套', '衣物', 800, 500, NULL);
INSERT INTO Shohin2 VALUES ('0010', '水壺', '廚房用品', 2000, 1700, '2009-09-20');
COMMIT;

-- 聯集運算UNION(Shohin+Shohin2)
SELECT shohin_id,shohin_name
    FROM Shohin
UNION
SELECT shohin_id,shohin_name
    FROM Shohin2;
    
-- 減法運算EXCEPT(Shohin-Shohin2)
SELECT shohin_id,shohin_name
    FROM Shohin
EXCEPT
SELECT shohin_id,shohin_name
    FROM Shohin2;
    
-- 交集運算INTERSECT
SELECT shohin_id,shohin_name
    FROM Shohin
INTERSECT
SELECT shohin_id,shohin_name
    FROM Shohin2;
```
### 結合資料表
- 先建立第二個資料表
```sql=
-- 先建立第二個資料表
CREATE TABLE StoreShohin
(store_id  CHAR(4)       NOT NULL,
 store_name  VARCHAR(200) NOT NULL,
 shohin_id CHAR(4)       NOT NULL,
 s_amount     INTEGER       NOT NULL,
 PRIMARY KEY (store_id, shohin_id));
 
START TRANSACTION;
INSERT INTO StoreShohin (store_id, store_name, shohin_id, s_amount) VALUES ('000A',	'東京','0001',30);
INSERT INTO StoreShohin (store_id, store_name, shohin_id, s_amount) VALUES ('000A',	'東京','0002',50);
INSERT INTO StoreShohin (store_id, store_name, shohin_id, s_amount) VALUES ('000A',	'東京','0003',15);
INSERT INTO StoreShohin (store_id, store_name, shohin_id, s_amount) VALUES ('000B',	'名古屋','0002',30);
INSERT INTO StoreShohin (store_id, store_name, shohin_id, s_amount) VALUES ('000B',	'名古屋','0003',120);
INSERT INTO StoreShohin (store_id, store_name, shohin_id, s_amount) VALUES ('000B',	'名古屋','0004',20);
INSERT INTO StoreShohin (store_id, store_name, shohin_id, s_amount) VALUES ('000B',	'名古屋','0006',10);
INSERT INTO StoreShohin (store_id, store_name, shohin_id, s_amount) VALUES ('000B',	'名古屋','0007',40);
INSERT INTO StoreShohin (store_id, store_name, shohin_id, s_amount) VALUES ('000C',	'大阪','0003',20);
INSERT INTO StoreShohin (store_id, store_name, shohin_id, s_amount) VALUES ('000C',	'大阪','0004',50);
INSERT INTO StoreShohin (store_id, store_name, shohin_id, s_amount) VALUES ('000C',	'大阪','0006',90);
INSERT INTO StoreShohin (store_id, store_name, shohin_id, s_amount) VALUES ('000C',	'大阪','0007',70);
INSERT INTO StoreShohin (store_id, store_name, shohin_id, s_amount) VALUES ('000D',	'福岡','0001',100);
COMMIT;
```


|| Shohin   | StoreShohin|
|-| -------- | -------- |
|商品id| O    | O    |
|商品名稱| O    |     |
|商品分類| O    |     |
|販售單價| O    |    |
|購買單價| O    |     |
|登入日期| O    |     |
|店鋪id|     | O    |
|店鋪名稱|     | O    |
|數量|    | O    |

#### 範例
```sql=
-- 將兩個資料表做內部連結(兩個表都有)
-- 重點1:FROM...INNER JOIN(這裡為了簡潔有使用別名)
-- 重點2:內部結合必須要有ON，用來連結兩個資料表的欄位
-- 重點3:資料表(別名).欄位名稱
SELECT SS.store_id,SS.store_name,SS.shohin_id,S.shohin_name,S.sell_price
    FROM StoreShohin AS SS INNER JOIN Shohin AS S
    ON SS.shohin_id = S.shohin_id
    ORDER BY store_id;

-- 將兩個資料表做外部連結
-- 重點1:會輸出其中一個資料表的全部資訊
-- 重點2:LEFT/RIGHT OUTER JOIN決定哪一個資料表為主
SELECT SS.store_id,SS.store_name,S.shohin_id,S.shohin_name,S.sell_price
    FROM StoreShohin AS SS RIGHT OUTER JOIN Shohin AS S
    ON SS.shohin_id = S.shohin_id
    ORDER BY store_id;
```


