# Database Design 数据库设计
数据库模型的ER图如下
![ER](../pic/07-02-Database Design/DatabaseDesign.png)  

* 一共有四个数据表,分别是user表，type表，menu表和list表:
    - user表是记录商家信息的表，以自增的商家id作为主键；
    - type表是记录分类信息的表，以自增的分类id作为主键，有一个外键是商家id，用于表示该分类属于哪个商家；
    - menu表是记录菜品信息的表，以自增的菜品id作为主键，有两个外键，分别是商家id和分类id，用于表示该菜品属于哪个商家的哪个分类；
    - list表是记录订单信息的表，以自增的订单id作为主键，有一个外键是商家id，用于表示该订单属于哪个商家；，