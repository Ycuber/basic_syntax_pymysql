=========================
三、使用Python3操作数据库
=========================
**3.1安装pymysql**
====================

-  使用pip命令安装pymysql

::

    pip install pymysql

.. image::  https://github.com/Ycuber/basic_syntax_pymysql/blob/master/img/7.png?raw=true

**3.2创建数据库**
==================

使用navicat创建数据库
 | 右键新建数据库并输入库名即可,这里我创建了名为school的数据库

.. image::  https://github.com/Ycuber/basic_syntax_pymysql/blob/master/img/8.png?raw=true

**3.3连接数据库**
==================

-  这里要用到pymysql.connect()来打开数据的连接，并用cursor()方法创建一个游标对象来接收游标

::

    import pymysql

    #打开连接
    link = pymysql.connect('localhost',user = "root",passwd = "20000926",db = "school", charset = "utf8")
    #获取游标
    cursor = link.cursor()
    
**3.4创建表**
==============

使用execute来执行SQL语句
 | execute(query,args=None)

 | 函数作用：执行单条的sql语句，执行成功后返回受影响的行数

 | 参数说明：

 | query：要执行的sql语句，字符串类型

 | args：可选的序列或映射，用于query的参数值。如果args为序列，query中必须使用%s做占位符；如果args为映射，query中必须使用%(key)s做占位

:: 
    
    import pymysql

    #打开连接
    link = pymysql.connect('localhost',user = "root",passwd = "20000926",db = "school", charset = "utf8")
    #获取游标
    cursor = link.cursor()

    #定义要执行的SQL语句,创建一个学生表 seuStudent
    sql = """
    CREATE TABLE seuStudent (
    id INT auto_increment PRIMARY KEY ,
    name CHAR(10) NOT NULL UNIQUE,
    age TINYINT NOT NULL
    )ENGINE=innodb DEFAULT CHARSET=UTF8MB4; 
    """

    #执行SQL语句
    cursor.execute(sql)
    #关闭游标
    cursor.close()
    # 关闭数据库连接
    link.close()
    
当执行完这段代码后，在navicat中刷新一下就可以看到我们刚才创建的表了。

.. image:: https://github.com/Ycuber/basic_syntax_pymysql/blob/master/img/9.png?raw=true


**3.5插入数据**
================

-  **插入单个数据**

*最后记得一定要用.commit()进行提交*

::
    
    import pymysql

    #打开连接
    link = pymysql.connect('localhost',user = "root",passwd = "20000926",db = "school", charset = "utf8")
    #获取游标
    cursor = link.cursor()

    sq1 = 'insert into seuStudent values(%s,%s,%s)'
    cursor.execute(sq1,(25,'byc',19))


    #另一种插入方法
    insert = cursor.execute("insert into seustudent values(22,'yy',18)")
    #这里的insert可以返回受影响的行数
    
    
    #关闭游标
    cursor.close()
    #提交数据
    link.commit()
    # 关闭数据库连接
    link.close()


可以看到表中多了两个数据。

.. image::  https://github.com/Ycuber/basic_syntax_pymysql/blob/master/img/10.png?raw=true

-  **插入多个数据**

*用到executemany(),与execute()类似*

::
    
    import pymysql

    #打开连接
    link = pymysql.connect('localhost',user = "root",passwd = "20000926",db = "school", charset = "utf8")
    #获取游标
    cursor = link.cursor()

    #定义要执行的SQL语句   
    sql = 'insert into seuStudent(id,name,age) values(%s,%s,%s)'
    data = [
        (23,'lh',19),
        (24,'ztx',18)
    ]
    #执行sql语句
    cursor.executemany(sql,data)
    
    
    #关闭游标
    cursor.close()
    #提交数据
    link.commit()
    # 关闭数据库连接
    link.close()


表中多了两个数据

.. image::  https://github.com/Ycuber/basic_syntax_pymysql/blob/master/img/11.png?raw=true

**3.6查询数据**
================

使用fetchone、fetchmany、fetchall三种方法提取数据

 | cursor.fetchone():获取游标所在处的一行数据，返回元组，没有则返回None

 | cursor.fetchmany(size):接受size行返回结果行。如果size大于返回的结果行的数量，则会返回全部数据。

 | cursor. fetchall():接收全部的返回结果行。


 | 在查询前必须先执行这一步cur.execute("select * from TABLE;")

::
    
    import pymysql

    #打开连接
    link = pymysql.connect('localhost',user = "root",passwd = "20000926",db = "school", charset = "utf8")
    #获取游标
    cursor = link.cursor()

    cursor.execute("select * from seuStudent;")
    res1 = cursor.fetchone()
    res2 = cursor.fetchmany(2)
    res3 = cursor.fetchall()
    
    #关闭游标
    cursor.close()
    #提交数据
    link.commit()
    # 关闭数据库连接
    link.close()


>>> print(res1)
(22, 'yy', 19)
>>> print(res2)
((22, 'yy', 19), (23, 'lh', 19))
>>> print(res3)
((22, 'yy', 19), (23, 'lh', 19), (24, 'ztx', 18), (25, 'byc', 19))



**3.7更新数据**
================

-  更新单条数据

::
    
    import pymysql

    #打开连接
    link = pymysql.connect('localhost',user = "root",passwd = "20000926",db = "school", charset = "utf8")
    #获取游标
    cursor = link.cursor()
    update = cursor.execute("update seuStudent set age = 20 where name = 'byc'")
    cursor.execute(select * from seuStudent where name = "byc";")
    res1 = cursor.fetchone()
    #关闭游标
    cursor.close()
    #提交数据
    link.commit()
    # 关闭数据库连接
    link.close()


>>> print(res1)
(25, 'byc', 20)




-  更新多条数据

::
    
    import pymysql

    #打开连接
    link = pymysql.connect('localhost',user = "root",passwd = "20000926",db = "school", charset = "utf8")
    #获取游标
    cursor = link.cursor()

    #定义SQL语句
    sql = "update seuStudent set age = %s where name = %s"
    data = [
        (18,'yy'),
        (21,'ztx'),
        (23,'lh')
    ]
    update = cursor.executemany(sql,data)
    cursor.execute('select * from seuStudent;')
    res1 = cursor.fetchmany(all)
    #关闭游标
    cursor.close()
    #提交数据
    link.commit()
    # 关闭数据库连接
    link.close()


>>> print(res1)
((22, 'yy', 18), (23, 'lh', 23), (24, 'ztx', 21), (25, 'byc', 20))

navicat中相应改变

.. image::  https://github.com/Ycuber/basic_syntax_pymysql/blob/master/img/12.png?raw=true


**3.8删除数据**
================

::
    
    import pymysql

    #打开连接
    link = pymysql.connect('localhost',user = "root",passwd = "20000926",db = "school", charset = "utf8")
    #获取游标
    cursor = link.cursor()

    cursor.execute('select * from seuStudent;')
    #删除单个数据
    cursor.execute("delete from seuStudent where id = 22")

    #定义SQL语句，删除多个数据
    sql = "delete from seuStudent where id = %s"
    id = [(23), (24)]
    cursor.executemany(sql,id)
    
    #关闭游标
    cursor.close()
    #提交数据
    link.commit()
    # 关闭数据库连接
    link.close()

可以看，表中只剩一个元素

.. image::  https://github.com/Ycuber/basic_syntax_pymysql/blob/master/img/13.png?raw=true




**至此，目前了解的有关python3操作MySQL数据库的基本操作都在上面了，如果以后学了更多会继续更新此文档**

时间:2020年6月17日