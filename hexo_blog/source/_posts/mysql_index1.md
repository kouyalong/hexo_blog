layout: mysql_index1_layout
title: Mysql的innodb存储引擎RR隔离级别mvcc、next-key locks和幻读关系
date: 2020-03-16 11:50:05
tags: 重点
comments: true
categories: Mysql

---

#### 一、InnoDB四种事务隔离级别
![](/images/mysql1.png)

**总结：正常的RR级别隔离，是有可能发生幻读的**

#### 二、幻读
- 百度百科摘要：幻读是指当事务不是独立执行时发生的一种现象
例如第一个事务对一个表中的数据进行了修改，比如这种修改涉及到表中的“全部数据行”。
同时，第二个事务也修改这个表中的数据，这种修改是向表中插入“一行新数据”。
那么，以后就会发生操作第一个事务的用户发现表中还存在没有修改的数据行，就好象发生了幻觉一样
举个例子：会话1两次select数量不一样
![](/images/mysql2.jpg)

**总结：其实不可重复读重点在于update和delete，而幻读的重点在于insert**

#### 三、Innodb里面常见的几种读
- 快照读：简单的select操作，属于快照读，不加锁。
select * from table where ?
- 当前读：特殊的读操作，插入/更新/删除操作，属于当前读，需要加锁 
select * from table where ? lock in share mode; （加S锁） 
select * from table where ? for update;（加X锁） 
insert into table values (…);（加X锁） 
update table set ? where ?;（加X锁） 
delete from table where ?;（加X锁） 

**总结：快照读是没有幻读存在的，当前读才会出现幻读**

#### 四、Next-Key-locks
Next-Key-locks是行锁和GAP（间隙锁）的合并;
行锁可以防止不同事务版本的数据修改提交时造成数据冲突的情况。
间隙锁该锁会锁定一个范围，但是不括记录本身。可以通过修改隔离级别为 READ COMMITTED 或者配置 innodb_locks_unsafe_for_binlog 参数为 ON 。

**总结：InnoDB存储引擎处理幻读的方式Next-Key-locks**

#### 五、MVCC多版本并发控制

- 每一行的隐藏列：DATA_TRX_ID、DATA_ROLL_PTR、DB_ROW_ID（如果没有主键）
- ReadView： LOW_TRX_ID、UP_TRX_ID、TRX_IDS  
- redo日志保证已提交事务的ACID特性，设计思路是，通过顺序写替代随机写，提高并发 
- undo日志用来回滚未提交的事务，它存储在回滚段里 
- 通过“读取旧版本数据”来降低并发事务的锁冲突，提高任务的并发度。
- 回滚段里的数据，其实是历史数据的快照（snapshot），这些数据是不会被修改，select可以肆无忌惮的并发读取他们。 

**总结：Innodb存储引擎高并发快照读的原因**

#### 引用
- [Innodb中的事务隔离级别和锁的关系](https://tech.meituan.com/2014/08/20/innodb-lock.html)
- [MySQL的InnoDB的幻读问题](http://blog.sina.com.cn/s/blog_499740cb0100ugs7.html)
- [脏读、幻读与不可重复读](https://juejin.im/entry/5b835dfbf265da43531d0593)
- [Innodb 中 RR 隔离级别能否防止幻读?](https://github.com/Yhzhtk/note/issues/42)
