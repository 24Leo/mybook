##共享锁、互斥锁
两段锁协议：阶段１：加锁阶段 阶段２：解锁阶段
* 读锁、共享锁
    * 如果事务T对数据A加上共享锁S后，则其他事务只能对A再加共享锁，不能加排他锁，直到已释放所有共享锁。**获准共享锁的事务只能读数据，不能修改数据**。
* 写锁、互斥锁
    * 如果事务T对数据A加上排他锁X后，则其他事务不能再对A加任任何类型的锁，直到在事务将资源上的锁释放为止。**获准排他锁的事务既能读数据，又能修改数据**。
* 性能上，纯互斥锁比共享+互斥锁要快非常多

锁的底层实现一般是在C的库或者操作系统实现的，有些地方需要原子计数器或者内存栅这种机制。

##事务：
是一系列的数据库操作，是数据库应用的基本逻辑单位。性质：
* 原子性。即不可分割性，事务要么全部被执行，要么就全部不被执行。
* 一致性。事务的执行使得数据库从一种正确状态转换成另一种正确状态
* 隔离性。在事务正确提交之前，不允许把该事务对数据的任何改变提供给任何其他事务。**需要看隔离级别设置**。
* 持久性。事务正确提交后，其结果将永久保存在数据库中，即使在事务提交后有了其他故障，事务的处理结果也会得到保存。
* 开启事务：
    * 显示开启： start transction | begin
    * 隐式：COMMIT、ROLLBACK

##读相关问题
####脏读
是指一个事物读取其他事物尚未提交的更改。
    * 一个事物修改数据（写到数据库中，其他事物能否看到具体看隔离级别以及MVCC），这时另外一个事物读取该数据，后来当前事物发现数据有问题，回滚了，那么另外一个事物读取的数据即错误
    * 因为修改的数据永远可能有错误，需要回滚！
    
####不可重复读
是指同一个事物中多次读取相同条件数据，由于其他事物修改提交后导致获得不同结果。
    * 当前事物读取数据，另外一个事物修改该数据后提交。那么当前事物再次读取会发现不同。

####幻读
是指对一种现象：对整个表操作（读个数、全表更新等）后，由于其他事物插入、更新操作提交后，导致整个表状态不一致。
    * 表原有100人，现更新生日格式为年月日，另一个事物按原来规则插入1个（比如仅有月日）并提交，此时当前事物读取表发现一个未修改，有“鬼”了（明明全部修改了）！
    
####格式
```C++
     当前事物        另外事物
－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－
begin                      
  update table_id   
                  read table_id   
                  ....            
  rollback   

begin                     
    read table_id             
                 update table_id           
                 commit                
    read table_id  
       
begin 
    read table_all
                insert table_one
                commit
    read table_all
```
####解决方案
#####脏读
* 第一种：添加写锁即排它锁，
* 第二种：添加读锁＝＝》可能不可重复读：别事物读并修改
* 第三种：设置隔离级别：后三个

#####不可重复读
* 第一种：写锁
* 第二种：设置隔离级别：后两个

#####幻读
* 设置隔离级别为最后一个

<hr>**会不会出现上述各种读情况情况，主要看设置的隔离级别**<hr>
##隔离级别
#####READ_UNCOMMITTED
事物允许读其他事物未提交数据
    * 脏读、不可重复读、幻读都会出现
    * 隔离最低，并发最高
    
#####READ_COMMITTED
事物仅可以读其他事物提交数据，否则等待；其他事物可以修改a当前事物读的数据；
    * 不会有脏读，但会出现不可重复读、幻读
    
#####REPEATABLE_READ
保证事物整个过程中数据不会被修改，多次读取结果一致；
    * 通过对当前事物**需要的行范围进行加锁（next-key locking）**，保证提交之前其他事物不能读取和修改。
    * 但是其他事物**可以修改其他行记录**；
    * 会出现幻读，不会出现脏读、不可重复读。（innodb解决了）

#####SERIALIZABLE
事物开始之前，其他事物已经提交。读取操作的结果是在这个事务开始之前其他事务就已经提交的记录。
    * 对**整个表加锁**，对表中任何行操作都不允许；
    * 不会出现任何一种问题
    * 隔离最高，并发最低

#####RC、RR 
在rc事务隔离级别下，对于快照数据，非一致性读总是读锁定行的最新一份快照数据.而在RR事务隔离级别下，对于快照数据，非一致性读总是读取**事务开始时**的行数据版本（通过事物ID判断，另外可能有时候不是事物开始，而是事物第一条有效命令开始时的版本，重点是可重复读，可重复，所以第一个有效指令才算）。


[return](README.md)