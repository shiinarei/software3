| 这个作业属于哪个课程 | [计科1/2班](https://edu.cnblogs.com/campus/gdgy/CSGrade21-12)|
| ----------------- |:---------------:|
| 这个作业要求在哪里|[结对项目](https://edu.cnblogs.com/campus/gdgy/CSGrade21-12/homework/13016) |
| 这个作业的目标 | 与队友共同交流完成结对项目：四则运算生成器 |
### 团队成员
| 姓名 | 学号 | Github作业链接 |
| ------ | ------ | ------- |
| 苏建澎 | 3121005007 |[苏建澎：GitHub作业链接](https://github.com/spottern/JianPeng-Su/tree/main/%E8%BD%AF%E5%B7%A5%E4%BD%9C%E4%B8%9A3%EF%BC%9A%E7%BB%93%E5%AF%B9%E9%A1%B9%E7%9B%AE)|
|黎灿宇 | 3121004867 | [黎灿宇：GitHub作业链接](https://github.com/shiinarei) |


## 1.PSP表格
| PSP2.1 | Personal Software Process Stages | 预估耗时(分钟) | 实际耗时(分钟) |
| ------------- | --------------| ----------- | --------|
| **Planning** | 计划 | 60 | 30 |
| ·Estimate | 估计这个任务需要多少时间 | 60 | 30 |
| **Development** | 开发 | 700 | 1080 |
| ·Analysis | 需求分析(包括学习新技术) |200 | 300 |
| ·Design Spec | 生成设计文档 | 60 | 70 |
| ·Design Review | 设计复审 | 60 | 100 |
| ·Coding Standard | 代码规范(为目前的开发制定合适的规范) | 40 | 30 |
| ·Design | 具体设计 | 120 | 100 |
| ·Coding | 具体编码 | 200 | 300 |
| ·Code Review | 代码复审 | 60 | 180 |
| **Test** | 测试(自我测试，修改代码，提交修改) | 300 | 410 |
| ·Reporting | 报告 | 150 | 180 |
| ·Test Repor | 报告测试 | 30 | 30 |
| ·Size Measurement | 计算工作量 | 30 | 20 |
| ·Postmortem & Process Improvement Plan | 事后总结，并提出过程改进计划 | 90 | 180 |
| | 合计 | 1060 | 1520 |

## 2.设计要求文档
- 一：设计目的：设计一个四则运算程序，使得能生产1-10以内的加减乘除题目以及结果，并能进行结果判断。
- 二：设计要求：使得设计的题目不重复，并能生成任意量的题目，能进行异常检测。
- 三：算法设计：二叉树，判断树是否同构使得满足要求。能检测出用户操作异常，并将模块异常部分告知用户让其重新填写。
- 四：进行性能分析，并尝试提出更优新能方案。
- 五：环境配置：Python3.11/3.10

## 3.效能分析
###耗时检测情况：
-函数generate_num生成10次题目所花的时间
![](https://img2023.cnblogs.com/blog/3273975/202309/3273975-20230922110928070-1322647704.png)
函数generate_num生成10次题目所花的时间为0.0010001659393310547s
<br>
-整个程序的执行所花的时间为：
![](https://img2023.cnblogs.com/blog/3273975/202309/3273975-20230922111503827-1900367266.png)
生成10道题目整个程序所花的时间为：0.0010001659393310547s
<br>
可以发现，生成10道题的整个过程里，generate_num函数所花的时间和整个程序所花的时间几乎相同，可以认为花费时间的算法部分只有generate_num函数
从数值上看，平均每道题生成的时间约为0.0001s。换而言之，生成一万道题目差不多花费1s就能完成，效率可以说是很高的
<br>
###内存占用检测
由于generate_num函数和isOmorphic函数是在repeat()函数钟一齐被调用的，所以通过监测repeat函数的执行情况即可监测generate_num和isOmorphic这两个核心算法的情况：
![](https://img2023.cnblogs.com/blog/3273975/202309/3273975-20230922112813848-1382740085.png)
<br>
由结果可知，生成1000个问题的过程中，generate_num占用了约0.7MiB的内存，isOmorphic占用了约0.1MiB的内存
即平均生成每道题generate_num函数占用的内存为0.0007MiB，isOmorphic占用了约0.0001MiB的内存，对于一个程序来说几乎可以忽略不计
<br>
###内存泄漏检测
整个程序运行得内存泄漏情况如下：  
![](https://img2023.cnblogs.com/blog/3273975/202309/3273975-20230922114026543-1949567443.png)
<br>
从结果可知，该程序的内存泄漏情况是比较严重的，原因是程序执行过程钟占用的内存没有及时释放
<br>
###总结
优点：整个程序在时间效率上是很高的，足以应对普通的商业需求。同时，程序在执行的过程中内存占用也几乎可以忽略不计，对运行的硬件的要求极低。
不足：本程序的内存泄漏情况严重，可以在释放内存的功能上做一些完善。
<br>
## 4.代码设计
- 整体代码设计思路流程图如下图所示：
![整体设计流程图](https://img2023.cnblogs.com/blog/3270350/202309/3270350-20230921102228448-1185042578.png)
以下是程序所定义的函数以及类
- 1)定义一个数字结构体生成随机产生的分数
```
class N_S:  # 数字结构体
    def __init__(self):
        self.number_sign = 0  # 数的类型
        self.numerator = 0  # 分子
        self.denominator = 1  # 分母

    def generate_number_sign(self):  # 随机生成数的类型，类型值为1时分母默认为1
        self.number_sign = random.randint(1, 2)

    def generate_numerator(self, min, max):  # 随机生成分子的大小
        self.numerator = random.randint(min, max)

    def generate_denominator(self, min, max):  # 随机生成分母的大小
        self.denominator = random.randint(min, max)
 ```
- 2)定义一个二叉树,并定义相关算法，进行中序遍历
```
class B_T:  # 二叉树结构体,以及相关
    def __init__(self, rootObj):
        self.key = rootObj  # 将运算符作为根节点
        self.leftChild = None
        self.rightChild = None

    def insert_left(self, newRoot):  # 插入左子树
        if (self.leftChild == None):
            self.leftChild = B_T(newRoot)
        else:
            t = B_T(newRoot)
            t.leftChild = self.leftChild
            self.leftChild = t

    def insert_right(self, newRoot):  # 插入右子树
        if (self.rightChild == None):
            self.rightChild = B_T(newRoot)
        else:
            t = B_T(newRoot)
            t.rightChild = self.rightChild
            self.rightChild = t

    def get_leftChild(self):  # 返回左孩子
        return self.leftChild

    def get_rightChild(self):  # 返回右孩子
        return self.rightChild

    def set_root(self, Obj):  # 修改当前结点的内容
        self.key = Obj

    def get_Root(self):  # 返回当前结点的内容
        return self.key

    def inorder(self):  # 中序遍历
        if self.leftChild:
            self.leftChild.inorder()
        if self.key:
            num.append(self.key)
        if self.rightChild:
            self.rightChild.inorder()
```
| 函数名 | 传递变量 | 返回值 | 函数功能 |
| ----- | ----- | ---- | --- |
| isOmorphic | (t1, t2)传递两个树 | TRUE/FALSE |判断树是否同构，防止出现重复的题目 |
| generate_operator | NULL | '+' '-' '*' '/' | 随机生成运算符号‘+’，‘-’，‘*’，‘/’ |
| num_size | NULL | 随机运算长度为1，2或3 | 随机生成题目长度 |
| generate_num | (min, max)题目范围的上下限 | 返回一个树t | 随机生成题目 |
| repeat | (n, min, max)n为生成题目的数量 | NULL | 多次生成不同的题目 |
| str_num | t(树进行中序遍历的结果) | 字符串result1存储到txt文件，result2进行结果运算 | 将num列表转换为字符串，并方便后续运算 | 
| main | NULL | NULL | 主函数引导过程，输出结果 | 
- 3）主要函数算法解析
- 3.1）isOmorphic(t1, t2)：  
![isOmorphic](https://img2023.cnblogs.com/blog/3270350/202309/3270350-20230921214806849-923228949.png)
![isOmorphic](https://img2023.cnblogs.com/blog/3270350/202309/3270350-20230921214827740-2011374161.png)
```
算法设计过程：
 函数isOmorphic接受两个参数t1和t2，分别表示两棵树的根节点。首先判断t1和t2是否都为None，如果是则返回True，表示两棵树都为空树，结构相同。如果t1和t2中有一个为空树而另一个不为空树，则返回False，表示两棵树结构不同。
接下来判断t1和t2的根节点的值是否相同，如果不同则返回False，表示两棵树结构不同。如果根节点的值相同，则分三种情况进行判断：
  一、如果t1和t2的左子树都为空，则递归判断t1和t2的右子树是否同构。
  二、如果t1和t2的左子树都不为空，且左子树的根节点的值相同，则递归判断t1和t2的左子树和右子树是否同构。
  三、如果t1和t2的左子树不同时为空，且左子树的根节点的值不同，则递归判断t1的左子树和t2的右子树是否同构，以及t1的右子树和t2的左子树是否同构。
  最终返回True或False，表示两棵树是否同构。
```
- 3.2) generate_num(min, max)：
![generate_num](https://img2023.cnblogs.com/blog/3270350/202309/3270350-20230921220032483-1110727554.png)
```
算法设计过程：
参数：生成题目的上max下min限
1. 首先调用 `num_size()` 函数生成题目长度 `size`。
2. 如果题目长度为2，随机生成一个运算符号 `operator`，并将其插入二叉树 `t` 的根节点。
3. 生成两个参与运算的数 `a` 和 `b`，并随机生成它们的类型（正数或分数）和大小（分子和分母）。
4. 将数 `a` 插入二叉树 `t` 的左子树，将数 `b` 插入右子树。
5. 如果题目长度为1或3，随机生成一个运算符号 `operator`，并将其插入二叉树 `t` 的根节点。
6. 如果题目长度为1，再随机生成一个运算符号 `operator`，并将其插入二叉树 `t` 的左子树。
7. 生成三个参与运算的数 `a`、`b` 和 `c`，并随机生成它们的类型和大小。
8. 将数 `c` 插入二叉树 `t` 的右子树。
9. 将数 `a` 插入二叉树 `t` 的左子树。
10. 将数 `b` 插入二叉树 `t` 的左子树的右子树。
11. 如果题目长度为3，再随机生成一个运算符号 `operator`，并将其插入二叉树 `t` 的右子树。
12. 将数 `b` 插入二叉树 `t` 的右子树的左子树。
13. 将数 `c` 插入二叉树 `t` 的右子树的右子树。
14. 返回生成的二叉树 `t`。
```
- 3.3）str_num(t)：
![str_num](https://img2023.cnblogs.com/blog/3270350/202309/3270350-20230921221130451-2077693801.png)
```算法设计过程
参数：生成题目的树t的中序遍历结果
1. 定义一个空列表 `num`，用于存储生成的题目字符及计算数字。
2. 如果列表 `num` 的长度为5，即已经生成了3个数字和2个运算符，取出根节点在列表中的位置。
3. 如果第二个字符是乘除，第一个字符是加减，则在加减的部分加上括号。
4. 如果第一个字符和第三个字符不相同，则在第二个运算符的两侧加上括号。
5. 根据列表 `num` 的长度，生成题目的显示结果 `result1` 和电脑进行运算的结果 `result2`。
6. 将 `result1` 和 `result2` 作为元组返回。
```
- 3.4）repeat(n, min, max)：
![repeat](https://img2023.cnblogs.com/blog/3270350/202309/3270350-20230921221928449-1493456329.png)
```算法过程如下
参数：生成题目的数量n，下限min，上限max
1. 定义一个循环，当 `n` 不等于0时，执行循环体。
2. 调用 `generate_num` 函数生成一个随机数 `t`，该随机数的范围在 `min` 和 `max` 之间。
3. 如果 `title` 列表的长度为0，说明是第一次生成题目，将 `t` 添加到 `title` 列表中，并将 `n` 减1。
4. 如果 `title` 列表的长度不为0，说明已经生成过题目，需要判断新生成的 `t` 是否与之前的题目重复。
5. 遍历 `title` 列表，对于每个已经生成的题目，调用 `isOmorphic` 函数判断新生成的 `t` 是否同构于该题目。
6. 如果新生成的 `t` 与某个已经生成的题目同构，则继续循环，生成新的随机数。
7. 如果新生成的 `t` 与所有已经生成的题目都不同构，则将 `t` 添加到 `title` 列表中，并将 `n` 减1。
8. 循环结束后，函数执行完毕。
```
- 3.5）main()：
![main](https://img2023.cnblogs.com/blog/3270350/202309/3270350-20230921223259508-2020638397.png)
```
算法过程如下
1. 定义 `main` 函数，该函数是程序的主要逻辑。
2. 通过 `input` 函数获取用户输入的功能选项，如果输入不是1或2，则提示用户重新输入。
3. 如果用户选择生成题目，则删除之前已有的文件，避免出现文本重叠的情况。
4. 通过 `input` 函数获取用户输入的题目数量 `n`，数值下限 `min` 和数值上限 `max`。
5. 进行模块异常检验，如果输入不符合要求，则提示用户重新输入。
6. 打开 `Exercises.txt` 和 `Answers.txt` 文件，以追加模式写入题目和答案。
7. 调用 `repeat` 函数生成题目，并将题目和答案写入文件。
8. 遍历 `title` 列表，对于每个题目，将其转换为字符串形式，并将其写入 `Exercises.txt` 文件。
9. 将答案字符串转换为表达式，通过 `eval` 函数计算表达式的值，并将其写入 `Answers.txt` 文件。
10. 如果用户选择自动查答案，则删除之前已有的文件，避免出现文本重叠的情况。
11. 打开 `Exercises.txt` 和 `Answers.txt` 文件，以读取模式读取题目和答案。
12. 逐行读取题目和答案，对于每个题目，将其转换为表达式，并通过 `eval` 函数计算表达式的值。
13. 如果计算结果与答案的差小于 `LIMIT`，则将该题目的编号添加到 `Correct` 列表中，否则将其添加到 `Wrong` 列表中。
14. 将 `Correct` 和 `Wrong` 列表转换为字符串形式，并将其写入 `Grade.txt` 文件。
15. 程序执行完毕。 
```
## 4.测试结果
- 1.上下限1-9，生成10道题目：
生成题目结果Exercises.txt：
![](https://img2023.cnblogs.com/blog/3270350/202309/3270350-20230921233341735-183973316.png)
题目对应结果Answer.txt：
![](https://img2023.cnblogs.com/blog/3270350/202309/3270350-20230921233350896-1027558065.png)
输入结果：（第三题和第七题答案出错）
![](https://img2023.cnblogs.com/blog/3270350/202309/3270350-20230921233417947-1833925964.png)
检查结果：Grade.txt内容：
![](https://img2023.cnblogs.com/blog/3270350/202309/3270350-20230921233437613-1596776613.png)
- 2.上下限1-9，生成100道题目：
生成题目结果Exercises.txt：
```
1. 3 - 9 - 9
2. ( 4÷9 + 2 ) ÷ 5
3. 6 ÷ ( 5÷9 - 4 )
4. 1 * 3/2
5. 8 - 1/2
6. 7 ÷ ( 3 + 3 )
7. 7 * ( 8 + 9 )
8. 6 + ( 7 / 1÷5 )
9. ( 4 + 7÷2 ) * 9
10. 2/5 - ( 2 + 5 )
11. 1 + 2
12. 4 - 1
13. 7/5 + ( 2 - 8÷3 )
14. 5/9 ÷ ( 5 + 1 )
15. 2 * 3/2 * 1
16. 2/7 * 1 * 1
17. 1 + 9
18. 4 * 4
19. 2/9 ÷ 4 ÷ 8
20. 8 - ( 8 + 7÷4 )
21. 1 * 5/7
22. ( 1÷7 - 2 ) ÷ 5/7
23. 3 ÷ 4
24. 1/9 * 1/2 * 1
25. 7/6 * ( 2÷3 / 7÷9 )
26. 6 ÷ ( 9 - 7÷4 )
27. 3/4 - 5
28. 3 ÷ ( 1 + 1÷2 )
29. 3 ÷ 9 ÷ 9
30. 5/3 - 9/4 - 7/6
31. 2 + ( 9÷8 / 5÷2 )
32. 1 ÷ 5 ÷ 4
33. ( 4 + 4÷7 ) ÷ 2
34. 2 - 7
35. 5/9 ÷ 1 ÷ 2/3
36. 7 - ( 1 + 4÷9 )
37. 2 ÷ 3
38. 9 + ( 5÷8 * 9÷7 )
39. 9 - 2
40. 4/9 + ( 5 / 6 )
41. 7/8 - 3 - 6
42. 8/3 - 7
43. 1/3 * 8
44. 5 * 8
45. 2/7 * 7
46. 6/5 ÷ 7/2
47. 7 + ( 2÷9 - 3÷2 )
48. 1 * 9/5
49. 1 * 9
50. 1/2 * ( 1÷5 + 7 )
51. 5/4 ÷ 1 ÷ 6
52. 2 ÷ ( 3÷4 * 4÷5 )
53. 3/4 * 8/5
54. 9/2 + ( 7 - 9 )
55. 2 + 7/6 + 4/3
56. 1/4 ÷ 9 ÷ 5
57. 2/7 + 3 + 2
58. 7/4 + 8/5
59. 4/3 - 2 - 6
60. 7 * 9
61. 3/8 ÷ 5/2
62. 9/5 ÷ 8
63. 8 - 8
64. 5 ÷ 7 ÷ 9
65. 3/2 - 8
66. 5/9 * 9
67. 2 ÷ ( 8 * 3 )
68. ( 8÷9 - 5 ) ÷ 5
69. 4 - 5 - 2
70. 3 - 6
71. 6 - 8
72. 9 ÷ 6 ÷ 7
73. 3 ÷ ( 5 * 9 )
74. 4 + 9
75. 8/9 + 5/7
76. 1 + 5
77. 8/3 + 4 + 1/5
78. 1 * ( 5÷4 + 3÷8 )
79. ( 6 - 2 ) * 5
80. 2/3 - ( 3 * 1 )
81. 5/9 ÷ ( 9÷2 * 6 )
82. 2/9 - 3
83. ( 3 - 5÷7 ) * 4
84. 4 + 3 + 3/5
85. 1 - 2
86. 2/5 * ( 2 / 1 )
87. 5 - ( 6÷7 / 3 )
88. ( 3÷8 - 6 ) * 2
89. 4/5 - ( 9÷5 / 3÷2 )
90. 5 - 7/9 - 8/3
91. 3/5 - 4
92. 1 + 2/3 + 1/5
93. 2/3 - 6
94. 7/8 * 8
95. 7/3 - ( 1÷3 + 1÷2 )
96. 1 ÷ 2/3
97. 9/4 * ( 6 - 5 )
98. 5 ÷ ( 1÷6 * 6 )
99. 1 * 3 * 9
100. 3/2 + 4
```
题目对应结果Answer.txt：
```
1. -15
2. 22/45
3. -54/31
4. 3/2
5. 15/2
6. 7/6
7. 119
8. 41
9. 135/2
10. -33/5
11. 3
12. 3
13. 11/15
14. 5/54
15. 3
16. 2/7
17. 10
18. 16
19. 1/144
20. -7/4
21. 5/7
22. -13/5
23. 3/4
24. 1/18
25. 1
26. 24/29
27. -17/4
28. 2
29. 1/27
30. -7/4
31. 49/20
32. 1/20
33. 16/7
34. -5
35. 5/6
36. 50/9
37. 2/3
38. 549/56
39. 7
40. 23/18
41. -65/8
42. -13/3
43. 8/3
44. 40
45. 2
46. 12/35
47. 103/18
48. 9/5
49. 9
50. 18/5
51. 5/24
52. 10/3
53. 6/5
54. 5/2
55. 9/2
56. 1/180
57. 37/7
58. 67/20
59. -20/3
60. 63
61. 3/20
62. 9/40
63. 0
64. 5/63
65. -13/2
66. 5
67. 1/12
68. -37/45
69. -3
70. -3
71. -2
72. 3/14
73. 1/15
74. 13
75. 101/63
76. 6
77. 103/15
78. 13/8
79. 20
80. -7/3
81. 5/243
82. -25/9
83. 64/7
84. 38/5
85. -1
86. 4/5
87. 33/7
88. -45/4
89. -2/5
90. 14/9
91. -17/5
92. 28/15
93. -16/3
94. 7
95. 3/2
96. 3/2
97. 9/4
98. 5
99. 27
100. 11/2
```
输入结果：（修改15、21题结果）
```
1. -15
2. 22/45
3. -54/31
4. 3/2
5. 15/2
6. 7/6
7. 119
8. 41
9. 135/2
10. -33/5
11. 3
12. 3
13. 11/15
14. 5/54
15. -3
16. 2/7
17. 10
18. 16
19. 1/144
20. -7/4
21. -5/7
22. -13/5
23. 3/4
24. 1/18
25. 1
26. 24/29
27. -17/4
28. 2
29. 1/27
30. -7/4
31. 49/20
32. 1/20
33. 16/7
34. -5
35. 5/6
36. 50/9
37. 2/3
38. 549/56
39. 7
40. 23/18
41. -65/8
42. -13/3
43. 8/3
44. 40
45. 2
46. 12/35
47. 103/18
48. 9/5
49. 9
50. 18/5
51. 5/24
52. 10/3
53. 6/5
54. 5/2
55. 9/2
56. 1/180
57. 37/7
58. 167/20
59. -20/3
60. 63
61. 3/20
62. 9/40
63. 0
64. 5/63
65. -13/2
66. 5
67. 1/12
68. -37/45
69. -3
70. -3
71. -2
72. 3/14
73. 1/15
74. 13
75. 101/63
76. 6
77. 103/15
78. 13/8
79. 20
80. -7/3
81. 5/243
82. -25/9
83. 64/7
84. 38/5
85. -1
86. 4/5
87. 33/7
88. -45/4
89. -2/5
90. 14/9
91. -17/5
92. 28/15
93. -16/3
94. 7
95. 3/2
96. 3/2
97. 9/4
98. 5
99. 27
100. 11/2
```
检查结果：Grade.txt内容：
```
Correct: 98(1,2,3,4,5,6,7,8,9,10,11,12,13,14,16,17,18,19,20,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100)
Wrong: 2(15,21)
```
- 3.测试上限1-2，生成20道题目
生成题目结果Exercises.txt：
```
1. 1/2 - 2
2. 2 + ( 2 / 1 )
3. 2 * ( 1 + 1 )
4. 2 - 2
5. 2 - ( 2 / 1 )
6. 2 * 1 * 1/2
7. 1 - ( 1 + 1 )
8. ( 2 - 2 ) ÷ 1
9. 2 ÷ 1
10. 2 - 2 - 2
11. 2 + 2
12. 2 ÷ ( 2 + 1 )
13. 1 * ( 2 / 1 )
14. 1/2 - 1
15. 2 + ( 2 * 2 )
16. 2 * 1 * 1
17. 1 * 1
18. 2 ÷ ( 2 * 2 )
19. 2 * ( 1 - 2 )
20. 2 * ( 1 / 1÷2 )
```
题目对应结果Answer.txt：
```
1. -3/2
2. 4
3. 4
4. 0
5. 0
6. 1
7. -1
8. 0
9. 2
10. -2
11. 4
12. 2/3
13. 2
14. -1/2
15. 6
16. 2
17. 1
18. 1/2
19. -2
20. 4
```
输入结果：（9,15,20题修改）
```
1. -3/2
2. 4
3. 4
4. 0
5. 0
6. 1
7. -1
8. 0
9. 1
10. -2
11. 4
12. 2/3
13. 2
14. -1/2
15. 4
16. 2
17. 1
18. 1/2
19. -2
20. 0
```
检查结果：Grade.txt内容：
```
Correct: 17(1,2,3,4,5,6,7,8,10,11,12,13,14,16,17,18,19)
Wrong: 3(9,15,20)
```
- 受篇幅影响只显示出三次测试结果：我们发现测试结果均无误且程序性能良好，能正确生成题目以及结果，并对二次做题后的结果进行批改。因此程序无误。

## 5.模块异常分析
- 1）当出现非法访问操作时：
![](https://img2023.cnblogs.com/blog/3270350/202309/3270350-20230921234814236-1965673562.png)
需要重新输入操作。
- 2）输入上下限错误时：
一、下限输入错误（超过10）：
![](https://img2023.cnblogs.com/blog/3270350/202309/3270350-20230921235044845-496235858.png)
二、上限输入错误（超过10）：
![](https://img2023.cnblogs.com/blog/3270350/202309/3270350-20230921235117619-39557141.png)
三、下限大于上限：
![](https://img2023.cnblogs.com/blog/3270350/202309/3270350-20230921235147436-904562286.png)
四、下限小于0：
![](https://img2023.cnblogs.com/blog/3270350/202309/3270350-20230921235241626-421665270.png)
当出现以上操作时会提示用户重新输入上下限。

## 6.Code Quality Analysis
![](https://img2023.cnblogs.com/blog/3270350/202309/3270350-20230922083038995-1388101503.png)
![](https://img2023.cnblogs.com/blog/3270350/202309/3270350-20230922083057280-223554931.png)
根据分析结果可知，以上都是弱警告，对于程序的运行不会有影响，但是对于代码的可读性可能有一定困难，但由于笔者已经对关键部分的算法和变量进行了注释，因此影响不大。
## 7.需要优化的部分
- 1.当需要出现大量的数据时，例如生成100000000条题目，回产生程序无法正常运行，长期处于运作状态。
- 2.当运算长度为3且出现括号时，生成的题目/号无法正常显示，而直接显示为÷。对于做题人无影响，但会降低程序效率。
- 3.代码过长，部分代码出现冗余，造成可读性上的困难。

- 解决方案的提出：优化算法，减少所需的运行内存，避免出现题目量过大时出现死循环。对于出现÷的部分优化用replace进行替代。把冗余和相同的部分写为一个新的函数，提高代码的可读性。

## 8.附录
```
import random
from fractions import Fraction
from memory_profiler import profile
import time
import os
import glob
```

## 9.项目小结：
1.团队分工：
| 姓名 | 分工 |
| ---- | ---- |
| 苏建澎 | 负责完成算法代码，博客园算法部分的编写，模块异常部分以及测设用例分析 | 
| 黎灿宇 | 负责对程序进行性能分析，优化相关代码，设计程序界面，完成博客园性能分析以及优化部分 |

2.团队设计流程：
- 1）团队两人共同对于项目的要求进行解读，对相关算法进行寻找，并制定项目文档要求。
- 2）苏建澎完成相关项目的算法设计编写，使得程序能完成基本要求和模块异常处理。
- 3）一名成员对于已有的程序代码进行测试训练，对于相关的bug或者优化部分及时告知另一名设计人员进行修改完善。
- 4）黎灿宇对已有的算法进行性能分析，对性能分析结果进行整理，尝试优化代码。
- 5）二人测试极限数据，测试程序在极端情况的结果，及时发现程序的bug。
- 6）二人共同编写博客园报告。

3.项目团队经验：
- 1）首先团队两人能及时进行沟通，进度最好实时同步，防止一人修改了代码，另一人无法收到实时的结果，造成工作量无故增加。
- 2）测试代码需要认真细致，编程人员有时候无法及时的关注到作为用户的使用情况，此时需要另一名成员进行测试并及时反馈情况。
- 3）团队中一人进行前一步骤的时候，另外一人可以帮助该成员进行搜索，从不同角度尝试各团队成员提供更高效的算法和结果。
- 4）团队中的成员能及时采纳其他成员的正确建议，并实时沟通分析。
- 5）每一个成员都是项目中不可缺少的一环，都有需要自己主要承担的部分，碰到无法解决的问题时能及时告知其他成员寻求帮助。
- 6）每一个成员都从非自己主要负责的板块中进行学习，提高个人能力。
- 7）不摆烂，不躺平，分工明确，及时沟通才能完成一个团队项目。

4.团队成员的闪光点：
- 黎灿宇同学：对于不熟悉的算法能及时学习了解，对于项目中的发现的异常能及时告知另一名成员。不断汲取不同的知识要点，认真细致，从不同角度给另一名成员帮助，例如：测试代码的bug，极限数据，从不同角度给团队成员提供便利，给全部代码补充注释等。
- 苏建澎同学：编程水平熟练，对于需求分析准确，并能够将需求转变为模块化的函数。在工作上积极主动承担，对工作有详细的安排和时间规划，工作效率高，。积极交流跟进进度以及代码问题，促进团队氛围融洽。善于学习新知识,例如：利用二叉树实现查重算法、使用markdown语言编写流程图等等。

## 10.详细代码
from fractions import Fraction
import os
import glob
import random
from fractions import Fraction
import time
from memory_profiler import profile
from pympler import muppy
from pympler import summary
import pstats
import cProfile
num = []  # 用于储存生成题目的字符及计算数字
title = []  # 用于储存生成题目的二叉树


class N_S:  # 数字结构体
    def __init__(self):
        self.number_sign = 0  # 数的类型
        self.numerator = 0  # 分子
        self.denominator = 1  # 分母

    def generate_number_sign(self):  # 随机生成数的类型，类型值为1时分母默认为1
        self.number_sign = random.randint(1, 2)

    def generate_numerator(self, min, max):  # 随机生成分子的大小
        self.numerator = random.randint(min, max)

    def generate_denominator(self, min, max):  # 随机生成分母的大小
        self.denominator = random.randint(min, max)


class B_T:  # 二叉树结构体,以及相关
    def __init__(self, rootObj):
        self.key = rootObj  # 将运算符作为根节点
        self.leftChild = None
        self.rightChild = None

    def insert_left(self, newRoot):  # 插入左子树
        if (self.leftChild == None):
            self.leftChild = B_T(newRoot)
        else:
            t = B_T(newRoot)
            t.leftChild = self.leftChild
            self.leftChild = t

    def insert_right(self, newRoot):  # 插入右子树
        if (self.rightChild == None):
            self.rightChild = B_T(newRoot)
        else:
            t = B_T(newRoot)
            t.rightChild = self.rightChild
            self.rightChild = t

    def get_leftChild(self):  # 返回左孩子
        return self.leftChild

    def get_rightChild(self):  # 返回右孩子
        return self.rightChild

    def set_root(self, Obj):  # 修改当前结点的内容
        self.key = Obj

    def get_Root(self):  # 返回当前结点的内容
        return self.key

    def inorder(self):  # 中序遍历
        if self.leftChild:
            self.leftChild.inorder()
        if self.key:
            num.append(self.key)
        if self.rightChild:
            self.rightChild.inorder()


# 判断树是否同构，防止出现重复的题目
def isOmorphic(t1, t2):
    if t1 == None and t2 == None: return True
    if (t1 != None and t2 == None) | (t1 == None and t2 != None):
        return False
    if t1.key != t2.key:
        return False
    if t1.leftChild == None and t2.leftChild == None:
        return isOmorphic(t1.rightChild, t2.rightChild)
    if t1.leftChild != None and t2.leftChild != None and t1.leftChild.key == t2.leftChild.key:
        return isOmorphic(t1.leftChild, t2.leftChild) and isOmorphic(t1.rightChild, t2.rightChild)
    else:
        return isOmorphic(t1.leftChild, t2.rightChild) and isOmorphic(t1.rightChild, t2.leftChild)


# 随机生成运算符号‘+’，‘-’，‘*’，‘/’
def generate_operator():
    operators = ['+', '-', '*', '/']
    return random.choice(operators)


# 随机生成题目长度
def num_size():
    size = [1, 2, 3]
    return random.choice(size)


# 随机生成题目
def generate_num(min, max):
    size = num_size()
    # 如果题目长度为2（1+2的情况）
    if size == 2:
        operator = generate_operator()  # 随机生成运算符号
        t = B_T(operator)  # 将随机生成的运算符插入二叉树作为根节点
        a = N_S()  # 参与运算的数a结构体
        b = N_S()  # 参与运算的数b结构体
        a.generate_number_sign()  # 参与运算的数a
        if a.number_sign == 1:  # 随机生成数的类型，类型值为1时分母默认为1
            a.generate_numerator(min, max)  # 随机生成分子的大小
        else:
            a.generate_numerator(min, max)  # 随机生成分子的大小
            a.generate_denominator(min, max)  # 随机生成分母的大小
        b.generate_number_sign()  # 参与运算的数2
        if b.number_sign == 1:
            b.generate_numerator(min, max)  # 随机生成分子的大小
        else:
            b.generate_numerator(min, max)  # 随机生成分子的大小
            b.generate_denominator(min, max)  # 随机生成分母的大小
        t.insert_left(Fraction(a.numerator, a.denominator))  # 将数a插入左子树
        t.insert_right(Fraction(b.numerator, b.denominator))  # 将数b插入右子树

    # 如果题目长度为1或3
    else:
        operator = generate_operator()  # 随机生成运算符号1
        t = B_T(operator)  # 将随机生成的运算符1插入二叉树作为根节点
        # 如果题目长度为1（1+2+3的情况）
        if size == 1:
            operator = generate_operator()  # 随机生成运算符号2
            t.insert_left(operator)  # 将该运算符2插入左子树
            a = N_S()  # 参与运算的数a结构体
            b = N_S()  # 参与运算的数b结构体
            c = N_S()  # 参与运算的数c结构体

            c.generate_number_sign()  # 随机生成数c的类型
            # 如果生成数c的类型为1
            if c.number_sign == 1:
                c.generate_numerator(min, max)  # 随机生成分子的大小（分母在该情况下为1）
            else:
                c.generate_numerator(min, max)  # 随机生成分子的大小
                c.generate_denominator(min, max)  # 随机生成分母的大小
            t.insert_right(Fraction(c.numerator, c.denominator))  # 将生成的数c插入右子树

            t1 = t.leftChild  # t1指向左子树
            a.generate_number_sign()  # 随机生成数a的类型
            if a.number_sign == 1:
                a.generate_numerator(min, max)
            else:
                a.generate_numerator(min, max)
                a.generate_denominator(min, max)
            t1.insert_left(Fraction(a.numerator, a.denominator))  # 将生成的数a插入左子树的左子树

            # 随机生成数b的类型
            b.generate_number_sign()
            if b.number_sign == 1:
                b.generate_numerator(min, max)
            else:
                b.generate_numerator(min, max)
                b.generate_denominator(min, max)
            t1.insert_right(Fraction(b.numerator, b.denominator))  # 将生成的数a插入左子树的右子树

        # 如果题目长度为3（3+2+1的情况）
        else:
            operator = generate_operator()  # 随机生成运算符2
            t.insert_right(operator)  # 将运算符2插入右子树
            a = N_S()
            b = N_S()
            c = N_S()

            a.generate_number_sign()  # 随机生成数a的类型
            if a.number_sign == 1:  # 如果数a的类型为1
                a.generate_numerator(min, max)
            else:
                a.generate_numerator(min, max)
                a.generate_denominator(min, max)
            t.insert_left(Fraction(a.numerator, a.denominator))  # 将数a插入左子树

            t2 = t.rightChild
            b.generate_number_sign()
            if b.number_sign == 1:
                b.generate_numerator(min, max)
            else:
                b.generate_numerator(min, max)
                b.generate_denominator(min, max)
            t2.insert_left(Fraction(b.numerator, b.denominator))  # 将数b加入右子树的左子树

            c.generate_number_sign()
            if c.number_sign == 1:
                c.generate_numerator(min, max)
            else:
                c.generate_numerator(min, max)
                c.generate_denominator(min, max)
            t2.insert_right(Fraction(c.numerator, c.denominator))  # 将数b加入右子树的右子树
    return t


# 将num列表转换为字符串
# num=[1,'/',2,'+',3]
def str_num(t):
    # 3数+2字符
    # num = []  用于储存生成题目的字符及计算数字
    if len(num) == 5:
        a = num.index(t.key)  # 取出根节点在列表中的位置
        # 如果第二个字符是乘除，第一个字符是加减，则在加减的部分加上括号
        if a == 3 and (num[3] == '*' or num[3] == '/') and (num[1] == '+' or num[1] == '-'):
            num.insert(0, '(')
            num.insert(4, ')')
        elif num[1] != num[3]:
            num.insert(2, '(')
            num.insert(6, ')')
    # result1代表输入到题目中显示的结果
    # result2表示电脑进行运算的结果,目的是为了将运算过程中的/改为÷
    if len(num) == 3:
        result1 = f"{num[0]} {str(num[1]).replace('/', '÷')} {num[2]}"
        result2 = '{} {} {}'.format(num[0], num[1], num[2])
    elif len(num) == 5:
        result1 = f"{num[0]} {str(num[1]).replace('/', '÷')} {num[2]} {str(num[3]).replace('/', '÷')} {num[4]}"
        result2 = '{} {} {} {} {}'.format(num[0], num[1], num[2], num[3], num[4])
    else:
        result1 = f"{num[0]} {str(num[1]).replace('/', '÷')} {num[2]} {str(num[3]).replace('/', '÷')} {num[4]} {str(num[5]).replace('/', '÷')} {num[6]}"
        result2 = '{} {} {} {} {} {} {}'.format(num[0], num[1], num[2], num[3], num[4], num[5], num[6])

    return result1, result2

@profile
# 重复生成题目
# n为生成题目的数量
def repeat(n, min, max):
    flag = n
    start_time1 = time.time()
    while n != 0:
        t = generate_num(min, max)
        if len(title) == 0:
            n -= 1
            title.append(t)
        else:
            for i in range(len(title)):
                if isOmorphic(t, title[i - 1]) == True:  # 通过是否同构表示是否重复
                    continue
            title.append(t)
            n -= 1
    end_time1 = time.time()
    print('\n')
    print(f"函数generate_num生成{flag}次题目所花的时间为{end_time1 - start_time1}s")

def main():
    print("****************************")
    function = int(input('生成题目输入1，自动查答案输入2:'))
    print("****************************")
    # function = 2
    while function != 1 and function != 2:
        print('输入错误，请重新输入\n')
        print("**************************")
        function = int(input('生成题目输入1，自动查询答案输入2:'))
    if function == 1:
        # 删除之前已有的文件，避免出现文本重叠的情况
        for file in glob.glob('Answers.txt'):
            os.remove(file)
        for file in glob.glob('Exercises.txt'):
            os.remove(file)
        for file in glob.glob('Exercises_test.txt'):
            os.remove(file)
        print()
        print(">>>>>>>>>>>>>>>>>>>>>>>>>>>")
        n = int(input('输入要生成题目的数量:'))
        print(">>>>>>>>>>>>>>>>>>>>>>>>>>>")
        print()
        while 1: # 进行模块异常检验
            print("**************************")
            min = int(input('输入数值的下限，必须是正数且小于10:'))
            print("**************************\n")
            print("**************************")
            max = int(input(f'输入数值的上限，需要大于{min}且小于10:'))
            print("**************************\n")
            if min <= 0:
                print("输入下限错误，请重新输入！")
                continue
            elif min >= 10:
                print("输入下限超过10，请重新输入！")
                continue
            elif max < min:
                print("输入上限错误，请重新输入！")
                continue
            elif max >= 10:
                print("输入上限超过10，请重新输入！")
                continue
            else:
                break
        print("以下时程序运行结果：")
        print(">>>>>>>>>>>>>>>>>>>>>>>>>>>")
        f1 = open('./Exercises.txt', 'a', encoding='utf-8')
        f2 = open('./Answers.txt', 'a', encoding='utf-8')
        f3 = open('./Exercises_test.txt', 'a', encoding='utf-8') # 作为电脑计算的文件
        start_time0 = time.time()
        repeat(n, min, max)
        print('\n')
        print("生成题目的结果如下：")
        for i in range(len(title)):
            num.clear()
            title[i].inorder()
            question1, question2 = str_num(title[i])
            print(question1)
            f1.write(f'{i + 1}. ' + question1 + '\n')
            temp = question2.split(' ')
            f3.write(f'{i + 1}. ' + question2 + '\n')
            if len(temp) == 5:
                problem = 'Fraction(temp[0])' + temp[1] + 'Fraction(temp[2])' + temp[3] + 'Fraction(temp[4])'
            elif len(temp) == 3:
                problem = 'Fraction(temp[0])' + temp[1] + 'Fraction(temp[2])'
            elif temp[0] == '(':
                problem = temp[0] + 'Fraction(temp[1])' + temp[2] + 'Fraction(temp[3])' + temp[4] + temp[
                    5] + 'Fraction(temp[6])'
            else:
                problem = 'Fraction(temp[0])' + temp[1] + temp[2] + 'Fraction(temp[3])' + temp[
                    4] + 'Fraction(temp[5])' + temp[6]
            f2.write(f'{i + 1}. ' + str(eval(problem)) + '\n')
        end_time0 = time.time()
        print('\n')
        print(f"整个程序运行的时间为{end_time0 - start_time0}s")

    else:
        # 删除之前已有的文件，避免出现文本重叠的情况
        for file in glob.glob('Grade.txt'):
            os.remove(file)
        LIMIT = 10e-10
        f1 = open('./Exercises_test.txt', 'r', encoding='utf-8')
        f2 = open('./Answers.txt', 'r', encoding='utf-8')
        f3 = open('./Grade.txt', 'w', encoding='utf-8')
        line1 = f1.readline()
        line2 = f2.readline()
        Correct = []
        Wrong = []
        while line1 and line2:
            a = line1.split('.')
            b = line2.split('.')
            operators = ['+', '-', '*', '/', '(', ')']
            c = a[1].strip()
            c = c.split(' ')
            n = 0
            while n < len(c):
                try:
                    operators.index(c[n])
                    n += 1
                except:
                    c.insert(n, '(')
                    c.insert(n + 2, ')')
                    n += 2
            c = ' '.join(c)
            if (eval(c) - eval(b[1])) < LIMIT:
                Correct.append(a[0])
            else:
                Wrong.append(b[0])
            line1 = f1.readline()
            line2 = f2.readline()
        length1 = len(Correct)
        Correct = ','.join(Correct)
        Correct = '(' + Correct + ')'
        print(Correct)
        f3.write('Correct: ' + f'{length1}' + Correct + '\n')

        length2 = len(Wrong)

        Wrong = ','.join(Wrong)
        Wrong = '(' + Wrong + ')'
        print(Wrong)
        f3.write('Wrong: ' + f'{length2}' + Wrong + '\n')


if __name__ == '__main__':
    # cProfile.run('main()')
    main()

    # all_objects = muppy.get_objects()
    # len(all_objects)
    # suml = summary.summarize(all_objects)
    # summary.print_(suml)
    # cProfile.run('main()', filename='C:/Users/Lenovo/Desktop/test_result.txt')
    # p = pstats.Stats('C:/Users/Lenovo/Desktop/test_result.txt')
    # p.sort_stats('cumulative').print_stats()
