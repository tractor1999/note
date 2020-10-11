### 了解SQL

数据库：保存有组织的数据的容器

数据库软件：DBMS，通过他来创建操纵容器

主键:一列(或一组列)，其值能够唯一区分表中每行

- 任意两行不允许有相同的主键
- 每个行都必须具有一个主键值

什么是SQL?SQL是结构化查询语句（Structured Query Languge）的缩写。SQL是专门用来与数据库通信的语言。


MySQL、Oracle以及MIcrosoft SQL Server等数据库是基于客户机-服务器的数据库。客户机与用户打交道的软件，服务器部分是负责所有数据访问的处理的一个软件。

```mysql
SHOW DATABASES; 返回一个可用的数据库列表
SHOW TABLES;获得数据库中表的列表
SHOW COLUMNS FROM customers;显示表中列对每个字段返回一行
DESCRIBE customers;是显示表中列的另一种快捷方式
```



### SELECT语句 

```mysql
SELECT prod_name FROM products;检索一个名为prod_name的列
SELECT prod_id,prod_name,prod_price FROM products;检索多个列
SELECT * FROM products;检索所有列
SELECT DISTINCT vend_id FROM products;检索不同值得列表
SELECT prod_name FROM products LIMIT 5;检索单个列返回不多于五行
SELECT prod_name FROM products LIMIT 4,5;检索单个列从第四行开始的五行
SELECT products.prod_name FROM products;使用完全限定的名字
```



### 排序检索数据 ORDER BY   DESC

```mysql
SELECT prod_name FROM products ORDER BY prod_name;//默认升序
SELECT prod_id,prod_price,prod_name FROM products ORDER BY prod_price,prod_name;//仅在多行具有相同的prod_price时才对产品按prod_name升序
SELECT prod_id,prod_price,prod_name FROM products ORDER BY prod_price DESC;//按prod_price降序
```



### 过滤数据  WHERE 

```mysql
SELECT prod_name,prod_price FROM products WHERE prod_price =2.50;//检索两个列但不返回prod_price为2.50的行
SELECT prod_name,prod_price FROM products WHERE prod_price <10;//检索两个列返回prod_price小于10的行
SELECT vend_id,prod_name FROM products WHERE vend_id <>1003;//检索两个列返回vend_id不等于1003的行 !=也是一样的
SELECT prod_name,prod_price FROM products WHERE prod_price BETWEEN 5 AND 10;//范围检索
SELECT prod__name FROM products WHERE prod_price IS NULL;//检查具有NULL值
```



### 数据过滤 AND OR IN

```mysql
SELECT prod_id,prod_price,prod_name FROM products WHERE vend_id =1003 AND prod_price<=10; <=10;检索由供应商1003制造且价格小于等于10美元的所有产品的名称和价格
SELECT prod_name,prod_price FROM products WHERE vend_id = 1002 OR vend_id = 1003;检索由供应商1003或1002提供的产品的名称和价格
注意AND优先级大于OR所以同时使用时要加括号
SELECT prod_name,prod_price FROM products WHERE vend_id IN(1002,1003) ORDER BY prod_name;
NOT可以和IN、BETWEEN和EXISTS子句取反，否定跟在它后面的条件
SELECT vend_id,prod_name,prod_price FROM products WHERE vend_id NOT IN(1002,1003) ORDER BY prod_name;
```



### 用通配符进行过滤 LIKE % _

```mysql
%表示任何字符出现任意次数
_表示任何字符出现一次
SELECT prod_id,prod_name FROM products WHERE prod_name LIKE 'jet%';
SELECT prod_id,prod_name FROM products WHERE prod_name LIKE '_ ton anvil';
```



### 使用MYSQL正则表达式 REGEXP

REGEXP和LIKE的区别:    REGEXP是包含 LIKE是相等
```mysql
.匹配任意一个字符
SELECT prod_name FROM products WHERE prod_name REGEXP '.000' ORDER BY prod_name;
OR 搜索含有两个串之一
SELECT prod_name FROM products WHERE prod_name REGEXP '1000|2000' ORDER BY prod_name;
[]匹配几个字符之一
SELECT prod_name FROM products WHERE prod_name REGEXP '[123] Ton' ORDER BY prod_name;
// 匹配特殊字符 
SELECT vend_name FROM vendors WHERE vend_name REGEXP '\\.' ORDER BY vend_name;
匹配字符类
[:alnum:] 任意字母和数字
[:alpha:] 任意字符(同[a-zA-Z])
[:digit:] 任意数字(同[0-9])
匹配多个实例
控制前面的一个字符
元字符         说明
*           0个或多个匹配
+           1个或多个匹配
？          0个或1个匹配
```



### 计算字段

```
Concat() 拼接串
SELECT Concat(vend_name,'(',vend_country,')') FROM vendors ORDER BY vend_name;
使用别名
SELECT Concat(RTrim(vend_name),'(',RTrim(vend_country),')') AS vend_title FROM vendors ORDER BY vend_name;
执行算数计算
SELECT prod_id,quantity,item_price,quantity*item_price AS expanded_price FROM orderitems  WHERE order_num = 20005;
```



### 汇总数据

```mysql
函数               说明
AVG()            返回某列的平均值
COUNT()          返回某列的行数
MAX()            返回某列的最大值
MIN()            返回某列的最小值
SUM()            返回某列之和

SELECT AVG(prod_price) AS avg_price FROM products;
SELECT COUNT(*) AS num_cust FROM customers;
SELECT MAX(prod_price) AS max_price FROM products;
SELECT MIN(prod_price) AS min_price FROM products;
SELECT SUM(quantity) AS items_ordered FROM orderitems WHERE 
order_num = 20005;
```



### 数据分组 GROUP BY

```mysql
SELECT vend_id,COUNT(*) AS num_prods FROM products GROUP BY vend_id;
过滤分组
SELECT cust_id,COUNT(*) AS orders FROM orders GROUP BY cust_id HAVING COUNT(*) >= 2;

WHERE和HAVING的区别:WHERE在分组前过滤，Having在分组后进行过滤

分组和排序
SELECT order_num,SUM(quantity*item_price) AS ordertotal FROM orderitems GROUP BY order_num HAVING SUM(quantity*item_price) >= 50 ORDER BY ordertotal;


```
### SELECT子句的顺序:

| 子句     | 说明               | 是否必须使用             |
| -------- | ------------------ | :----------------------- |
| SELECT   | 要返回的列或表达式 | 是                       |
| FROM     | 从中检索数据的表   | 仅在从表中选择数据时使用 |
| WHERE    | 行级过滤           | 否                       |
| GROUP BY | 分组说明           | 仅在按组计算聚集时使用   |
| HAVING   | 组级过滤           | 否                       |
| ORDER BY | 输出排序顺序       | 否                       |
| LIMIT    | 要检索的行数       | 否                       |



### 子查询

子查询总是从内往外处理
```mysql
列出订购物品TNT2的所有客户
SELECT cust_id FROM orders WHERE order_num IN (SELECT order_num FROM orderitems WHERE prod_id = 'TNT2';
                                               
解释：先从 orderitems表中找到产品编号为TNT2的订单编号，列出orders表中相对应订单编号的客户id
SELECT cust_name,cust_state,(SELECT COUNT(*) FROM orders WHERE orders.cust_id = customers.cust_id) AS orders FROM customers ORDER BY cust_name;
解释：对每个客户执行COUNT(*)计算，将COUNT(*)作为子查询

相关子查询 涉及外部查询的子查询
任何时候只要列名涉及多义性就必须使用这种语法
```

### 联结表
```mysql
SELECT vend_name,prod_name,prod_price FROM vendors,products WHERE vendors.vend_id = products.vend_id ORDER BY vend_name,prod_name;
解释：vendors表的主键又是products表的外键它将vendors表和products表关联起来利用供应商ID能从vendors表中找出相应供应商的详细信息。
```

### 高级联结
```mysql
SELECT cust_name,cust_contact FROM customers AS c,orders AS o,orderitems AS oi WHERE c.cust_id = o.cust_id AND oi.order_num = o.order_num AND prod_id = 'TNT2';
解释：原本customers表中是没有prod_id列通过联结后过滤出prod_id为'TNT2'.所有使用子查询也是可以实现的

使用子查询：
SELECT cust_name,cust_contact FROM customers WHERE cust_id IN (SELECT  cust_id FROM orders WHERE order_num IN (SELECT order_num FROM orderitems WHERE prod_id ='TNT2'));

自联结:
子查询：查找ID为DTNTR的物品供货商,找到这个供应商的其他产品
SELECT prod_id,prod_name FROM products WHERE vend_id =(SELECT vend_id FROM products WHERE prod_id ='DTNTR');
自联结方法：
SELECT p1.prod_id,p1.prod_name FROM products AS p1,products AS p2 WHERE p1.vend_id = p2.vend_id AND p2.prod_id ='DTNTR';
自联结效率比子查询高得多！！！

自然联结：排除重复出现的列，一般对当前表使用通配符,对其他的表的列使用明确的子集来完成的。
SELECT c.*,o.order_num,o.order_date,oi.prod_id,oi.quantity,oi.item_price FROM customers AS c,orders AS o,orderitems AS oi WHERE c.cust_id = o.cust_id AND oi.order_num = o.order_num AND prod_id = 'FB';

外部联结：包含了那些在相关表中没有关联行的行，这种类型的联结称为外部联结。
内部联结：检索所有客户及其订单
SELECT customers.cust_id,orders.order_num FROM customers INNER JOIN orders ON customers.cust_id = orders.cust_id;
外部联结：检索所有客户包括没有订单的客户
SELECT customers.cust_id,orders.order_num FROM customers LEFT OUTER JOIN orders ON customers.cust_id =orders.cust_id;
SELECT customers.cust_id,orders.order_num FROM customers RIGHT OUTER JOIN orders ON customers.cust_id =orders.cust_id;
RIHGT指出是OUTER JOIN 选择右边的表所有行, 而LEFT指出是OUTER JOIN 左边的表所有行。

带聚集函数的联结：
SELECT customers.cust_name,customers.cust_id,COUNT(orders.order_num) AS num_ord FROM customers INNER JOIN orders ON customers.cust_id = orders.cust_id GROUP BY customers.cust_id;
分析：先内部关联 再以cust_id分组，统计每个客户的订单数量。
SELECT customers.cust_name,customers.cust_id,COUNT(orders.order_num) AS num_ord FROM customers LEFT OUTER JOIN orders ON customers.cust_id = orders.cust_id GROUP BY customers.cust_id;
分析：外部左侧关联以cust_id 分组统计每个客户的订单量即使为0；
```

### 组合查询    UNION
```mysql
SELECT vend_id,prod_id,prod_price FROM products WHERE prod_price <=5 UNION SELECT vend_id,prod_id,prod_price FROM products WHERE vend_id IN (1001,1002);
或使用
SELECT vend_id,prod_id,prod_price FROM products WHERE prod_price <=5 OR vend_id IN (1001,1002);

注意点：UNION默认去除重复行，若返回所有行使用UNION ALL
```



###  全文本搜索 

MySQL最常用的引擎：MyISAM 发音为 "my-z[ei]m";InnoDB 发音为 "in-no-db"。

PRIMARY KEY 指明主键

FULLTEXT 指明索引列

注意：不要在导入数据时使用FULLTEXT，应该导完所有数据再定义FULLTEXT;

使用Match()和Against()执行全文本搜索

查询扩展极大增加了返回的行数

布尔文本搜索没有定义FULLTEXT索引也可以使用，但是非常慢

```mysql
建表时：
CREATE TABLE productnotes
(
    note_id	  int 	NOT NULL AUTO_INCREMENT,
    prod_id	  char(10) NOT NULL,
    note_date  datetime NOT NULL,
    note_text   text	 NULL,
    PRIMARY KEY(note_id),
    FULLTEXT(note_text)
)ENGINE=MyISAM;

使用Match()和Against()执行全文本搜索：
SELECT note_text FROM productnotes WHERE Match(note_text) Against('rabbit');
也可以用LIKE达到上述表达式的效果
SELECT note_text FROM productnotes WHERE note_text LIKE '%rabbit%';

上述两条SELECT语句都不包含ORDER BY 子句。后者(使用LIKE)以不特别有用的顺序返回数据，前者使用全文本搜索返回以文本匹配良好程度排序的数据。等级由行中词的数目、唯一词的数目、整个索引中词的总数以及包含该词的行的数目计算出来。
SELECT note_text, Match(note_text) Against('rabbit') AS rank FROM productnotes;
```



### 数据插入

