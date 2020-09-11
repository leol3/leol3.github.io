---
title: MySQL可重复读隔离级别的实现原理
date: 2020-09-11 09:46:17
tags:
---

<h4>原理</h4>  
Mysql默认的隔离级别是可重复读，即：事务A在读到一条数据之后，此时事务B对该数据进行了修改并提交，那么事务A再读该数据，读到的还是原来的内容。那么Mysql可重复读是如何实现的呢？  
使用的一种叫MVCC的控制方式，即Mutli-version Concurrency Control，多版本并发控制，类似于乐观锁的一种实现方式。  

**实现方式：**  
> <font size="2">InnoDB在每行记录后面保存两个隐藏的列，分别保存这个行的创建时间和删除时间。这里存储的并不是实际的时间值，而是系统版本号，当数据被修改时，版本号加一  
在读取事务开始时，系统会给当前读事务一个版本号，事务会读取版本号<=当前版本号的数据  
此时如果其他写事务修改了该数据，那么这条数据的版本号就会加一，从而比当前读事务的版本号高，该事务自然就读不到更新后的数据了</font>  

假设初始版本号为1：  
`insert into user (id,name) values (1,'Tom');`  
下面模拟一下文章开头的场景：  
`select * from user where id = 1;` // 事务A   
此时读到的版本号为1  
`update user set name = 'Jerry' where id = 1;`  // 事务B  
在更新操作的时候，该事务的版本号在原来的基础上加1，所以版本号为2.  
先将这条要更新的数据标记为已删除，并且删除的版本号是当前事务的版本号，然后插入一行新的记录  
<table class="table table-bordered table-striped table-condensed">
    <tr>
        <td>id</td>
        <td>name</td>
        <td>create_version</td>
        <td>delete_version</td>
    </tr>
    <tr>
        <td>1</td>
        <td>Tom</td>
        <td>1</td>
        <td>2</td>
    </tr>
    <tr>
        <td>1</td>
        <td>Jerry</td>
        <td>2</td>
        <td></td>
    </tr>
</table>  
此时事务A再重新读取数据，由于事务A一直没提交所以此时读到的版本号还是为1，读到的还是Tom这条数据，也就是可重复读
