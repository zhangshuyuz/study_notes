# MySQL调优

## 1. MySQL5.7安装

1. MySQL5.7安装

   > 1. 安装指南
   >    1. 因为一台机器只能安装一个MySQL，因此如果想安装5.7必须要把5.5完全卸载，太麻烦。因此直接创建一个新的虚拟机。
   >    2. 卸载Linux自带的mariadb：```rpm -e --nodeps mariadb-libs```，排除依赖强制卸载
   >    3. 检查是否安装有libaio和net-tools
   >    4. 检查/tmp文件夹权限是否为777
   >    5. 安装MySQL5.7
   >    
   >        * MySQL5.7安装完毕后不会自动初始化，必须要使用命令来初始化
   >    6. 初始化MySQL5.7：```mysqld --initialize -user=mysql```
   >    7. 查看初始密码：```cat /var/log/mysqld.log```
   >    8. 启动MySQL服务：```systemctl start mysqld```
   >    9. 登录MySQL：```mysql -uroot -p```，输入root密码
   >    10. 重置密码：```ALTER USER 'root'@'localhost' IDENTIFIED BY 'root';```，重置root用户密码为root
   >    11. 退出，重新登录MySQL
   >    12. 查看MySQL是否为自启动：```systemctl list-unit-files| grep mysqld```
   >    13. 进入MySQL配置文件，修改MySQL字符集编码：```vim /etc/my.cnf```，在最后写入```character_set_server=utf8```
   >    14. 重启MySQL服务：```systemctl restart mysqld```
   >    15. 手动修改之前没修改字符集的库和表：进入MySQL，修改之前创建的库和表的字符集```alter database 数据库名 character set 'utf8';```、```alter table 表名 convert to character set 'utf8';```
   >    
   > 2. 小知识点：```user xxx```是MySQL的命令，可以不用带;。```create database xxx;```是SQL，必须带;。
   >
   > 3. MySQL5.7安装位置：1、MySQL的命令放在/usr/bin/下；2、MySQL数据库文件放在/var/lib/mysql/下；3、MySQL进程pid文件存放在/var/run/mysqld/mysqld.pid
   >
   > 4. 远程访问MySQL
   >
   >    1. 远程连接想要连接MySQL或Redis，必须保证防火墙已经关闭
   >
   >    2. 查看mysql库下的user表
   >
   >    3. 在user表中添加远程用户：```create user 用户名 identified by '密码';```
   >
   >    4. 给远程用户授权，一般指定权限，这里为了方便，授予全部权限：```grant all privileges on *.* to root@'%' identified by '密码';```
   >    
## 2. 杂项知识点
2. 杂项知识点
   > 1. 假如有一个表mytable，结构为：id, name, age, dept。我们想求每个dept中，age最大的人，写出SQL。
   >
   >    第一反应，我们会写：```select name, dept, MAX(age) from mytable group by dept;```
   >    
   >    但是这样写是错误的，会直接报错。这里就引出了GROUP BY语句的原则
   >    
   > 2. GROUP BY 语句的原则
   >
   >    select 后只能写聚合函数和group by 的字段。
   >    
   >    **因为在MySQL中有一个sql_mode的配置项，它配置了各种规范。sql_mode常用的值有很多，最常用的only_full_group_by，对于group by的聚合操作，如果在select中的列没有在group by中出现，则sql不合法。**
   >    
   >    **可以直接通过查询语句：```show variables like 'sql_mode';```来查看sql_mode的值。如果进入公司拷贝数据库的数据，一定要先拷贝sql_mode中的值放入本地数据库的sql_mode**
   >    
   >    因此上述查询应该改变为连表查询：```select * from mytable mt1 inner join (select dept, max(age) maxage from mytable group by dept) mt2 on mt1.dept=mt2.dept and mt1.age=mt2.maxage;```
   >    
## 3. MySQL逻辑架构
3. MySQL逻辑架构（必须牢记）
   
   >    1. 图解形式参见MySQL逻辑架构的图。
   >
   >       * 程序访问数据库
   >       * 和连接池进行沟通
   >       * 缓存、缓冲中查询，有则返回，没有则下一步
   >       * SQL接口分析SQL语句
   >       * 解析器解析复杂SQL
   >       * 优化器，在不影响结果的情况下改变SQL顺序，生成执行计划
   >       * 从存储引擎中按计划分类执行
   >       * 执行完毕后将结果存入缓存中
   >       * 返回数据给用户
   >
   >    2. 小知识点：缓存用来读信息；缓冲用来写信息
   >
   >    3. 小知识点：只要修改MySQL配置文件，重启MySQL后，必须用```systemctl status mysqld```来查看MySQL启动状态
   >
   >    4. 如何查看sql的执行周期？
   >
   >       上述中我们已经知道了MySQL的逻辑架构，要验证逻辑架构，我们必须追踪slq的执行周期：
   >
   >       1、我们可以修改配置文件/etc/my.cnf，添加一行```query_cache_type=1```启用缓存，重启MySQL
   >
   >       2、开启profile：进入MySQL，输入```show variables like '%profiling';```，输入```set profiling=1;```开启profile
   >
   >       3、至此，我们以后的每次SQL，都可以看到执行计划了。
   >
   >       4、输入```show profiles;```查看简略执行计划；输入```show profile cpu, block io for query 具体的查询id;```查看某个查询id的执行计划详细信息
   >       
   >    5. 开启缓存后，什么样的SQL会命中缓存？
   >
   >       **必须保证SQL完全一致，才会命中缓存**。
   >       
   >       因为MySQL缓存中和Redis一样使用key-value存储，key为SQL，value为数据。因此必须保证SQL完全一致才能命中MySQL缓存
   >       
   >    6. 小知识点：having 一般都会跟在group by后，作用是对group by分组后的组内数据再次进行筛选，having后可以使用一些函数
   >    
## 4. MySQL存储引擎
4. MySQL存储引擎
   
    > 1. MySQL存储引擎介绍
    >
    >    MySQL存储引擎是真正和数据库表交互的东西。
    >
    >    可以在MySQL中使用```show engines;```来查看所有的MySQL存储引擎
    >
    > 2. 最常用的MySQL存储引擎：MyISAM和InnoDB
    > 
    > 3. MyISAM和InnoDB的区别。前四个是最重要的
    > 
    >     * 外键：MyISAM不支持，InnoDB支持
    >     
    >     * 事务：MyISAM不支持，InnoDB支持
    >     
    >     * 行表锁：MyISAM使用表锁，即使只操作一条数据也会锁住整个表，不适合高并发；InnoDB使用行锁，操作时只会锁住某一行，适合高并发
    >     
    >     * 缓存：MyISAM只会缓存索引不会缓存真实数据；InnoDB不仅缓存索引，还缓存真实数据，因此对内存要求很高
    >     
    >     * 关注点：MyISAM节省资源，消耗少，简单的业务；InnoDB支持并发写，事务，占用更大资源
    >     
    >     * 系统自带表：系统自带表全部使用的MyISAM引擎。
    >     
    > 4. 小知识点：绝对不要在工作中使用物理外键，必须使用逻辑外键。
    > 
    > 5. 小知识点：只有行锁会造成死锁的情况，表锁不会。
    > 
## 5. SQL预热

5. SQL预热：常见通用的Join查询。非常重要

   > 1. 内连接：两种写法，不再赘述
   >
   > 2. 左外连接：不再赘述
   >
   > 3. 右外连接：不再赘述
   >
   > 4. 左外连接，取差集：```select * from A left join B on A.key = B.key where B.key is null;```
   >
   > 5. 右外连接，取差集：同左外连接取差集，不再赘述
   >
   > 6. 全连接：```select * from A full outer join B on A.key = b.key;```。
   >
   >    * 注意，MySQL没有全连接，但是可以通过union拼接来做到，
   >
   >      使用union进行拼接。union可以拼接两个字段一致的结果集，并且可以进行自动去重。
   >
   >      ```
   >      select A.*, B.* from A left join B on A.id=B.id 
   >      union 
   >      select A.*, B.* from B left join A on A.id = B.id where A.id is null;
   >      ```
   >
   >      **注意，使用union必须要指定字段顺序**
   >
   > 7. 全连接，取差集：
   >
   >     ```select * from A full outer join B on A.key = B.key where A.key is null or B.key is null;```
   >     
   >     MySQL中可以用union all拼接来做：
   >     
   >     ```
   >     select A.*, B.* from A left join B on A.id = B.id where B.id is null
   >     union all
   >     select A.*, B.* from B left join A on A.id = B.id where A.id is null;
   >     ```
   >     
   > 8. union 和 union all
   >
   >    union 和 union all都是拼接两个结果集，并且都必须指定字段顺序。
   >    
   >    但是union会自动去重，union all不会自动去重
   >    
   >    因此，如果确定两个结果集中没有重复数据，使用union all，因为会更快

## 6. 索引优化分析

6. 索引优化分析

   > 1. 当数据库性能下降，SQL执行很慢时，要考虑的情况
   >
   >    1、数据过多，此时要分库分表；
   >
   >    2、关联太多的表，使用了太多的join。此时要进行SQL优化；
   >
   >    3、没有充分的利用索引。此时要建立索引
   >
   >    4、服务器参数没有优化。修改my.cnf配置文件，优化服务器配置
   >    
   > 2. 索引的优势和劣势
   >
   >     1. 优势：
   >     
   >        1、提高了检索的效率，降低CPU消耗
   >        
   >        2、数据都是有序的，降低了排序的成本
   >        
   >     2. 劣势：
   >     
   >        1、对表进行写操作时，因为不光要更新数据，还要更新索引，因此速度降低
   >        
   >        2、因为索引也是一张表，该表保存了主键和索引字段，指向实体表的记录，因此索引表也需要空间
   >     
   > 3. MySQL索引结构
   >
   >     1. MySQL使用B+Tree作为索引。因为B+Tree比BTree的索引占空间小，一次能加载多个索引，IO效率更高。
   >     
   >     2. 聚簇索引和非聚簇索引
   >     
   >         1. 聚簇索引：数据行在磁盘的排列顺序和索引保持一致。**只有主键索引才是聚簇索引**
   >         
   >         2. 非聚簇索引：索引顺序和数据行在磁盘顺序没有对照关系。**其他索引都是非聚簇索引**
   >         
   >         3. 聚簇索引因为数据存储顺序和索引保持一致，因此可以使用二分查找来缩短查找时间；非聚簇索引顺序查找所有
   >     
   > 4. MySQL索引分类和使用
   >
   >     1. 索引分类
   >     
   >         1. 单值索引：即一个索引只包含单个列，一个表中允许有多个单值索引。
   >         
   >             * 创建单值索引：```create index 索引名称 on 表名(字段名); ```
   >             
   >         2. 唯一索引：索引列的值必须唯一，但是允许有null值
   >         
   >             * 创建唯一索引：```create unique index 索引名称 on 表名(字段名);```
   >             
   >         3. 主键索引：设置主键后，数据库自动建立主键索引。**InnoDB主键索引为聚簇索引**
   >         
   >         4. 复合索引：对多个字段建立一个索引。
   >         
   >             * 创建复合索引：```create index 索引名 on 表名(字段1, 字段2, ...);```
   >     
   >     2. 基本语法
   >     
   >         * 创建索引：```create [unique] index [index_name] on 表名(字段名); ```
   >         
   >         * 删除索引：```drop index index_name on 表名;```
   >         
   >         * 查看索引：```show index from 表名;```
   >         
   >     3. 一般定义索引名称格式：idx_字段名
   >     
   > 5. 什么时候建立索引，什么时候不建立索引
   >
   >     1. 建立索引：
   >     
   >        1、主键自动建立索引
   >        
   >        2、频繁作为查询条件的字段，建立索引
   >        
   >        3、与其他表作为关联查询的字段，逻辑外键的关系建立索引。
   >        
   >        4、组合索引性价比高于单建索引
   >        
   >        5、查询中排序的字段，排序字段如果通过索引去访问，将大大提高排序速度
   >        
   >        6、查询中分组的字段，建立索引。
   >        
   >         * group by 比 order by 更加消耗性能。因为group by会先进行order by。因此分组字段建立索引比排序字段更迫切
   >        
   >     2. 不建立索引
   >     
   >        1、表记录很少
   >        
   >        2、经常增删改的字段
   >        
   >        3、where条件里用不到的字段
   >        
   >        4、过滤性不好的字段
   >     
   > 6. 性能分析Explain
   >
   >     1. 是什么
   >     
   >        MySQL提供的，查看执行计划的关键字
   >        
   >        使用explain关键字，可以模拟优化器执行SQL语句，从而知道MySQL是如何处理你自己的SQL的。之后可以分析查询语句或表结构的瓶颈
   >        
   >     2. 能干什么
   >     
   >        1、表的读取顺序
   >        
   >        2、哪些索引可以使用
   >        
   >        3、数据读取操作的操作类型
   >        
   >        4、哪些索引实际使用到
   >        
   >        5、表之间的引用
   >        
   >        6、每张表有多少行被物理查询
   >        
   >     3. 怎么用
   >     
   >        在自己的SQL语句前，加入explain关键字即可：```explain SQL语句```
   >        
   >     4. explain关键字使用分析：主要分析性能优化相关字段，全部字段的信息参见思维导图
   >     
   >        使用explain执行SQL后，会返回执行计划的信息，该信息一共包含10列，我们着重关注其中几个对性能优化更大的字段。
   >        
   >         * id字段：SQL执行顺序。
   >        
   >            id字段相同，执行顺序从上到下；
   >            
   >            id字段不同，说明有子查询，id序号会递增，id值越大，优先级越高，越先执行；
   >            
   >            id字段有相同，有不同，先按照id值由大到小执行，然后id值相同的从上而下执行
   >            
   >            **每个不同的id号，都表示一趟查询。一个SQL保证查询趟数越少越好**
   >            
   >         * type字段：显示查询使用了何种类型。对于优化非常重要，详细参见思维导图，这里只写几个最关键的
   >        
   >            all类型：全表扫描。效果最差
   >            
   >            index类型：SQL使用了索引，但是没有根据索引进行where过滤。效果倒数第六
   >            
   >            range类型：SQL使用了范围限定，一般是在where后有```<```、```>```、```between```、```in```等。效果第五
   >            
   >            ref类型：非唯一索引扫描，本质也是一种索引访问。效果第四
   >            
   >            eq_ref类型：唯一索引扫描。效果第三
   >            
   >            const类型：表示通过索引一次就可以查询到，const类型通常出现在where后主键索引或唯一索引字段的比较上，例如where id=1、where name='xxx'等。效果第二好
   >            
   >            system类型：表中只有一条记录。效果最好，但是一般不可能出现。
   >            
   >            效率从高到底：system、const、eq_ref、ref、range、index、all
   >            
   >            **一般来说，要保证查询至少达到range级别，最好达到ref级别**
   >            
   >         * key_len字段：where过滤条件命中索引个数。个数越大越好
   >        
   >         * rows字段：物理扫描的行数。越少越好
   >        
   >         * Extra字段：很重要的额外信息。这里主要列出必须优化的额外信息
   >        
   >             * Using filesort：使用了文件排序。**order by 没有用到索引会出现，导致查询效率非常差，必须优化**
   >             
   >             * Using temporary：使用了。**group by没用到索引会出现，导致查询效率非常非常差，必须优化**。
   >             
   >                因为group by会先order by一次，因此会出现Using temporary和Using filesort，效果叠加效率令人发指
   >                
   >             * using join buffer：**两个表关联的关联字段没有用索引会出现，导致查询效率非常差，必须优化**
   >             
   >             * impossible where：where后过滤条件不可能存在，说明SQL直接错误
   >             
## 7. 面试真题：往一张表中插入100万条数据，如何设计SQL让查询效率最高
7. 面试真题：往一张表中插入100万条数据，如何设计SQL让查询效率最高
   > 1. 只使用一条Insert into语句插入全部数据
   > 
   > 2. 因为索引会让写操作变慢，因此先将表中的除了主键之外索引全部删除
   > 
   > 3. 利用数据库事务操作，不让数据库自动commit，而是全部插入后，手动commit一次
   > 
   > 4. Java多线程来优化
   > 
   > ...
   > 
## 8. 数据库表中索引的删除
8. 数据库表中索引的删除
   > 1. 删除索引，要分成如下几个步骤
   > 
   >     1. 查询索引
   >     
   >        MySQL中的information_schema库中，存储了其他所有表的元数据。数据库表的索引信息，就在information_schema库的STATISTICS表中。
   >        
   >     2. 取出索引名
   >     
   >        通过SQL编程，将所有索引变成游标，遍历游标取出索引名
   >        
   >     3. 拼接SQL字符串和索引名形成删除索引的SQL字符串，然后让删除索引的SQL字符串变成真正的SQL
   >     
   >        通过SQL编程，预编译SQL字符串，之后执行预编译后的SQL字符串
   >        
## 9. 索引优化
9. 索引优化
   > 1. 单表索引优化
   >
   >     1. 案例：参见思维导图
   >
   >     2. 总结
   >
   >         1. where后有几个字段，建立几个字段的索引
   >         
   >         2. 查询语句中，and前后顺序不会影响索引的使用，因为MySQL中有优化器，优化的结果都是一样的
   >         
   >         3. 复合索引是分层的，复合索引的命中，是根据最佳左前缀法则来命中的
   >         
   >             * **最佳左前缀法则：如果索引了多列，要遵守最左前缀法则。指的是查询从索引的最左前列开始并且不跳过索引中的列。**
   >             
   >         4. **索引失效：**
   >         
   >             1. 情况一
   >         
   >                where后的索引字段，给索引的字段，加入计算、函数、自动或手动类型转化，会导致索引失效，转为全表扫描。
   >               
   >                严禁对where后的索引字段使用函数、计算、自动或手动类型转化
   >               
   >             2. 情况二
   >         
   >                存储引擎无法使用范围查询的索引右边的索引字段。
   >         
   >                例如有个复合索引idx_name_age_deptId_salary，如果where后对age使用了范围查询(```<、>、between等```)，age后的索引会全部失效
   >               
   >                因此，如果复合索引中，有范围查询的索引，应该放在最右边
   >                
   >             3. 情况三
   >             
   >                对于使用了不等于(```!= 或 <>```)判断的索引字段，例如where emp.name != 'zhangsan'，会导致索引字段索引失效
   >                
   >                如果项目经理一定要求增加不等于判断呢？先沟通，告诉他加可以，但是查询速度会变慢很多；如果他坚持要这个需求，必须让他发送邮件、信息，留下证据
   >                
   >             4. 情况四
   >             
   >                对于使用了is not null修饰的索引字段，例如where emp.name is not null，也会造成索引失效
   >                
   >                但是，使用is null修饰，不会造成索引失效
   >                
   >             5. 情况五
   >             
   >                对于like模糊匹配，只要使用了通配符%开头，例如where emp.name like '%sa_n%'，会造成索引失效
   >                
   >             6. 情况六
   >             
   >                对于字符串不使用单引号情况，例如where emp.name = 123123，会造成索引失效
   >                
   >                这其实就是MySQL的自动类型转换，导致的索引失效
   >                
   >                工作中的案例就是，如果Java类中的属性为Integer，数据库中字段为String，从数据库中查询时，就会导致索引失效。因此我们规定，数据库字段和Java类中属性，类型必须完全一致
   >             
   >         3. 一般性建议
   >         
   >             * 对于单键索引，尽量选择针对当前query过滤性更好的索引
   >             
   >                 * 过滤性更好指的是更精确的筛选，例如id、time等都是比较好的，而sex就是过滤性不好的
   >             
   >             * 在选择组合索引的时候，当前Query中过滤性最好的字段在索引字段顺序中，位置越靠前越好。
   >             
   >             * 在选择组合索引的时候，尽量选择可以能够包含当前query中的where字句中更多字段的索引
   >             
   >             * 在选择组合索引的时候，如果某个字段可能出现范围查询时，尽量把这个字段放在索引次序的最后面
   >             
   >             * 书写sql语句时，尽量避免造成索引失效的情况
   >
   > 2. 关联、子查询索引优化
   > 
   >     1. 关联查询的先后顺序
   >     
   >        关联查询是，遍历第一个表，然后根据第一个表的数据，关联到第二个表。
   >        
   >        第一个表叫做驱动表，第二个表叫做被动表
   >        
   >        驱动表的全表扫描无法避免，索引优化效果有限；被动表可以通过索引，大幅提升优化效果
   >        
   >        对于left join的外连接，左边是驱动表，右边是被驱动表
   >        
   >        对于inner join的内连接，驱动表和被驱动表由MySQL自动选择，MySQL选择有索引的表为被驱动表，没有的为驱动表
   >        
   >     2. 关联查询，如何优化
   >     
   >        1、保证join on 的字段有索引
   >        
   >        2、索引所在的表作为被驱动表；
   >        
   >        3、数据少的表，作为驱动表
   >        
   >     3. 关联查询+子查询时，如何优化
   >     
   >        1、能不用子查询，就不用子查询，因为子查询会增加SQL趟数
   >        
   >        2、子查询的查询后的表为虚拟表，无法建立索引。因此，子查询不要放在被驱动表的位置
   >        
   >     4. 避免不了子查询，如何对子查询优化
   >     
   >        尽量不要使用is not null、not exists等导致索引失效的情况，使用取差集、并集等方式来优化
   >     
   > 3. 排序、分组优化
   > 
   >     1. order by排序优化
   >     
   >        想要通过索引优化：
   >        
   >        1、必须有过滤条件，如where、limit
   >        
   >        2、必须保证过滤条件的字段、order by后的字段都有索引，同时因为优化器不会优化oder by后字段的顺序，因此必须保证order by后字段索引顺序一致
   >        
   >        3、必须保证order by后索引字段，同时是DESC或ASC的
   >        
   >     2. order by尽量避免使用Using filesort。
   >     
   >         * 如果有如下的SQL：```select * from emp where age = 20 and salary < 10000 order by deptId;```，如何建立索引和索引的选择
   >         
   >            上述我们可以建立两套索引，一套(age, salary)，一套(age, deptId)
   >            
   >            (age, salary)无法避免Using filesort，(age, deptId)可以避免Using filesort
   >            
   >            这两套索引MySQL会自动选择最为适合的索引来查询。因此不一定避免使用Using filesort的索引是最好的
   >         
   >     3. Using filesort，filesort算法在MySQL中有两种。单路排序和双路排序
   >     
   >        双路排序出现较早，比较慢
   >        
   >        单路排序出现较晚，比较快
   >        
   >     4. group by优化
   >     
   >        优化原则和order by几乎一样。
   >        
   >        只有一个区别：没有过滤条件，使用group by，也可以用到索引
   >     
   > 4. 最后使用索引的手段：覆盖索引。这种手段只能说是聊胜于无的索引优化
   > 
   >    覆盖索引在，过滤条件无法使用到索引的情况下使用到
   >    
   >    覆盖索引，就是在select后写上具体查询的字段。只要字段有索引，可以用上覆盖索引
   >    
   > 5. 如果使用了inner join，然后explain后发现MySQL选择的驱动表和被驱动表不好，我们可以修改inner join为```straight_join```。
   > 
   >     * ```straight_join```：不要忘了中间有下划线
   >     
   >         1. 功能：直连，功能和inner join一样，不过驱动表和被驱动表的选择不交给MySQL，而是自己指定
   >           
   >         2. 适用条件：前后表的数量级关系不变
   >         
## 10. 8个SQL语句的编写
10. 8个SQL语句的编写
   > 1. 具体参见视频
   >
   > 2. SQL中的IF()函数，IF()中有三个参数，第一个是判断条件，第二个是为真时的展示，第三个为假时的展示
   >
   > 3. 详解第8个SQL：
   >
   >     * 要求：显示每个门派中，年龄第二大的人
   >     
   >     * 分析：1、这里涉及到了简单的SQL编程知识，有些超纲；2、我们可以这样分析，先按照门派内部根据age排好序；然后取每个部门age排名第2的人
   >     
   >     * 具体SQL：
   >     
   >         ```
   >         # 先定义两个SQL变量，rank代表排名；last_deptid代表最终门派id；last_age代表门派最大年龄，这是为了预防并列第一的情况
   >         SET @rank=0;
   >         SET @last_deptId=0;
   >         SET @last_age=0;
   >         
   >         # :=在SQL编程中是赋值的意思；=在SQL中就是等于的意思
   >         
   >         select t_order.id, t_order.name, t_order.age
   >         from
   >         # 按照门派内部根据age排好序
   >         (select t.*, 
   >             IF(@last_deptId=deptId, 
   >                 IF(@last_age=age, @rank, @rank:=@rank+1),
   >                 @rank:=1
   >             ) as rk,
   >             @last_deptId:=deptId as last_deptId,
   >             @last_age:=age as last_age
   >         from t_emp t
   >         order by t.deptId, t.age DESC) t_order
   >         where t_order.rk = 2
   >         ;
   >         ```
   >         
## 11. 查询截取分析
11. 查询截取分析
    > 1. 并不是所有的SQL都需要进行索引优化，我们只需要对查询次数多和查询时间慢的SQL进行优化
    > 
    > 2. 慢查询日志
    > 
    >     1. 介绍
    >     
    >        慢查询日志，指的是MySQL提供的一种日志记录，用来记录在MySQL中响应时间超过阈值的语句，具体指的是运行时间超过long_query_time值的SQL，则会被记录到慢查询日志中。 
    >     2. 具体的详细介绍，参见思维导图
    >     
    > 3. 日志分析工具：参见思维导图
    > 
## 12. 视图
12. 视图
    > 1. 是什么
    > 
    >    将一个查询SQL封装为一个虚拟的表。
    >    
    >    虚拟表只会保存SQL逻辑，不会保存任何查询结果
    >    
    > 2. 作用
    > 
    >    1、封装复杂的SQL，提高复用性
    >    
    >    2、逻辑放在了数据库上，不用修改程序即可更新，面对频繁的改动更加灵活
    >    
    > 3. 适用场景
    > 
    >    例如报表的SQL，财务的SQL
    >    
    > 4. 语法和使用
    > 
    >     * 创建：```create view 视图名 as 具体的SQL```，视图名自己定义一般是```view_具体名称```格式
    >     * 使用：视图封装成功后，使用复杂查询SQL，只需要```select * from 视图名```即可
    >     * 更新：```create or repalce 视图名 as 具体SQL```，更新具体的视图
    >     
    > 5. MySQL5.5中，from后有子查询不允许创建视图；MySQL5.7后取消了该限制
    > 
## 13. 主从复制
13. 主从复制
    > 1. MySQL主从复制和Redis主从复制的异同
    >
    >     * 同：都是只能有一台主机，可以有多台从机；都是必须由从机slave主机
    >     * 异：Redis主从复制建立后，主机所有数据会先复制给从机，之后数据同步；MySQL主从复制建立后，主机之前数据不会给从机，建立接入点后的数据才会同步；
    >     * MySQL主从复制，必须要等主从复制搭建好后，再进行具体的创建数据库、创建表、添加数据等操作
    >     
    > 2. MySQL主从复制步骤和原理：这里列出基本步骤，具体参见思维导图
    >
    >     1. 步骤
    >     
    >        1、主机将写操作（增删改）的语句写入一个BinaryLog中
    >     
    >        2、从机申请读取BinaryLog
    >     
    >        3、主机的MySQL验证了从机身份后，将BinaryLog通过IO传给从服务器
    >     
    >        4、从服务器接收到BinaryLog后，通过IO写入从机的RelayLog
    >        
    >        5、从机的MySQL将RelayLog中的写操作进行执行
    >        
    >     2. MySQL主从复制特点：因为牵扯到多次IO操作，因此数据同步有轻微延时性
    >     
    > 3. 复制的基本原则
    >
    >     1. 每个slave必须只有一个master
    >     
    >     2. 每个slave只能有一个唯一的服务器ID
    >     
    >     3. 每个master可以有多个slave
    >     
    > 4. MySQL一主一从的搭建实例：具体步骤参见思维导图
    >
    >     * 注意：修改配置文件之前，建议先做个备份，然后修改备份文件；修改成功后看，再替换旧配置文件。
    >
    >     * 面试常问的MySQL主从复制配置：binlog_format，即binlog文件格式
    >
    >       * binlog_format有三种格式，一般使用默认的STATMENT即可
    >
    >         1. STATMENT：默认使用。
    >
    >            特点是：会记录所有的写操作的SQL，放入binlog。
    >
    >            缺点：如果主机中的SQL中有函数，例如主机SQL中有写入或更新当前时间的函数，传给从机时间SQL效果肯定不一致
    >
    >         2. ROW：
    >         
    >            特点是：不记录SQL放入binlog，而是记录执行一个SQL后每一行的改变。
    >            
    >            好处：解决了STATMENT数据可能不一致的问题
    >            
    >            缺点：如果整表或者大范围的记录改变，则ROW的效率会很低
    >            
    >         3. MIXED：
    >         
    >            特点：取上述两个模式的折中。如果SQL中有函数，使用ROW的记录行变化；否则使用STATMENT的记录SQL
    >            
    >            缺点：如果出现@@host name，获取主机名，即SQL中有获取系统变量的要求，则MIXED也会导致SQL不一致。
    >            
## 14. 分库分表
14. 分库分表
    > 1. 为什么要分库分表
    > 
    >    因为MySQL是有瓶颈的，单表瓶颈为500万条数据，单库瓶颈为5000万条数据
    >    
    >    如果超过瓶颈，就要分库分表了
    >    
## 15. Mycat
15. Mycat
    > 1. 介绍
    > 
    >     1. Mycat介绍
    >     
    >        Mycat是一个数据库中间件，它连接Java程序和数据库
    >        
    >        前身是阿里的cobar
    >        
    >         * 中间件：为两边建立联系的。例如Tomcat就是Web中间件，它为请求和具体的Java应用建立联系。
    >        
    >     2. Mycat能干什么
    >     
    >         1. 读写分离
    >         
    >         2. 数据库分片：分库分表。垂直拆分、水平拆分、垂直+水平拆分
    >            
    >         3. 多数据源整合
    >         
    >     3. 原理
    >     
    >        拦截
    >     
    > 2. Mycat安装启动：详细步骤参见思维导图
    > 
    > 3. Mycat登录：和登录MySQL方式一模一样，用户名和密码就是在安装时配置的用户名和密码
    > 
    >     * 管理员后台管理登录：```mysql -uxxx -pxxx -h xxxx -P 9066```，Mycat默认后台登录窗口端口号为9066
    >     
    >     * 数据窗口登录：```mysql -uxxx -pxxx -h xxx -P 8066```，Mycat默认数据窗口端口号为8066
    >     
    > 4. Mycat分库
    > 
    >     1. 如何选择分库的表
    >     
    >        分库后，不同库的表之间，无法进行连接
    >        
    >        因此不同库之间的表，必须保证不会进行连接查询
    >        
    >     2. 如何分库
    >     
    >        1、修改schema.xml文件
    >        
    >        2、根据需要创建多个干净的库
    >        
    >        3、登录Mycat，在新的库中，创建分给不同库的表
    >     
    > 5. Mycat分表
    > 
    >     1. 简单分表
    >     
    >         1. 修改schema.xml文件
    >     
    >         2. 修改rule.xml文件，在自带的规则外，添加新规则 
    >     
    >         3. 在对应的多个库中，创建分的表
    >     
    >         4. 启动Mycat，并登陆
    >     
    >         5. use 逻辑库，将原有表中数据重新插入，Mycat会自动帮我们将数据分到不同的表中 
    >         
    >     2. 跨库join问题
    >     
    >         1. 问题描述
    >         
    >            跨库无法进行关联查询
    >           
    >            因此如果两个原本就要进行连接查询的表，它们进行分表，要配置成ER表或全局表
    >            
    >         2. ER表：参见思维导图
    >         
    >         3. 全局表：参见思维导图
    >     
    > 6. 全局序列
    > 
    >     1. 因为分表分到不同的库，如果用普通的主键自增字段，可能会出现id重复的情况。因此，我们需要使用全局序列
    >     
    >     2. 三种全局序列的实现方式：参见思维导图
    >     
## 16. 经典面试问题：如果磁盘不够了，执行了很多delete删除多余的数据后，突然在MySQL运行过程中报错，问：MySQL报错信息为什么
16. 经典面试问题：如果磁盘不够了，执行了很多delete删除多余的数据后，突然在MySQL运行过程中报错，问：MySQL报错信息为什么
    > 1. 报错信息为：磁盘空间不足，因为delete会写入日志文件，剩余磁盘空间可能被delete语句的日志占满了
    > 
    > 2. 考察点：delete删除有回滚，delete的每条SQL执行后，都会写入到日志文件，为了日后可能的回滚，日志文件也会占用磁盘空间。truncate table不会回滚，不会写入日志文件。


