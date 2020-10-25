## 入门


### 基础
- 运行 Shell 脚本有两种方法
  ```bash 
  # 1 作为可执行程序
  chmod +x ./test.sh  #使脚本具有执行权限
  ./test.sh  #执行脚本
  # 2 作为解释器参数
  /bin/sh test.sh
  /bin/php test.php
  ```
- 变量
  - 变量名和等号之间不能有空格，这可能和你熟悉的所有编程语言都不一样。同时，变量名的命名须遵循如下规则
    - 命名只能使用英文字母，数字和下划线，首个字符不能以数字开头
    - 中间不能有空格，可以使用下划线（_）
    - 不能使用标点符号
    - 不能使用bash里的关键字（可用help命令查看保留关键字）
    ```bash
      your_name="runoob.com"
    ```
  - 使用变量
    - 使用一个定义过的变量，只要在变量名前面加美元符号即可
    ```bash
      your_name="qinjx"
      echo $your_name
      echo ${your_name}
      for skill in Ada Coffe Action Java; do
          echo "I am good at ${skill}Script"
      done
    ```
  - 只读变量
    ```bash 
      #!/bin/bash
      myUrl="https://www.google.com"
      readonly myUrl
      myUrl="https://www.runoob.com"
      #ERROR:/bin/sh: NAME: This variable is read only.
    ```
  - 删除变量
    - 使用 unset 命令可以删除变量。
    ```bash
      #!/bin/sh
      myUrl="https://www.runoob.com"
      unset myUrl
      echo $myUrl
      # 以上实例执行将没有任何输出。
    ```
  - 变量类型
    - 运行shell时，会同时存在三种变量
      - 局部变量在脚本或命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量。
      - 环境变量 所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。
      - shell变量 shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行
- 字符串
  - 字符串可以用单引号，也可以用双引号，也可以不用引号。
  ```bash
    your_name='runoob'
    str="Hello, I know you are \"$your_name\"! \n"
    echo -e $str
    #Hello, I know you are "runoob"! 
    your_name="runoob"
    # 使用双引号拼接
    greeting="hello, "$your_name" !"
    greeting_1="hello, ${your_name} !"
    echo $greeting  $greeting_1
    # 使用单引号拼接
    greeting_2='hello, '$your_name' !'
    greeting_3='hello, ${your_name} !'
    echo $greeting_2  $greeting_3
    # hello, runoob ! hello, runoob !
    # hello, runoob ! hello, ${your_name} !
  ```
  - 获取字符串长度
  ```bash
    string="abcd"
    echo ${#string} #输出 4

    string="hello,everyone my name is xiaoming"
    expr length "$string"
    # 输出:34
    # 使用 expr 命令时，表达式中的运算符左右必须包含空格，如果不包含空格，将会输出表达式本身:
    expr 5+6    # 直接输出 5+6
    expr 5 + 6  # 输出 11
    #对于某些运算符，还需要我们使用符号"\"进行转义，否则就会提示语法错误。
    expr 5 * 6  # 输出错误
    expr 5 \* 6     # 输出30
  ```
  - 提取子字符串
  ```bash
    string="runoob is a great site"
    echo ${string:1:4} # 输出 unoo
  ```
  - 查找子字符串
  ```bash
    string="runoob is a great site"
    echo `expr index "$string" io`  # 输出 4
  ```
  - \# 号截取，删除左边字符，保留右边字符。
  ```bash 
  var=http://www.aaa.com/123.htm
  echo ${var#*//}
  # 其中 var 是变量名，# 号是运算符，*// 表示从左边开始删除第一个 // 号及左边的所有字符 即删除 http://  结果是 ：www.aaa.com/123.htm
  ```
  - \#\# 号截取，删除左边字符，保留右边字符。
  ```bash 
  var=http://www.aaa.com/123.htm
  echo ${var##*/}
  # ##*/ 表示从左边开始删除最后（最右边）一个 / 号及左边的所有字符  即删除 http://www.aaa.com/   结果是 123.htm
  ```
  - %号截取，删除右边字符，保留左边字符
  ```bash 
  var=http://www.aaa.com/123.htm
  echo ${var%/*}
  # %/* 表示从右边开始，删除第一个 / 号及右边的字符  结果是：http://www.aaa.com
  ```
  - %% 号截取，删除右边字符，保留左边字符
  ```bash 
  var=http://www.aaa.com/123.htm
  echo ${var%%/*}
  # %%/* 表示从右边开始，删除最后（最左边）一个 / 号及右边的字符 结果是：http:
  ```
  -  从左边第几个字符开始，及字符的个数
  ```bash 
  var=http://www.aaa.com/123.htm
  echo ${var:0:5}
  # 其中的 0 表示左边第一个字符开始，5 表示字符的总个数。 结果是：http:
  ```
  - 从左边第几个字符开始，一直到结束。
  ```bash 
  var=http://www.aaa.com/123.htm
  echo ${var:7}
  # 其中的 7 表示左边第8个字符开始，一直到结束。 结果是 ：www.aaa.com/123.htm
  ```
  - 从右边第几个字符开始，及字符的个数
  ```bash 
  var=http://www.aaa.com/123.htm
  echo ${var:0-7:3}
  # 其中的 0-7 表示右边算起第七个字符开始，3 表示字符的个数。 结果是：123
  ```
  - 从右边第几个字符开始，一直到结束。
  ```bash 
  var=http://www.aaa.com/123.htm
  echo ${var:0-7}
  # 表示从右边第七个字符开始，一直到结束。  结果是：123.htm
  # 注：（左边的第一个字符是用 0 表示，右边的第一个字符用 0-1 表示）
  ```
  - [参考链接](https://www.runoob.com/linux/linux-shell-variable.html)
- 数组
  - bash支持一维数组（不支持多维数组），并且没有限定数组的大小。
  ```bash
  # 在 Shell 中，用括号来表示数组，数组元素用"空格"符号分割开。定义数组的一般形式为：
  # 数组名=(值1 值2 ... 值n)
  # 列如 array_name=(value0 value1 value2 value3)
   array_name=(
    value0
    value1
    value2
    value3
    )
  # 还可以单独定义数组的各个分量：
  array_name[0]=value0
  array_name[1]=value1
  array_name[n]=valuen
  ```
  - 读取数组
  ```bash 
  # 读取数组元素值的一般格式是：
  # ${数组名[下标]}
   valuen=${array_name[n]}
  # 使用 @ 符号可以获取数组中的所有元素，例如：
   echo ${array_name[@]}
  ```
- 获取数组的长度
```bash
# 取得数组元素的个数
length=${#array_name[@]}
# 或者
length=${#array_name[*]}
# 取得数组单个元素的长度
lengthn=${#array_name[n]}
```
- 注释
  - 单行注释:以 # 开头的行就是注释，会被解释器忽略。
  ```bash 
    #--------------------------------------------
    # 这是一个注释
    # author：个人学习网站
    # site：www.qccjs.com
    # slogan：学的不仅是技术，更是梦想！
    #--------------------------------------------
    ##### 用户配置区 开始 #####
    #
    #
    # 这里可以添加脚本描述信息
    # 
    #
    ##### 用户配置区 结束  #####
  ```
  - 多行注释:多行注释还可以使用以下格式
  ```bash 
  :<<EOF
  注释内容...
  注释内容...
  注释内容...
  EOF
  # or
  :<<'
  注释内容...
  注释内容...
  注释内容...
  '
  # or
  :<<!
  注释内容...
  注释内容...
  注释内容...
  !
  ```
- read 命令用于获取键盘输入信息
  ```BASH
    # 它的语法形式一般是：
    read [-options] [variable...]
    # 以下实例读取键盘输入的内容并将其赋值给shell变量，为：-p 参数由于设置提示信息：
    read -p "input a val:" a    #获取键盘输入的 a 变量数字
    read -p "input b val:" b    #获取键盘输入的 b 变量数字
    r=$[a+b]                    #计算a+b的结果 赋值给r  不能有空格
    echo "result = ${r}"        #输出显示结果 r
    # 测试结果：
    input a val:1
    input b val:2
    result = 3
  ```
- 汇总 ###%%%* 的用法
  ```bash 
  #!/bin/bash
  # 字符串截取（界定字符本身也会被删除）
  str="www.runoob.com/linux/linux-shell-variable.html"
  echo "str    : ${str}"
  echo "str#*/    : ${str#*/}"   # 从 字符串开头 删除到 左数第一个'/'
  echo "str##*/    : ${str##*/}"  # 从 字符串开头 删除到 左数最后一个'/'
  echo "str%/*    : ${str%/*}"   # 从 字符串末尾 删除到 右数第一个'/'
  echo "str%%/*    : ${str%%/*}"  # 从 字符串末尾 删除到 右数最后一个'/'
  echo
  echo "str#/*    : ${str#/*}"   # 无效果
  echo "str##/*    : ${str##/*}"  # 无效果
  echo "str%*/    : ${str%*/}"   # 无效果
  echo "str%%*/    : ${str%%*/}"  # 无效果
  # 输出结果：
  str     : www.runoob.com/linux/linux-shell-variable.html
  str#*/  : linux/linux-shell-variable.html
  str##*/ : linux-shell-variable.html
  str%/*  : www.runoob.com/linux
  str%%/* : www.runoob.com

  str#/*  : www.runoob.com/linux/linux-shell-variable.html
  str##/* : www.runoob.com/linux/linux-shell-variable.html
  str%*/  : www.runoob.com/linux/linux-shell-variable.html
  str%%*/ : www.runoob.com/linux/linux-shell-variable.html
  ```