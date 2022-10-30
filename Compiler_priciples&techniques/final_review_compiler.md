# 词法分析

## 子集构造法

* NFA-》DFA
* 步骤
  * 算出一个状态的闭包（跟着$\epsilon$走），标记为一个状态
  * 算出此状态的move元素后的闭包
  * 重复，直到没有更多状态

## Thompson 构造法

* R.E.构造NFA
  * 关键：
    <img src="final_review_compiler.assets\image-20220903143932067.png" alt="image-20220903143932067" style="zoom:80%;" />
    <img src="final_review_compiler.assets\image-20220903143951359.png" alt="image-20220903143951359" style="zoom:80%;" />
* 开销
  <img src="final_review_compiler.assets\image-20220903151118466.png" alt="image-20220903151118466" style="zoom:80%;" />

## 直接构造法

* followpos
  <img src="final_review_compiler.assets\image-20220903153651267.png" alt="image-20220903153651267" style="zoom:80%;" />
  * 知道上述信息之后，继续算出followpos
    <img src="final_review_compiler.assets\image-20220903160213007.png" alt="image-20220903160213007" style="zoom:80%;" />
    * 连接节点：lastpos(c1)中的每一个元素i的followpos(i)，均包括firstpos(c2)
    * 星节点：lastpos(n)中的所有元素i的followpos(i)，均包括firstpos(n)
    * 反复利用上面两条
  * 步骤
    * 先列出firstpos(root)标记为S0
    * S0中所有的a的数字i挑选出来，所有并上followpos(i)，标记为move(S0,a)=S1。b同理
    * 重复，直到没有更多状态
    * **注意：**不要标号$\epsilon$。不要遗忘followpos(4)={}

## Minimize the Number of States of DFA

* 第一步分为终止状态和其他状态
* 在其他状态集合中列举出来，是不是有一些状态，他们都move a、move b……都一样，那这些状态可以归为一类
* 分开之后继续在每一个集合里面重复上述过程，直到不能再分



# 语法分析

* 二义性：有多个最左或最右推导（不是和，因为任何一个都有一个最左和一个最右）
* 没有办法CFG去检查二义性，无法通用，依据经验。可以给规则一个优先级
* **左递归**：top-down解析无法处理左递归。
* 直接左递归，一眼看出来

## 消除左递归

<img src="final_review_compiler.assets\image-20220903234204341.png" alt="image-20220903234204341" style="zoom:67%;" />

间接左递归：做几次代换，再用直接左递归的方法消解

## 公共左因子

<img src="final_review_compiler.assets\image-20220904004359359.png" alt="image-20220904004359359" style="zoom:80%;" />

例如

<img src="final_review_compiler.assets\image-20220904004417459.png" alt="image-20220904004417459" style="zoom:80%;" />

## LL(1)

* L（从左到右扫描）L（最左推导）1（一次只看一个symbol来决定解析操作）
* 不能百分百保证
* 注意，如果所有都无法保证，则考虑有没有机会$\epsilon-production$
* 注意压栈的顺序，语法靠左的字符应该在栈顶
* 注意**先消除左递归、左因子**
* **FIRST**
  * 若X是终止符，FIRST(X)={X}
  * 若有$X\rightarrow \epsilon$，则FIRST(X).add($\epsilon$)
  * 若X是非终止符，产生式为$X\rightarrow Y_1Y_2...Y_k$
    * FIRST(X).add( FIRST(Y1) )
    * 若$\epsilon \in $FIRST(Y1)，FIRST(X).add( FIRST(Y2) )
    * 以此类推
* **FOLLOW**
  * 若S是开始符，FOLLOW(S).add( $ )
  * 若有$ A \rightarrow \alpha B \beta$，FOLLOW(B).add ( FIRST($\beta$) $-$ $\epsilon$ )
  * 若有$A \rightarrow \alpha B$，或者上一条中$\epsilon \in FIRST(\beta)$，FOLLOW(B).add( FOLLOW(A) )
  * 直到FOLLOW集不再更新
* **构造表**
  * 对FIRST($\alpha$)的所有终结符a，在M[A, a]中添加$A \rightarrow \alpha$
  * 若$\epsilon \in $FIRST($\alpha$)，对FOLLOW(A)中的所有终结符b（包括\$)，在M[A, b]中添加$A \rightarrow \alpha$
  * /*若$\epsilon \in$FIRST($\alpha$)且\$$\in$FOLLOW(A)，M[A, \$]中添加$A \rightarrow \alpha$\*/
  * 重复上述规则
* 如果LL(1)表中某一个框里包含多个Production Rule，则不满足LL(1)文法，可能有左递归或二义性。这二义性无法避免，只有做出来表了才知道
* 其他属性。对$A \rightarrow \alpha 和 A \rightarrow \beta$来说：
  * $\alpha 和 \beta$不能同时推导出相同开头的终结符。这通过预处理公共左因子解决
  * $\alpha 和 \beta$最多只有一个可以推导出$\epsilon$
  * 若$\beta$可以推导出$\epsilon$，则$\alpha$不能推导出以FOLLOW(A)中的终结符作为开头的string。即FIRST($\alpha$)和FOLLOW(A)不能有交集。（否则二义性）

## 错误处理

* Panic-Mode Error Recovery
  * 若栈顶是非终结符，和input不匹配，则栈顶pop？？？
  * 若栈顶是终结符，没有产生式，则栈顶pop，input忽略字符，直到和新栈顶匹配？？？
* Phrase-Level Error Recovery
* Error-Producitions（很少用）
* Global-Correction（太理想）



## Bottom up

* 句柄？
* <img src="final_review_compiler.assets\image-20220904194241117.png" alt="image-20220904194241117" style="zoom:80%;" />
* **LR(k)**
  * L（从左到右扫描）R（最右推导）k（一次看k个）
  * 没有二义性
* LR parser
  * <img src="final_review_compiler.assets\image-20220904195722770.png" alt="image-20220904195722770" style="zoom:80%;" />
    表达能力
  * SLR太简单
  * LR有冗余
  * LALR主流
  * 以上都是相同是工作，只是paring table不一样

## SLR(1) / LR(0)

* **Closure**
  * 初始，$I$中的每个LR(0)项都添加进closure($I$)中
  * 如果closure($I$)中包含$ A \rightarrow \alpha.B\beta$，则把clousure($I$).add( 所有B开头的产生式如$B \rightarrow \gamma$ )
  * 重复上面两条，直到closure($I$)没有更新元素
* **goto**(I, X)
  * 对$I$中的每一项$A \rightarrow \alpha.X\beta$，goto(I, X).add( **closure**({$A \rightarrow \alpha X.\beta$}) )
* **Collection集族**
  * C = closure( {S' $\rightarrow$ S )
  * 对C中的每一项$I$，对每一个符号X：
    * 若**goto**(I, X)非空，且不再C中，则C.add( goto(I, X) )
  * 重复上面一条规则直到C不再更新
* **action table**
  * 对$I_i里的A \rightarrow \alpha.a\beta$（a是终结符），goto($I_i$, a)=$I_j$，则action[i, A]=$shift \space j$
  * 对$I_i里的A \rightarrow \alpha.$，**A不是S'**，对**FOLLOW**(A)里的所有a，则action[i, a]=$reduce \space A \rightarrow \alpha$
  * 对$I_i里的S' \rightarrow S.$，则action[i,\$]=$accept$
* **goto table**
  * 对所有非终结符A，若goto($I_i$, A)=$I_j$，则goto[i, A]=j



## LR(1)&各种对比

* 为避免无效规约，状态需保留更多信息
* 表达能力最强
* 自己看例题

## LALR

* LookAhead LR
* SLR和LALR的状态数一样，但是LALR能识别更多的语法
* 合并后，表达能力不如LR(1)



# 语义分析

## 语法制导定义

* 属性文法=CFG+语义规则

* **综合**属性：一定来自于产生式左边的非终结符的属性，来自产生式右边的属性或自己的其他属性
* **继承**属性：来自于产生式右边，继承属性来自右边的其他属性或左边的属性。没啥规律 
* 同一个属性不可能既是综合又是继承
* 注释语法树：语法树+显示属性值
* S属性文法：语法制导定义只有综合属性
* L属性文法：既含有综合属性，还包含特定的继承属性(A->X1X2X3...Xj-1Xj...Xn，Xj的继承属性只能来自于Xj-1左侧的属性，或者A的继承属性)

## 翻译模式

* 如何做
* L属性文法
* 综合属性文法没有用到翻译模式，语义规则最后来做，bottom-up
* S属性文法的翻译模式，把语义规则用大括号括起来放到最后面即可。可有可无
* 继承属性，需要在symbol之前就算好属性值
* S属性文法，Bottomup好用
* topdown翻译
  * 消除左递归
    * 增加E‘并继承和综合属性
    * 继承传给综合
    * 综合还给E.val
  * <img src="final_review_compiler.assets\image-20220905203358091.png" alt="image-20220905203358091" style="zoom:80%;" />
    <img src="final_review_compiler.assets\image-20220905203452949.png" alt="image-20220905203452949" style="zoom:80%;" />
  * 
* Bottomup对L当然也可以做。
  * L通过代换翻译成S文法
  * 也不是所有L都可以做，翻译成value栈的时候可能会有就发现问题。why？
  * 注意占位符