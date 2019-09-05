# GROUP BY 与 COUNT 用法详解

## 聚合函数
- 在介绍GROUP BY 和 HAVING 子句前，我们必需先讲讲sql语言中一种特殊的函数：聚合函数， 例如SUM, COUNT, MAX, AVG等。这些函数和其它函数的根本区别就是它们一般作用在多条记录上。

- SELECT SUM(population) FROM bbc

- 这里的SUM作用在所有返回记录的population字段上，结果就是该查询只返回一个结果，即国家的总人口数。

## GROUP BY 用法
- Group By语句从英文的字面意义上理解就是“根据(by)一定的规则进行分组(Group)”。它的作用是通过一定的规则将一个数据集划分成若干个小的区域，然后针对若干个小区域进行数据处理。 

- 注意:group by 是先排序后分组； 

- 举例子说明：如果要用到group by 一般用到的就是“每这个字段” 例如说明现在有一个这样的表：每个部门有多少人 就要用到分组的技术

- select DepartmentID as '部门名称',

- COUNT(*) as '个数' from BasicDepartment group by DepartmentID12

- 这个就是使用了group by +字段进行了分组，其中我们就可以理解为我们按照了部门的名称ID，DepartmentID将数据集进行了分组；然后再进行各个组的统计数据分别有多少； 

- 通俗一点说：group by 字段1，字段2。。。(整个表中不止这两个字段)表示数据集中字段1相等，字段2也相等的数据归为一组，只显示一条数据。那么你可以对字段3进行统计（求和，求平均值等）

- 注意 

- select DepartmentID,DepartmentName from BasicDepartment group by DepartmentID

- 将会出现错误
  
- 选择列表中的列 ‘DepartmentName’ 无效，因为该列没有包含在聚合函数或 GROUP BY 子句中。这就是我们需要注意的一点，如果在返回集字段中，这些字段要么就要包含在Group By语句的后面，作为分组的依据；要么就要被包含在聚合函数中。为什么呢，根据前面的说明：DepartmentID相等的数据归为一组，只显示一条记录，那如果数据集中有这样三条数据。

            DepartmentID                              DepartmentName 
            dept001                                            技术部 
            dept001                                            综合部 
            dept001                                            人力部 

- 那我只能显示一条记录，我显示哪个？没法判断了。到这里有三种选择：

1. 把DepartmentName也加入到分组的条件里去（GROUP BY DepartmentID,DepartmentName），那这三条记录就是三个分组。

2. 不显示DepartmentName字段。

3. 用聚合函数把这三条记录整合成一条记录count(DepartmentName)



## WHERE和 HAVING

- HAVING子句可以让我们筛选成组后的各组数据。HAVING子句可以使用聚合函数 

- WHERE子句在聚合前先筛选记录．也就是说作用在GROUP BY 子句和HAVING子句前． WHERE字句中不能使用聚合函数 

- 举例说明： 

一、显示每个地区的总人口数和总面积．

SELECT region, SUM(population), SUM(area)

FROM bbc

GROUP BY region123

- 先以region把返回记录分成多个组，这就是GROUP BY的字面含义。分完组后，然后用聚合函数对每组中的不同字段（一或多条记录）作运算。
   
二、 显示每个地区的总人口数和总面积．仅显示那些面积超过1000000的地区。

SELECT region, SUM(population), SUM(area)

FROM bbc8 F4 w2 v( P- f

GROUP BY region

HAVING SUM(area)>10000001234

- 在这里，我们不能用where来筛选超过1000000的地区，因为表中不存在这样一条记录。相反，HAVING子句可以让我们筛选成组后的各组数据

- 需要注意说明：当同时含有where子句、group by 子句 、having子句及聚集函数时，执行顺序如下： 

- 执行where子句查找符合条件的数据； 

- 使用group by 子句对数据进行分组；对group by 子句形成的组运行聚集函数计算每一组的值；最后用having 子句去掉不符合条件的组。 

- having子句和where子句都可以用来设定限制条件以使查询结果满足一定的条件限制。 

- having子句限制的是组，而不是行。where子句中不能使用聚集函数，而having子句中可以。



## GROUP BY 用法 实例

- group by语法可以根据给定数据列的每个成员对查询结果进行分组统计，最终得到一个分组汇总表。

- SELECT子句中的列名必须为分组列或列函数。列函数对于GROUP BY子句定义的每个组各返回一个结果。

- 某个员工信息表结构和数据如下：

| id | name   | dept_id | salary | level | hiredate   |
|-|-|-|-|-|-|
|  1 | 张三   |       1 | 6666   |     3 | 2009-10-11 |
|  2 | 李四   |       1 | 2500   |     3 | 2009-10-01 |
|  3 | 王五   |       2 | 2600   |     3 | 2009-10-02 |
|  4 | 王六   |       2 | 2300   |     5 | 2005-10-02 |
|  5 | 大象   |       3 | 2800   |     2 | 2000-10-02 |
|  6 | 松鼠   |       3 | 4800   |     1 | 2003-10-02 |
|  7 | 斑马   |       1 | 4200   |     1 | 2006-10-02 |
|  8 | 豆芽   |       2 | 3333   |     2 | 2016-10-02 |
|  9 | 土豆   |       3 | 2233   |     3 | 2009-10-02 |



- 例如，我想列出每个部门最高薪水的结果，sql语句如下：

- select `dept_id`, max( `salary` ) as max_money  from `group` group by `dept_id`;

- 查询结果如下：

| dept_id | max_money |
|-|-|
|       1 | 6666      |
|       2 | 3333      |
|       3 | 4800      |


- 解释一下这个结果：

- 1、满足“SELECT子句中的列名必须为分组列或列函数”，因为SELECT有GROUP BY DEPT中包含的列DEPT。

- 2、“列函数对于GROUP BY子句定义的每个组各返回一个结果”，根据部门分组，对每个部门返回一个结果，就是每个部门的最高薪水。

- 注意：计算的是每个部门（由 GROUP BY 子句定义的组）而不是整个公司的 MAX(SALARY)。

- 例如，查询每个部门的总的薪水数

- select `dept_id`, sum( `salary` ) as total  from `group` group by `dept_id`;

- 查询结果如下：

| dept_id | total |
|-|-|
|       1 | 13366 |
|       2 |  8233 |
|       3 |  9833 |


## 将 WHERE 子句与 GROUP BY 子句一起使用

- 分组查询可以在形成组和计算列函数之前具有消除非限定行的标准 WHERE 子句。必须在GROUP BY 子句之前指定 WHERE 子句。

- 例如，查询公司2010年入职的各个部门每个级别里的最高薪水

- SELECT `dept_id`, `level`, MAX(`salary`) AS MAXIMUM FROM `group`  WHERE HIREDATE > '2010-01-01' GROUP BY `dept_id`, `level` ORDER BY `dept_id`, `level`;

- 查询结果如下：

| dept_id | level | MAXIMUM |
|-|-|-|
|       2 |     2 | 3333    |

- 注意：在SELECT语句中指定的每个列名也在GROUP BY子句中提到。未在这两个地方提到的列名将产生错误。

- GROUP BY子句对DEPT和EDLEVEL的每个唯一组合各返回一行。

## 在GROUP BY子句之后使用HAVING子句

- 可应用限定条件进行分组，以便系统仅对满足条件的组返回结果。为此，在GROUP BY子句后面包含一个HAVING子句。HAVING子句可包含一个或多个用AND和OR连接的谓词。每个谓词将组特性（如AVG(SALARY)）与下列之一进行比较：

- 例如：寻找雇员数超过2个的部门的最高和最低薪水：

- select `dept_id`,  max(`salary`) as MAXIMUM, min(`salary`)  as MINIMUM from `group` group by `dept_id` having count(*) > 2 order by `dept_id`;

- 查询结果如下：

| dept_id | MAXIMUM | MINIMUM |
|-|-|-|
|       1 | 6666    | 2500    |
|       2 | 3333    | 2300    |
|       3 | 4800    | 2233    |

- 例如：寻找雇员平均工资大于3000的部门的最高和最低薪水：

- select `dept_id`, max(`salary`) as MAXIMUM, min(`salary`)  as MINIMUM from `group` group by `dept_id` having avg(`salary`) > 3000 order by `dept_id`;

- 查询结果如下：

| dept_id | MAXIMUM | MINIMUM |
|-|-|-|
|       1 | 6666    | 2500    |
|       3 | 4800    | 2233    |

--------------------- 
作者：UFO

原文：https://github.com/lidawei-ufo/MYSQL/blob/master/group%20by%20%E7%94%A8%E6%B3%95.md

版权声明：本文为博主原创文章，转载请附上博文链接！
