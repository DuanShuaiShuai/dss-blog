## 入门
### 基础
- 显示普通字符串:
```bash
echo "It is a test"
# 等价
echo It is a test

```
- 显示转义字符
```bash 
echo "\"It is a test\""
```
- 显示变量
```bash 
#!/bin/sh
read name 
echo "$name It is a test"
```
- 显示换行
```bash 
echo -e "OK! \n" # -e 开启转义
echo "It is a test"
# 结果
OK!

It is a test
```
- 显示不换行
```bash 
#!/bin/sh
echo -e "OK! \c" # -e 开启转义 \c 不换行
echo "It is a test"
# 结果
OK! It is a test
```
- 显示结果定向至文件
```bash 
#!/bin/sh
echo "It is a test" > myfile
```

- 原样输出字符串，不进行转义或取变量(用单引号)
```bash 
#!/bin/sh
echo '$name\"'
# 结果
$name\"
```

- 显示命令执行结果
```bash 
#!/bin/sh
echo `date`
# 结果 这里使用的是反引号 `, 而不是单引号 '。
Thu Jul 24 10:08:46 CST 2014
```
# 笔记
- 单引号/双引号/无引号区别
```bash
===================================================================

-----  |能否引用变量  |  能否引用转移符  |  能否引用文本格式符(如：换行符、制表符)

单引号  |    否       |      否         |         否

双引号  |    能       |      能         |         能

无引号  |    能       |      能         |         否                          

===================================================================
```


- read 命令实践
```bash
# read 命令一个一个词组地接收输入的参数，每个词组需要使用空格进行分隔；如果输入的词组个数大于需要的参数个数，则多出的词组将被作为整体为最后一个参数接收。
# test.sh
 read firstStr secondStr
 echo "第一个参数:$firstStr; 第二个参数:$secondStr"
# 执行测试：
$ sh test.sh 一 二 三 四
第一个参数:一; 第二个参数:二 三 四


实例, 文件 test1.sh:
read -p "请输入一段文字:" -n 6 -t 5 -s password
echo -e "\npassword is $password"

$ sh test1.sh 
请输入一段文字:
password is asdfgh

参数说明：
 -p 输入提示文字
 -n 输入字符长度限制(达到6位，自动结束)
 -t 输入限时
 -s 隐藏输入内容
```


- 特殊符号
```bash
> 重定向输出到某个位置，替换原有文件的所有内容。
>> 重定向追加到某个位置，在原有文件的末尾添加内容。
< 重定向输入某个位置文件。
2> 重定向错误输出。
2>> 重定向错误追加输出到文件末尾。
&> 混合输出错误的和正确的都输出。
```

