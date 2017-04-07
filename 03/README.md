#PHP技能评测
###1. 魔术函数有哪些,分别在什么时候调用?
__construct()，类的构造函数
__destruct()，类的析构函数
__call()，在对象中调用一个不可访问方法时调用
__callStatic()，用静态方式中调用一个不可访问方法时调用
__get()，获得一个类的成员变量时调用
__set()，设置一个类的成员变量时调用
__isset()，当对不可访问属性调用isset()或empty()时调用
__unset()，当对不可访问属性调用unset()时被调用。
__sleep()，执行serialize()时，先会调用这个函数
__wakeup()，执行unserialize()时，先会调用这个函数
__toString()，类被当成字符串时的回应方法
__invoke()，调用函数的方式调用一个对象时的回应方法
__set_state()，调用var_export()导出类时，此静态方法会被调用。
__clone()，当对象复制完成时调用

###2.isset和empty函数有什么区别?
PHP的isset()函数 一般用来检测变量是否设置
格式：bool isset ( mixed var [, mixed var [, ...]] )

功能：检测变量是否设置

返回值：

若变量不存在则返回 FALSE
若变量存在且其值为NULL，也返回 FALSE
若变量存在且值不为NULL，则返回 TURE
同时检查多个变量时，每个单项都符合上一条要求时才返回 TRUE，否则结果为 FALSE
版本：PHP 3, PHP 4, PHP 5
更多说明：
使用 unset() 释放变量之后，它将不再是 isset()。
PHP函数isset()只能用于变量，传递任何其它参数都将造成解析错误。
检测常量是否已设置可使用 defined() 函数。

PHP的empty()函数 判断值为否为空

格式：bool empty ( mixed var )

功能：检查一个变量是否为空

返回值：

若变量不存在则返回 TRUE
若变量存在且其值为""、0、"0"、NULL、、FALSE、array()、var $var; 以及没有任何属性的对象，则返回 TURE
若变量存在且值不为""、0、"0"、NULL、、FALSE、array()、var $var; 以及没有任何属性的对象，则返回 FALSE
版本：PHP 3, PHP 4, PHP 5
更多说明：
empty()的返回值=!(boolean) var，但不会因为变量未定义而产生警告信息。参见转换为布尔值获取更多信息。
empty() 只能用于变量，传递任何其它参数都将造成Paser error而终止运行。
检测常量是否已设置可使用 defined() 函数。

###3.PHP的与定义变量有哪些,分别是什么?
超全局变量 — 超全局变量是在全部作用域中始终可用的内置变量
$GLOBALS — 引用全局作用域中可用的全部变量
$_SERVER — 服务器和执行环境信息
$_GET — HTTP GET 变量
$_POST — HTTP POST 变量
$_FILES — HTTP 文件上传变量
$_REQUEST — HTTP Request 变量
$_SESSION — Session 变量
$_ENV — 环境变量
$_COOKIE — HTTP Cookies
$php_errormsg — 前一个错误信息
$HTTP_RAW_POST_DATA — 原生POST数据
$http_response_header — HTTP 响应头
$argc — 传递给脚本的参数数目

PHP 中的许多预定义变量都是“超全局的”，这意味着它们在一个脚本的全部作用域中都可用。在函数或方法中无需执行 global $variable; 就可以访问它们。

这些超全局变量是：

$GLOBALS
$_SERVER
$_GET
$_POST
$_FILES
$_COOKIE
$_SESSION
$_REQUEST
$_ENV

###4.简述PHP的垃圾回收机制
php 5.3之前使用的垃圾回收机制是单纯的“引用计数”，也就是每个内存对象都分配一个计数器，当内存对象被变量引用时，计数器+1；当变量引用撤掉后，计数器-1；当计数器=0时，表明内存对象没有被使用，该内存对象则进行销毁，垃圾回收完成。

“引用计数”存在问题，就是当两个或多个对象互相引用形成环状后，内存对象的计数器则不会消减为0；这时候，这一组内存对象已经没用了，但是不能回收，从而导致内存泄露；

php5.3开始，使用了新的垃圾回收机制，在引用计数基础上，实现了一种复杂的算法，来检测内存对象中引用环的存在，以避免内存泄露。

php变量存在一个叫"zval"的变量容器中,"zval"变量容器包括含变量的类型和值，还包括额外的两个字节信息，分别是“is_ref”表示变量是否属于引用，“refcount”指向这个zval变量容器的变量个数。

###5.列举PHP的性能优化方法和技巧

opcache
通讯缓存
查询缓存

###6.MySQL存储引擎中,innodb和myisam的区别

MyISAM 和 InnoDB 讲解

InnoDB和MyISAM是许多人在使用MySQL时最常用的两个表类型，这两个表类型各有优劣，视具体应用而定。基本的差别为：MyISAM类型不支持事务处理等高级处理，而InnoDB类型支持。MyISAM类型的表强调的是性能，其执行数度比InnoDB类型更快，但是不提供事务支持，而InnoDB提供事务支持已经外部键等高级数据库功能。

以下是一些细节和具体实现的差别：

◆1.InnoDB不支持FULLTEXT类型的索引。

◆2.InnoDB 中不保存表的具体行数，也就是说，执行select count(*) from table时，InnoDB要扫描一遍整个表来计算有多少行，但是MyISAM只要简单的读出保存好的行数即可。注意的是，当count(*)语句包含 where条件时，两种表的操作是一样的。

◆3.对于AUTO_INCREMENT类型的字段，InnoDB中必须包含只有该字段的索引，但是在MyISAM表中，可以和其他字段一起建立联合索引。

◆4.DELETE FROM table时，InnoDB不会重新建立表，而是一行一行的删除。

◆5.LOAD TABLE FROM MASTER操作对InnoDB是不起作用的，解决方法是首先把InnoDB表改成MyISAM表，导入数据后再改成InnoDB表，但是对于使用的额外的InnoDB特性(例如外键)的表不适用。

另外，InnoDB表的行锁也不是绝对的，假如在执行一个SQL语句时MySQL不能确定要扫描的范围，InnoDB表同样会锁全表，例如update table set num=1 where name like “%aaa%”

两种类型最主要的差别就是Innodb 支持事务处理与外键和行级锁.而MyISAM不支持.所以MyISAM往往就容易被人认为只适合在小项目中使用。

我作为使用MySQL的用户角度出发，Innodb和MyISAM都是比较喜欢的，但是从我目前运维的数据库平台要达到需求：99.9%的稳定性，方便的扩展性和高可用性来说的话，MyISAM绝对是我的首选。

原因如下：

1、首先我目前平台上承载的大部分项目是读多写少的项目，而MyISAM的读性能是比Innodb强不少的。

2、MyISAM的索引和数据是分开的，并且索引是有压缩的，内存使用率就对应提高了不少。能加载更多索引，而Innodb是索引和数据是紧密捆绑的，没有使用压缩从而会造成Innodb比MyISAM体积庞大不小。

3、从平台角度来说，经常隔1，2个月就会发生应用开发人员不小心update一个表where写的范围不对，导致这个表没法正常用了，这个时候MyISAM的优越性就体现出来了，随便从当天拷贝的压缩包取出对应表的文件，随便放到一个数据库目录下，然后dump成sql再导回到主库，并把对应的binlog补上。如果是Innodb，恐怕不可能有这么快速度，别和我说让Innodb定期用导出xxx.sql机制备份，因为我平台上最小的一个数据库实例的数据量基本都是几十G大小。

4、从我接触的应用逻辑来说，select count(*) 和order by 是最频繁的，大概能占了整个sql总语句的60%以上的操作，而这种操作Innodb其实也是会锁表的，很多人以为Innodb是行级锁，那个只是where对它主键是有效，非主键的都会锁全表的。

5、还有就是经常有很多应用部门需要我给他们定期某些表的数据，MyISAM的话很方便，只要发给他们对应那表的frm.MYD,MYI的文件，让他们自己在对应版本的数据库启动就行，而Innodb就需要导出xxx.sql了，因为光给别人文件，受字典数据文件的影响，对方是无法使用的。

6、如果和MyISAM比insert写操作的话，Innodb还达不到MyISAM的写性能，如果是针对基于索引的update操作，虽然MyISAM可能会逊色Innodb,但是那么高并发的写，从库能否追的上也是一个问题，还不如通过多实例分库分表架构来解决。

7、如果是用MyISAM的话，merge引擎可以大大加快应用部门的开发速度，他们只要对这个merge表做一些select count(*)操作，非常适合大项目总量约几亿的rows某一类型(如日志，调查统计)的业务表。

当然Innodb也不是绝对不用，用事务的项目如模拟炒股项目，我就是用Innodb的，活跃用户20多万时候，也是很轻松应付了，因此我个人也是很喜欢Innodb的，只是如果从数据库平台应用出发，我还是会首选MyISAM。

另外，可能有人会说你MyISAM无法抗太多写操作，但是我可以通过架构来弥补，说个我现有用的数据库平台容量：主从数据总量在几百T以上，每天十多亿 pv的动态页面，还有几个大项目是通过数据接口方式调用未算进pv总数，(其中包括一个大项目因为初期memcached没部署,导致单台数据库每天处理 9千万的查询)。而我的整体数据库服务器平均负载都在0.5-1左右。

###7.Mysql的存储类型有哪几种?什么是聚簇索引非聚簇索引?

1、B+树索引(O(log(n)))：关于B+树索引，可以参考 MySQL索引背后的数据结构及算法原理

2、hash索引：
a 仅仅能满足"=","IN"和"<=>"查询，不能使用范围查询
b 其检索效率非常高，索引的检索可以一次定位，不像B-Tree 索引需要从根节点到枝节点，最后才能访问到页节点这样多次的IO访问，所以 Hash 索引的查询效率要远高于 B-Tree 索引
c 只有Memory存储引擎显示支持hash索引

3、FULLTEXT索引（现在MyISAM和InnoDB引擎都支持了）

4、R-Tree索引（用于对GIS数据类型创建SPATIAL索引）

从物理存储角度

1、聚集索引（clustered index）

2、非聚集索引（non-clustered index）

从逻辑角度

1、主键索引：主键索引是一种特殊的唯一索引，不允许有空值

2、普通索引或者单列索引

3、多列索引（复合索引）：复合索引指多个字段上创建的索引，只有在查询条件中使用了创建索引时的第一个字段，索引才会被使用。使用复合索引时遵循最左前缀集合

4、唯一索引或者非唯一索引

5、空间索引：空间索引是对空间数据类型的字段建立的索引，MYSQL中的空间数据类型有4种，分别是GEOMETRY、POINT、LINESTRING、POLYGON。MYSQL使用SPATIAL关键字进行扩展，使得能够用于创建正规索引类型的语法创建空间索引。创建空间索引的列，必须将其声明为NOT NULL，空间索引只能在存储引擎为MYISAM的表中创建

CREATE TABLE table_name[col_name data type]
[unique|fulltext|spatial][index|key][index_name](col_name[length])[asc|desc]
1、unique|fulltext|spatial为可选参数，分别表示唯一索引、全文索引和空间索引；

2、index和key为同义词，两者作用相同，用来指定创建索引

3、col_name为需要创建索引的字段列，该列必须从数据表中该定义的多个列中选择；

4、index_name指定索引的名称，为可选参数，如果不指定，MYSQL默认col_name为索引值；

5、length为可选参数，表示索引的长度，只有字符串类型的字段才能指定索引长度；

6、asc或desc指定升序或降序的索引值存储

###8.Memcache和Redis的过期机制是什么?什么是一致性哈希?

数据存储方式：Slab Allocation

数据过期方式：Lazy Expiration + LRU

Slab Allocator的基本原理是按照预先规定的大小，将分配的内存分割成特定长度的块，以完全解决内存碎片问题。

Slab Allocation的原理相当简单。 将分配的内存分割成各种尺寸的块（chuk），并把尺寸相同的块分成组（chunk的集合）

Page：分配给Slab的内存空间，默认是1MB。分配给Slab之后根据slab的大小切分成chunk。

Chunk：用于缓存记录的内存空间。

Slab Class：特定大小的chunk的组。

memcached根据收到的数据的大小，选择最适合数据大小的slab。

memcached中保存着slab内空闲chunk的列表，根据该列表选择chunk，然后将数据缓存于其中。

Slab Alloction 缺点

这个问题就是，由于分配的是特定长度的内存，因此无法有效利用分配的内存。例如，将100字节的数据缓存到128字节的chunk中，剩余的28字节就浪费了。

数据过期方式

Lazy Expiration

memcached内部不会监视记录是否过期，而是在get时查看记录的时间戳，检查记录是否过期。这种技术被称为lazy（惰性）expiration。因此，memcached不会在过期监视上耗费CPU时间

LRU

memcached会优先使用已超时的记录的空间，但即使如此，也会发生追加新记录时空间不足的情况，此时就要使用名为 Least Recently Used（LRU）机制来分配空间。这是删除“最近最少使用”的记录的机制。因此，当memcached的内存空间不足时（无法从slab class 获取到新的空间时），就从最近未被使用的记录中搜索，并将其空间分配给新的记录

大家常常说 memcached命中率低也是LRU策略引起的。大家可能常常遇到当我的内存足够大的时候，为何还会触发LRU那。因为LRU 是针对SLAB 来说的。

例如：我存储的数据在100K 左右。内部会选择适合大小的SLAB，这时候他会选择合适他大小的，他会选择上图的SLBA CLASS 2. 如果这时候SLAB CLASS 2 满了或者不足100K。他就会调用LRU机制。会把SLAB CLASS 2 中chunck中最近很少使用的数据清理掉，导致数据被清理掉，即使它没有过期。

所以使用memcached 时候 一定要注意数据大小匹配模式和增长因子。

REDIS 过期时间机制

1.volatile-lru:从设置了过期时间的数据集中，选择最近最久未使用的数据释放

2.allkeys-lru:从数据集中(包括设置过期时间以及未设置过期时间的数据集中)，选择最近最久未使用的数据释放

3.volatile-random:从设置了过期时间的数据集中，随机选择一个数据进行释放

4.allkeys-random:从数据集中(包括了设置过期时间以及未设置过期时间)随机选择一个数据进行入释放

5.volatile-ttl：从设置了过期时间的数据集中，选择马上就要过期的数据进行释放操作

6.noeviction：不删除任意数据(但redis还会根据引用计数器进行释放呦~),这时如果内存不够时，会直接返回错误

默认的内存策略是noeviction，在Redis中LRU算法是一个近似算法，默认情况下，Redis随机挑选5个键，并且从中选取一个最近最久未使用的key进行淘汰，在配置文件中可以通过maxmemory-samples的值来设置redis需要检查key的个数,但是检查的越多，耗费的时间也就越久,但是结构越精确(也就是Redis从内存中淘汰的对象未使用的时间也就越久~), 设置多少，具体业务权衡吧一般都是按照默认。

REDIS 还有定期策略，定期删除过期的缓存数据，来节省内存。这种方式还是蛮好的。这种策略优先于LRU。

目前对比MEMCACHED 和REDIS 过期时间机制对比。

REDIS 命中率明显高于MEMCACHED，对于业务适合哪种场景，大家各自匹配吧！目前来说 我认为REDIS 是 MEMCACHED 补充的一款NOSQL 产品。

一致性哈希,一种分布式节点key分布算法,可选;

###9.MySQL索引底层数据结构是怎样存储的,为什么使用索引会查询的快?

数据结构及算法基础

索引的本质

B-Tree和B+Tree

特殊的存储结构,寻道成本低;

MySQL索引实现

MyISAM索引实现
非聚簇索引

InnoDB索引实现
聚簇索引

第一个重大区别是InnoDB的数据文件本身就是索引文件。

第二个与MyISAM索引的不同是InnoDB的辅助索引data域存储相应记录主键的值而不是地址。

聚集索引这种实现方式使得按主键的搜索十分高效，但是辅助索引搜索需要检索两遍索引：首先检索辅助索引获得主键，然后用主键到主索引中检索获得记录。

###10.优化mysql的方法

避免复查查询
避免模糊查询
避免数据库内运算
避免大量吞吐

尽可能缩小检索范围
尽可能使用索引,唯一或者接近唯一的索引
联查中尽量使用const字段

###11.find 和 grep的区别

find是查找文件
grep是查找文件内的内容

###12.写出下列的服务的用途和默认端口

FTP： | 21/tcp | 依照FTP协议提供服务,专门用来传输文件的协议。
SSH: | 22/tcp | 专为远程登录会话和其他网络服务提供安全性的协议。利用 SSH 协议可以有效防止远程管理过程中的信息泄露问题。
HTTP: | 80/tcp | 是互联网上应用最为广泛的一种网络协议。所有的WWW文件都必须遵守这个标准。
telnet:| 23/tcp | Telnet协议是TCP/IP协议族中的一员，是Internet远程登陆服务的标准协议和主要方式,它为用户提供了在本地计算机上完成远程主机工作的能力
https：| 443/tcp 443/udp | 是以安全为目标的HTTP通道，简单讲是HTTP的安全版。即HTTP下加入SSL层，HTTPS的安全基础是SSL，因此加密的详细内容就需要SSL

###13.给text.txt文件除所有者之外增加执行权限,最终以数字写出文件权限

chmod g+x,o+x text.txt