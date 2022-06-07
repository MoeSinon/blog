# 每日测验

* MySQL中常用查询关键字及作用
* 针对多表查询有哪些查询方式，各自有什么特点

# 昨日内容回顾

* 查询主要关键字

  ```python
  select distinct 字段1,字段2,... from 表名
  	where 分组之前的筛选条件
      group by  分组条件
      having  分组之后的筛选条件
      order by  排序字段1 asc,排序字段2 desc
      limit 5,5
  ```

* where

  ```python
  where id>=3 and id<=6;
  where id between 3 and 6;
  
  where salary=18000 or salary=17000;
  where salary in (18000,17000);
  where salary not in (18000,17000);
  
  where salary*20 from emp;
  
  # 模糊匹配 like
  """
  %  任意多个字符
  _  任意单个字符
  """
  where name like '%mode%';
  where name like '____';
  where char_length(name) = 4;
  
  # 针对null数据 判断的时候用is不要用=
  where post_comment is null;
  ```

* group by

  ```python
  # 分组的应用场景非常多 
  	每个
      平均
      最大
      最小
      ...
  """
  分组之后只能直接获取到分组的依据 其他字段都无法直接获取
  set global sql_mode = 'only_full_group_by'
  """
  select * from emp group by post;  报错
  select post from emp group by post;
  
  # group_concat:帮助我们获取到分组之外的字段信息并且可以拼接多个字段
  select post,group_concat(salary,':',name) from emp;
  # concat:分组之前帮助我们获取字段信息并且可以拼接多个字段
  select concat(name,'??') from emp;
  # concat_ws:如果多个字段之间的连接符号是相同的情况下 你可以直接使用concat_ws来完成
  select concat_ws(':',name,age,sex) from emp;
  """
  复习
  '?'.join([111,222,333,444])  报错！
  """
  
  # as语法
  	1 可以给展示字段起别名
  	2 可以给表起别名
  
  # 聚合函数
  	max
      min
      sum
      count
      avg
    聚合函数必须在分组之后使用
  ```

* having

  ```python
  # 用法根where一模一样 只不过它是作用于分组之后的再次筛选
  ....group by post having avg(salary) > 30000;
  ```

* distinct

  ```python
  # 数据必须是一模一样的情况下才能去重 
  select distinct post from emp;
  ```

* order by

  ```python
  # 排序 默认是升序
  order by salary;  ===   order by salary asc;
  order by salary desc;
  
  order by salary asc,age desc;  # 还支持多个字段备用比较
  ```

* limit

  ```python
  """
  限制数据的展示条数    效果就是分页的效果
  """
  select * from emp limit 5;
  
  limit 5;
  limit 5,5  第一个参数是起始位置 第二个参数是条数
  ```

* regexp

  ```python
  """
  正则是一门独立的语言
  在python中如果你想使用正则需要借助于re模块
  	面试题
  		1.re模块中常用的方法
  			findall:分组优先展示
  				^j.*(n|y)$
  				不会展示所有正则表达式匹配到的内容
  				而仅仅展示括号内正则表达式匹配到的内容
  			match:从头匹配
  			search:从整体匹配
  		2.贪婪匹配与非贪婪匹配
  			正则表达式默认都是贪婪匹配的
  			将贪婪变成非贪婪只需要在正则表达式后面加?
  			.*  贪婪
  			.*? 非贪婪
  """
  select * from emp where name regexp '^j.*n$'
  ```

* 多表查询

  ```python
  # 联表操作
  select * from emp,dep;  笛卡尔积
  	inner join
      	只拼接两种表中都公有的部分
          select * from emp inner join dep 
          	on emp.dep_id = dep.id;
              # 要加上表的前缀 不然容易造成冲突 
      left join
      	左表数据全部展示  没有对应的就用NULL补全
      right join
      	右表数据全部展示  没有对应的就用NULL补全	
      union
      	左右全书展示 没有对应的就用NULL补全	
  # 子查询
  """
  子查询就是我们平时解决问题的思路 分步处理
  将一张表的查询结果当做另外一条sql语句的查询条件
  (当做条件的时候 用括号括起来)
  select * from emp where id in (select id from dep);
  """
  ```

* **总结**

  ```python
  # 书写sql语句的时候 select后面先用*占位 之后写完再改
  # 在写较为复杂的sql语句的时候 不要想着一口气写完 写一点查一点看一点再写！！！(只要是涉及到数据查询相关的语法都不应该一次性写完 不太现实)
  # 在做多表查询的时候 联表操作和子查询可能会结合使用
  ```

### 知识点补充

```python
# 查询平均年龄在25岁以上的部门名称
"""只要是多表查询 就有两种思路    联表    子查询"""
# 联表操作
	1 先拿到部门和员工表 拼接之后的结果
	2 分析语义 得出需要进行分组
    select dep.name from emp inner join dep
    	on emp.dep_id = dep.id
        group by dep.name
        having avg(age) > 25
        ;
	"""涉及到多表操作的时候 一定要加上表的前缀"""
# 子查询
	select name from dep where id in
		(select dep_id from emp group by dep_id 
    		having avg(age) > 25);

# 关键字exists(了解)
	只返回布尔值 True False
    返回True的时候外层查询语句执行
    返回False的时候外层查询语句不再执行
	select * from emp where exists 
    	(select id from dep where id>3);
        
        
   select * from emp where exists 
    	(select id from dep where id>300);
```

# 今日内容概要

* navicat可视化界面操作数据库
* 数据库查询题目讲解(多表操作)
* python如何操作MySQL(pymysql模块)
* sql注入问题
* pymysql模块增删改查数据操作

# 今日内容详细

### Navicat软件

```python
"""
一开始学习python的时候 下载python解释器然后直接在终端书写
pycharm能够更加方便快捷的帮助你书写python代码
excel word pdf

我们在终端操作MySQL 也没有自动提示也无法保存等等 不方便开发
Navicat内部封装了所有的操作数据库的命令 
用户在使用它的时候只需要鼠标点点即可完成操作 无需书写sql语句
"""
```

**安装**

```python
直接百度搜索 有破解版的也有非破解
非破解的有试用期 你如果不嫌麻烦 你就用使用
到期之后重新装再使用 或者破解一下也很简单
https://www.cr173.com/soft/126934.html
    
下载完成后是一个压缩包 直接解压 然后点击安装 有提醒直接点击next即可

navicat能够充当多个数据库的客户端


navicat图形化界面有时候反应速度较慢 你可以选择刷新或者关闭当前窗口再次打开即可

当你有一些需求该软件无法满足的时候 你就自己动手写sql

```

### 提示

```python
"""
1 MySQL是不区分大小写的
	验证码忽略大小写
		内部统一转大写或者小写比较即可
			upper
			lower

2 MySQL建议所有的关键字写大写

3 MySQL中的注释 有两种
	--
	#

4 在navicat中如何快速的注释和解注释
	ctrl + ？  加注释
	ctrl + ？  基于上述操作再来一次就是解开注释
	如果你的navicat版本不一致还有可能是
	ctrl + shift + ？解开注释
"""
```

### 练习题

```python
"""
课下一定要把握上课将的这几道题全部自己独立的理解并写出来

在解决sql查询问题的时候 不要慌
一步一步慢慢来  最终能够东拼西凑出来就过关了！！！

"""
-- 1、查询所有的课程的名称以及对应的任课老师姓名
-- SELECT
-- 	course.cname,
-- 	teacher.tname 
-- FROM
-- 	course
-- 	INNER JOIN teacher ON course.teacher_id = teacher.tid;

-- 4、查询平均成绩大于八十分的同学的姓名和平均成绩
-- SELECT
-- 	student.sname,
-- 	t1.avg_num 
-- FROM
-- 	student
-- 	INNER JOIN (
-- 	SELECT
-- 		score.student_id,
-- 		avg( num ) AS avg_num 
-- 	FROM
-- 		score
-- 		INNER JOIN student ON score.student_id = student.sid 
-- 	GROUP BY
-- 		score.student_id 
-- 	HAVING
-- 		AVG( num ) > 80 
-- 	) AS t1 ON student.sid = t1.student_id;


-- 7、 查询没有报李平老师课的学生姓名
# 分步操作
# 1 先找到李平老师教授的课程id
# 2 再找所有报了李平老师课程的学生id
# 3 之后去学生表里面取反 就可以获取到没有报李平老师课程的学生姓名
-- SELECT
-- 	student.sname 
-- FROM
-- 	student 
-- WHERE
-- 	sid NOT IN (
-- 	SELECT DISTINCT
-- 		score.student_id 
-- 	FROM
-- 		score 
-- 	WHERE
-- 		score.course_id IN ( SELECT course.cid FROM teacher INNER JOIN course ON teacher.tid = course.teacher_id WHERE teacher.tname = '李平老师' ) 
-- 	);

-- 8、 查询没有同时选修物理课程和体育课程的学生姓名
--     (只要选了一门的 选了两门和没有选的都不要)
# 1 先查物理和体育课程的id
# 2 再去获取所有选了物理和体育的学生数据
# 3 按照学生分组 利用聚合函数count筛选出只选了一门的学生id
# 4 依旧id获取学生姓名
-- SELECT
-- 	student.sname 
-- FROM
-- 	student 
-- WHERE
-- 	student.sid IN (
-- 	SELECT
-- 		score.student_id 
-- 	FROM
-- 		score 
-- 	WHERE
-- 		score.course_id IN ( SELECT course.cid FROM course WHERE course.cname IN ( '物理', '体育' ) ) 
-- 	GROUP BY
-- 		score.student_id 
-- 	HAVING
-- 		COUNT( score.course_id ) = 1 
-- 	);

-- 9、 查询挂科超过两门(包括两门)的学生姓名和班级
# 1 先筛选出所有分数小于60的数据
# 2 按照学生分组 对数据进行计数获取大于等于2的数据
SELECT
	class.caption,
	student.sname 
FROM
	class
	INNER JOIN student ON class.cid = student.class_id 
WHERE
	student.sid IN (
	SELECT
		score.student_id 
	FROM
		score 
	WHERE
		score.num < 60 GROUP BY score.student_id HAVING COUNT( score.course_id ) >= 2 
	);
```

### pymysql模块

```python
"""
支持python代码操作数据库MySQL
"""
pip3 install pymysql
```

### sql注入

```python
"""
利用一些语法的特性 书写一些特点的语句实现固定的语法
MySQL利用的是MySQL的注释语法
select * from user where name='jason' -- jhsadklsajdkla' and password=''

select * from user where name='xxx' or 1=1 -- sakjdkljakldjasl' and password=''
"""
日常生活中很多软件在注册的时候都不能含有特殊符号
因为怕你构造出特定的语句入侵数据库 不安全

# 敏感的数据不要自己做拼接 交给execute帮你拼接即可
# 结合数据库完成一个用户的登录功能？
import pymysql


conn = pymysql.connect(
    host = '127.0.0.1',
    port = 3306,
    user = 'root',
    password = '123456',
    database = 'day48',
    charset = 'utf8'  # 编码千万不要加-
)  # 链接数据库
cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)

username = input('>>>:')
password = input('>>>:')
sql = "select * from user where name=%s and password=%s"
# 不要手动拼接数据 先用%s占位 之后将需要拼接的数据直接交给execute方法即可
print(sql)
rows = cursor.execute(sql,(username,password))  # 自动识别sql里面的%s用后面元组里面的数据替换
if rows:
    print('登录成功')
    print(cursor.fetchall())
else:
    print('用户名密码错误')
```

**作业布置**

```python
"""
1 navicat自己玩一玩
2 练习题一定要搞懂 照着我的思路一遍遍的看敲
3 熟悉pymysql的使用
4 sql注入产生的原因和解决方法 了解
5 思考:如何结合mysql实现用户的注册和登录功能？
"""
```































