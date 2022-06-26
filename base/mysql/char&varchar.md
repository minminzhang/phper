## char和varchar的区别

char和varchar是MySQL提供的两种相似的列，都能存储字符或字符串；

#### 区别

对于char类型来说，它存储的列的长度是不可变的，而varchar类型的列可以存储可变长度的字符串。

插入时：比如char(5)表示这个列占用的存储空间一直是5个字符，你可以往char(5)的a列里插入"abc"，很显然是3个字符而不是5个字符，此时MySQL底层会替你在"abc"后面追加两个空格字符为"abc  "。

检索时：当你往外检索上面提到的a列时，MySQL会自动帮你做一次trim()操作，去掉最后的空格，返回"abc"


###### 假如建表语句是这样的

```
create table t( a char(5))
charset=utf8 engine=innoDB;
```
注意表的字符集是GBK

问题1，：下面两条语句能执行成功吗？为什么？
```
insert into t select '12345';
insert into t select '这次是汉字';
```
解释：以上两条sql都能执行成功，创建数据表时指定char(5)意思是这列可以存储5个字符，而不是五个字节。
对于数字来说是包含在utf8中的，并且每个数字占用一个字节，中文也被包含在utf8中，每个中文占三个字节。

综上：当charset为utf8时，char(5)可以存储的字节范围为[5, 5*3]
当使用的字符编码不同时，char能存储的字节数是会变化的。

问题2：
建表
```
create table t(b varchar(2))
charset=utf8 engine=innoDB;
```
插入数据
```
insert into t select 'abcd';
```
很明显varchar的期望长度是2，但是插入的字符串长度为4，此时sql能否执行成功取决于sql mode ,
当sql mode 为严格（strict）模式时，上面的sql会报错
当sql mode 为非严格模式时，MySQL将超出2个字符以外的字符砍掉了，保留'ab'然后存入数据库。



