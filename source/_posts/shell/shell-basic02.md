## 入门


### 基础
- [参考文章](https://www.runoob.com/linux/linux-shell-basic-operators.html)
- Shell 传参
  - 我们可以在执行 Shell 脚本时，向脚本传递参数，脚本内获取参数的格式为：$n。n 代表一个数字，1 为执行脚本的第一个参数，2 为执行脚本的第二个参数，以此类推……
  ```bash 
    #!/bin/bash
    echo "Shell 传递参数实例！";
    echo "执行的文件名：$0";
    echo "第一个参数为：$1";
    echo "第二个参数为：$2";
    echo "第三个参数为：$3";
    # $ chmod +x test.sh 
    # $ ./test.sh 1 2 3
    # Shell 传递参数实例！
    # 执行的文件名：./test.sh
    # 第一个参数为：1
    # 第二个参数为：2
    # 第三个参数为：3
  ```
  - 特殊字符
    - $# 传递到脚本的参数个数
    - $* 以一个单字符串显示所有向脚本传递的参数。 如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。
    - $$ 脚本运行的当前进程ID号
    - $! 后台运行的最后一个进程的ID号
    - $@ 与$*相同，但是使用时加引号，并在引号中返回每个参数。如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。
    - $- 显示Shell使用的当前选项，与set命令功能相同。
    - $? 显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。
    ```bash 
    #!/bin/bash
    echo "Shell 传递参数实例！";
    echo "第一个参数为：$1";
    echo "参数个数为：$#";
    echo "传递的参数作为一个字符串显示：$*";
    # $ chmod +x test.sh 
    # $ ./test.sh 1 2 3
    # Shell 传递参数实例！
    # 第一个参数为：1
    # 参数个数为：3
    # 传递的参数作为一个字符串显示：1 2 3
    ```
- 运算符
  - 原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用。expr 是一款表达式计算工具，使用它能完成表达式的求值操作。
  ```bash 
    #!/bin/bash
    val=`expr 2 + 2`
    echo "两数之和为 : $val"
    # 注意点
    # 表达式和运算符之间要有空格，例如 2+2 是不对的，必须写成 2 + 2，这与我们熟悉的大多数编程语言不一样。
    # 完整的表达式要被 ` ` 包含，注意这个字符不是常用的单引号，在 Esc 键下边
  ```
  - 算术运算符
  ```bash 
  #!/bin/bash
  a=10
  b=20

  val=`expr $a + $b`
  echo "a + b : $val"

  val=`expr $a - $b`
  echo "a - b : $val"

  val=`expr $a \* $b`
  echo "a * b : $val"

  val=`expr $b / $a`
  echo "b / a : $val"

  val=`expr $b % $a`
  echo "b % a : $val"

  if [ $a == $b ]
  then
    echo "a 等于 b"
  fi
  if [ $a != $b ]
  then
    echo "a 不等于 b"
  fi
  # a + b : 30
  # a - b : -10
  # a * b : 200
  # b / a : 2
  # b % a : 0
  # a 不等于 b

  #############################
  # 加 `expr $a + $b` 结果为 30。
  # 减 `expr $a - $b` 结果为 -10。
  # 乘 `expr $a \* $b` 结果为  200。
  # 除 `expr $b / $a` 结果为 2。
  # 余 `expr $b % $a` 结果为 0。
  # 赋 a=$b 将把变量 b 的值赋给 a。
  # 相等 [ $a == $b ] 返回 false。
  # 不相等 [ $a != $b ] 返回 true。
  # 注意：条件表达式要放在方括号之间，并且要有空格，例如: [$a==$b] 是错误的，必须写成 [ $a == $b ]。


  ##############注意点#################
  # 乘号(*)前边必须加反斜杠(\)才能实现乘法运算；
  # 在 MAC 中 shell 的 expr 语法是：$((表达式))，此处表达式中的 "*" 不需要转义符号 "\" 。
  ```
- 关系运算符
  - 关系运算符只支持数字，不支持字符串，除非字符串的值是数字。
  ```bash 
  #!/bin/bash
  a=10
  b=20
  if [ $a -eq $b ]
  then
    echo "$a -eq $b : a 等于 b"
  else
    echo "$a -eq $b: a 不等于 b"
  fi
  if [ $a -ne $b ]
  then
    echo "$a -ne $b: a 不等于 b"
  else
    echo "$a -ne $b : a 等于 b"
  fi
  if [ $a -gt $b ]
  then
    echo "$a -gt $b: a 大于 b"
  else
    echo "$a -gt $b: a 不大于 b"
  fi
  if [ $a -lt $b ]
  then
    echo "$a -lt $b: a 小于 b"
  else
    echo "$a -lt $b: a 不小于 b"
  fi
  if [ $a -ge $b ]
  then
    echo "$a -ge $b: a 大于或等于 b"
  else
    echo "$a -ge $b: a 小于 b"
  fi
  if [ $a -le $b ]
  then
    echo "$a -le $b: a 小于或等于 b"
  else
    echo "$a -le $b: a 大于 b"
  fi
  #####################总结#######################
  # 10 -eq 20: a 不等于 b
  # 10 -ne 20: a 不等于 b
  # 10 -gt 20: a 不大于 b
  # 10 -lt 20: a 小于 b
  # 10 -ge 20: a 小于 b
  # 10 -le 20: a 小于或等于 b
  ###############################################
  # -eq	检测两个数是否相等，相等返回 true。	[ $a -eq $b ] 返回 false。
  # -ne	检测两个数是否不相等，不相等返回 true。	[ $a -ne $b ] 返回 true。
  # -gt	检测左边的数是否大于右边的，如果是，则返回 true。	[ $a -gt $b ] 返回 false。
  # -lt	检测左边的数是否小于右边的，如果是，则返回 true。	[ $a -lt $b ] 返回 true。
  # -ge	检测左边的数是否大于等于右边的，如果是，则返回 true。	[ $a -ge $b ] 返回 false。
  # -le	检测左边的数是否小于等于右边的，如果是，则返回 true。	[ $a -le $b ] 返回 true。
  ```
- 布尔运算符
  - 布尔运算符 ！ \-o  \-a
  ```bash 
  #!/bin/bash
  a=10
  b=20

  if [ $a != $b ]
  then
    echo "$a != $b : a 不等于 b"
  else
    echo "$a == $b: a 等于 b"
  fi
  if [ $a -lt 100 -a $b -gt 15 ]
  then
    echo "$a 小于 100 且 $b 大于 15 : 返回 true"
  else
    echo "$a 小于 100 且 $b 大于 15 : 返回 false"
  fi
  if [ $a -lt 100 -o $b -gt 100 ]
  then
    echo "$a 小于 100 或 $b 大于 100 : 返回 true"
  else
    echo "$a 小于 100 或 $b 大于 100 : 返回 false"
  fi
  if [ $a -lt 5 -o $b -gt 100 ]
  then
    echo "$a 小于 5 或 $b 大于 100 : 返回 true"
  else
    echo "$a 小于 5 或 $b 大于 100 : 返回 false"
  fi
  #####################结果######################
  # 10 != 20 : a 不等于 b
  # 10 小于 100 且 20 大于 15 : 返回 true
  # 10 小于 100 或 20 大于 100 : 返回 true
  # 10 小于 5 或 20 大于 100 : 返回 false
  ###############################################
  # !	非运算，表达式为 true 则返回 false，否则返回 true。	[ ! false ] 返回 true。
  # -o	或运算，有一个表达式为 true 则返回 true。	[ $a -lt 20 -o $b -gt 100 ] 返回 true。
  # -a	与运算，两个表达式都为 true 才返回 true。	[ $a -lt 20 -a $b -gt 100 ] 返回 false。
  ```
- 逻辑运算符
  - 逻辑运算符 && ||
  ```bash 
  #!/bin/bash
  a=10
  b=20

  if [[ $a -lt 100 && $b -gt 100 ]]
  then
    echo "返回 true"
  else
    echo "返回 false"
  fi

  if [[ $a -lt 100 || $b -gt 100 ]]
  then
    echo "返回 true"
  else
    echo "返回 false"
  fi
  #########################结果##############################
  # 返回 false
  # 返回 true
  ###########################################################
  # &&	逻辑的 AND	[[ $a -lt 100 && $b -gt 100 ]] 返回 false
  # ||	逻辑的 OR	[[ $a -lt 100 || $b -gt 100 ]] 返回 true
  ```
- 字符串运算符
  - 下表列出了常用的字符串运算符，假定变量 a 为 "abc"，变量 b 为 "efg"：
  ```bash 
  #!/bin/bash
  a="abc"
  b="efg"

  if [ $a = $b ]
  then
    echo "$a = $b : a 等于 b"
  else
    echo "$a = $b: a 不等于 b"
  fi
  if [ $a != $b ]
  then
    echo "$a != $b : a 不等于 b"
  else
    echo "$a != $b: a 等于 b"
  fi
  if [ -z $a ]
  then
    echo "-z $a : 字符串长度为 0"
  else
    echo "-z $a : 字符串长度不为 0"
  fi
  if [ -n "$a" ]
  then
    echo "-n $a : 字符串长度不为 0"
  else
    echo "-n $a : 字符串长度为 0"
  fi
  if [ $a ]
  then
    echo "$a : 字符串不为空"
  else
    echo "$a : 字符串为空"
  fi
  ###########################结果###############################
  # abc = efg: a 不等于 b
  # abc != efg : a 不等于 b
  # -z abc : 字符串长度不为 0
  # -n abc : 字符串长度不为 0
  # abc : 字符串不为空
  ############################总结##############################
  # =	检测两个字符串是否相等，相等返回 true。	[ $a = $b ] 返回 false。
  # !=	检测两个字符串是否相等，不相等返回 true。	[ $a != $b ] 返回 true。
  # -z	检测字符串长度是否为0，为0返回 true。	[ -z $a ] 返回 false。
  # -n	检测字符串长度是否不为 0，不为 0 返回 true。	[ -n "$a" ] 返回 true。
  # $	检测字符串是否为空，不为空返回 true。	[ $a ] 返回 true。
  ```
- 文件测试运算符
  - 文件测试运算符用于检测 Unix 文件的各种属性
  ```bash 
    #!/bin/bash
    # url:www.runoob.com

    file="/var/www/runoob/test.sh"
    if [ -r $file ]
    then
      echo "文件可读"
    else
      echo "文件不可读"
    fi
    if [ -w $file ]
    then
      echo "文件可写"
    else
      echo "文件不可写"
    fi
    if [ -x $file ]
    then
      echo "文件可执行"
    else
      echo "文件不可执行"
    fi
    if [ -f $file ]
    then
      echo "文件为普通文件"
    else
      echo "文件为特殊文件"
    fi
    if [ -d $file ]
    then
      echo "文件是个目录"
    else
      echo "文件不是个目录"
    fi
    if [ -s $file ]
    then
      echo "文件不为空"
    else
      echo "文件为空"
    fi
    if [ -e $file ]
    then
      echo "文件存在"
    else
      echo "文件不存在"
    fi
    #######################结果####################
    # 文件可读
    # 文件可写
    # 文件可执行
    # 文件为普通文件
    # 文件不是个目录
    # 文件不为空
    # 文件存在
    ##############################################
    # -b file	检测文件是否是块设备文件，如果是，则返回 true。	[ -b $file ] 返回 false。
    # -c file	检测文件是否是字符设备文件，如果是，则返回 true。	[ -c $file ] 返回 false。
    # -d file	检测文件是否是目录，如果是，则返回 true。	[ -d $file ] 返回 false。
    # -f file	检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。	[ # -f $file ] 返回 true。
    # -g file	检测文件是否设置了 SGID 位，如果是，则返回 true。	[ -g $file ] 返回 false。
    # -k file	检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true。	[ -k $file ] 返回 # false。
    # -p file	检测文件是否是有名管道，如果是，则返回 true。	[ -p $file ] 返回 false。
    # -u file	检测文件是否设置了 SUID 位，如果是，则返回 true。	[ -u $file ] 返回 false。
    # -r file	检测文件是否可读，如果是，则返回 true。	[ -r $file ] 返回 true。
    # -w file	检测文件是否可写，如果是，则返回 true。	[ -w $file ] 返回 true。
    # -x file	检测文件是否可执行，如果是，则返回 true。	[ -x $file ] 返回 true。
    # -s file	检测文件是否为空（文件大小是否大于0），不为空返回 true。	[ -s $file ] 返回 # true。
    # -e file	检测文件（包括目录）是否存在，如果是，则返回 true。	[ -e $file ] 返回 true。
  ```

- 笔记
```bash
# 1 缩写
EQ 就是 EQUAL等于
NE 就是 NOT EQUAL不等于 
GT 就是 GREATER THAN大于　 
LT 就是 LESS THAN小于 
GE 就是 GREATER THAN OR EQUAL 大于等于 
LE 就是 LESS THAN OR EQUAL 小于等于

# 2 [[]]
使用 [[ ... ]] 条件判断结构，而不是 [ ... ]，能够防止脚本中的许多逻辑错误。比如，&&、||、< 和 > 操作符能够正常存在于 [[ ]] 条件判断结构中，但是如果出现在 [ ] 结构中的话，会报错。

# 3 
1、进行数值比较时，可以使用 [ expression1 OPexpression2 ]，OP 可以为 -gt、-lt、-ge、-le、-eq、-ne 也可以使用 ((expression1 OP expression2))，OP 可以为 >、<、>=、<=、==、!=。这几个关系运算符都是测试整数表达式 expression1 和 expression2 之间的大小关系。
2、 >、<、==、!= 也可以进行字符串比较。
3、进行字符串比较时，== 可以使用 = 替代。
4、 == 和 !=进行字符串比较时，可以使用 [ string1 OP string2 ] 或者 [[ string1 OP string2 ]] 的形式。
5、 > 和 < 进行字符串比较时，需要使用[[ string1 OP string2 ]] 或者 [ string1 \OP string2 ]。也就是使用 [] 时，> 和 < 需要使用反斜线转义。


# 4 
# 字符串比较是否为 null 这里:
a=""
if [ -n $a ]
then
   echo "-n $a : 字符串长度不为 0"
else
   echo "-n $a : 字符串长度为 0"
fi
# 结果 -n  : 字符串长度不为 0  ,这里面的if [ -n $a ] 等价于 if [ -n ]   永远为true


a=""
if [ -n "$a" ]
then
   echo "-n $a : 字符串长度不为 0"
else
   echo "-n $a : 字符串长度为 0"
fi
# -n  : 字符串长度为 0  比较字符串的好习惯 带上双引号

# 5 Shell 相加目前发现有 4 种写法：
a=10
b=20
c=`expr ${a} + ${b}`
echo "$c"

c=$[ `expr 10 + 20` ]
echo "$c"

c=$[ 10 + 20 ]
echo "$c"

# 感觉符合$xx 与xx的格式就行
# 6 推荐用 $() 代替 ``
val=`expr 10 + 20`
val=$(expr 10 + 20)

# 7 表达式
# 注意：在 [] 表达式中，常见的 >, < 需要加转义字符，表示字符串大小比较，以 acill 码位置作为比较。不直接支持 >, < 运算符，还有逻辑运算符 || 、&& ，它需要用 -a[and] –o[or] 表示。

# 注意：[[]] 运算符只是 [] 运算符的扩充。能够支持 >, < 符号运算不需要转义符，它还是以字符串比较大小。里面支持逻辑运算符：|| && ，不再使用 -a -o。

```
