# C语言

- 锁定黑马程序员视频---知识重塑 

## 1. C语言入门

### 1.1 C语言开篇

- C简介--汇编生C，C生万物

  ![image-20240703145430514](images/C/image-20240703145430514.png)

  - 学习C的好处

    ![image-20240703151050315](images/C/image-20240703151050315.png)

    

### 1.2 hello_world

- hello world 程序--详解

  ```c
  #include <stdio.h>  //头文件-预处理，即调用需要的函数库
  int main()   //主函数，函数名只能是main，执行函数的唯一入口
  {            //花括号--函数执行范围
  	printf("hello world!");   //函数执行语句
  	return 0;     //函数结束，返回整数0，与<整型int>呼应
  }
  ```

  - 程序执行流程详解
    - 程序编写--".c"文件
    - 编译--".obj"文件，即将c语言编译成计算机认识的"0/1"
    - 连接/链接--将".obj"文件和".h"文件组合在一起
    - 运行--控制台输出

## 2. 核心算法

### 2.1 注释

- 注释作用：对程序进行解释
- 注释分类：
  - 单行注释符号：//
  - 多行注释符号：/**/
- 补充：VS字体大小设置
  - VS界面上端工具选项卡中选择“工具-->选项-->字体和颜色”

### 2.2 注释加更

- 注释快捷键：(单行点击单行任意处，多行则全部选中再按快捷键)

  - 加注释："ctrl K" + "ctrl C"
  - 消注释："ctrl K" + "ctrl U"

- 注释擦除：注释的内容不参与运行，即".obj"文件内不包含注释内容；

- 注释嵌套：最好不要用注释嵌套，即多行注释里又加多行注释，这样不符合规定

  - ```c
    #include <stdio.h>
    int main()
    {
        printf("hello world!");
        /*
        /*-------
        --------*/
        ---------
        */
        return 0;
    }
    ```

    - 由上可知，在一套多行注释里，“ /* ”只与离它最近的“ */ ”对应，所以最外层的“ */ ”就失效导致代码报错

### 2.2 关键字

- 什么是关键字：被C语言赋予了特殊含义的英文单词
- 关键字的特点：关键字都是小写；关键字有特殊的颜色标记
  - VS：关键字呈蓝色和紫色

### 2.3 常量

#### 2.3.1 常量

- 什么是常量？
  - 在程序运算中，一直保持不变的值
- 常量的分类？每种常量的书写格式？
  - ![image-20240722165438796](images/C/image-20240722165438796.png)
  - 其中字符常量只包括“字母、数字和英文符号”，英文符号指在英文输入法下键盘可敲击的任何符号
  - 字符串"123"与整型123的区别
    - 字符串"123"只能作为数字显示，不能进行数学运算
    - 整型123可以进行数学运算
- 对常量进行分辨
  - '':两个单引号中间啥也没有是**语法错误**
  - "":两个双引号中间啥也没有也表示字符串，字符串内没有特定字符限制，写啥都行

#### 2.3.2 实型常量细节

- 细节1
  - ![image-20240722170624183](images/C/image-20240722170624183.png)

#### 2.3.3 输出常量

- printf() 函数使用规则：

![image-20240725091441195](images/C/image-20240725091441195.png)

- 常量如何输出：

![image-20240725091428428](images/C/image-20240725091428428.png)

- 代码实例

  ```c
  #include <stdio.h>
  int main()
  {
  	//输出整型常量
  	printf("整型常量：%d\n", 23);
  	//输出实型常量
  	printf("实型常量：%.2f\n", 23.02);
  	//输出字符型常量
  	printf("字符型常量：%c\n", 'a');
  	//输出字符串型常量
  	printf("字符串常量：%s\n", "hello world");
  	//输出我的学校
  	printf("我从%s毕业\n", "许昌学院");
  	//我的高考分数
  	printf("我的高考分数：%.2f\n", 461.0);
  	return 0;
  }
  ```

  ![image-20240725092638665](images/C/image-20240725092638665.png)

#### 2.3.4 输出常量扩展

- <img src="images/C/image-20240725093449963.png" alt="image-20240725093449963" style="zoom: 67%;" />
  - C语言中换行符直接写"\n"，然后C语言会根据对应的操作系统将换行符转换为对应的换行符
  - 输出多个常量时，填补数据用逗号隔开

### 2.4 变量

#### 2.4.1 变量

- 变量的理解

  - 变量就是一个容器/盒子，用来存储数据
    - 容器类型：变量类型
    - 容器名称：变量名

- 变量如何定义

  - "数据类型 变量名;"   int a;

- 变量如何使用

  - 赋值/修改值    a = 10;
  - 获取值，直接可以使用变量名获取变量值

- 变量使用细节

  - 先定义再赋值   int a;  a = 10;
  - 明确变量值得前提下，定义的同时直接赋值：int a = 10;

- 在项目中，变量如何使用

  - 经常改变的数据可以定义为变量

- 代码实例

  ```c
  #include <stdio.h>
  /*
  * 练习题
  * 在游戏中，定义任务的血量为变量blood
  * 初始血量为100
  * 对战时受到80点伤害
  * 自己用技能恢复60点血量
  * 请问：目前人物的最终血量为多少？
  */
  int main()
  {
  	int blood = 100;        //定义人物初始血量
  	blood = blood - 80 + 60;    //作战时任务血量变化
  	//输出人物的最终血量
  	printf("人物的最终血量为%d\n", blood);
  	return 0;
  }
  ```

  ![image-20240725100311179](images/C/image-20240725100311179.png)

#### 2.4.2 变量的注意细节

- 一个变量只能存储一个值，

  ```c
  int a = 9;
  a = 10;  //此时变量内的值9就被值10给覆盖了
  ```

- 变量名不能重复，否则会报错

  - 重复定义变量的报错代码，点击报错代码，会有网页报错代码解决方案

  ![image-20240725111332149](images/C/image-20240725111332149.png)

- 一个语句可以定义多个变量，但不建议使用，因为阅读性差

  ```c
  int a = 9,b = 10,c = 11;
  ```

- 变量在使用前一定要先赋值，否则会报错

- 变量的作用范围？

### 2.5 计算机的存储规则

![image-20240725150714012](images/C/image-20240725150714012.png)

- 图片由像素、分辨率、三原色、 运算符组成，这里的三原色是光学三原色红绿蓝，扩展美学三原色是红黄蓝

### 2.6 常见的进制

- 常见的进制有4种：二进制、十进制、八进制和十六进制

- 进制是怎么来的

  - 二进制：1010001111000101
  - 八进制：每3位2进制组成一个8进制，简便书写
  - 十六进制：每4位2进制组成一个16进制，进一步简便书写，使数据更明确

- 进制怎么书写

  - 二进制：以0B/0b作为前缀
  - 八进制：以0作为前缀
  - 十六进制：以0x/0X作为前缀
  - 十进制：没有前缀，直接书写

- 进制之间怎么转换

  ![image-20240725154555319](images/C/image-20240725154555319.png)

### 2.7 数据类型

- ![image-20240725163559317](images/C/image-20240725163559317.png)

#### 2.7.1 数据类型的作用

- 数据类型指变量中能存储什么类型的数据
- 不同的数据类型表示的存储空间不同

#### 2.7.2 整数类型

![image-20240725163654033](images/C/image-20240725163654033.png)

- 取值范围计算
  - short：-2^15~2^15-1
  - int：-2^31~2^31-1
  - long：
    - 32位 4字节：-2^31~2^31-1
    - 64位 8字节：-2^63~2^63-1
  - long long：-2^63~2^63-1

- 四种整数类型如何定义变量以及如何计算它们占用的字节

- ```c
  #include <stdio.h>
  int main()
  {
  	//短整型
  	short a = 10;
  	printf("a = %d\n", a);
  	//整型
  	int b = 20;
  	printf("b = %d\n", b);
  	//长整型
  	long c = 30L;  //数字后面加L后缀，表明是long型
  	printf("c = %ld\n", c);
  	//长长整型
  	long long d = 40LL; //数字后面加LL后缀，表明是long long型
  	printf("d = %lld\n", d);
  	printf("a:%zu\n", sizeof(a));
  	printf("b:%zu\n", sizeof(b));
  	printf("c:%zu\n", sizeof(c));
  	printf("d:%zu\n", sizeof(d));
  	return 0;
  }
  ```

- 使用sizeof()计算变量值占几个字节

  - sizeof(变量名/数据类型)

  - sizeof()计算出的值占位符用zu表示

  - ```c
    int a = 3;
    printf("%zu\n",sizeof(a));
    ```

- 整数类型扩展
  - ![image-20240726150201228](images/C/image-20240726150201228.png)

#### 2.7.3 小数类型

![image-20240726150437259](images/C/image-20240726150437259.png)

#### 2.7.4 字符型

- char 

#### 2.7.5 小结

![image-20240729143149081](images/C/image-20240729143149081.png)

### 2.8 标识符

- 命名规则

  - ![image-20240729143509638](images/C/image-20240729143509638.png)

  - ![image-20240729143658580](images/C/image-20240729143658580.png)

  - ![image-20240729144028953](images/C/image-20240729144028953.png)

    

### 2.9 键盘录入

![image-20240729144236848](images/C/image-20240729144236848.png)

- ```c
  #include <stdio.h>
  int main()
  {
  	int a;
  	printf("请输入一个整数：");
  	scanf("%d", &a);
  	printf("a = %d\n",a);
  	return 0;
  }
  ```

  - 这里使用scanf()会报错，可以使用AI(如文心一言)，生成解决方案，使用文心一言中注意一个问题一个对话框，换问题的话更换新的对话框

    - ![image-20240729145835574](images/C/image-20240729145835574.png)
      - 跟ai对话的形式问问题，可以更高效的编程！

  - ![image-20240729152437852](images/C/image-20240729152437852.png)

  - 键盘录入多个数据

    ```c
    #include <stdio.h>
    int main()
    {
    	int a, b;
    	printf("请输入两个整数值：");
    	scanf("%d%d", &a, &b);
    	printf("a+b = %d\n", a + b);
    	return 0;
    }
    ```

    ![image-20240729153452386](images/C/image-20240729153452386.png)

    

## 3. 运算符

![image-20240730144935116](images/C/image-20240730144935116.png)

### 3.1 算数运算符

![image-20240730145006266](images/C/image-20240730145006266.png)

- %取余运算中，整数只能跟整数进行运算

- 基本用法：数值拆分

  ![image-20240730145520168](images/C/image-20240730145520168.png)

  - 代码

    ```c
    #include <stdio.h>
    int main()
    {
    	int num;
    	printf("请输入一个三位数的整数：");
    	scanf("%d", &num);
    	printf("整数的个位数是%d\n", num % 10);
    	printf("整数的十位数是%d\n", num/10 % 10);
    	printf("整数的百位数是%d\n", num/100 % 10);
    	return 0;
    }
    ```

    ![image-20240730150122214](images/C/image-20240730150122214.png)

  - 高级用法-数字相加、字符相加

    - 隐式转换

    ![image-20240730150915873](images/C/image-20240730150915873.png)

    - 强制转换

      ![image-20240730151707701](images/C/image-20240730151707701.png)

      - 强制转换时刻：大范围数赋值给小范围数时，大范围数强转至小范围的数据类型，如果大范围数值大于小范围的最大值，那么强转就会报错
      - 强制转换：在转换的变量前加括号，括号里写强转的类型，若是要强转为指针类型，就在变量后加星
  
  - 字符相加
  
    - 字符 char short运算时会自动转换为int类型，两个字符相加时，会查找ASCII码表，使用对应的十进制进行运算，结果是十进制对应的字符
  
    - ```c
      #include <stdio.h>
      int main()
      {
      	short a = 'a';
      	short b = 'b';
      	int c = a + b;
      	short d = (short)c;
      	printf("c = %d\n", c);
      	printf("d = %d\n", d);
      	return 0;
      }
      ```
  
      ![image-20240730153307761](images/C/image-20240730153307761.png)
  
  - 小结
  
    ![image-20240730153702715](images/C/image-20240730153702715.png)

### 3.2 自增自减运算符

#### 3.2.1 单独使用

- 自增、自减运算符单独一行时，++/--前后运算都是一样的

  ```c
  #include <stdio.h>
  int main()
  {
  	int a = 10;
  	a++;
  	printf("a = %d\n", a);
  	a--;
  	printf("a = %d\n", a);
  	++a;
  	printf("a = %d\n", a);
  	--a;
  	printf("a = %d\n", a);
  	return 0;
  }
  ```

  ![image-20240730154620814](images/C/image-20240730154620814.png)

#### 3.2.2 参与运算

- 先加加(++a)与后加加(a++)的区别

  ![image-20240730155013792](images/C/image-20240730155013792.png)

- 小结

  - ![image-20240730160607476](images/C/image-20240730160607476.png)

### 3.3 赋值运算符

- 赋值运算符分类

  ![image-20240730160825700](images/C/image-20240730160825700.png)

  

### 3.4 关系运算符

- 关系运算符分类
  - ![image-20240730160924926](images/C/image-20240730160924926.png)
  - 关系运算符的结果：
    - 关系成立则是1
    - 关系不成立则是0

### 3.5 逻辑运算符

- 逻辑运算符

  ![image-20240730161300395](images/C/image-20240730161300395.png)

- 真题练习

  ![image-20240730161626860](images/C/image-20240730161626860.png)

  - 关系运算符的使用

    ```c
    #include <stdio.h>
    /*
    * 键盘输入一个两位数，该数值内包含7则输出0，否则输出1
    */
    int main()
    {
    	int num;
    	printf("请输入一个两位数的整数：");
    	scanf("%d", &num);
    	int ge = num % 10;
    	int shi = num / 10 % 10;
    	printf("%d", ge != 7 && shi != 7);
    	return 0;
    }
    ```

    

### 3.6 三元运算符

- 名称：三元运算符/三元表达式/问号冒号运算符
- 表达式：![image-20240731100253883](images/C/image-20240731100253883.png)

### 3.7 逗号运算符(分隔符)

![image-20240731102010308](images/C/image-20240731102010308.png)

### 3.8 运算符的优先级

![image-20240731102237540](images/C/image-20240731102237540.png)

- 一元、二元、三元分别指运算符包含几个变量
- 三元运算符的嵌套-解题关键
  - 从左边的第一个问号找冒号
  - 如果过程中遇到了其他问号，那么找冒号的数量加1
    - ![image-20240731103546920](images/C/image-20240731103546920.png)
- 运算符优先级
  - 小括号最优先
  - 一元>二元>三元
  - &&>||>赋值
    - 关系运算符>赋值运算符
- 解题思路
  - 拆分

## 4. 流程控制语句

### 4.1 顺序结构

- 顺序结构：自上而下依次运行

### 4.2 分支结构

#### 4.2.1 if语句

- 格式1

  ```c
  if(表达式)
  {
      ---
  }
  ```

  

![image-20240731105347773](images/C/image-20240731105347773.png)

- 格式2

  - ```c
    if(表达式1)
    {
        ---
    }
    else
    {
        ---
    }
    ```

    

- 格式3

  - ```c
    if(表达式1)
    {
        ---
    }
    else if(表达式2)
    {
        ---
    }
    else if(表达式3)
    {
        ---
    }---
    else
    {
        ---
    }
    
    ```

    

#### 4.2.2 switch()case语句

![image-20240731110125448](images/C/image-20240731110125448.png)

- 注意点：
  ![image-20240731110411616](images/C/image-20240731110411616.png)
- ![image-20240731110442602](images/C/image-20240731110442602.png)
  - switch：有限个case进行匹配的情况，10个左右---执行效率高
  - if的第三种格式：一般是对一个范围进行，需要一个一个去匹配，直到符合条件，进行语句体
- case穿透的规则
  - 根据小括号中的表达式去匹配对应的case值
  - 执行对应case里的代码
  - 如果执行过程中遇到了break，就会结束整个switch，但是如果没有遇到break，就会继续执行下面case中的代码，直到遇到break，或者把整个switch中的代码全都执行完之后，才会结束

### 4.3 循环结构

#### 4.3.1 for循环

![image-20240731111817422](images/C/image-20240731111817422.png)

- for循环的执行流程

  ![image-20240731112012379](images/C/image-20240731112012379.png)

  

#### 4.3.2 while循环

![image-20240731112418746](images/C/image-20240731112418746.png)

- for和while的区别

  ![image-20240731112607913](images/C/image-20240731112607913.png)

#### 4.3.3 do...while循环

![image-20240731112720148](images/C/image-20240731112720148.png)

- do...while 与 while 的区别
  - do...while：先执行后判断
  - while：先判断后执行

## 5. 高级循环

### 5.1 无限循环

- 三种格式的无限循环
  - ![image-20240731113512827](images/C/image-20240731113512827.png)

### 5.2 跳转控制语句

#### 5.2.1 break

- break：不能单独书写，只能写在switch，或者循环中，表示结束，跳出的意思
  - 只能跳出单层循环
- goto：结合标号，可以到代码的任意地方，一般只用于跳出循环嵌套
  - ![image-20240731115118256](images/C/image-20240731115118256.png)

#### 5.2.2 continue

- continue：结束本次循环，继续下次循环

### 5.3 循环嵌套

- 格式：

![image-20240731114429873](images/C/image-20240731114429873.png)



## 6. 函数

### 6.1 函数

- 函数：程序中独立的功能
  - ![image-20240731143343472](images/C/image-20240731143343472.png)

### 6.2 带有参数的函数

- 形参与实参的使用

![image-20240731143727619](images/C/image-20240731143727619.png)

### 6.3 带有返回值的函数

![image-20240731144216170](images/C/image-20240731144216170.png)

### 6.4 定义函数的终极绝杀

- 定义函数的三个口诀
  - 定义函数，是为了干什么——>函数体
  - 干这件事情，需要什么——>是否需要形参
  - 这件事干完了，结果是否需要被调用——>是否需要返回值

#### 6.5 函数的注意事项

![image-20240731145343351](images/C/image-20240731145343351.png)

- 代码报错后，知道怎么改

### 6.6 C语言中常见的函数

![image-20240731145905544](images/C/image-20240731145905544.png)

- 不同的函数导入的头文件不同，不需要背，用时直接问AI就行

- time()

  - ```c
    #include <stdio.h>
    #include <time.h>
    int main()
    {
    	long long TIME = time(NULL);  //形参指获取的时间还要在其他地方用的话有形参，不用的话形参为NULL/空
    	printf("当前时间是%lld\n", TIME); //返回值是获取时间的时间戳，可以通过工具转换为北京时间
    	return 0;
    }
    ```

    

### 6.7 随机数

- 使用的有两个函数

  ```c
  srand();             //设置种子
  rand();				//获取随机数
  ```

  ```c
  #include <stdio.h>
  #include <stdlib.h>
  #include <time.h>
  int main()
  {
  	srand(1);       //设置种子,这里的种子没有设参数，即种子唯一，生成的随机数也唯一
  	for (int i = 0; i < 10; i++)
  	{
  		printf("%d\n", rand());
  	}
  	return 0;
  }
  ```

- 随机数的两个弊端

  - 种子不变，随机数结果就是固定的

    - 解决方案

      - 需要一个变化的数据去充当种子，当然时间是最好的值

      - ```c
        #include <stdio.h>
        #include <stdlib.h>
        #include <time.h>
        int main()
        {
        	srand(time(NULL));       //设置种子,以时间为种子，每次生成的随机数都是随机不同的
        	for (int i = 0; i < 10; i++)
        	{
        		printf("%d\n", rand());
        	}
        	return 0;
        }
        ```

      - 如果没有设置种子，直接生成随机数也可以，只不过这里默认种子设为1

  - 随机数的范围不定

    - 绝招触发-用于生成任意范围的随机数

      - 把这个范围变成包头不包尾，包左不包右的
      - 用尾巴减开头
      - 修改代码

    - 代码举例

      - ```c
        #include <stdio.h>
        #include <stdlib.h>
        #include <time.h>
        int main()
        {
        	srand(time(NULL));     //设置种子
        	int num;
        	//获取[12,87]的随机数
        	printf("[12,87]之间任意10个随机数：\n");
        	for (int i = 0; i < 10; i++)
        	{
        		num = rand() % 76 + 12;
        		printf("%d\n", num);
        	}
        	printf("[17,39)之间任意10个随机数：\n");
        	for (int i = 0; i < 10; i++)
        	{
        		num = rand() % 22 + 17;
        		printf("%d\n", num);
        	}
        	return 0;
        }
        ```

- 猜数字游戏

  - 代码

    ```c
    #include <stdio.h>
    #include <stdlib.h>
    #include <time.h>
    /*
    * 猜数字游戏
    * 生成[1,100]之间的随机数，使用键盘录入去猜，猜中为止
    */
    int main()
    {
    	int num = 0;
    	int randNum;
    	srand(time(NULL));
    	num = rand() % 100 + 1;
    	printf("num = %d\n", num);
    	printf("请任意输入一个[1,100]之间数：");
    	scanf("%d", &randNum);
    	while (1)
    	{
    		if (randNum == num)
    		{
    			printf("恭喜你，猜对了！\n");
    			break;
    		}
    		else if (randNum > num)
    		{
    			printf("很遗憾，猜大了，请重新输入：");
    			scanf("%d", &randNum);
    		}
    		else if (randNum < num)
    		{
    			printf("很遗憾，猜小了，请重新输入：");
    			scanf("%d", &randNum);
    		}
    	}
    	
    	return 0;
    }
    ```

    ![image-20240731155612278](images/C/image-20240731155612278.png)

## 7. 数组  

### 7.1  数组的定义

- 数组：是一个容器，可以存储同种数据类型的多个值
- 数组的特点：
  - 连续的空间
  - 一旦定义，长度不可变

### 7.2 数组的初始化

- 初始化：定义数组时，第一次给数组赋值

- ![image-20240731172149730](images/C/image-20240731172149730.png)

### 7.3 数组中元素的访问和修改

- 索引
  - ![image-20240731172615732](images/C/image-20240731172615732.png)
- 获取
  - ![image-20240731172715156](images/C/image-20240731172715156.png)

### 7.4 数组的遍历

- 使用for循环进行遍历

  - ```c
    #include <stdio.h>
    
    int main()
    {
    	int arr[] = { 1,2,3,4,5 };
    	for (int i = 0; i < 5; i++)
    	{
    		printf("arr[%d] = %d\n", i, arr[i]);
    	}
    	return 0;
    }
    ```

    ![image-20240731173100492](images/C/image-20240731173100492.png)

### 7.5 内存中数组

- 内存地址

  - ![image-20240801093548168](images/C/image-20240801093548168.png)

- 变量的内存地址

  - ![image-20240801093714063](images/C/image-20240801093714063.png)
  - 变量在内存中的地址由系统分配，这里取四个字节的首地址作为变量的存储地址

- 数组在内存中的存储

  - ```c
    #include <stdio.h>
    
    int main()
    {
    	int arr[] = { 1,2,3 };
    	printf("%p\n", &arr);  //数组的首地址，等于首个元素的地址
    	printf("%p\n", &arr[0]);
    	printf("%p\n", &arr[1]);
    	printf("%p\n", &arr[2]);
    	return 0;
    }
    ```

    ![image-20240801095127308](images/C/image-20240801095127308.png)

- 总结

  - ![image-20240801094528066](images/C/image-20240801094528066.png)

- 思考

  - ![image-20240801095350908](images/C/image-20240801095350908.png)

  - 确定中存储的数据有两个要素

    - 变量的首地址
    - 变量的数据类型

  - 已知变量的首地址，根据变量的数据类型占几个字节往后推几个字节，才能获取变量中的完整数据

  - 数组中的索引从0开始：与偏移量有关系，索引等于偏移量，0索引等于0偏移量，数据地址位于首地址

  - 数组的长度计算：总长度/数据类型占的字节数

    - ```c
      #include <stdio.h>
      
      int main()
      {
      	int arr[] = { 1,2,3 };
      	printf("%p\n", &arr);
      	printf("%p\n", &arr[0]);
      	printf("%p\n", &arr[1]);
      	printf("%p\n", &arr[2]);
      	printf("数组长度为%d\n", sizeof(arr) / sizeof(int));
      	return 0;
      }
      ```

      ![image-20240801100038239](images/C/image-20240801100038239.png)

### 7.6 数组中的常见问题

- 数组作为参数，要注意什么 int arr[10];
  - 数组作为参数时，要传递数组的首地址和数组的长度，即需要两个参数
  - 定义处的**arr**表示完整的数组
  - 函数中的**arr**只是一个变量，用来记录数组的首地址
- 数组的索引越界
  - 最小索引：0
  - 最大索引：数组长度-1
  - 索引越界的话，程序不会报错，但输出结果是乱码

## 8. 指针

### 8.1 指针的定义

- 指针/指针变量：用来指向变量的地址的首地址

- ![image-20240801105729025](images/C/image-20240801105729025.png)

  - 定义指针变量时的*用来标记，说明这是定义的指针变量，而不是普通的变量

  - *p：用来获取指针指向地址的数据，这里的**星号**是**解引用运算符**

  - 利用指针去查询修改数据

    - ```c
      #include <stdio.h>
      
      int main()
      {
      	int a = 10;
      	int* p = &a;
      	printf("%d\n", *p);   //利用指针存储数据
      	*p = 30;   //利用指针修改数据
      	printf("%d\n", *p);
      	printf("%d\n", a);
      	return 0;
      }
      ```

- 指针的使用细节

  - ![image-20240801110630281](images/C/image-20240801110630281.png)

    - 指针变量的名字不带星号，定义时的星号只是为了标记这是指针变量

    ![image-20240801111001842](images/C/image-20240801111001842.png)

- 小结

  - ![image-20240801111159904](images/C/image-20240801111159904.png)
  - 指针的使用细节-4点

### 8.2 指针的第一个作用

- 作用一：操作其他函数中的变量

  - 函数形参设为指针变量的情况下，是传入变量的地址，这样就可以改变该地址对应变量的值

  - 代码举例-要求交换两个变量的值

    - ```c
      #include <stdio.h>
      /*
      交换两个变量的值
      */
      //函数声明
      void swap_ab(int num1, int num2);  //传入变量的值
      void swap_pab(int* num1, int* num2);  //传入变量的地址
      int main()
      {
      	int a = 10;
      	int b = 20;
      	printf("交换前：\na = %d\nb = %d\n", a, b);
      	swap_ab(a, b);
      	printf("变量值交换后：\na = %d\nb = %d\n", a, b);
      	swap_pab(&a, &b);
      	printf("变量地址交换后：\na = %d\nb = %d\n", a, b);
      	return 0;
      }
      //交换num1和num2的值
      void swap_ab(int num1, int num2)
      {
      	int temp;
      	temp = num1;
      	num1 = num2;
      	num2 = temp;
      }
      //交换指针变量num1和num2的值，指针变量存储内存地址
      void swap_pab(int* num1, int* num2)
      {
      	int temp;
      	temp = *num1;
      	*num1 = *num2;
      	*num2 = temp;
      }
      ```

      ![image-20240801143749520](images/C/image-20240801143749520.png)

      - 根据代码分析
        - 单纯的交换变量的值，对主函数变量本身并没有影响，因为它们本身就没有交集，有各自的存储地址
        - 交换变量的地址，就可以直接在子函数中操作主函数中变量的值，达到修改、变换的效果

- 作用一细节

  - 函数中变量的生命周期与函数相关，函数结束后，变量也会消失，但也不会立即消失，需要一点时间

  - 此时函数中的变量在其他函数中就不能通过指针使用

  - 如果不想变量被回收，可以在变量前加static关键字，**static表示静态变量**

  - **即使在函数内部局部变量的生命周期结束时，返回的值并没有消失，而是被妥善地存储并等待后续的使用**

    - 举例

      ```c
      int remainder(int a, int b) 
      {
          int result = a % b;
          return result;
      }
      ```

      - 当你调用这个函数，如 `int result = remainder(10, 3);`，在 `remainder` 函数内部执行完 `result = a % b;` 后，`result` 的值实际上被保存在了栈内存的一个特定位置，直到 `return result;` 语句完成时，这个值会被立即返回给 `remainder` 函数的外部，也就是赋值语句 `int result =` 的右侧。这个返回的值可以被正确地赋给变量 `result`，并继续在代码中使用。

### 8.3 指针的第二个作用

- 在函数中返回多个值

  - 将需要返回的值作为指针变量的参数传入，然后就可以获取多个返回值

  - 代码举例

    ```c
    #include <stdio.h>
    /*
    * 在子函数中求一个数组中的最大值和最小值，并返回这两个值
    */
    //函数声明
    void GetMaxAndMin(int arr[], int len, int* max, int* min);
    int main()
    {
    	int arr[] = { 1,2,3,4,5,6,7,8,9,12,34,990,15,80,46,0 };
    	int len = sizeof(arr) / sizeof(int);
    	int max = arr[0];
    	int min = arr[0];
    	GetMaxAndMin(arr, len, &max, &min);
    	printf("最大值是%d\n最小值是%d\n", max, min);
    	return 0;
    }
    void GetMaxAndMin(int arr[], int len, int* max, int* min)
    {
    	//求最值
    	for (int i = 0; i < len; i++)
    	{
    		if (arr[i] > *max)
    		{
    			*max = arr[i];  //求最大值
    		}
    		if (arr[i] < *min)
    		{
    			*min = arr[i];   //求最小值                        
    		}
    	}
    }
    ```

    

### 8.4 指针的第三个作用

- 将函数的结果和计算状态分开

  - 例如：求两个整数的余数，结果就是余数，计算状态就是成功或失败

  - ```c
    #include <stdio.h>
    /*
    * 计算两个整数的余数
    */
    //函数声明
    int getRemainder(int num1, int num2, int* res);
    int main()
    {
    	int a = 25;
    	int b = 0;
    	int res;   //余数变量
    	if (getRemainder(a, b, &res))
    	{
    		printf("计算不成立\n");
    	}
    	else
    	{
    		printf("余数是%d\n", res);
    	}
    	return 0;
    }
    int getRemainder(int num1, int num2, int* res)
    {
    	if (num2 == 0)
    	{ 
    		return 1;   //计算不成立
    	}
    	*res = num1 % num2;
    	return 0;
    }
    
    ```

    

## 9. 指针高级

### 9.1 指针的运算

- ![image-20240802101640837](images/C/image-20240802101640837.png)

#### 9.1.1 指针运算有意义和无意义的操作

- 指针运算有意义的操作（前提：保障内存空间是连续的，如数组）
  - 指针跟整数进行加、减操作（每次移动一个步长）
  - 指针跟指针进行减操作（计算间隔步长）
- 指针运算无意义的操作
  - 指针跟整数进行乘除
  - 指针跟指针进行加、乘、除操作

#### 9.1.2 野指针和悬空指针

- 野指针：指针指向的空间未分配

- 悬空指针：指针指向的空间已分配，但是被释放了

- ```c
  #include <stdio.h>
  int* test();
  int main()
  {
  	//野指针
  	int a = 10;
  	int* p1 = &a;
  	int* p2 = p1 + 11;   //这里指针指向的变量并未分配
  	printf("%p\n", p2);
  	printf("%d\n", *p2);
  	//悬空指针
  	int* p3 = test();  //test()函数结束后，局部变量num被释放，p3就变成了悬空指针
  	printf("hhhhhhhhh\n");
  	printf("%p\n", p3);
  	printf("%d\n", *p3);
  	return 0;
  }
  int* test()
  {
  	int num = 10;
  	int* p = &num;
  	return p;   //返回num的地址
  }
  
  ```

  

#### 9.1.3 void类型的指针

- void没有任何类型

  - 好处：可以接受任意指针类型记录的内存地址

  - 缺点：void类型的指针，无法获取变量里的值，也不能进行加减运算，只能做第三方存储

  - ```c
    #include <stdio.h>
    //交换两个变量的值
    void swap(void* num1, void* num2, int len);
    int main()
    {
    	long a = 100L;
    	long b = 20L;
    	printf("交换前：\na = %ld\nb = %ld\n", a, b);
    	swap(&a, &b, sizeof(long));
    	printf("交换后：\na = %ld\nb = %ld\n",a,b);
    	return 0;
    }
    void swap(void* num1, void* num2, int len)
    {
    	char* p1 = num1;
    	char* p2 = num2;
    	char temp;
    	for (int i = 0; i < len; i++)  //按字节交换，至少是一个字节，所以用char定义temp
    	{
    		temp = *p1;
    		*p1 = *p2;
    		*p2 = temp;
    		p1++;
    		p2++;
    	}
    }
    ```

    

### 9.2 二级指针和多级指针

#### 9.2.1 二级指针

![image-20240802150646044](images/C/image-20240802150646044.png)

- 作用1：利用二级指针可以修改一级指针里记录的内存地址；

- 作用2：利用二级指针可以获取变量中的数据

- ```c
  #include <stdio.h>
  
  int main()
  {
  	int a = 90;
  	int b = 80;
  	int c = 99;
  	int* p = &a;
  	int** pp = &p;
  	*pp = &b;  //二级指针修改一级指针中的值
  	printf("a = %d\n", a);  //保持不变，因为指针p里的内存地址被修改，与a的地址没有关系
  	printf("a = %d\n", **pp);  //修改后的p中内存地址的值
  	*p = 88; 
  	printf("b = %d\n", b); //修改p指向的地址里存储的数据
  	return 0;
  }
  
  ```

  ![image-20240802152055085](images/C/image-20240802152055085.png)

### 9.3 数组和指针

#### 9.3.1 数组指针

- 数组指针：指向数组的指针

- 代码-使用指针遍历打印数组

  - ```c
    #include <stdio.h>
    
    int main()
    {
    	int arr[] = { 90,80,70,60,89,87,79 };
    	int* p = arr;   //定义数组指针，指向指针的首地址
    	//遍历打印数组
    	for (int i = 0; i < sizeof(arr) / sizeof(int); i++)
    	{
    		printf("arr[%d] = %d\n", i, *p++);
    	}
    	return 0;
    }
    
    ```

- 数组指针的细节

  ![image-20240806155109749](images/C/image-20240806155109749.png)

  - ```c
    #include <stdio.h>
    
    int main()
    {
    	int arr[] = { 90,80,70,60,89,87,79 };
    	printf("%p\n", arr);  
    	printf("%p\n", &arr);
    	printf("%p\n", arr+1); //arr参与计算，会作为一个元素的指针
    	printf("%p\n", &arr+1);  //&arr获取的是数组的首地址，这里arr是做为一个整体
    	return 0;
    }
    
    ```

- ![image-20241021194755538](images/C/image-20241021194755538.png)

#### 9.3.2 二维数组

- 二维数组：把多个小数组，放在一个大数组当中

- 定义格式一以及遍历格式一

  - ```c
    #include <stdio.h>
    /*
    * 定义格式一：
    *	m:表示二维数组的长度
    *	n:表示每一个一维数组的长度
    *	数据类型 arr[m][n]
    *   {
    *		{...},
    *		{...}
    *   }
    */
    int main()
    {
    	//定义二维数组
    	int arr[3][8] = 
    	{
    		{1,2,3,4,5,6,7,8},
    		{11,22,33,44,55,66,77,88},
    		{111,222,333,444,555,666,777,888}
    	};
    	//二维数组的遍历
    	for (int i = 0; i < 3; i++)  //遍历二维数组里的每个一维数组
    	{
    		for (int j = 0; j < 8; j++)  //遍历每个一维数组里的元素
    		{
    			printf("%d ", arr[i][j]);
    		}
    		printf("\n");
    	}
    	return 0;
    }
    
    ```

- 定义格式二以及遍历方式二----每个一维数组的长度不等

  - ```c
    #include <stdio.h>
    /*
    * 定义格式二：
    *	先定义好每个一维数组
    *	再定义二维数组，二维数组的元素是每个一位数组的首地址
    */
    int main()
    {
    	//定义一维数组
    	int arr1[] = { 9,8,7 };
    	int arr2[] = { 8,7,6,5 };
    	int arr3[] = { 9,8,7,6,5,4,3,2,1 };
    	//计算一维数组的长度
    	int len1 = sizeof(arr1) / sizeof(int);
    	int len2 = sizeof(arr2) / sizeof(int);
    	int len3 = sizeof(arr3) / sizeof(int);
    	//定义数组长度数组
    	int arrLen[] = { len1,len2,len3 };
    	//定义二维数组 数组的数据类型要跟数组内部存储的元素数据类型保持一致
    	int* arr[3] = { arr1,arr2,arr3 };
    	//遍历二维数组
    	for (int i = 0; i < 3; i++)
    	{
    		for (int j = 0; j < arrLen[i]; j++)
    		{
    			printf("%d ", arr[i][j]);
    		}
    		printf("\n");
    	}
    
    	return 0;
    }
    
    ```

#### 9.3.3 利用指针去遍历二维数组

- 利用指针去遍历二维数组一

  ```c
  int arr[3][5];
  ```

  - 获取二维数组的指针

  - 数组指针的数据类型要跟数组内部元素的保持一致

    - 数组指针定义格式：数据类型 * 指针名 = arr
    - 这里的数据类型要跟二维数组里的一维数组数据类型保持一致，一维数组数据类型是int，并且包含5个元素，所以这里定义的数据类型是int[5]
    - **int[5] * p = arr**，但是花括号要写在右边，就成了 **int * p[5] = arr**，这样还不够，会误以为定义的是一维数组指针，需要加个括号  **int (*p)[5] = arr**
    - **int (*p)[5] = arr**：表示定义了一个二维数组，里面的一维数组包含5个元素

  - 代码

    - ```c
      #include <stdio.h>
      /*
      * 定义格式二：
      *	先定义好每个一维数组
      *	再定义二维数组，二维数组的元素是每个一位数组的首地址
      */
      int main()
      {
      	int arr[3][5] =
      	{
      		{1,2,3,4,5},
      		{11,22,33,44,55},
      		{111,222,333,444,555}
      	};
      	int(*p)[5] = arr;   //定义数组指针
      	//利用数组指针去遍历二维数组
      	for (int i = 0; i < 3; i++)
      	{
      		for (int j = 0; j < 5; j++)
      		{
      			printf("%d ", *(*p + j));  //指向一维数组里的每一个元素的地址再进行取值
      		}
      		printf("\n");
      		p++;  //指向下一个一维数组
      	}
      
      	return 0;
      }
      
      ```

- 利用指针去遍历二维数组二

  - 先定义多个一维数组，再将一维数组的指针定义到一个数组里，为二维数组
  - 与直接定义二维数组的区别是，这里的一维数组之间的地址是不连续的

  ```c
  #include <stdio.h>
  /*
  * 定义格式二：
  *	先定义好每个一维数组
  *	再定义二维数组，二维数组的元素是每个一位数组的首地址
  */
  int main()
  {
  	int arr1[] = {1,23,3,4,5};
  	int arr2[] = { 11,22,33,44,55 };
  	int arr3[] = { 111,222,333,444,555 };
  	int* arr[] = { arr1,arr2,arr3 };
  	int** p = arr;
  	for (int i = 0; i < 3; i++)
  	{
  		for (int j = 0; j < 5; j++)
  		{
  			printf("%d ", *(*p + j));
  		}
  		printf("\n");
  		p++;
  	}
  	return 0;
  }
  
  ```

#### 9.3.4 数组指针和指针数组 

- 区别

  - ![image-20240806173625946](images/C/image-20240806173625946.png)

  - 数组指针代码举例

    ```c
    #include <stdio.h>
    
    int main()
    {
    	int arr1[] = {1,23,3,4,5};
    	int* p1 = arr1;   //定义指针指向数组arr1
    	int(*p2)[5] = &arr1;  //定义指针指向一整个一维数组
    	printf("%p\n", p1+1);  //指向一维数组里的第二个元素的地址
    	printf("%p\n", p2+1);  //指向下一个一维数组的首地址
    	return 0;
    }
    ```

    

### 9.4 函数和指针

#### 9.4.1 函数指针

- 函数指针的定义，将函数名替换为"(*p)"，有形参的话，将形参删掉，只保留数据类型

- ```c
  #include <stdio.h>
  void function1();
  int function2(int num1, int num2);
  int main()
  {
  	//定义函数指针
  	void (*p1)() = function1;
  	int (*p2)(int, int) = function2;
  	p1();
  	int num = p2(12, 80);
  	printf("num = %d\n", num);
  	return 0;
  }
  void function1()
  {
  	printf("function1\n");
  }
  int function2(int num1, int num2)
  {
  	printf("function2\n");
  	return num1 + num2;
  }
  ```

#### 9.4.2 函数指针与函数指针数组的练习

- ```c
  #include <stdio.h>
  /*
  * 键盘录入3个数字
  * 前两个表示计算的数字
  * 第三个表示加减乘除的哪一个
  */
  int add(int num1, int num2);
  int subtract(int num1, int num2);
  int multiply(int num1, int num2);
  int except(int num1, int num2);
  int main()
  {
  	//定义一个数组去装四个函数的指针----指针数组
  	int(*p[4])(int,int) = { add,subtract,multiply,except };
  	//键盘输入两个数
  	int num1;
  	int num2;
  	int choose;
  	printf("请输入两个整数：");
  	scanf("%d%d", &num1, &num2);
  	printf("请输入0~3中的一个数字（它们依次表示加减乘除）：");
  	scanf("%d", &choose);
  	int result = p[choose](num1, num2);
  	printf("%d\n", result);
  	return 0;
  }
  //定义功能函数
  int add(int num1, int num2)
  {
  	return num1 + num2;
  }
  int subtract(int num1, int num2)
  {
  	return num1 - num2;
  }
  int multiply(int num1, int num2)
  {
  	return num1 * num2;
  }
  int except(int num1, int num2)
  {
  	return num1 / num2;
  }
  
  ```

  

## 10. 字符串

### 10.1 获取字符串的两种方式

#### 10.1.1 字符数组——字符串

![image-20240807092540160](images/C/image-20240807092540160.png)

#### 10.1.2 字符串指针

![image-20240807092655153](images/C/image-20240807092655153.png)

![image-20240807093140932](images/C/image-20240807093140932.png)

- 只读常量区强行写的话，会报错

#### 10.1.3 键盘录入字符串并遍历

- 键盘录入字符串，首先需要定义一个字符串变量，这里不能用指针定义，因为指针定义的字符串是位于只读常量区，不可修改

- ```c
  #include <stdio.h>
  
  int main()
  {
  	char str[100];
  	printf("请输入一个字符串：");
  	scanf("%s", str);
  	printf("输入字符串为：%s\n", str);
  	//遍历字符串
  	char* p = str;
  	while (1)
  	{
  		char c = *p;
  		if (c == '\0')  //字符串结束标志符
  		{
  			return 0;
  		}
  		printf("%c\n", c);
  		p++;
  	}
  	return 0;
  }
  
  ```

#### 10.1.4 二维字符串

- 只用二维数组获取/指针数组获取

  - ```c
    #include <stdio.h>
    /*
    * 现在有五个学生的名字，
    现在需要使用字符串定义
    */
    int main()
    {
    	//使用二维数组定义
    	char str[5][100] =
    	{
    		"张三",
    		"李四",
    		"王五",
    		"liyi",
    		"youyou"
    	};
    	//遍历二维数组
    	for (int i = 0; i < 5; i++)
    	{
    		char* str1 = str[i];  //定义数组指针
    		printf("%s\n", str1);
    	}
    	//使用指针字符串定义二维字符串
    	char* strArr[5] =
    	{
    		"张三",
    		"李四",
    		"王五",
    		"liyi",
    		"youyou"
    	};
    	for (int i = 0; i < 5; i++)
    	{
    		char* str2 = strArr[i];
    		printf("%s\n", str2);
    	}
    	return 0;
    }
    
    ```

### 10.2 字符串中的常见函数

- strlen()---可计算字符串的长度

  - 细节1：strlen()统计长度的时候，不包括结束符'\0'

  - 细节2：在windows中，默认一个中文占2个字节

  - ```c
    #include <stdio.h>
    
    int main()
    {
    	char str1[] = "aaaa";
    	printf("%d\n", strlen(str1));
    	return 0;
    }
    
    ```

- strcat()——拼接

  - 细节1：把第二个字符串的全部内容拼接到第一个字符串的末尾，即拼接后的字符串存储在第一个字符串里

    - 前提1：第一个字符串是可以修改的
    - 前提2：第一个字符串剩余的空间可以容纳第二个字符串

  - ```c
    #include <stdio.h>
    
    int main()
    {
    	char str2[40] = "abd";  //目标字符串容量必须足够大
    	char str3[9] = "nnnbb";
    	strcat(str2, str3);
    	printf("%s\n", str2);
    	return 0;
    }
    
    ```

  - 定义的字符串str2/str3一定要说明字符串长度，不然系统会报错，因为不确定字符串的长度，可能导致字符串溢出

- strcpy()——拷贝

  - 细节：把第二个字符串拷贝到第一个字符串的首地址，把第一个字符串给覆盖

    - 前提1：第一个字符串是可以修改的
    - 前提2：第一个字符串剩余的空间可以容纳第二个字符串

  - ```c
    #include <stdio.h>
    
    int main()
    {
    	char str2[40] = "abd";
    	char str3[9] = "nnnbb";
    	strcpy(str2, str3);
    	printf("%s\n", str2);
    	printf("%s\n", str3);
    	return 0;
    }
    ```

- strcmp()

  - 比较函数

    - 细节：要求顺序和内容完全一致，则表示两个字符串相同，返回0
    - 前者大于后者返回1
    - 前者小于后者返回-1

  - ```c
    #include <stdio.h>
    
    int main()
    {
    	char str1[40] = "abd";
    	char str2[9] = "nnnbb";
    	int res = strcmp(str1, str2);
    	printf("%d\n", res);
    	return 0;
    }
    ```

- strlwr()——将字母变小写

  - 只能转换英文的大小写

- strupr()——将字母变大写

  - 只能转换英文的大小写

- ```c
  #include <stdio.h>
  
  int main()
  {
  	char str1[40] = "ADB";
  	char str2[9] = "nnnbb";
  	strlwr(str1); //变小写
  	strupr(str2);  //变大写
  	printf("%s\n", str1);
  	printf("%s\n", str2);
  	return 0;
  }
  ```


### 10.3 练习

- 键盘输入一个字符串，判断里面的大写字母、小写字母和数字分别又多少个

  - ```c
    #include <stdio.h>
    #include <string.h>
    int main()
    {
    	char str[100];
    	printf("请输入一串字符串：");
    	scanf("%s", str);
    	//分别统计，使用计数器思维
    	int Dcont = 0;  //大写
    	int Xcont = 0;  //小写
    	int Scont = 0;  //数字
    	for (int i = 0; i < strlen(str); i++)
    	{
    		char c = str[i];   //依次获取每一个字符
    		if (c >= 'a' && c <= 'z')
    		{
    			Xcont++;
    		}
    		else if (c >= 'A' && c <= 'Z')
    		{
    			Dcont++;
    		}
    		else if (c >= '0' && c <= '9')
    		{
    			Scont++;
    		}
    	}
    	printf("大写字母有%d个\n", Dcont);
    	printf("小写字母有%d个\n", Xcont);
    	printf("数字有%d个\n", Scont);
    	return 0;
    }
    ```

- 已知账户和用户名，用户键盘输入账户和用户名，正确则登录成功，有三次机会，若最后一次也输错的话，账户会被锁定

  - ```c
    #include <stdio.h>
    #include <string.h>
    int main()
    {
    	char name[] = "zhangsan";
    	char password[] = "123yyy";
    	char Yname[100];
    	char Ypassword[100];
    	for (int i = 1; i <= 3; i++)
    	{
    		printf("请输入用户名：");
    		scanf("%s", Yname);
    		printf("请输入用户密码：");
    		scanf("%s", Ypassword);
    		if (!strcmp(name, Yname) && !strcmp(password, Ypassword))
    		{
    			printf("登录成功\n");
    			break;
    		}
    		else if(i != 3)
    		{
    			printf("登录失败，你还有%d次机会\n", 3 - i);
    		}
    		else
    		{
    			printf("用户账号被锁定，请联系官方平台进行解决\n");
    		}
    	}
    	return 0;
    }
    ```

    ![image-20240808103315274](images/C/image-20240808103315274.png)

## 11. 结构体

### 11.1 结构体

![image-20240808110526995](images/C/image-20240808110526995.png)

```c
#include <stdio.h>
#include <string.h>
//定义结构体
struct student
{
	char name[100];
	int age;
};
int main()
{
	//定义学生信息结构体变量，并初始化
	struct student stu1 = { "zhangsan",13 };
	struct student stu2 = { "lisi",14 };
	struct student stu3 = { "wangwu",12 };
	//将学生信息放在一个结构体类型的数组里
	struct student stu[3] = {stu1,stu2,stu3};
	
	//遍历学生信息
	struct student temp;
	for (int i = 0; i < 3; i++)
	{
		temp = stu[i];   //将学生信息赋值给结构体变量
		printf("学生姓名：%s，学生年龄：%d\n", stu[i].name, stu[i].age);
	}
	return 0;
}
```

#### 11.1.1 结构体指针

- 结构体类型的指针获取结构体内元素的值：指针名->元素

- ```c
  #include <stdio.h>
  #include <string.h>
  
  // 定义结构体类型
  typedef struct {
      int value;
      char *description;
  } MyStruct;
  
  int main() {
      MyStruct myStruct = {10, "example description"};
      MyStruct *myStructPtr = &myStruct;
  
      // 输出结构体内元素的值
      printf("Value: %d\n", myStructPtr->value);
      printf("Description: %s\n", myStructPtr->description);
  
      return 0;
  }
  ```

  

### 11.2 起别名

- 使用关键字**typedef**

  - ```c
    #include <stdio.h>
    #include <string.h>
    //定义结构体
    typedef struct student
    {
    	char name[100];
    	int age;
    }S;
    int main()
    {
    	//定义学生信息结构体变量，并初始化
    	S stu1 = { "zhangsan",13 };
    	S stu2 = { "lisi",14 };
    	S stu3 = { "wangwu",12 };
    	//将学生信息放在一个结构体类型的数组里
    	S stu[3] = {stu1,stu2,stu3};
    	
    	//遍历学生信息
    	S temp;
    	for (int i = 0; i < 3; i++)
    	{
    		temp = stu[i];   //将学生信息赋值给结构体变量
    		printf("学生姓名：%s，学生年龄：%d\n", stu[i].name, stu[i].age);
    	}
    	return 0;
    }
    ```

### 11.3 结构体作参数

- 传结构体变量的数值——>不能改变结构体变量的值

- 传结构体变量的地址——>可以改变结构体变量的值

- ```c
  #include <stdio.h>
  #include <string.h>
  //定义结构体
  typedef struct student
  {
  	char name[100];
  	int age;
  }S;
  void mem(S* p);
  //修改结构体中的值
  int main()
  {
  	S stu = { "zhangsan",18 };
  	printf("修改前\n姓名：%s 年龄：%d\n", stu.name, stu.age);
  	mem(&stu);
  	printf("修改后\n姓名：%s 年龄：%d\n", stu.name, stu.age);
  	return 0;
  }
  //传入结构体变量地址
  void mem(S* p)
  {
  	printf("请输入修改后的名字：");
  	scanf("%s", (*p).name);
  	printf("请输入修改后的年龄：");
  	scanf("%d", &((*p).age));
  }
  ```

### 11.4 结构体嵌套

- 结构体变量里包含结构体变量

- ```c
  #include <stdio.h>
  #include <string.h>
  //定义结构体
  typedef struct message
  {
  	char phone[12];
  	char maile[100];
  }M;
  typedef struct student
  {
  	char name[100];
  	int age;
  	M mes;
  }S;
  
  int main()
  {
  	//定义结构体变量并赋值
  	S stu = { "zhangsan",19,{"12367878989","12344@qq.com"} };
  	//打印
  	printf("学生信息为：\n");
  	printf("姓名：%s\n", stu.name);
  	printf("年龄：%d\n", stu.age);
  	printf("电话：%s\n", stu.mes.phone);
  	printf("邮箱：%s\n", stu.mes.maile);
  	return 0;
  }
  
  ```

### 11.5 综合练习

- 题目

  - ![image-20240808165239116](images/C/image-20240808165239116.png)

  - ```c
    #include <stdio.h>
    #include <string.h>
    #include <time.h>
    //定义结构体
    
    typedef struct mesage
    {
    	char spot;
    	int votes;
    }mes;
    int main()
    {
    	mes spots[4] = { {'A',0},{'B',0},{'C',0},{'D',0} };  //定义结构体数组
    	//80个随机数投票
    	srand(time(NULL));
    	for (int i = 0; i < 80; i++)
    	{
    		int choose = rand() % 4;   //0~3,既表示数组的索引，又表示给对应的景点投一票
    		spots[choose].votes++;
    	}
    	//找票数最大值
    	int max = spots[0].votes;
    	for (int i = 1; i < 4; i++)
    	{
    		if (spots[i].votes > max)
    		{
    			max = spots[i].votes;
    		}
    	}
    	//按照优先级，找出票数最高的景点
    	for (int i = 0; i < 4; i++)
    	{
    		if (spots[i].votes == max)
    		{
    			printf("票数最高的景点是%c 票数为%d\n", spots[i].spot, spots[i].votes);
    		}
    	}
    	//遍历
    	for (int i = 0; i < 4; i++)
    	{
    		mes temp = spots[i];
    		printf("景点：%c 票数：%d\n", temp.spot, temp.votes);
    	}
    	return 0;
    }
    
    ```

### 11.6 **内存对齐**-重难点

- 内存对齐：不管是结构体，还是普通变量都存在内存对齐

- 规则：变量位置只能放在自己类型整数倍的地址上

  - 内存地址/占用字节 = 整数，就可以使用该内存地址
  - 结构体的内存对齐还包括：结构体的总大小，是最大类型的整数倍（用来确定最后一个数据的补位情况）
  - 切记：对齐的时候会补空白字节，但是不会改变原来字节的大小。char补位之后，本身还是1个字节

- 心得：定义结构体变量时，数据类型小的写上面，大的写下面，目的是节约空间

- ```c
  #include <stdio.h>
  #include <string.h>
  #include <time.h>
  //定义结构体
  
  typedef struct mesage
  {
  	char spot; //1
  	char c;    //1 + 2(补两个空白字节)
  	int b;   //4
  	int votes;  //4 + 4
  	double d;   //8 
  }mes;
  int main()
  {
  	mes stu;
  	printf("%zu", sizeof(stu));   //24
  	return 0;
  }
  
  ```

  - **定义结构体时不占用内存，当定义结构体变量时系统才会分配内存！**

#### 11.6.1 如何实现1字节对齐

- 一字节对齐就是结构体的每个成员都从内存地址的最低有效位开始存储，不会有填充字节

  - 应用场景：例如当结构体成员需要与硬件寄存器或外部设备对齐时

- 使用指令 #pragma pack(1)，可实现结构体一字节对齐

  - ```c
    #pragma pack(1)
    struct MyStruct {
        uint8_t a;
        uint16_t b;
        uint32_t c;
    };
    #pragma pack()  // 恢复默认对齐方式
    ```

  - `#pragma pack(1)` 指令只影响紧随其后的结构体定义。**因此，如果有多个结构体定义，需要在每个结构体定义之前使用 `#pragma pack(1)`。

    **另外，`#pragma pack(1)` 指令可能会降低编译器的优化效率。**这是因为编译器无法对一字节对齐的结构体进行某些优化，例如缓存对齐。因此，只在你真正需要一字节对齐时才使用这个指令。

## 12. 共用(同)体/联合体

### 12.1 共用体

- 共用体：一个数据有多个数据类型，需要定义共用体，但是每次只能赋值一个值
- ```c
  #include <stdio.h>
  #include <string.h>
  #include <time.h>
  //定义共用体
  typedef union moneyT {
  	int money1;
  	double money2;
  	long money3;
  }MT;
  
  int main()
  {
  	MT money;
  	money.money1 = 14;
  	printf("%d\n", money.money1);  //14
  	printf("%.2lf\n", money.money2);  //随机数，并未赋值
  	money.money2 = 12;
  	printf("%d\n", money.money1);  //赋值被覆盖，已经不是14
  	printf("%.2lf\n", money.money2);   //12.00
  	return 0;
  }
  
  ```

  

### 12.2 共用体的特点

- 共用体，也叫联合体，共同体
- 共用体里所用变量都使用一个内存空间，且都从首地址开始赋值
- 所占内存大小=最大成员的长度（也受内存对齐影响）
  - 所占内存以最大成员长度为准
  - 总大小一定是最大成员长度的整数倍
- 每次只能给一个变量赋值，第二次赋值时会覆盖原有的数据

### 12.3 共用体和结构体的区别

- 代码层面
  - 结构体：一种事物中包含了多种属性
  - 共用体：一个属性有多种类型
- 存储方式
  - 结构体：各存各的
  - 共用体：存一起，多次会覆盖
- 内存占用
  - 结构体：各个变量的总和，总内存是最大类型的整数倍，受内存对齐影响
  - 共用体：等于最大类型，总内存是最大类型的整数倍，受内存对齐影响

## 13. 动态内存分配

### 13.1 常用函数

- ![image-20240809160819147](images/C/image-20240809160819147.png)

- ```c
  #include <stdio.h>
  #include <stdlib.h>
  int main()
  {
  	int* p1 = malloc(10 * sizeof(int));  //返回连续空间的首地址
  	int* p2 = malloc(10 * sizeof(int));  //返回连续空间的首地址
  	int* p3 = calloc(10, sizeof(int));    //分配连续空间，并初始化为0
  	int* p4 = realloc(p1, sizeof(int)*20);    //p1地址的空间后面追加到20，p4指向地址p1
  	printf("%p\n", p1);   //p1与p2地址并不连续，且每次动态分配的内存地址是随机的
  	printf("%p\n", p4);
  	printf("%p\n", p2);
  	printf("%p\n", p3);
  	//赋值
  	for (int i = 0; i < 10; i++)
  	{
  		*(p1 + i) = i + 1;
  	}
  	//遍历
  	for (int i = 0; i < 10; i++)
  	{
  		printf("%d\n", *(p1 + i));
  	}
  	printf("--------------------------------\n");
  	for (int i = 0; i < 10; i++)
  	{
  		printf("%d\n", *(p3 + i));
  	}
  	printf("===================================\n");
  	for (int i = 0; i < 20; i++)
  	{
  		printf("%d\n", *(p4 + i));
  	}
  	free(p1);
  	free(p2);
  	free(p3);
  	//free(p4);   //p4和p1指向同一地址，只需释放一次，这里p4指向的地址已经被释放，再释放就报错
  	return 0;
  }
  
  ```

  - 小结
    - malloc()每次分配的动态内存都是随机的连续空间，即两次分配的空间地址不连续
    - realloc()追加空间是在原有的基础上，返回的地址是依然是原有的地址
    - free()不能重复释放相同的地址，否则会报错

### 13.2 malloc函数的细节

- malloc创建的空间单位是字节
- malloc返回的是void类型的指针，没有步长概念，也无法获取空间中的数据，需强转
- malloc返回的仅仅是首地址，没有总大小，最好定义一个变量记录大小
- malloc申请的空间不会自动消失，如果不能正确释放，会导致内存泄漏
- malloc申请的空间过多时，会产生虚拟内存
- malloc申请的空间没有初始化值，需要先赋值才能使用

### 13.3  其他三个函数的细节

- free释放空间之后，空间中的数据叫脏数据，可能被清空，也可能被修改为其他值
- calloc就是在malloc的基础上多了初始化
- realloc修改之后的空间，首地址值可能变化，也可能不变，但原本的数据不会丢失，如果内存中无法申请空间了，会返回NULL
  - 原来空间后面有足够容量可以容纳扩充的空间的话，首地址可能不变；
  - 原来空间后面没有足够空间的话，首地址变化，但原来的内容会同步粘贴过来；
- realloc修改之后，无需释放原空间，函数底层会进行处理
  - 原地址是p1，扩充之后是p2，用完之后只需释放p2，p1底层函数会处理

### 13.4 C语言的内存结构

![image-20240812144004107](images/C/image-20240812144004107.png)



### 13.5 变量数组在内存中的运行情况

- ![image-20240812144323779](images/C/image-20240812144323779.png)
- 函数中的声明周期跟函数息息相关，函数进栈，变量存在，函数结束出栈，变量消失

### 13.6 全局变量和static变量在内存中的运行情况

- ![image-20240812144904810](images/C/image-20240812144904810.png)
- 代码区：只做临时存储，不运行代码

### 13.7 字符串在内存中的运行情况

- ![image-20240812145306177](images/C/image-20240812145306177.png)

### 13.8 malloc函数在内存的运行情况

- ![image-20240812145709628](images/C/image-20240812145709628.png)
  - 栈里的变量跟函数有关，函数结束，变量也会消失；
  - 堆里的变量，不释放就永远存在

## 14. 文件

- ![image-20240812145955301](images/C/image-20240812145955301.png)

### 14.1 路径

- ![image-20240812154803268](images/C/image-20240812154803268.png)

  

### 14.2 转义字符

- 转义字符：'\'

  - 作用：改变后面第一个字符的含义，对c语言中的任何字符串都起作用

    - ```c
      #include <stdio.h>
      #include <stdlib.h>
      int main()
      {
      	char* file = "C:\aaa\a.c";   //这里的\是转义字符，不会打印出来
      	printf("%s\n", file);
      	return 0;
      }
      
      ```

      ![image-20240812155721143](images/C/image-20240812155721143.png)

    - ```c
      #include <stdio.h>
      #include <stdlib.h>
      int main()
      {
      	char* file = "C:\\aaa\\a.c";  //第一个\是转义字符，第二个\是原本的含义
      	printf("%s\n", file);
      	return 0;
      }
      
      ```

      ![image-20240812155815011](images/C/image-20240812155815011.png)

  

### 14.3 利用fgetc一次读一个字节

- ![image-20240812160051608](images/C/image-20240812160051608.png)

- ![image-20240812162707480](images/C/image-20240812162707480.png)

- 利用fgetc读取文件

  - ```c
    #include <stdio.h>
    int main()
    {
    	//打开文件
    	FILE* file = fopen("C:\\Users\\asus\\Desktop\\test\\a.txt", "r");    //打开模式要用双引号
    	//读取.txt文件，fgetc读取一个字符，读到则返回字符，没有则返回-1
    	int c;
    	while ((c = fgetc(file)) != -1)  //等读取结束标志，以字符为单位依次读取
    	{
    		printf("%c", c);
    	}
    	//关闭文件
    	fclose(file);
    	return 0;
    }
    
    ```

    - .txt文件设置为ANSI编码，即可正常读取打印中文字符
      - ![image-20240812162601564](images/C/image-20240812162601564.png)

### 14.4 利用fets一次读一行字节  

- 一次读一行，以换行符为准，读不到返回NULL

- ```c
  #include <stdio.h>
  int main()
  {
  	//打开文件
  	FILE* file = fopen("C:\\Users\\asus\\Desktop\\test\\a.txt", "r");
  	//读取.txt文件
  	char arr[1024];
  	char* str;
  	while ((str = fgets(arr, 1024, file)) != NULL)  //以行为单位读取，到下一行地址会同步跟上
  	{
  		printf("%s", str);   //每一行都读到了换行符，这里就不需要再写换行符
  	}
  	//关闭文件
  	fclose(file);
  	return 0;
  }
  
  ```

  ![image-20240812163754532](images/C/image-20240812163754532.png)

### 14.5 利用fread一次读多个字节

- 一次读多个字节，没有则返回0，有则返回读取到的字节数

- 细节：在读取的时候，每次都尽可能把数组填满，返回读取到的有效字节数

  - ![image-20240812170106049](images/C/image-20240812170106049.png)

- ```c
  #include <stdio.h>
  int main()
  {
  	//打开文件
  	FILE* file = fopen("C:\\Users\\asus\\Desktop\\test\\a.txt", "r");
  	//读取.txt文件
  	char arr[1024];
  	int n;   //存储返回读取的几个字节数，读不到返回0，有则返回读取的字节数
  	while (n = fread(arr, 1, 1024, file))
  	{
  		for (int i = 0; i < n; i++)  //按照读取的字节数打印出来
  		{
  			printf("%c", arr[i]); //这里输出""内只能有%c，因为中文是两个字节，需要拼接，所以不能有其他字符干扰
  		}
  	}
  	//关闭文件
  	fclose(file);
  	return 0;
  }
  
  ```

  

### 14.6 三种写出数据的方式

- 写数据步骤

  - ![image-20240812171158197](images/C/image-20240812171158197.png)

- fputc：一次写一个字符，返回写出的字符

- fputs：一次写一个字符串，写成功则返回非负数，一般忽略返回值

  - 写失败的话，就返回一个EOF的错误

- fwite：一次写多个，返回写出的字节数

- ```c
  #include <stdio.h>
  #include <string.h>
  int main()
  {
  	//打开文件
  	FILE* file = fopen("C:\\Users\\asus\\Desktop\\test\\a.txt", "w");
  	//写数据
  	//fputc
  	char c = fputc('b',file);
  	printf("%c\n", c);
  	//fputs
  	int r = fputs("\nhuhuhuuh\n", file);  //换行符\n在字符串也同样起换行的作用
  	printf("%d\n", r);
  	//fwrite
  	char str[20] = "bhbhbhbhbhbhbhbhb\n";
  	int n = fwrite(str, 1, 20, file);
  	printf("%d\n", n);
  	//关闭文件
  	fclose(file);
  	return 0;
  }
  
  ```

  

### 14.7 多种读写模式

- ![image-20240812174125466](images/C/image-20240812174125466.png)
- ![image-20240812174219550](images/C/image-20240812174219550.png)
- ![image-20240812174240348](images/C/image-20240812174240348.png)
- 

### 14.8 练习-拷贝文件

- ![image-20240812174645710](images/C/image-20240812174645710.png)

- ```c
  #include <stdio.h>
  #include <string.h>
  int main()
  {
  	//打开文件
  	FILE* file1 = fopen("C:\\Users\\asus\\Desktop\\uuuu.mp4", "rb");
  	FILE* file2 = fopen("C:\\Users\\asus\\Desktop\\test\\copy.mp4", "wb");
  	//复制粘贴
  	char arr[1024];
  	int n;
  	while ((n = fread(arr, 1, 1024, file1)) != 0)
  	{
  		fwrite(arr, 1, n, file2);
  	}
  	//关闭文件
  	fclose(file1);
  	fclose(file2);
  	return 0;
  }
  
  ```

  



## 15. C题库

### 15.1 描写C语言的基本数据类型有哪些

- 数据类型的大小跟操作系统和编译环境有关，具体的话可以在编译环境里写代码测试，Visual Studio

- 指针的大小只与操作系统有关系，32位是4字节，64位是8字节

- unsigned只能与整数类型组合

- 数据长度排序：short <= int <= long <= long long <= float <= double

  - ```c
    #include <stdio.h>
    
    int main()
    {
        int a = 10;
    	int* p = &a;
    	printf("p：%zu\n",sizeof(p));
    	printf("short:%zu\n",sizeof(short));
    	printf("int:%zu\n", sizeof(int));
    	printf("long:%zu\n", sizeof(long));
    	printf("long long:%zu\n", sizeof(long long));
    	printf("float:%zu\n", sizeof(float));
    	printf("double:%zu\n", sizeof(double));
    	printf("char:%zu\n", sizeof(char));
    	return 0;
    }
    
    ```

    

- 整型

  - |      数据类型      | 32位  数据长度字节（x86） | 64位 数据长度字节（x64架构) |
    | :----------------: | :-----------------------: | :-------------------------: |
    |    短整型 short    |             2             |              2              |
    |      整型 int      |             4             |              4              |
    |    长整型 long     |             4             |              4              |
    | 长长整型 long long |             8             |              8              |

    

- 实型

  - | 数据类型 | 32位  数据长度字节（x86架构） | 64位 数据长度字节（x64架构） |
    | :------: | :---------------------------: | :--------------------------: |
    |  float   |               4               |              4               |
    |  double  |               8               |              8               |

    

- 字符型

  - | 数据类型 | 32位  数据长度字节（x86架构） | 64位 数据长度字节（x64架构） |
    | :------: | :---------------------------: | :--------------------------: |
    |   char   |               1               |              1               |

    

### 15.2 在C语言中，#include <stdio.h>和#include "stdio.h"有什么区别

- #include <xxx.h>：表明引用的头文件来自于标准库，即编译器安装时预设的，或者可以通过编译器的设置进行配置
- #include "xxx.h"：表明引用的头文件是用户自定义的，需用指定文件路径，若没有找到，再到标准库中寻找

### 15.3 解释一下什么是数组，并举例说明在C语言中如何定义和使用数组

- 数组就是定义一个连续的存储空间，里面可存储多个相同数据类型的数据，数组定义过后，数据长度不可改变，数组中每个元素都可以通过索引来访问，其中元素数量是已知的

- 数组在定义的时候可以同时进行初始化，若定义的时候没有初始化，之后再引用数组时，只能针对数组中的某一个元素

  - ```c
    #include <stdio.h>
    /* 数组 */
    int main()
    {
    	int buf[10] = { 0 };   //定义一个整型的数组，数组长度为10，并初始化数组中元素都为0
    	/* 赋值数组 */
    	for (int i = 0;i < 10;i++)
    	{
    		buf[i] = i;
    	}
    	/* 在控制台打印数组 */
    	for (int j = 0;j < 10;j++)
    	{
    		printf("buf[%d] = %d\n",j,buf[j]);
    	}
    	return 0;
    }
    
    ```

    - ![image-20241024105132642](images/C/image-20241024105132642.png)

### 15.4 C语言中的指针是什么，请给出一个指针的简单应用案例

- C指针指向另一个变量的地址

- 指针变量：存储指针，而指针的值就是指向另一个变量的地址

- 一级指针与二级指针

  - 一级指针指向变量的地址
  - 二级指针指向一级指针的地址
    - `int **p;` 表示 `p` 是一个指向指针的指针，也就是说，它能够指向一个 `int*` 类型的指针，而这个指针又可以指向一个 `int` 类型的变量的地址
    - `int *` 是一个单级指针，指向一个整数。
    - `int **` 是一个二级指针，指向一个单级指针。

- ```c
  #include <stdio.h>
  /* 指针 */
  int main()
  {
  	int a = 10;
  	int* p = &a;
  	int** p1 = &p;
  	printf("a的地址：%p\n",&a);
  	printf("p存储的地址：%p\n",p);
  	printf("p指向的内容：%d\n",*p);
  	printf("p变量的地址：%p\n", &p);
  	printf("p1指向的地址存储的内容：%p\n", *p1);   //*p1表示p存储的地址
  	printf("p1指向的地址：%p\n", p1);    //存储指向变量p的地址
  	/* 操作指针改变变量a的值 */
  	*p = 8;
  	printf("a = %d\n",a);
  	return 0;
  }
  
  ```

  - ![image-20241024110640514](images/C/image-20241024110640514.png)

### 15.5 描述一下C语言的函数指针，并举例说明其用途

- 

### 15.6 解释C语言中的内存分配函数malloc和calloc的区别

### 15.7 什么是结构体？如何在C语言中定义和使用结构体

### 15.8 在C中如何定义和使用联合体union？它与结构体有什么区别

### 15.9 请解释C语言中的预处理指令，并给出几个常见的预处理指令示例

### 15.10 C语言中的文件操作有哪些，请举例如何打开、读取和关闭一个文件

### 15.11 在C语言中，如何实现字符串的拼接

### 15.12 请描述C语言中的switch语句，与if-else进行比较

### 15.13 C语言中的循环语句有哪些，请分别给出示例

### 15.14 解释一下什么是变量的作用域和生命周期，在C语言中它们是如何体现的

### 15.15 请描述一下C语言中的动态内存分配，并举例说明其应用场景

