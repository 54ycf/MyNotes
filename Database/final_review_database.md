# Intro

* **DB**是数据的集合，存储在磁盘中
* DBS**数据库系统**是针对一个用户的数据管理需求开发的应用系统，包括需要管理的**数据**、访问数据的应用**程序**以及支撑方便和高效访问数据的**运行环境**
* DBMS称作**数据库管理系统**，是一个基础软件，这个软件专门用来**存储和管理数据**库的，提供了一个方便有效使用的环境

## 文件系统实现DBM的不足

* 数据的冗余和不一致
  * 多种文件格式，不同文件汇总信息重复
* 数据访问困难
  * 需要编写一个新程序来执行每个新任务
* 数据孤立--数据分散在不同文件中多个文件和格式
* 完整性问题
  * 完整性约束在程序代码中变得“埋没”，而不是明确说明
  * 难以添加新约束或更改现有约束
* 原子性问题
* 并发访问异常
* 安全性问题

## 数据视图

面对数据的时候，不同的**抽象层次**看到的数据表现形式不一样。分层抽象的方法可以在不同层次关注不同的细节

* 从视图的角度数据库包括三层：
  * **物理层**
    * 描述数据是**怎样**存储的
  * **逻辑层**
    * 描述数据库中存储**什么**数据以及数据间的关系
  * **视图层**
    * 隐藏数据类型的细节，最高层抽象

## Instance & Schemas

* **实例**指某个时刻数据库中的实际数据
* 数据库的总体设计称作数据库**模式**
* 物理独立性：修改数据库的物理模式不会影响数据库的逻辑模式的一种能力。数据库应用依赖的是逻辑模式

## Data Models

* **数据模型**是描述数据、数据联系、数据语义以及一致性约束的概念工具的集合
* 常用数据模型
  * 关系模型
  * E-R模型
  * 基于对象的数据模型
  * 半结构化数据模型

## Database Language

* DDL（Data Definition Language）数据定义语言：定义数据库模式
  * 约束？
  * 输出放在数据字典，数据字典包含了元数据
* DML（Data Manipulation Language）数据操纵语言：表达数据库的查询和更新
  * 理解两类语言：纯语言和商用语言/过程语言和非过程语言
* SQL：称作结构化查询语言，但是不仅仅执行查询，包括定义数据库模式、修改数据库以及说明安全约束等都可以完成
  * 非过程语言

## 数据库体系结构

<img src="final_review_database.assets\image-20220826100812153.png" alt="image-20220826100812153" style="zoom:80%;" />



# Relational Model

* 关系有名字
* 每个关系可以看做一张**二维表**，表达了一个数据对象或者数据和数据之间的关系。每一列表示数据的**属性**，用名字区分，图中蓝色部分示意。每一行是一个**数据记录**，也可称作**元组**tuple。看这张表时，从横向和纵向两个维度看
  <img src="final_review_database.assets\image-20220826103050766.png" alt="image-20220826103050766" style="zoom:67%;" />

## 属性

* 属性有类型，描述的是每个属性允许取值的范围
* 属性值是原子不可分的
* 属性可以取一个特殊的值：null

## 模式&实例

* 实例：数据库中数据在给定时间点的快照
* **模式**：数据库的逻辑结构
* **关系模式**是属性的**集合**
* 关系r的一个元素t就是一个元组，记录，行

## Keys

* **超**码super key：一个或多个的属性组合在一个关系中唯一地表示一个元组
* **候选**码candidate key：**最小**超码
* **主**码primary key：被**选中**的候选码
* **外**码foreign key：一个关系中的属性可能出现在另一个关系中

## Schema Diagram

<img src="final_review_database.assets\image-20220826142301674.png" alt="image-20220826142301674" style="zoom:70%;" />

## Relational Algebra

* select: $\sigma$
  * $\sigma_{dept\_name=Physics}(instructor)$
  * <img src="final_review_database.assets\image-20220826150144127.png" alt="image-20220826150144127" style="zoom: 80%;" /><img src="final_review_database.assets\image-20220826150207323.png" alt="image-20220826150207323" style="zoom:80%;" />

* project: $\Pi$
  * 对输入关系的所有行输出指定属性，去除重复元组

* union: $\cup$
* set-intersection: $\cap$
* set difference: –
  * 找到在一个关系中但不在另一个关系中的元组
  * r - s

* assignment: $\leftarrow$
* Cartesian product: x
  * 从两个输入关系中输出所有元组对

* natural join: ⋈
  * 从两个输入关系中输出在相同名字的所有属性上取值相同的元组对

* rename: $\rho$
  * 重命名运算符来引用关系代数表达式的结果
  * <img src="final_review_database.assets\image-20220826152036054.png" alt="image-20220826152036054" style="zoom:80%;" />  or  ![image-20220826152107045](final_review_database.assets\image-20220826152107045.png)

* Deletion：
  * $r \leftarrow r - E$
  * r是关系，E是关系代数查询

* Insertion：
  * $r \leftarrow r \cup E $

* Updating：
  * $r \leftarrow \Pi_{F_1,F_2...F_i}(r)$




# Entity-Relationship Model

* 用实体（Entity）和联系（Relationship）的术语来表达数据以及数据和数据之间的关系，还有约束等信息
* E-R模型用于数据库的**概念设计**阶段
* E-R模型三个基本概念：实体集、联系集、属性
## **实体**
* 是现实世界课区别于所有其它对象的事物或对象：例如特定的人，公司，车
* 实体具有属性：公司名、Id
* **实体集**是共享**相同属性**的相同类型的实体集合
* 和对象的区别：
  * 实体是静态的，没有针对实体的操作，只有属性描述
## **联系**
* 指多个实体之间的关联
* **联系集**是相同类型联系的集合，规范表达：n≥2个实体集上的数学关系
  * <img src="final_review_database.assets\image-20220826160529018.png" alt="image-20220826160529018" style="zoom: 33%;" /><img src="final_review_database.assets\image-20220826160536023.png" alt="image-20220826160536023" style="zoom: 50%;" />
* 联系集的**度**：表达了参与联系的实体集书目，常见二元联系、三元联系
    <img src="final_review_database.assets\image-20220826160727480.png" alt="image-20220826160727480" style="zoom:80%;" />
* 联系集的超码：参与实体集的主键的组合
    * 联系集多对多：主码是两方的主键的**并集**
    * 多对一：主键是**多方的主键**
    * 一对一：主键是两方的候选码中**任意一个**
## **属性**
* **简单**simple和**复合**composite属性：例如身份证号是简单属性，地址是符合属性
* 单值single-valued和多值multivalued属性：例如ID是单值属性，电话号码是多值属性、学位是多值属性
  * 一个特定实体都只有单独的一个值的属性
  * 对某个特定实体，一个属性可能对应一组值
* 派生属性：例如年龄属性可以由出生日期推导出来
* 删除属性冗余

## 约束

* **映射基数约束**Mapping Cardinality Constraints：实体集合中的实体通过联系集合能关联的实体个数
  <img src="final_review_database.assets\image-20220826163015049.png" alt="image-20220826163015049" style="zoom:67%;" />
  <img src="final_review_database.assets\image-20220826163056266.png" alt="image-20220826163056266" style="zoom:67%;" />
* **参与约束**Participation Constraints：实体集合中的实体参与联系时是否全体参与还是部分参与
  <img src="final_review_database.assets\image-20220826163857350.png" alt="image-20220826163857350" style="zoom:80%;" />

## E-R Diagram

* **矩形**表示实体集
* **菱形框**表示联系集
* 属性在矩形框罗列
* 主键用**下划线**标识
* 组合属性、多值属性和导出属性的表达，缩进的方式
  <img src="final_review_database.assets\image-20220826185440412.png" alt="image-20220826185440412" style="zoom: 50%;" />
* 具有属性的联系集
  <img src="final_review_database.assets\image-20220826185606769.png" alt="image-20220826185606769" style="zoom:80%;" />
* 基数约束：
  * 箭头所指实体集表示映射的“1”方，线段对应的实体集表示映射的“多”一方
  * one-to-one
    <img src="final_review_database.assets\image-20220826190156700.png" alt="image-20220826190156700" style="zoom:80%;" />
  * one-to-many。一名教师可以指导多名学生
    <img src="final_review_database.assets\image-20220826190214551.png" alt="image-20220826190214551" style="zoom:80%;" />
  * many-to-many
    <img src="final_review_database.assets\image-20220826190239250.png" alt="image-20220826190239250" style="zoom:80%;" />
  * 全参与用**双线**
  * 另一种表达：
    <img src="final_review_database.assets\image-20220826191624152.png" alt="image-20220826191624152" style="zoom:80%;" />
    一名学生只能被一位教师指导，一位教师可以指导多名学生
* Ternary Relationship自学
  <img src="final_review_database.assets\image-20220826191804145.png" alt="image-20220826191804145" style="zoom:67%;" />
* **弱实体**：没有主键的实体。弱实体集依赖于一个实际存在的确定的实体集
  * 强实体与弱实体的联系只能是1：1或1：N。弱实体参与联系时应该是“完全参与，因此弱实体的联系侧画成双线边
  * 若要唯一区分弱实体，需要其依赖的强实体的key+弱实体的区分符
  * <img src="final_review_database.assets\image-20220826201757299.png" alt="image-20220826201757299" style="zoom:80%;" />
* Diagram for university
  <img src="final_review_database.assets\image-20220826203101763.png" alt="image-20220826203101763" style="zoom:80%;" />



## Reduction to Relation Schemas

* 强实体：
  * 关系名即实体名，强实体集的主码就是生成的模式的主码
* 弱实体：
  * 关系名即实体名，属性外加所依赖的强实体的key，最终弱实体转换的关系的key为强实体key+discriminator。
  * A依赖B，还要在A上简历外码约束，参照B的主码
* 组合属性
  * 不为单独的符合属性创建一个属性，将组合属性的每一个部分作为关系模式的一个顺序性
* 多值属性
  * 对于多值属性M，构建一个**新的关系模式**R，包含对应M的属性A，以及对应M所在实体集/联系集的主码
* 联系集
  * 多对多：参与的实体集的**主码属性的并集**成为主码
  * 一对一：任选一个实体集的主码
    * 无需转化为一个关系，只需在一个实体中加入外键
  * 多对一：选多的一方的实体集的主码
    * 无需转化为一个关系，将“1”方的key加入到“N”方中外键约束
  * n元联系集
    * 没有箭头：所有参与实体集的主码的并集
    * 有一个箭头：不在箭头侧的实体集的主码
  * 联系集有属性要额外加上

## Extended E-R Features

老师好像没讲？

## Translation Steps

<img src="final_review_database.assets\image-20220827003830283.png" alt="image-20220827003830283" style="zoom:80%;" />

1. 为每个实体创建表;包括单值属性。选择key
2.  为每个弱实体类型创建表;包括单值属性。在弱实体中将所有者的key作为外键包括在内。将 key 设置为所有者的外键加上本地部分键。
3.  对于每个 1：1 关系，向关系中涉及的某个实体添加一个外键（向关系中另一个实体添加一个外键）
4. 对于每个 1：N 关系，向关系 N 端的实体添加一个外键（以引用关系 1 端的实体）
5. 对于每个 M：N 关系，创建一个新表。在关系中包括每个参与者实体的外键。新表的键是所有此类外键的集合
6.  对于每个多值属性，构造一个单独的表。对此新表中的实体重复键。它既可以用作此表的键，也可以用作实体的原始表的外键

## Examples

<img src="final_review_database.assets\image-20220827004223237.png" alt="image-20220827004223237" style="zoom:80%;" />

<img src="final_review_database.assets\image-20220827004252547.png" alt="image-20220827004252547" style="zoom:80%;" />

<img src="final_review_database.assets\image-20220827004310422.png" alt="image-20220827004310422" style="zoom:80%;" />

<img src="final_review_database.assets\image-20220827004345426.png" alt="image-20220827004345426" style="zoom:80%;" />

<img src="final_review_database.assets\image-20220827004357755.png" alt="image-20220827004357755" style="zoom:80%;" />



# Relational Database Design

## First Normal Form

* 一个关系模式R的所有属性的域都是原子的（该域的元素是不可分的单元），比如CS00123就不是
* 反例
  <img src="https://img-blog.csdnimg.cn/20190304103038296.png#pic_center" alt="在这里插入图片描述" style="zoom:67%;" />
  应该为
  <img src="https://img-blog.csdnimg.cn/20190304103149949.png#pic_center" alt="在这里插入图片描述" style="zoom: 67%;" />

## Functional Dependency

* $\alpha$&$\beta$是属性集
* 满足**函数依赖**$ \alpha \rightarrow \beta$的条件：对实例中所有的元组对$t_1$和$t_2$，若$t_1[\alpha]=t_2[\alpha]$，则$t_1[\beta]=t_2[\beta]$
* 如果函数依赖K$\rightarrow$R在r(R)上成立，则K是r(R)的一个**super key**
* 如果K是**candidate key**，当且仅当K$\rightarrow$R 成立而且不存在a$\subset$K且a$\rightarrow$R成立
* trivial**平凡函数依赖**：$\alpha \rightarrow \beta$是平凡函数依赖，如果$\beta \subseteq \alpha$（右侧是左侧子集）
  * 如：ID, name $\rightarrow$ ID, name $\rightarrow$ name



**函数依赖理论**

* <img src="final_review_database.assets\image-20220827151550801.png" alt="image-20220827151550801" style="zoom:80%;" />
* <img src="final_review_database.assets\image-20220827151616537.png" alt="image-20220827151616537" style="zoom: 40%;" />
* 例如
  <img src="final_review_database.assets\image-20220827151801657.png" alt="image-20220827151801657" style="zoom:80%;" />

## Closure

给定一个函数依赖集合$F$，能够从$F$逻辑推断的的所有函数依赖的集合称作$F$的**闭包**，记做$F^+$

* 计算F+方法1
  <img src="final_review_database.assets\image-20220827152654003.png" alt="image-20220827152654003" style="zoom: 50%;" />
  初始化时F+=F ，通过不断应用公理的三个规则，经产生的新的函数依赖添加到F+中，直到F+中的元素不再变化
* **方法2**，先了解**属性集合的闭包**（肯定考概念、计算）
  <img src="final_review_database.assets\image-20220827153150370.png" style="zoom: 60%;" />
  * 如果$\alpha^+$包含了R的所有属性，则属性集合$\alpha$是superkey。
    * 如果是superkey，且$\alpha$的子集没有superkey，则是candidate key
  * **如果$\beta \subseteq \alpha^+$则函数依赖$\alpha \rightarrow \beta $成立**
  * 对R中的每一个属性$\gamma$，计算$\gamma^+$，然后对于属性闭包中的每一个元素S，输出函数依赖$\gamma \rightarrow S$

## Canonical Cover 正则覆盖

* **无关属性**：去除函数依赖中的一个属性，不改变函数依赖集的闭包，则是无关的extraneous。以$\alpha \rightarrow \beta$为例
  * $\alpha$中的无关属性A：
    * F 逻辑蕴含 $(F - \{\alpha \rightarrow \beta\}) \cup \{(\alpha - A)\rightarrow \beta\}$
    * 检测：计算$(\alpha - A)^+$，如果包含$\beta$，则A是无关属性
  * $\beta$中的无关属性A：
    * F 逻辑蕴含  $(F - \{\alpha \rightarrow \beta\}) \cup \{\alpha\rightarrow (\beta-A)\}$
    * 检测：利用上面的函数依赖$F'$计算$\alpha^+$，如果包含A则A是无关属性
  * 例如
    <img src="final_review_database.assets\image-20220827161346942.png" alt="image-20220827161346942" style="zoom:80%;" />
* **正则覆盖**：函数依赖集合$F$的正则覆盖$F_c$与$F$等价（两者互相逻辑蕴含）
  * 且$F_c$不包含无关属性
  * $F_c$中函数依赖的左侧是是**唯一**的（$F_c$中不存在两个依赖$\alpha_1 \rightarrow \beta_1$和$\alpha_2 \rightarrow \beta_2$满足$\alpha_1=\alpha_2$，不会一个函数依赖集合去决定多个beta）
  * <img src="final_review_database.assets\image-20220827175649122.png" alt="image-20220827175649122" style="zoom:80%;" />
    例：
    <img src="final_review_database.assets\image-20220827175738782.png" alt="image-20220827175738782" style="zoom: 50%;" />
  * 正则覆盖不一定唯一

## Lossless-join Decomposition

* R1和R2是R的**无损连接分解**，但且仅当以下两个函数依赖之一属于F+
  * $R_1 \cap R_2 \rightarrow R_1$
  * $R_1 \cap R_2 \rightarrow R_2$
  * 即，$R_1 \cap R_2 $是$R_1$或$R_2$的superkey
  * 就是拆开后再join不会数据不会变多
* 例如
  <img src="final_review_database.assets\image-20220827181037054.png" alt="image-20220827181037054" style="zoom:50%;" />

## Dependency Preservation

* **依赖保持**：令$F$是关系模式$R$上的一个函数依赖集，$R_1,R_2...R_n$为$R$的一个分解，$F_i$是只包含$R_i$属性的函数依赖集合
  * 如果$(F_1\cup F_2 \cup ... \cup F_n)^+ = F^+$，这个分解是依赖保持的
* 检测：
  <img src="final_review_database.assets\image-20220827183016154.png" alt="image-20220827183016154" style="zoom: 50%;" />

## Boyce-Codd Normal Form

* 一个关系模式**R**在给定的函数依赖集合**F**下，符合**BCNF**的条件是：对于$F^+$中所有的函数依赖$\alpha \rightarrow \beta$ *，*其中$\alpha \subseteq R $且$ \beta \subseteq R$，以下至少有一项成立：
  * $\alpha \rightarrow \beta$是平凡函数依赖
  * $\alpha$是R的一个super key
* 反例
  * 关系R的模式为R = (A, B, C)，函数依赖集合为F = {A*-->*B,B -->C}，Key = {A}。不符合：B-->C中B不是super key
* 分解
  * 设有一个关系模式R和函数依赖集合F，其中一个函数依赖a -->b 导致R不是BCNF，这时可以将R进行分解为两个子模式R1和R2：
    * **R1= (a U b)，R2= ( R - (b - a) )**
  * 例：某设计者设计的关系模式为*instr_dept* (*ID,* *name, salary,* *dept_name, building, budget* )，希望管理教师和院系的数据以及教师所在院系的信息。假设a= *dept_name*(不是key)，*b* = *building, budget，*F: {*dept_name* --> *building,budget*}，*instr_dept*不是BCNF。分解得到
    * R1 = (dept_name, building, budget), R2 = (ID, name, salary, dept_name)
* **最优先**选择 BCNF + 依赖保持。
* BCNF分解有可能会导致某些**函数依赖**不在一个子模式中，不能高效地检查一些函数依赖增加代价。当无法达到BCNF和依赖保持这个目标时，就考虑“弱”一点的3NF
* **检验**非平凡函数依赖$\alpha \rightarrow \beta$是否违反BCNF：
  * 计算$\alpha$的闭包
  * 检验闭包是否包含R。这就是R的superkey
  * 其实就是看函数依赖的左边是不是superkey
* 分解算法
  <img src="final_review_database.assets\image-20220827184012015.png" alt="image-20220827184012015" style="zoom:67%;" />
  * 例子
    <img src="final_review_database.assets\image-20220827192156182.png" alt="image-20220827192156182" style="zoom: 67%;" />
    <img src="final_review_database.assets\image-20220827192311417.png" alt="image-20220827192311417" style="zoom: 50%;" />
  * <img src="final_review_database.assets\image-20220827192358827.png" alt="image-20220827192358827" style="zoom: 50%;" />
    <img src="final_review_database.assets\image-20220827192411888.png" alt="image-20220827192411888" style="zoom:50%;" />

## Third Normal Form

* 3NF是BCNF的最小放宽
* 具有函数依赖集合**F**的关系模式**R**是**3NF**的条件：对于F+中的所有形如a --> *b* 的函数依赖，a 和 *b* 是R 的子集，以下条件至少成立一个：
  * a --> b是平凡函数依赖
  * a 是 R 的一个superkey（满足这一条就是**BCNF**）
  * b *-* a中的每个属性A都包含在R的一个candidate key中（b-a中的每个属性可以包含在不同的candidate key中）
* 网上：非主键列必须直接依赖于主键，不能存在传递依赖











# Storage and File Structure

* Cylinder柱面

* Platter盘面

* Track磁道

* Sector扇区
* 物理地址：CHS：柱面、读写头（盘面）、扇区
* 逻辑地址：Block

## Optimization

缓冲技术：磁盘读取的数据暂存在内存缓冲区中以满足将来的访问需要

预读技术：当一个磁盘块被访问时，相同磁道上的数据一起被读到内存缓冲区中。好处是什么？不好之处呢？从顺序读和随机读的两个方面分析

调度技术：根据磁盘的运转机理，如果需要访问的数据块分布在不同的柱面上，可以针对磁盘的机器臂进行优化，使机械臂按照移动距离最短的顺序发出访问请求，通常使用**电梯算法**。

文件组织技术：为了减少块的访问时间，按照与预期数据访问方式最接近的方式组织磁盘上的块。例如通常顺序访问日志文件，这时可以将日志文件的所有数据块存储在连续的柱面上。可回忆操作系统中文件系统部分的内容以及教材437页的介绍。

非易失性缓冲区：利用non-volatile RAM（例如利用备用电源支持的RAM），当数据库请求写一个数据块到磁盘上时，磁盘控制器将这个数据块写到non-volatile RAM上，然后通知写操作完成，只有当non-volatile RAM满了才会有一个延迟。当non-volatile RAM满了或磁盘访问空闲时间的时候，磁盘控制器将数据块写入到磁盘相应的目标位置，减少了机械臂的移动。异步写的方式。

日志磁盘技术：一种专门用于写顺序日志的磁盘，所有访问都是都是顺序的，根本上消除了寻道时间，一次可以连续读多个数据块。支持日志磁盘的文件系统称作日志文件系统，也可以在没有日志磁盘的情况下实现。数据库系统实现自己的日志。为了保证系统的安全性，在应用系统层面通常也要实现日志。有一些专门的实现日志的框架。日志对以系统恢复很重要。

## Redundant Arrays of Independent Disks

通过**冗余**提高可靠性

通过**并行**提高性能

RAID级别的含义：通过**条带**与**校验位**结合，在成本和性能之间权衡，以较低成本提供数据冗余的方案

### Striping

* **Bit-level striping** – split the bits of each byte across multiple disks
* **Block-level striping** – with *n* disks, block *i* of a file goes to disk (*i* mod *n*) + 1（最常用的方法）
  <img src="final_review_database.assets\image-20220827211553617.png" alt="image-20220827211553617" style="zoom:80%;" />
  一格表示sector，block=4sector，segment=2block，strip=4segment。strip是有四块磁盘地址空间统一管理

### RAID 0

Block striping

块级拆分但**没有冗余**。在块级做**条带**，没有冗余。即多个磁盘只作条带化来存储数据，不冗余存储数据。

### RAID 1+0

Mirrored disks

块级拆分的镜像磁盘。磁盘通过块**条带**化做镜像，**冗余**存储数据

<img src="final_review_database.assets\image-20220827213835974.png" alt="image-20220827213835974" style="zoom:80%;" />

### RAID 5

Block-Interleaved Distributed Parity

采用了块级拆分且**校验信息分布式**存储，即数据和其他的数据的校验信息可以存储在同一个磁盘上，但是该数据自己的校验信息不能和自己存储在用一块磁盘上

<img src="final_review_database.assets\image-20220827213847649.png" alt="image-20220827213847649" style="zoom:80%;" /><img src="final_review_database.assets\image-20220827214131762.png" alt="image-20220827214131762" style="zoom:80%;" />

一共5块磁盘做RAID，P0~P4表示纠错位（校验信息），数据块按照(*n mod* 5) + 1的方式存储在5块盘上，相应数据块的校验信息存储在另外一块盘上

**IO代价大**，写一次读两块写两块，写为主则不建议（因为写之前要先看校验信息，邂逅的数据去计算新的校验值）

RAID5**至少3块盘**

### RAID 6

P+Q Redundancy scheme

在RAID 5的基础上进行了改进，采用**双校验**，可靠性更高，但成本也更高，应用不广泛
<img src="final_review_database.assets\image-20220827214541704.png" alt="image-20220827214541704" style="zoom:80%;" />

### Choice of RAID level

* 成本
* 性能
* 磁盘故障时的性能
* 磁盘故障后数据重建的性能
* RAID 0 仅在数据安全不重要时使用，例如：可以从其他来源快速恢复数据
* Level 6很少使用，因为Level 1和Level 5为几乎所有应用提供足够的安全性。常用1或者5
* Level 1 提供的写入性能比Level 5 好得多
  * Level  5 至少需要 2 个块读取和 2 个块写入来写入单个块，而Level  1 只需要 2 个块写入
  * Level 1优先用于high update新环境（如日志磁盘）。Level  1 的存储成本高于Level 5
* RAID 1比RAID 5 的存储成本高。因为镜像需要额外的存储，需要额外的资金投入到存储上。大量数据和低更新率的应用程序
* Level 1是所有其他应用程序的首选



## Fixed-Length Records

可能会跨block存储

* 删除数据有几种方法，删除第i个记录时
  * 第i记录的后续记录依次向前移动
  * 第n个记录移动到第i个位置
  * 不需要移动记录，删除记录的位置做标记，记下来。当有新的记录增加时（通常的应用增加数据的操作比删除数据的操作频繁），放到这个被删除记录的位置上。用**Free List**记录这些位置
    * 在文件的开始的地方，预留一些字节作为文件头（file header），记录文件的一些信息：被删除的第一个记录的地址。然后第一个记录的位置存储第二个被删除的记录的地址，依次类推，所有被删除记录的space都被记录了下来，形成一个free list
      <img src="final_review_database.assets\image-20220827233121827.png" alt="image-20220827233121827" style="zoom:60%;" />

## Variable-Length Records

* 变长记录在数据库系统中以下面几种方式出现：
  * 一个文件中存储多种记录类型的记录；
  * 允许一个记录的一个或多个字段是变长的；
  * 允许数据类型是array或multiset类型的记录
* 表示一个变长记录包括两个部分：
  * 开始部分是**定长**属性的数据，如日期、数值型、定长字符串
  * 第二部分是**变长**属性的数据，如变长字符串，则在记录的开始部分存储表示 （*offset, length*）对， 其中*offset*表示记录中该属性的数据开始的位置，length表示某记录变长属性的实际字节数
* <img src="final_review_database.assets\image-20220827235010731.png" alt="image-20220827235010731" style="zoom:80%;" />
  一个变长记录如上所示：前面三个矩形是offset-length对，指向后面的三个变长值（ID：10101，name：Srinivasan，dept：Comp.Sci）。salary：65000是定长。20位置处有一个占用了1个Byte的Null Bitmap，表示哪一个值是Null。
* **Slotted-Page Structure 分槽页式结构**
  * 每个block的头部记录了以下信息：
    * Record个数
    * 块的free space的末端地址
    * 每个记录的位置和大小
  * 插入记录时，总是从空闲空间的尾部开始，并在块首部登记插入记录的开始位置和大小
  *  删出记录时，释放所占用空间，在块首部的该记录大小作标记（如-1），然后左边的记录**移**过来填补，使实际记录仍然紧连。 同时修改块首部的信息
  * <img src="final_review_database.assets\image-20220828000337873.png" alt="image-20220828000337873" style="zoom:80%;" />
    这是一个数据块中的多个变长记录的组织。最左边是块的头，第一个slot记录了**记录的个数**和块的free space的**末端地址**；接下来的slot分别记录了每个记录的**位置和size**。Free space之后是每一个**具体的记录**。

## Organization of Records in Files

关系是一组记录。给定一组记录，如何在文件中组织它们

### Heap File Organization

* 记录可以放置在文件中有free space的**任何位置**
* 记录在分配后通常**不会移动**
* 在文件中高效找到free space，而不是顺序搜索文件的所有块，很重要
* **free-space map**
  ![image-20220828004242663](final_review_database.assets\image-20220828004242663.png)
  通常放在内存中去看每一个块还有多少占比的空间剩余

### Sequential File Organization

* 顺序文件组织是为了高效处理按照某个属性值的顺序排序的记录
* 为了减少顺序文件处理的块访问数量，在物理上（块空间中）按照search key的顺序或者尽可能接近search key的顺序存储记录
* 删除数据：使用**指针链表**形式，修改链表指针完成
* 插入数据：首先定位插入记录的位置上（保持顺序），如果所在的数据块有空闲空间（之前删除记录留下来的），就插入新的记录；否则在新的**溢出块**中插入新的记录
  <img src="final_review_database.assets\image-20220828005310146.png" alt="image-20220828005310146" style="zoom:67%;" />
* 需要时不时组织文件让它有序，代价大

### Multitable Clustering File Organization 多表聚集

在每一个块中存储两个或多个关系的相关西路的文件结构。如果两个表**经常join**，这样提高查询效率

<img src="final_review_database.assets\image-20220828005542333.png" alt="image-20220828005542333" style="zoom:80%;" />



## Data Dictionary Storage

* 关于关系的数据称为“**元数据**”。关于关系的关系模式和其他元数据存储在称作“**数据字典**”或系统目录的结构中。系统必须存储的信息包括：

  * 关于关系的信息：关系的名字、每个关系的属性名和类型、在数据库上定义的视图和视图的定义、完整性约束（如，key）
  * 用户和审计信息：授权用户名、用户的授权和账户信息、用户密码等信息
  * 统计数据和描述数据：每个关系的元组个数、每个关系的存储方法
  * 物理文件组织信息：
    * 如果关系存储在操作系统文件中，则记录每个关系的名字
    * 如果所有关系存储在一个文件中，则将记录每个关系的位置，将每个关系中记录的存储块记在链表这样的数据结构中。
    * 文件中的记录是如何组织的，例如hash还是顺序，还是树结构
  * 索引信息：索引的名字、被索引的关系的名字、在其上定义索引的属性、索引类型等

  <img src="final_review_database.assets\image-20220828010525680.png" alt="image-20220828010525680" style="zoom:80%;" />

## DBMS and Storage Access

<img src="final_review_database.assets\image-20220828010711970.png" alt="image-20220828010711970" style="zoom:80%;" />

* 块：磁盘空间固定大小的存储单元，数据存储分配和传输的单位。DBMS不依赖OS的块管理，而是自己实现了一套
* Buffer：内存中用于存储磁盘块拷贝的部分空间。
* Buffer manager：负责在内存中分配buffe的子系统
* Buffer Management in DBMS
  <img src="final_review_database.assets\image-20220828011313628.png" alt="image-20220828011313628" style="zoom:80%;" />
  <img src="final_review_database.assets\image-20220828011418877.png" alt="image-20220828011418877" style="zoom:80%;" />
* Buffer Manager Technology
  * buffer替换策略：类似OS的LRU
    * 大量的join，LRU不好
      <img src="final_review_database.assets\image-20220828012114835.png" alt="image-20220828012114835" style="zoom:80%;" />
  * Pinned block：为了能够使数据库系统从系统崩溃中**恢复**，**限制**一些块写回磁盘，这些块称作被定住的块。
  * Forced output block：为了保证数据的**持久性**存储，需要强制将数据块写回磁盘。
  * Toss-immediate strategy：在处理块的最后一个元组后立即释放该块所占用的空间
  * Most recently used：系统必须固定当前正在处理的块。 处理完该块的最后一个元组后，该块将被取消固定，并成为最近使用的块



# Indexing and Hashing

* **Search key**：一个关系中一个或多个属性，常用于查找文件中的数据记录
* Index文件：一个特殊的文件，由一些记录构成，称作索引项（**index entries**），两个部分：search key和pointer，pointer指向数据文件中search key对应的数据记录
* 顺序索引：基于记录中某个属性值的顺序建立的索引，比如成绩顺序
* hash索引：根据记录的查找键的值，使用一个函数计算得到的函数值作为磁盘块的地址，对记录进行存储和访问，通常用**桶**作为基本的存储单位，一个桶可以存放多个记录
* 评估：
  * 查找具有特定值的记录还是查找属性值在一定范围的记录
  * 访问时间：查询时找到数据项或数据项集合的时间
  * 修改时间：在数据文件中增加或删除数据的时间，以及更新索引（插入索引项和删除索引项）的时间
  * 空间开销：索引文件占用的存储空间

## Indexes on Sequential Files

**顺序索引**：索引项根据search key排好序，索引文件中的索引记录都是按照search key排好序的。

* 对于数据文件，可以按照某个属性的值排好序存放在文件中，**主索引**（聚集索引）指的是索引项中的search key是数据文件中排序的属性，而且顺序与数据文件一致
* 当数据文件中的记录顺序与索引文件中索引项的顺序不一致时，这个索引是**辅助索引**（非聚集索引）

### Dense Index Files

* **稠密索引**：索引文件中search key所有取值都出现在索引文件中
* <img src="final_review_database.assets\image-20220828102138156.png" alt="image-20220828102138156" style="zoom:80%;" />
  <img src="final_review_database.assets\image-20220828102149638.png" alt="image-20220828102149638" style="zoom:80%;" />

### Sparse Index Files

* **稀疏索引**：索引项中只包含search key的部分取值
* 要查找搜索键值为 K 的记录，我们： 查找搜索键值最大< K 的索引记录，按顺序从索引记录指向的记录开始搜索文件
  <img src="final_review_database.assets\image-20220828102434251.png" alt="image-20220828102434251" style="zoom:80%;" />

### Dense vs. Sparse

* 稀疏索引的空间开销比稠密索引**小**，定位数据比稠密索引**慢**
* 将两种索引结合起来形成**多级索引**。首先为顺序文件的每一块建立一个索引记录，得到一个以块为基本单位的**稠密**索引，然后再在稠密索引的基础上建立一个**稀疏**索引。查找时，首先在稀疏索引中确定记录在哪一块，最后在数据文件的块中顺序查找，找到所在的记录

### Multilevel Index

* 如果索引文件小到可以放入内存，搜索一个索引项的时间会很短。但是索引文件很大**不能放在内存中**时，需要从磁盘调用索引文件的数据块，开销变大
* 方法1：索引文件是按照索引项排好序的，那么用二分查找算法来定位索引项可以吗？如果有了溢出块就不能用二分查找了？
* 方法2：建立多级索引：**内层**索引是主索引，**外层**索引是主索引之上的稀疏索引，为索引文件建立索引。如果有数据更新时，所有层的索引都要更新。
  <img src="final_review_database.assets\image-20220828104134034.png" alt="image-20220828104134034" style="zoom:80%;" />

### Secondary/Nonclustered Index

* **辅助索引**和主索引的目的一样，但是索引文件中search key的顺序与数据文件的顺序不一样

  <img src="final_review_database.assets\image-20220828105122958.png" alt="image-20220828105122958" style="zoom:80%;" />

* 如果主索引的search key 不是candidate key ，索引指向具有该特定 search key的第一条记录，其他记录都连续存放，顺序扫描得到。
  如果辅助索引的search key**不是candidate key**，具有该特定search key的记录不是连续存放，**散布**在文件的任何地方，辅助索引的索引项必须包含指向第一个记录的指针。索引记录指向一个**存储桶**，该存储桶包含指向具有该特定搜索键值的所有实际记录的指针。<img src="final_review_database.assets\image-20220828105300888.png" alt="image-20220828105300888" style="zoom:80%;" />

* 如果索引是主索引，顺序扫描可以高效地完成数据更新；但是如果是辅助索引，那么代价很高，因为会访问不同的数据块来完成数据的更新，有可能增加新的数据块。磁盘访问时间增大。

### B+ Tree Index Files

* 索引顺序文件的缺陷：随着文件的增大，**溢出块**不断产生，为了保持顺序，文件需要**阶段性地重组**，导致索引**查找性能**和**数据顺序扫描**的性能都会下降

* **B+树**是一种最广泛使用的索引文件结构之一，采用平衡树结构，是在数据插入和删除的情况下仍能保持执行效率的结构，只需很小的、局部的变化自动重组文件。针对更新频繁的文件，减少了文件重组的代价

* B+树的**缺点**：会有插入数据和删除数据的额外开销，增加空间开销

* B+树：

  * **树中非根、非叶子节点有$\lceil n/2 \rceil $~ $ n$ 个孩子**
  * **叶子节点有$\lceil (n-1)/2 \rceil$~ $(n-1)$个值**
  * 特殊情况：
    * 如果根不是叶子，则**至少有2个孩子**
    * 如果根是叶子，则根节点存储$0$ **~** $n-1$个值
  * 例如$n=4$
    <img src="final_review_database.assets\image-20220828114645823.png" alt="image-20220828114645823" style="zoom:80%;" />

* 叶子节点：若search-key不是数据文件的主键，且search-key值的顺序也不是数据文件的顺序（数据文件按照另外的属性值排序），则指向一个**桶**，桶中的记录具有search-key的值

* *P*1指向的子树的所有search-keys值**小于**K1，*Pn*指向的子树的所有search-keys值**大于等于**Kn*–1，*Pi*指向的子树的所有search-keys值大于等于*Ki*–1且小于*Ki* 
  <img src="final_review_database.assets\image-20220828142112528.png" alt="image-20220828142112528" style="zoom:67%;" />

* B+树**查询**：

  * 根结点中找>K的最小查找键值
  * 如果存在这样的值， 设为*Ki* 然后沿着*Ki*左边的指针*Pi*到达第二层的结点；
  * 如果不存在这样的结点（ *k* $\geq$ *Kn*–1），沿着指针*Pn*到达第二层的结点
  * 直到叶子结点，找到一个指针直接指向数据文件的记录，或指向一个桶。

  <img src="final_review_database.assets\image-20220828142844132.png" alt="image-20220828142844132" style="zoom:80%;" />

* 假设有k个search key，树的高度不超过$\lceil log_{\lceil n/2 \rceil}(K) \rceil$，即从跟节点到叶子结点的遍历长度
  <img src="final_review_database.assets\image-20220828144221308.png" alt="image-20220828144221308" style="zoom:50%;" />

* **插入**：

  * 分裂时，把n个 (search-key value, pointer) 对按顺序放在两个结点中，前 $\lceil n/2 \rceil$个value放在原来的结点中，余下的放在新的结点中。
  * 在父结点中插入**新结点中的最小查找键**值
  * 如果父结点满了，那么分裂父结点，再在上一层结点中加入一个新的查找键值
  * <img src="final_review_database.assets\image-20220828151630744.png" alt="image-20220828151630744" style="zoom:80%;" />
    <img src="final_review_database.assets\image-20220828151652656.png" alt="image-20220828151652656" style="zoom:80%;" />

* **删除**：

* 字符串上的索引：前缀压缩

  * Silberschatz只用在非叶子节点存储Slib就行

## Hash Index

<img src="final_review_database.assets\image-20220828154225480.png" alt="image-20220828154225480" style="zoom:80%;" />

* An ideal hash function is **uniform**（均匀的）：每个桶内分配的查找键值数目大体相同。
* Ideal hash function is **random**（随机的）：Hash函数值不受查找键各种顺序的影响
* 每个桶的空间是固定的，如果桶已装满记录，那么插入新记录时，桶溢出。原因在于：
  * 桶的数量少
  * 记录分布不均匀
* 如果一条记录必须插入到桶b中，如果桶b已满，系统会为桶b提供一个**溢出桶**，如果溢出桶也满了，会继续提供另一个溢出桶。一个给定的溢出桶用链表链接起来，这种处理称为**溢出链**
  <img src="final_review_database.assets\image-20220828155527490.png" alt="image-20220828155527490" style="zoom:80%;" />
  查询时除了查询桶b中的数据，还需要检查桶b的溢出桶中的记录
* Hash索引将search-key值及其相应的指针组成hash文件结构，将hash函数作用于search-key来确定对应的桶，然后将这个search-key值已经相应的指针存入桶中（或溢出链中）
  * 严格地说，hash索引是**辅助索引**。如果文件本身是hash结构，建立一个hash主索引就没必要了
  * <img src="final_review_database.assets\image-20220828155756121.png" alt="image-20220828155756121" style="zoom:80%;" />
    在instructor ID上建立索引。蓝色框是数据记录所在的数据块。Hash索引文件一共有8个桶，每个桶存储的search-key（ID）。可以看到桶5有溢出桶。

### Bitmap Indices

位图索引对于查询多个属性非常有用，对于单个属性查询不是特别有用

<img src="final_review_database.assets\image-20220828160207205.png" alt="image-20220828160207205" style="zoom:80%;" />
用于集合枚举比较方便

## Index Selection

在建立索引、维护索引与加速查询之间进行权衡

* 如果应用是**查询密集型**的，就在针对频繁查询的属性上建立索引；
* 如果应用是**修改密集型**的，如经常需要对数据进行增删改操作，慎重考虑索引的构建；
* 如果数据集较**小**，不必用索引了

# Query Processing

<img src="final_review_database.assets\image-20220828161627550.png" alt="image-20220828161627550" style="zoom:80%;" />

SQL非过程式，关系代数过程式

## **Measures of Query Cost**

* **查询代价**可以通过该查询对各种资源的使用情况来度量，如磁盘访问、CPU时间（实时系统会考虑），甚至网络传输时间（分布式数据库会考虑网络因素）。通常在大型数据库系统中，**磁盘访问时间**是最主要的代价，决定整个查询的时间，且容易估计。具体分析以下因素：
  * 查找次数（寻道的数量）×平均查找时间
  * 读数据块的个数×平均读数据块的代价
  * 写数据块的个数×平均写数据块的代价
* 通过将磁盘读和磁盘写区分开，可以细化磁盘存存取的代价估算。因为写磁盘块的代价是读磁盘块的代价的2倍，原因是**写完之后要重新读**以验证写操作已完成。
* $t_T$：传输一个数据块的时间
* $t_S$：磁盘平均访问时间（搜索+旋转）
* $b*t_T+S*t_S$表示b个数据块执行S次磁盘搜索的时间

## Selection

### A1

* **线性搜索**：系统扫描每一个文件块，对其中的记录进行检查是否满足选择条件
* **查询代价**（$b_r$是关系r中record一共的存储块数）
  * $b_r * t_T + t_S $。花费一次访问定位找到头部，顺序读完所有块
  * 如果是对key属性进行判断（唯一的key），平均查询代价是$(b_r)/2 * t_T + t_S $
* 最基础，最慢，任何条件都可以

### A1'

* **二分**查询
* **条件**：属性等值比较且有序
* **代价**：
  * $\lceil log_2 (b_r) * (t_T + t_S ) \rceil$
  * 查找时不连续，每一次都需要传输和定位

### A2

A2~A6**索引**扫描：应用索引的查找算法，查找条件必须是**index的search-key**上的比较

* **主索引**，**key**上等值比较
* **代价**：
  * $h_i * ( t_T + t_S) + (t_T + t_S)$即
    $(h_i+1) * ( t_T + t_S)$
  * hi是B+树的高度，前面是访问索引文件的代价，后面是访问数据块的代价

<img src="final_review_database.assets\image-20220828170336077.png" alt="image-20220828170336077" style="zoom:80%;" />
create index on ID
select * from instructor where ID=22222

### A3

* **主索引**，在**非key**属性上等值比较
* **代价**：
  * $h_i * (t_T+t_S) + t_S + t_T*b$
  * 数据文件的访问有一次seek和b次传输，因为非key属性查询返回多个记录，这些记录连续存储

<img src="final_review_database.assets\image-20220828170351330.png" alt="image-20220828170351330" style="zoom: 80%;" />create index on dept_name
select * from instructor where dept_name=‘Comp.Sci’

### A4

* **辅助索引**，等值查询
* **代价**：
  * 如果search-key是**candidate key**：$(h_i +1) * (t_T + t_S)$
    * 查询返回一条记录
  * 如果search-key不是candidate key：$(h_i + n) * (t_T + t_S)$
    * 查询返回多条记录且这些记录不连续，每一块都需要seek和传输

<img src="final_review_database.assets\image-20220828170410012.png" alt="image-20220828170410012" style="zoom:80%;" />create index on salary
select * from instructor where salary=80000

### A5

* **主索引**，**比较**
* 对于查询$\sigma_{A\geq V}(r) $，用index查找到A>=V的第一个记录后顺序扫描文件
* 对于$\sigma_{A\leq V}(r) $，不需要访问index文件，只需从**文件头开始**扫描直到A> v的记录出现

### A6

* **辅助索引**，**比较**
* 对于查询$\sigma_{A\geq V}(r) $，index查找到A>=V的第一个记录后顺序访问索引文件，通过索引记录的指针找到数据记录
* 对于$\sigma_{A\leq V}(r) $，只需访问索引的叶子节点，通过可一个K的指针找到数据记录直到第一个A>V，查询结束
* 连续的记录可能存储在不连续的存储块中，找到每一个数据记录都需要一次I/O，代价比直接线性扫描数据文件的代价还要大。因此，辅助索引只有在找到的**记录数目很小**时使用

### A7

A7~A9 Conjunction ^

* 应用索引的合取选择
* 首先判断是否存某个简单条件中的某个属性上的存储路径，若存在，则选择A2~A6之一的算法检索满足该条件的记录，然后再**内存缓冲区中测试**检索到的每一个记录是否满足其余的条件

### A8

* 应用多值索引的合取选择
* 在多个属性上建立的一个索引

### A9

* 通过标识符的交运算实现合取
* 查询条件涉及到的属性上要求有索引，利用这个索引查找满足相应条件的记录的指针，进行**交集**，然后找到相应的数据记录

### A10

* 通过标识符的并运算实现析取disjunction V
* 如果所有的析取选择条件上**均有索引**，利用这个索引查找满足相应条件的记录的指针，进行**并集**，然后找到相应的数据记录
* 否则线性扫描

### Negation

$\sigma_{\lnot \theta} (r) $

* 线性扫描
* 如果满足的record很少且$\theta$适用索引，则使用索引

## Sorting（External sort-merge）

* 内存缓冲区能容纳**M**页，一次可以读取的块数M

* **第一阶段** 创建归并段**runs**
  <img src="final_review_database.assets\image-20220828184336117.png" alt="image-20220828184336117" style="zoom:80%;" />

  * 从数据文件中读M个块到内存
  * 在内存中对读入的额数据进行排序；
  * 排好序的数据写回到归并文件Ri中
  * i=i+1
  * 最后得到N个归并好的文件R，N是最后循环的次数i

* N-way merge：N个缓冲块输入归并文件，一块缓存输出。将每个归并段的第一块读入各自的缓冲区
  <img src="final_review_database.assets\image-20220828184009712.png" alt="image-20220828184009712" style="zoom:80%;" />

* **归并阶段**：循环完成以下工作，直到输入缓冲区中的buffer页为空。
  <img src="final_review_database.assets\image-20220828184401868.png" alt="image-20220828184401868" style="zoom:80%;" />

  * 从所有的buffer页中有序选出第一个记录（基于内存的归并排序的思想）
  * 将该记录写到输出buffer，并从input buffer中删除这个记录；如果输出buffer满了将其中的数据记录写到磁盘；
  * 如果任何一个归并段Ri对应的buffer页空了并且没有到达Ri文件的末尾，则读入Ri的下一块到对应的input buffer中

  <img src="final_review_database.assets\image-20220828184603203.png" alt="image-20220828184603203" style="zoom:80%;" />

## 代价

* $b_r$表示关系r中记录一共占用的**存储块**数
* 在**建立**排好序的归并段Ri阶段，要读入关系（待排序的关系表）的每一个块并写出，因此每一趟块**传输**为$b_r + b_r = 2 b_r$
* 在**归并阶段**传输的数据块数与归并的**趟数**有关。每一趟归并，归并段数减少为上一次的1/（M-1），因此归并的趟数为$ \lceil log_{M-1} (b_r / M) \rceil $
* 忽略最后一次写操作。加上建立R阶段的代价，**总传输块数**为$ 2b_r\lceil log_{M-1} (b_r / M) \rceil + b_r$
* 在**建立**排好序的归并段Ri阶段，读取每个归并段都需要seek，写回磁盘也需要seek，一共br/M个归并段。因此，这个阶段的seek代价为$ 2\lceil b_r / M \rceil $
* 在**归并阶段**，设$b_b$是一次从归并段读取到内存的块数，则每次归并需要做$\lceil b_r / b_b \rceil$次seek来读数据，写回磁盘又需要$\lceil b_r / b_b \rceil$次seek，一共趟归并$ \lceil log_{M-1} (b_r / M) \rceil $，则需要$2\lceil b_r / b_b \rceil \times\lceil log_{M-1} (b_r / M) \rceil $ 次seek。但是，最后一趟只需要读数据块的seek，忽略写回磁盘。因此这个阶段的seek代价为$2\lceil b_r / b_b \rceil \times\lceil log_{M-1} (b_r / M) \rceil - \lceil b_r / b_b \rceil$
* 两次seek相加得到$ 2\lceil b_r / M \rceil +\lceil b_r / b_b \rceil (2\lceil log_{M-1} (b_r / M) \rceil-1) $

## Join

重点介绍Nested-Loop join & Block Nested-loop join

### Nested-loop join

$n_r$是外关系的记录数，$n_s$是内关系的

* 最坏情况下，如果内存仅能容纳每个关系的一个数据块
  * **传输块数**为$b_r + n_r * b_s$ 。其中br表示关系r的每一个数据块都需要读，nr\*bs表示对于关系r的每一个记录，都要读关系s的每个数据块*，*做匹配计算，共nr 次
  * **Seek次数** $b_r+n_r  $，其中关系r的每一个块都需要seek，需要br次（读完r的第一块后，块中每条记录与s中记录做匹配计算，然后再读第二块，因此读r的数据块不是连续读，所以每个块都需要seek），关系r的每一条记录需要与关系s中的每条记录做匹配计算，但是s只需seek一次（顺序读每一块，所以只需一次seek）

### Block Nested-Loop Join

* 这个算法以**块**的方式而不是以记录的方式来处理。内层关系的每一块与外层关系的每一块成对处理，每一块中的每一个记录与另一个块中的每一个记录形成记录对，做条件匹配，满足条件的作为结果关系的记录

* 最坏情况：

  * **传输块数**：$b_r + b_r * b_s$
  * **Seek次数**：$2b_r$

* 最好情况

  * **传输块数**：$b_r + b_s$
  * **Seek次数**：$2$

* **改进**：

  * 读入外层关系的数据块时，可以按照内存大小M（设内存大小为M个块），一次读入外层关系的**M-2**个数据块，然后读内层关系的每一块与M-2个数据块的数据进行join条件的匹配计算。2表示留出来的内存空间给内层关系的数据块以及输出结果用。

    <img src="final_review_database.assets\image-20220828195150501.png" alt="image-20220828195150501" style="zoom:67%;" />$ b_r + \lceil b_r /(M-2)\rceil * b_s $ 传输
    $ 2\lceil b_r/(M-2)\rceil $次seek

  * 如果join运算时，join的属性是内层关系的key，那么在外层关系与内层关系的记录进行匹配时，找到满足条件的第一个记录，算法就停止运行

  * 对内层关系轮流做向前和向后的扫描，这种方法对磁盘块的读写请求进行排序，使得之前扫描的数据块留在内存中，采用LRU策略。从而减少数据传输和seek的代价（不太懂）

### Indexed Nested-Loop Join

* 条件：join是等值连接或自然连接。内层循环的连接属性上有索引
* **最坏**情况：每次只能向内存读入外层关系的一个数据块
  * 外层关系r的每一块都要读，因此**传输**br次，**seek**也是br次；对于r关系的每条记录，需要在s索引文件上进行查找，所以代价是 nr* *c*（*nr*是外层关系的记录数目，c是使用join条件对内层关系s进行**单次查找的代价**（之前介绍的selection的计算代价）
  * join的**代价**：$b_r * t_T + b_r * t_S + n_r * c $
  * 记录**数目较小的关系作为外层**关系，这样c的系数会小，*tT*和*tS*的系数都会小
* <img src="final_review_database.assets\image-20220828203631821.png" alt="image-20220828203631821" style="zoom:80%;" />

### Sort-Merge-Join

两个关系都按其连接属性排序

* 条件：等值自然连接

<img src="final_review_database.assets\image-20220828203949466.png" alt="image-20220828203949466" style="zoom:80%;" />

* 设$b_b$是为每个关系分配的缓冲块数，在join属性上具有相同值的记录是连续存放的，因此排好序的记录只需读一次，因此每一个数据块只需读一次。由于两个关系都只需读一遍，则**传输**的数据块数为$b_r + b_s$ ；**seek的次数**为$\lceil b_r /b_b \rceil + \lceil b_s /b_b \rceil $

### Hash Join

<img src="final_review_database.assets\image-20220828204943914.png" alt="image-20220828204943914" style="zoom:80%;" />

* 条件：等值自然连接
* 如果关系r的一个记录与关系s的一个记录满足join条件，它们在JoinAttrs（这是join的公共属性）就会有共同的值，经过hash函数的映射，给定i，他们必定在ri和si的划分中。这样进行join条件判断时，**不需要与其他划分中的记录进行比较**。s，r有n个partition
* **实现算法**：对于每一个i，依次完成以下步骤：
  * 将si读入内存中，然后在JoinAttribute上建立内存中的hash索引。这个hash函数与生成划分的hash函数不一样
  * 将ri的数据块依次读入到内存中，对于其中的每一个记录tr，应用内存中的hash索引找到si中的记录ti，然后输出拼接好的结果
  * 其中s称作build输入，r称作探查probe输入
  * **小关系**作为build更好
* **代价**：
  * **传输**：$3(b_r + b_s) + 4 * n_h$
  * **seek**：$2(\lceil b_r / b_b \rceil + \lceil b_s / b_b \rceil)$
  * 递归传输：$2(b_r + b_s)\lceil log_{\lfloor M/b_b-1\rfloor}(b_s / M) \rceil +b_r + b_s $
  * 递归seek：$2(\lceil b_r / b_b \rceil + \lceil b_s / b_b \rceil) \lceil log_{\lfloor M/b_b-1\rfloor}(b_s / M) \rceil $



# 事务

当**多个用户**或进程并发访问数据库时，当系统出现**故障**时（天灾人祸）会导致数据库中出现**不一致**的数据或错误的数据，这样用户看到的数据就不正确。因此，需要数据库具有**并发控制机制**和**故障恢复机制**。

**目标：**

* 提高事务**吞吐率**，快速响应；
* 提高系统资源的**利用率**：例如一个进程读数据，另一个进程计算或者从另一个磁盘读数据
* 系统故障时**保护数据**，系统恢复时保持数据**一致性**。

## Transaction Concept

* 事务是构成**单个逻辑工作单元的操作集合**。实际上事务由若干条语句构成，而**面向用户则是单个原子性操作**
* 事务看到的数据库是一致的，符合业务逻辑的数据，不是中间状态
* 事务执行**过程中数据可能不一致**
* **事务提交之后数据库一定保持一致性**（持久化）
* 主要解决问题：
  * 系统执行过程中可能出现故障
  * 并发访问，多个事务执行。不能因为共享数据破坏一致性。 

## ACID Properties of the transaction

必考内容

* **Atomicity**原子性：执行事务的所有操作或者全部执行成功，或者一条指令都不执行。
* **Consistency**一致性：隔离执行的一个事物保持数据库的一致性。即数据是正确的，符合应用逻辑的。
* **Isolation**隔离性：多个并发执行的事务感知不到其他事务的执行。自己执行自己的逻辑。
* **Durability**持久性：当事务成功执行后，对数据库的修改必须持久存储，即使系统出现故障也不会丢失。

## 相关子系统

不会考

和事务管理相关的子系统（从DBMS系统实现的视角来看）：查询处理器、事务管理器、日志管理器、恢复管理器。涉及到的数据包括具体数据和日志数据。
<img src="final_review_database.assets\image-20220820132312237.png" alt="image-20220820132312237" style="zoom:67%;" />

 ## Transaction State

* Active：**活动**状态，初始状态，事务执行时的状态，执行对数据库的读写操作
* Partially committed：事务的最后一个语句执行之后进入**部分提交**状态，数据在缓冲区中
* Failed：**失效**状态，正常的执行无法继续后的状态
* Aborted：**中止**状态，事务回滚且数据库恢复到事务执行之前的状态
* Committed：**提交**状态，事务成功执行后的状态


<img src="final_review_database.assets\image-20220820141114936.png" alt="image-20220820141114936" style="zoom:67%;" />

## Schedules 调度

* 调度：**执行并发事务指令的时间顺序的序列**
* 针对一组事务的调度必须包含这些事务的**全部指令**，并且保持事务中指令**原来的顺序**，即不能改变事务指令的顺序。如果破坏了事务指令的顺序，就破坏了事务的逻辑
* 一个事务成功执行后，最后要有一条commit语句，默认最后一步执行**commit**指令
* 如果事务没有成功执行，最后要有一条**abort**语句

等价：
<img src="final_review_database.assets\image-20220820145538183.png" alt="image-20220820145538183" style="zoom:67%;" />

不等价：
<img src="final_review_database.assets\image-20220820145614913.png" alt="image-20220820145614913" style="zoom:67%;" />

## Serializability 串行化

* 事务在并发执行中，如果保证所执行的任何调度的结果都和没有并发调度的结果一样，也就是都和串行执行的结果一样，就可以保证数据库的一致性
* 如果一个调度在某种意义上等价于一个串行调度，那么这个调度称作**可串行化调度**
* 判断是否等价方法：
  * 冲突可串行化
  * 视图可串行化

### Conflicting Instruction 冲突指令

* 考虑一个调度S，Ii和 Ij分别是两个事务 Ti和Tj 的指令，且**连续**（时序上）。当且仅当两个指令都访问数据项**Q**，且至少一条指令是针对Q的write指令时，Ii和 Ij是冲突的。即，当两个指令都是针对Q的读操作时，或者一个对P进行操作、一个对Q进行操作时，两个指令不冲突

### Conflict Serializability 

* 如果一个调度S通过一系列非冲突指令的交换变换为调度S‘，那么可以说调度S和调度S’是**冲突等价**的
* 如果一个调度冲突等价与一个串行调度，那么这个调度是**冲突可串行化**的


<img src="final_review_database.assets\image-20220820185155146.png" alt="image-20220820185155146" style="zoom:67%;" />

不冲突就可以交换先后顺序，不断交换，最后等价于两个串行调度

### Precedence Graph

**优先图**：是一个**有向图**，顶点集是所有参与调度的事务，边集是满足下列条件之一的边Ti -- > Tj 组成：

* 在Tj 执行read（Q）之前，Ti 执行write（Q）；
* 在Tj 执行write（Q）之前，Ti 执行read（Q）；
* 在Tj 执行write（Q）之前，Ti 执行write（Q）；

当且仅当优先图**无环**时，一个调度是冲突可串行化的。如果优先图无环，可以通过拓扑排序得到事务的执行顺序，可以得到多个线性序。

如果优先图存在在边Ti-->Tj，则在任何等价于S的串行调度S'中，Ti一定出现在Tj之前

n个节点的图的Cycle-detection需要花费n方的运算

有可能存在并非冲突等价，但是产生相同结果的调度

## Recoverable Schedules 可恢复调度

在事务并发执行的过程中，事务发生故障，读调度会产生影响。 如果事务发生故障，进入fail状态，必须撤销这个事务的影响**确保原子性**，事务rollback。那么，在允许并发执行的系统中，如果T1事务发生故障被撤销，**依赖**T1的任何事务也要中止。为确保这一点，在上述调度的基础上加一些限制。

* 一个可恢复调度满足：**对于每对事务Ti和Tj ，如果Tj 读取了由Ti所写的数据项，则Ti 要先于 Tj 提交**


<img src="final_review_database.assets\image-20220820233825462.png" alt="image-20220820233825462" style="zoom:50%;" />

这个调度有两个事务参与，T8和T9。T8事务首先读数据A然后修改数据A，修改后再查询A（读）；T9事务查询数据A（读）。两个事务并发执行，如果T9事务执行完read（A）指令后，但是没有commit，这时候系统crash了，为了确保事物的原子性，T8回滚了，数据A恢复到修改之前的数值。如果没有相应的控制机制，T9读到数据A的值是被T8修改后的值，但是这个值最后被撤销了。导致T9查询的结果是未提交的数据（这种数据称作脏数据）。

DBMS必须确保调度是可恢复的，即**修改数据的T8要先于读这个数据的T9提交**，数据持久化。T9在T8提交之后提交

总结一句话：写先提交以免读到中间数据

## Cascading Rollbacks 级联回滚

一个事务的失效导致一系列并发执行的事务回滚的现象。代价大，需要加以限制，避免这种现象发生

**Cascadeless Schedule**无级联调度

对于每对事务 Ti 和 Tj ， 如果 Tj 读取 Ti 之前修改后的数据，那么 Ti  要在 Tj 这个读数据操作之前提交

无级联调度是**可恢复**的

## Weak Levels of Consistency 弱一致性

对于某些应用，希望保持较弱的数据一致性，允许调度不一定可串行化。例如，涉及统计计算的事务，统计结果数据是**近似值**，不要求精确结果时，不要求强一致性。这样可以提高系统的**性能**（不必等待另一个事务）

## Levels of Consistency in SQL-92 一致性级别

事务隔离性级别有以下规定（SQL规定）：

* 串行化：默认级别
* 可重复读：只允许读**已提交**的数据，而且一个事务两次读取一个数据项期间，其他事务不得更新该数据，但该事务不要求与其他事务可串行。例如当一个事务在查询满足条件的数据时，他可能找到一个已经提交事务更新的数据，但是可能找不到该事务更新的其他数据。
* 已提交读：只允许读取**已提交**的数据，但不要求可重复读。比如事务两次读取一个数据项期间，另一个事物更新了该数据并提交。
* 未提交读：允许读**未提交**的数据。SQL允许的**最低**一致性级别。

# Concurrency Control 并发控制

并发操作**问题**

* 丢失修改
* 读脏数据（未提交随后被撤销的数据）
* 不可重复读

并发控制机制的任务

* 对并发操作进行**正确调度**
* 保证事务的**隔离性**
* 保证数据库的**一致性**

重点学习

* **Lock-Based**  Protocols（老师主要讲）
* Timestamp-Based Protocols
* Multi-version Schemes
* Snapshot Isolation

## Lock-Based Protocols 基于锁的并发控制 

确保事务隔离性的方法之一是**互斥访问数据**。锁机制是常用的并发控制机制，基于锁的并发控制要求事务在读写数据之前必须拥有锁。即当一个事务访问某数据项时，其他事务不能修改这个数据。只允许事务访问当前该事务持有锁的数据项。

* 事务在读写数据时必须持有锁，不再访问数据时要释放锁（不能一直霸占着）
* 不存在两个事务同时对一个数据项持有锁。

只考虑两种锁

* **互斥锁** exclusive（**X**）：如果事务持有数据项Q上的互斥锁，该事务**即可以读也可以写**数据项Q。Lock-X请求锁
* **共享锁**shared（**S**）：如果事务持有数据项Q上的共享锁，该事务**只能读**数据项Q不可以写数据项Q。Lock-S请求锁

事务将锁请求发送给**并发控制管理器**

### Lock-compatibility matrix 锁相容矩阵


锁相容矩阵：T1已经拥有锁，T2申请对同一数据项加锁。矩阵中true表示批准，false表示冲突不相容，不批准，需要等待锁释放<img src="final_review_database.assets\image-20220821235852228.png" alt="image-20220821235852228" style="zoom:50%;" />

### 死锁

T3事务需要访问数据项B，并进行写操作。然后需要访问数据项B进行写操作，因此事务T3首先发出lock-X(B)请求，这时没有其他事务锁住数据B，事务T3继续执行以下语句。执行write（B）之后，CPU执行事务T4的代码。因为T4事务需要读数据A，接下来需要读数据B，因此首先发出lock-S(A)，这时没有其他事务锁住数据A，事务T4继续执行以下语句。当发出lock-s（B）时，由于T3事务锁住了数据B，T4无法得到访问数据B的权限，等待。而T3事务需要访问数据A，发出lock-x（A）的请求，但是这个数据A被事务T4锁住，无法得到授权。至此，两个事务都无法执行下去，陷入死锁状态
<img src="final_review_database.assets\image-20220822001750573.png" alt="image-20220822001750573" style="zoom:67%;" />

###  饥饿

事务T2在数据项A上拥有S锁，事务T1申请在A上加X锁，T1必须等待T2释放锁；同时T3申请在A上加S锁，T1需要等T3释放锁，这时可能有T4申请在A上加S锁，T1可能将永远轮不上加锁的机会（只要还有读不断进来，就没有写的机会）
<img src="final_review_database.assets\image-20220822001923307.png" alt="image-20220822001923307" style="zoom:67%;" />

**解决：**

当事务T申请数据Q上的M型锁，并发控制器授权加锁的条件是：

* 在数据Q上不存在持有与M冲突的锁的其他事务
* 不存在等待对数据Q加锁且先于T申请加锁的事务；**例如**：T1事务为数据A申请互斥锁时，T2持有数据A上的共享锁，根据锁相容矩阵，是冲突的锁，并发控制管理器不会授予T1互斥锁；若T3事务为数据A申请共享锁，但是在申请之前，T1早已申请互斥锁，在等着T2释放锁。并发控制管理器不授予T3共享锁。

### The Tow-Phase Locking Protocol 两阶段加锁协议2PL

* 此协议保证了**冲突串行化调度**
* 协议要求事务**分两个阶段**提出加锁和解锁申请
  * 增长阶段：事务可以获得锁，但不能释放锁
  * 缩减阶段：事务可以释放锁，但不能获得锁
* 事务一开始处于增长阶段，根据需要申请锁。一旦事务释放了锁，就进入缩减阶段
* 对任何一个数据进行写操作之前，事务必须获得对该数据的锁
* 在释放一个锁之后，事务不再获得任何其他锁

 <img src="final_review_database.assets\image-20220822142457063.png" alt="image-20220822142457063" style="zoom:67%;" />

* 通俗来理解：不要加锁和解锁交错


<img src="final_review_database.assets\image-20220822143338077.png" alt="image-20220822143338077" style="zoom:80%;" />

* 无法避免**死锁**，**级联回滚**也可能发生（红箭头时T5故障）

#### 2PL&级联回滚

* Strict 2PL：
  * 两阶段加锁
  * 事务一直拥有**互斥锁**，直到事务提交或中止。未提交事务写的数据在提交之前被互斥锁封锁，防止其他事务读这些数据
* Rigorous 2PL：
  * 两阶段加锁
  * 事务持有**所有的锁**，直到事务提交或中止。也就是说事务提交之前不释放任何锁

以上的严格两阶段加锁协议**避免级联回滚**，是商用DBMS常用加锁协议

#### Lock Conversions

* 第一阶段可以将共享锁**升**级为互斥锁
* 第二阶段可以将互斥锁**降**级为共享锁
* 锁转换可以提高**并发度**

#### Automatic Acquisition of Locks

* 事务进行读写操作时，系统**自动**地为事务产生适当的加锁、解锁指令
* 事务进行**read**（D）操作时，系统就产生一条lock-**S**(D）指令，该read（D）操作紧随其后
* 事务进行**write**（D）操作时，系统检查事务是否已在数据D上持有**共享锁**。如果有，则系统刚发出升级指令**upgrade**（D），write紧随其后；否则系统发出lock-**X**（D）指令，write（D）紧随其后

### Implementation of Locking

* 加锁操作的实现由**锁管理器**来完成，它可以是单独的一个进程，接收事务发来的加锁、解锁请求
* 锁管理器针对**锁请求消息**返回**授予锁**的消息，或者要求事务**rollback**的消息
* 发出请求的事务一直**等待**直到请求被响应
* 锁管理器为每一个数据项维护一个称作**“锁表”**的数据结构，每一个请求为链表中的一个记录，记录事务发出的请求、请求什么类型的锁，是否请求被应答且授予锁，按请求到达顺序排序
* 锁表通常是内存中的哈希表，该表根据被锁定的数据项的名称编制索引


<img src="final_review_database.assets\image-20220822154554535.png" alt="image-20220822154554535" style="zoom: 67%;" />

这个表包含5个不同数据项的锁，数据项分别为 I4, I7, I23, I44, and I912，锁表采用溢出链，因此每一个数据项有一个已授予锁或等待授予锁的事务列表。图中示意事务T23（蓝色标识）在数据项I7、I912上已被授予锁；事务T1和T8在数据项I23上已被授予锁，T12等待被授予。T8在数据I44上已被授予锁，T1在数据I4上已被授予锁，事务T23等待数据I4被授予锁。

* 锁管理器处理请求的过程
  * 当一条锁请求消息到达时，如果相应数据项的链表存在，再该链表末尾增加一个记录；否则新建一个仅包含该请求记录的链表。
  * 当锁管理器收到一个事务的解锁消息时，删除与该事务对应的数据项链表中的记录，然后检查随后的记录，如果有，就判断该请求能否被授权；如果能，锁管理器授权该请求并处理后面的记录，如果还有记录就逐一处理。
  * 如果一个事务中止，锁管理器删除该事务产生的正在等待加锁的所有请求。一但数据库采取适当操作撤销该事务（redo/undo）,该中止事务持有的锁全被释放。
* 算法保证了锁请求**无“饿死”**现象

### Deadlock Handling

* 死锁预防prevention，一些策略如下
  * 每个事物开始执行前获得所有的锁，同时获得所有的锁来保证不会发生循环
    * 方法简单，但是事务开始执行之前很难预知哪些数据需要封锁；而且有些数据会长时间封锁，导致数据的使用效率降低。
  * 通过对加锁请求进行排序
    * 对所有的数据项强加一个次序，要求事务按照这个词序加锁。这种方法的一个变种是使用数据项与2PL封锁关联的全序，即一旦一个事务封锁了某个特定的数据型，就不能申请位于该数据项前面的数据项的锁
  * 每当等待有可能死锁时，事务回滚而不是等待加锁
    * 通过**抢占**或**回滚**来预防死锁。仍然使用加锁机制，但是个事务分配一个是时间戳，系统利用时间戳来决定事物等待还是回滚。有两种不同的死锁预防模式：
      * **wait-die**（non-preemptive非抢占）：**年长的事务等待**年轻的事务释放锁；年轻的事务不等待年长事务释放锁，而是rollback
      * **wound-wait**（preemptive抢占）：**年长事务强迫**年轻事务回滚，“抢占”锁资源；年轻事务会等待年长事务释放锁
  * Timeout
    * 申请锁的事务最多等待一段给定的时间，若此时段内未被授予锁，该事务超时，就rollback，避免了死锁。这种方法看似简单，但是确定等待时长很困难。
* 死锁检测detection
  * wait-for图
    * 是一个有向图。顶点代表系统中的所有事务，如果Ti-->Tj 有一条边，表示Ti等待Tj 释放锁。如wait-for图有环则进入死锁
* 死锁恢复recovery
  * 选择一个事务作为牺牲品，rollback。选择哪些事务rollback是个关键，采用**代价**最小的原则
    * 事务已经执行很久，还需要多长时间完成
    * 事务已经访问了多少数据项
    * 该事务还需访问多少数据项
  * 还要明确rollback多远
    * total rollback：中止事务重新开始
    * partial rollback： rollback到可以解除死锁处效率更高
  * 如果基于代价来选择牺牲的事务，有可能某个事务总被选上，导致这个事务无法完成，发生**饿死**情况
    * 一种解决的方法是设置rollback的次数限制

## Timestamp-Based Protocols 基于时间戳

* 每个事务分配一个时间戳（时间戳系统时钟或逻辑计数器），TS(Ti) <TS(Tj)表示事务Ti的时戳值<Tj 的时戳值，Ti 是老事务，Tj 是新事务
* 根据时间戳确定可串行化的顺序来控制并发执行的事务
* 每个数据项Q需要与两个时间戳关联
  * W-timestamp（Q）：成功执行write（Q）的所有事务的最大时间戳（**最新**的）
  * R-timestamp（Q）：成功执行read（Q）的所有事务的最大时间戳（**最新**的）
* 基于时间戳的并发控制协议保证**冲突的write和read**操作以**时戳的次序**执行的方式执行。（没看懂，现在看懂了）
  * 假设事务Ti发出read（Q）的操作：
    * 如果 TS(Ti) ＜ **W**-timestamp(Q), 那么 Ti 需要读的数据项Q的值已经**被覆盖**（Q的值更新过），因此这个**read** 操作被**拒绝**，Ti rolled back
    * 如果TS(Ti) >= **W**-timestamp(Q), 那么执行**read**操作**，**更新R-timestamp(Q) 为**max**(R-timestamp(Q), TS(Ti))
  * 假设事务Ti发出write（Q）的操作：
    * 如果 TS(Ti) ＜ **R**-timestamp(Q), 那么 Ti 更新后的数据项Q的值是先前需要读的值（先前读过），且系统已经假设该值不会再产生，因此这个**write** 操作被拒绝，Ti rolled back
    * 如果TS(Ti)＜**W**-timestamp(Q), 那么Ti试图写的值是过时的（最新值已经产生），因此write（Q）被拒绝，Ti Rollback
    * 否则，执行write操作，W-timestamp（Q）更新为TS(Ti)
* 协议可以保证**冲突串行化**，因为冲突操作可以按照时间戳排序
* 协议保证**不会出现死锁**，因为没有事务在等待
* 但是这个协议可能导致调度**不是无级联**的，且可能产生**不可恢复**的调度。可以通过以下方法来保证调度可恢复：
  * 在事务的末位执行所有的写操作，这些写操作必须正在执行的过程中，任何事务都不允许访问已写完的任何数据项
  * 通过受限的封锁形式来保证可恢复性和无级联性，对于未提交数据项的读操作推迟到更新该数据项的事务提交之后，即不要读到脏数据
  * 通过跟踪未提交写操作来保证可恢复性。事务Ti读取了其他事务所写的数据，只有在其他事务提交之后，Ti再提交

## Multiversion Schemes 多版本

商用DBMS常用的方法之一。这个方法应用的动机是：前面几种并发控制机制要么延迟一项操作，要么中止发出该操作的事务来保证可串行性。如果每一项数据的旧值也拷贝保存在系统中，可以用于并发控制

* 有两种实现方法：多版本**时间戳排序**和多版本**2PL**
* 在多版本并发控制机制中，每个write（Q）操作创建Q的一个新版本，版本用时间戳标记版本号。每当事务发出read（Q）操作时，并发控制管理器选择Q的一个合适版本进行读操作
* 多版本**时间戳**排序方法，在时间戳协议的基础上扩展
  * 每一个数据项Q有一个版本序列<Q1, Q2,...., Qm>，其中每个版本Qk包含三个字段
    * **Content** -- Qk的值
    * **W-timestamp**(Qk) – 创建或写 Qk版本的事务的时间戳；
    * **R-timestamp**(Qk) – 所有成功读Qk的事务的最大时间戳
  * 当事务Ti创建Q的新版本Qk时，Qk的W-timestamp 和R-timestamp 初始化为TS(Ti). 
  * 当事务Tj读Qk的值且TS(Tj) > R-timestamp(Qk)时，R-timestamp的值更新（Tj成功读了Qk，**R-timestamp**取最大值，就要更新）
* 多版本**两阶段加锁**机制将读操作和更新操作分开处理
  * 更新事务（写操作）请求**读**和**写**锁，而且一直持有直到事务提交。也就是说，遵守强2PL（rigorous two-phase locking），可以实现可串行化。数据项的每个版本有一个时间戳，不是系统时间，用一个计数器**ts-counte**来标记
  * **只读**事务在执行前，分配当前的计数器的值作为时间戳，然后遵守多版本时间戳协议进行读操作
  * 当一个**更新**事务（有写操作的事务）**读**一个数据时，获得共享锁后读最近的版本。当一个更新事务想要**写**数据时，获得互斥锁后**创建**数据项的新版本，并将版本的时间戳设置为无穷大
  * 当更新事务Ti完成后，提交处理时，将创建的数据版本的时间戳值设置为**ts-counter** + 1；这样在Ti完成**ts-counter**递增之后的只读事务会读到更新后的值，而在**ts-counter**递增之前的只读事务只能读到Ti更新操作之前的值
* MVCC的问题
  * 增加空间开销：多个版本的存储
  * 垃圾回收问题：旧版本不再需要时需要删掉

## Snapshot Isolation 快照隔离

* 是一种特殊的并发控制机制
* 在事务开始执行时，给他数据库的一个快照，事务在快照上操作，和其他并发事务隔离开，快照中的数据值仅包括已经提交的事务所写的值（持久化的）
* 动机是：在做一些决策支持查询时，会读大量的数据（大数据应用），可能会和修改少量数据的OLTP（联机事务处理）事务发生并发冲突。决策分析时可以忽略一些小量的更新数据
* 解决此类并发执行的方法（这两种方法都有一些问题，商用的DBMS有自己的实现变种）：
  * 对于只读的事务（查询应用），让他们访问数据库的一个逻辑快照；读写事务采用正常的加锁机制
  * 让所有事务访问访问数据库的快照，修改操作的事务采用2PL来保证避免并发修改。
* 事务T采用快照隔离方法执行时，
  * 执行开始时采用提交后数据的快照
  * 在快照上完成读和写操作
  * 并发事务的修改操作对事务T不可见
  * 事务T提交时完成写操作
* 两个并发的事务可能会更新同一个数据项，由于两个事务各自采用自己的快照，看不到对方的更新。如果允许两个事务写入数据库，就导致第一个事务的写入被第二个事务的写入覆盖，即更新丢失。必须预防这个现象发生。快照隔离方法的两个变种方法避免了更新丢失：**first committer wins**（先提交者获胜）和**first updater wins** （先更新者获胜）
* 按照**先提交者**获胜方法，当事务T进入部分提交状态，以下操作作为一个原子操作执行：
  * 检查是否有与事务T并发执行的事务，对于T打算写入的某些数据，该事务已经将更新写入数据库
  * 如果有这样的事务，T中止
  * 如果没有这样的事务，则T提交，并将更新写入数据库
* 按照**先更新者**获胜方法，系统采用仅用于更新操作的加锁机制（读操作不受影响）。当事务Ti试图更新数据时，请求该数据项的互斥锁，如果没有另一个并发事务持有该数据项的锁，则获得锁后执行：
  * 如果数据项被其他并发事务更新过，那么Ti中止；
  * 否则，Ti执行其操作，可能会提交（成功执行完）
* 若另一个并发事务Tj持有该数据的互斥锁，则Ti不能执行，执行以下步骤
  * Ti等待直到Tj提交或中止
  * 如果Tj中止，则释放锁，Ti获得锁，Ti执行上述的更新检查。如果有另一个事务更新过这个数据项，Ti中止，否则执行其操作。  
  * 如果Tj提交，Ti中止（不再更新数据）

## Other

Benefits of SI, Weak Levels of Consistency, Weak Levels of Consistency in SQL, Transaction across User Interaction

自学、了解

# Recovery System

## Failure Classification 故障分类

从数据管理的角度，本章考虑以下故障：

* 事务故障：两种错误导致事务故障，逻辑错误和系统错误，其中逻辑错误就是程序逻辑写错了；系统错误指DBMS本身原因导致的错误，如死锁等
* 系统崩溃：软硬件故障导致的系统中止，主存中的信息丢失。例如掉电、软件漏洞等
* 磁盘故障：磁盘设备故障，如读写头、磁盘本身写数据问题等

## Storage Structure

* 易失性存储：当系统故障时，其中的数据丢失，例如主存、cache等都属于易失性存储
* 非易失性存储：系统故障时，其中的数据不会丢失，例如磁盘、磁带、flash以及有电源支撑的RAM
* 稳定存储：是一种**理想**的存储器，可以在任何故障情况下不损坏数据，通常用异地保存多个**副本**的方式来实现，如两地三备份
  * 在建立多个副本的数据传输过程中，即从内存传输到磁盘的过程中，数据会发生不一致的情况：成功完成传输、部分失败、完全失败
  * 为了保护数据传输过程中存储介质免受故障的影响：可以采用将每个数据块分别写到两个物理数据块中的方法，步骤如下（了解即可）：
    * 将数据写到第一个物理块中；
    * 当第一个写操作成功完成之后，将同样的数据写到第二个磁盘块上
    * 当第二次写操作成功完成时，发出确认信息，完成数据传输
    * 如果在数据传输中系统发生故障，有可能导致两个拷贝的数据不一致，那么系统要恢复数据必须能够检测到不一致性且能调用故障恢复算法来恢复数据，操作如下
      * 找到不一致的数据块，有两种方式
        * 代价较大的方式：比较每一个数据块的两个拷贝
        * 代价较小的方法：跟踪正在写的数据块，将正在进行的数据块写操作记录到非易失性存储上，恢复时只需比较正在写的数据块
      * 如果其中一个数据块有错误，则用另一个数据块重写这个数据块；如果两个都没有错误，但是内容不同，就用第一块的内容覆盖第二块的内容

### Data Access

数据库系统常驻于非易失性存储器上（以磁盘和SSD为主）

* 事务由磁盘向主存输入信息，然后将信息输出到磁盘，输入和输出操作以数据块为单位，磁盘和主存之间的块移动由以下两个操作完成：
  * **input**(B) 将物理块B传输到主存
  * **output**(B) 将缓冲块B传输到磁盘，并替换磁盘上的相应的物理块
* 每个事务在内存中有自己的私有**工作区**，用于保存该事务访问及更新的所有**数据的拷贝**，该工作区在事务初始化时由系统创建。当事务提交或中止时，系统删除这个私有区域。事务Ti的工作区中保留的数据项X的拷贝称作xi。简单起见，假设每个数据项都能fit到一个数据块中
* 事务Ti通过其私有工作区和系统缓冲区之间传输数据，与数据库系统进行交互，使用以下两个操作完成数据传递：
  * **read**(X) 将数据项X的值赋予局部变量xi
  * **write**(X) 将局部变量xi的值赋予缓冲块中的数据项X
  * 两个操作都需要考虑数据块是否在主存中，如果不在就发指令**input**(BX) 
* 事务第一次访问数据时，必须执行**read**(X) ，之后的操作作用于局部的拷贝x，最后一次访问数据后，事务执行**write**(X)
* **output**(BX) 不必要在**write**(X)之后立刻执行（因为数据块B中可能还有其他正在被访问的数据），系统在合适的时候执行**output**(BX)将数据写到磁盘上

<img src="final_review_database.assets\image-20220824003539969.png" alt="image-20220824003539969" style="zoom:80%;" />

## Log-Based Recovery

系统发生故障后重新启动很重要的技术是恢复算法：确保数据库一致性、事务原子性和持久性的技术

* 恢复算法包括两个核心部分：
  * 在事务正常执行期间确保有足够的信息可以从故障中恢复而采取的操作
  * 故障后恢复数据库内容恢复到能够确保原子性、一致性和持久性的状态所采取的操作
* 为了达到保持事务原子性的目的，必须在稳定存储器中记录要修改的数据以及修改后的数据。这些信息可以帮助确保已提交的事务所做的修改都反映到数据库中，或故障恢复过程中反映到数据库中（持久），还能保证中止的事务所作的任何修改不会持久到数据库中
* 为了保证事务的**原子性**，需**先**将描述数据库修改的信息输出到稳定存储上而不修改数据库本身
* 日志是日志记录的序列，记录数据库所有更新数据的活动，包括以下字段：
  * 事务的标识
  * 数据项标识
  * 数据更新前的值
  * 数据更新后的值
* 在日志中，一些记录是
  * <Ti **start**> ，记录事务Ti的开始
  * <Ti, X, V1, V2>，记录事务Ti对数据项X进行了一个写操作，写操作前X的值是 V1，写操作之后更新为V2
  * <Ti  **commi**t>，记录事务Ti成功提交
  * <Ti  **abort**>，记录事务中止 
* 假设日志记录**直接写入稳定存储器**，不经过buffer
* 每当数据库有修改操作时，就在log中新增一个日志记录，有了日志，当事务必须终止的时候能够对所做的修改进行撤销；或者事务已经提交但在所做的修改结果持久到磁盘上的数据库之前系统崩溃时，对事务所做的修改重新做一次
* 如果一个事务执行了对**磁盘缓冲区或磁盘**的更新，则这个事务修改了数据库；如果仅仅更新了事务私有工作区中的数据，不算**数据库修改**
* 数据库修改有两种实现方式
  * **立即**数据库修改：允许未提交事务的更新在**事务提交前**更新缓冲区或磁盘中的数据。因此log中需要记录数据修改前的值和修改后的新值。在数据库中的数据修改发生之前，先更新log记录，即先记录要修改的数据项以及旧值和新值
  * 延迟数据库修改：不讲
* 如果事务的“commit” 这个**log记录**输出到了稳定存储上，所有commit指令之前的log记录已经输出到稳定存储上了，则一个事务**已提交**。但是事务执行的修改操作可能在事务提交时依然停留在buffer中，它会稍晚输出到持久存储上。（立即）

##  Recovery  Algorithms

* 采用立即数据库修改模式时，系统恢复算法有两个重要的操作：undo和redo
  * **undo**操作：将log记录 <Ti, X, V1, V2>中**旧**值写到X中，即将事务Ti更新过的所有数据设置成旧值
  * **redo**操作：将log记录 <Ti, X, V1, V2>中**新**值写到X中，即将事务Ti更新过的所有数据设置成新值
  * 两个操作都是等幂idempotent的，即执行多次的结果与执行一次的结果一样
* 当恢复系统故障时：
  * 如果日志包含\<Ti start>但**不包含 \<Ti commit>**，**undo**(Ti)
  * 如果日志既包含\<Ti start>又**包含** \<Ti commit>，**redo(**Ti)

### Checkpoints

当故障发生时，必须检查log文件，需要在log文件中进行搜索，确定哪些事务需要撤销，哪些事务需要重做，原则上需要搜索这个log来确定。但是大多数需要重做的事务已经将其更新持久化到数据库中，重复重做不会影响数据库中的数据，但是消耗时间，恢复时间加长了

* 解决方法：在log中加入checkpoint。执行checkpoint操作时
  * 将当前**主存中的日志**记录写到稳定存储上（持久化）
  * 将所有修改的**buffer**块写到磁盘上（持久化）
  * 在稳定存储器上加一条记录< **checkpoint** *L*>，其中L是执行checkpoint时**活跃active的事务**列表
* 当执行checkpoint操作时，所有更新操作都停止
* 利用checkpoint来恢复，只需考虑checkpoint之前最近的事务，然后从该事务开始的事务。这样search log时不必扫描所有的记录
  * 从log文件的末端开始反向扫描，知道找到最近的<checkpoint **L**> 记录
  * 继续反向扫描直到记录<Ti **start**> 出现
  * 只需考虑从这个start开始的log部分记录，再往前的记录不需要访问了
  * 对于所有的事务，如果没有<Ti **commit**>，则执行undo(Ti)（以立即数据库修改模式为例）
  * 继续正向扫描log，如果遇到 <Ti  **commit**>, 这个事务既有<Ti **start**> ，也有<Ti  **commit**>, 则执行**redo**(Ti)

<img src="final_review_database.assets\image-20220824154727215.png" alt="image-20220824154727215" style="zoom:80%;" />

* 找到checkpoint最近的事务的start记录，图中示意是T2。然后从这里开始正向扫描，log中T2有commit（图示），T3有commit，T4没有。所以redoT2、T3，undoT4

### 算法实现

* 正常运行，在log中追加
  * <Ti **start**> *，*事务开始执行时
  * <Ti,Xj, V1, V2>，事务每一次修改数据时
  * <Ti commit\> *，*事务结束时
* **事务回滚**
  * 从log末端反向扫描，对于Ti的每一个log记录<Ti, Xj, V1, V2>执行undo操作
  * 在log中加一条记录<Ti, Xj, V1>作为补偿记录compensation log records，不需要做undo
  * 一旦发现记录<Ti start\>，停止扫描log，在log中添加一条记录<Ti **abort**\>
* 系统崩溃后数据库系统重新启动，恢复分为两阶段
  * redo阶段：重新执行所有事务的修改操作，不论是有提交、中止或没有完成。(这一阶段输出输出**undo-list**)
    * 在log文件中，找到最近的<checkpoint *L*> 记录，将undo-list设置为*L*
    * 从<checkpoint *L*> 记录开始**正向**扫描log
      * 每当找到记录 <Ti, Xj, V1, V2> *，*redo（Ti）
      * 每当找到记录<Ti  **start**> 就将Ti 加到undo-list
      * 每当找到记录 <Ti **commit**> 或<Ti **abort**> ，将Ti 从undo-list中移除
  * undo阶段：撤销所有没有完成的事务，系统rollback undo-list中的所有事务
    * 从log文件的末尾**反向**扫描
    * 每当发现log记录 <Ti, Xj, V1, V2> ，其中Ti在undo-list中，就执行undo操作
    * 每当发现undo-list中事务Ti的log记录 <Ti **start**> ，执行下面的操作
      * 在log文件中追加一条记录<Ti  **abort**>
      * 将Ti从undo-list中删掉
    * 当undo-list为空时，系统找到了初始时undo-list中所有的事务的  <Ti **start**> 记录，undo阶段结束
    * undo阶段完成后，正常事务开始执行

**重要例题**
<img src="final_review_database.assets\image-20220824183102593.png" alt="image-20220824183102593" style="zoom:80%;" />

左边是log文件，记录了T0,T1和T2三个事务的数据操作信息，其中checkpoint的位置如图示意，L={T0,T1}。红色剪头处表示系统在插入<T0,abort>之后崩溃了。可以看到，系统恢复时，根据恢复算法：

* 在redo阶段
  * undo-list初始化为{T0，T1}，当发现T1的<T1，commit>时，将T1从undo-list中删除；
  * 当发现T2事务的<T2，start>s时，T2加入到undo-list中；
  * 当发现事务T0的<T0，abort>时，T0从undo-list中删掉，这时undo-list中只剩下T2
* 接下来开始undo阶段
  * 从log文件的末尾反向扫描log，发现undo-list中T2的数据更新记录<T2,A,500,400>
  * 执行undo（T2），数据A的恢复为500，并在log中加一条记录redo-only记录<T2,A,500>
  * 当发现<T2,start>记录时，将T2从undo-list中删除，在log中加一条记录<T2 abort\>
  * 这时undo-list为空，undo阶段完成，恢复工作完成



# 课纲

* 数据库基本概念
* 关系模型
  * 关系代数
  * SQL
* 数据库设计
  * 概念设计
  * 关系数据库设计
  * 规范化设计
  * denormalization
* 存储
  * block
  * IO
  * index
    * order
    * hash
* 查询
  * 算法
  * select
  * sort
  * join
  * 代价、评估、指标、优化
* 事务
  * 概念
  * ACID
  * 并发控制保证
    * 方法
    * 技术
      * 锁
      * timestamp
      * 多版本（讲得比较快，新型数据库比较普遍。结合教材再学一下）
* 恢复