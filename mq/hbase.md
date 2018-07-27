# HBase

HBase 是一种类似于数据库的存储层，适用于结构化的存储，是一种列式的分布式数据库。HBase适合用来进行大数据的实时查询，ACID只支持单个Row级别。

## HBase 与 RDBMS的区别

### 表1. 数据在RDBMS中的排布示例:

| ID   | fn   | ln   | passwd | timestamp |
| ---- | ---- | ---- | ------ | --------- |
| 1    | 三   | 张   | 111    | 20160719  |
| 2    | 四   | 李   | 222    | 20160720  |



### 表2. 数据在HBase中的排布(逻辑上)

HBase 有Column Family的概念, 简称为CF. CF一般用于将相关的列(Column) 组合起来.  将表1中的列分成两个CF, 分别为info 和 pwd. info存储着姓名相关列的数据, 而pwd则是密码相关的数据. 

| Row-key | Value(CF, Qualifier,Version)                         |
| ------- | ---------------------------------------------------- |
| 1       | info{'fn':'三', 'ln':'张'} <br />pwd{'passwd':'111'} |
| 2       | info{'fn':'四', 'ln':'李'}<br />pwd{'passwd':'222'}  |

###  数据在HBase中的排布(物理上)

在物理上HBase是按CF存储的, 只是按照Row-key将相关CF中的列关联起来.

#### 表3. info 列族

| Row-Key | CF:Column-Key | 时间戳    | Cell Value |
| ------- | ------------- | --------- | ---------- |
| 1       | info:fn       | 123456789 | 三         |
| 1       | info:ln       | 123456789 | 张         |
| 2       | info:fn       | 123456789 | 四         |
| 2       | info:ln       | 123456789 | 李         |

表3 是info存储在HBase中的数据排布, 表中的fn和ln为 Column-key 或者 Qulifimer. 在HBase中, Row-key + CF + Qulifier + 时间戳才可以定位到一个单元格数据(HBase 中每个单元格默认有3个时间戳的版本数据). 

#### 表4 pwd列族

| Row-Key | CF:Column-Key | 时间戳    | Cell Value |
| ------- | ------------- | --------- | ---------- |
| 1       | pwd:passwd    | 123456789 | 111        |
| 2       | pwd:passwd    | 123456789 | 222        |

 从表3和表4可以看到, Row1到Row2的数据分布在两个CF中, 并且每个CF对应一个HFile。RDBMS逻辑上每一行中的一个单元格数据，对应于HFile中的一行，然后当用户按照Row-key查询数据的时候，HBase会遍历两个HFile，通过相同的Row-key标识，将相关的单元格组织成行返回。

## HBase 相关模块

