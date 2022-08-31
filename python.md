split()：拆分字符串。通过指定分隔符对字符串进行切片，并返回分割后的字符串列表（list）

a=['1.2','2']
a=[int(i) for i in a]****str->int->float
		str->float

eval():去掉参数最外侧引号并执行余下语句的函数(num类)
格式化输入格式：list（set）（eval（input））、、可输入【】

表示虚数的语法：real+imagj 虚数部分必须有j或J

isdigit()  isalpha()    isalnum()
数字        英文中文	dig+alp
1.python官方定义中的字母：大家默认为英文字母+汉字即可
2.python官方定义中的数字：大家默认为阿拉伯数字+带圈的数字即可

交换两个数的值
a,b=b,a
多个值赋值
a,b,c,d=0,0,0,0

sin()函数-->import math
math.sin(参数)
sin（30°）==math.sin(30*math.pi/180)
使用e时要用math.e
log()默认底数为e

分离时%取余（记得多分配一个变量，作比较）
%取后  //取前
最后一位%10
第一位  //位数

使用函数时不要忘记传入参数

join函数的迭代对象要为字符
[1,2,3,3]不可
['2','1']可

二进制     八进制      十进制    十六进制
bin（）    oct（）     int（）   hex（）
   无             %o         %d          %x不带前缀
{:b}	{:o}		{:x}.format-->不带前缀

int（str，进制）

方式1：直接使用参数格式化：{:.2%}
方式2：格式化为float，然后处理成%格式： {:.2f}%

对字典的值排序：x=sorted(d.items(), key=lambda item:item[1],reverse=True)
d = {'a': 1, 'c': 3, 'b': 2}
# d1=sorted(d.items(),key=lambda x:x[0],reverse=False)
# print(d1)

d2=list(d.items())
d2.sort(key=lambda x:x[1],reverse=False)
print(d2)

list.sort() 不会返回对象，会改变原有的list
(这点与sorted()不同，sorted()函数会返回一个列表，而sort（）函数是直接在原来的基础上修改)

ord--->求字符ascii只能接受str
chr--->ascii转化为字符

对数字分离时不可直接list
要先str然后才list

join要连接对象列表，元组，字典，不能单个字符在循环中

对于“  str”
list（）会直接一个一个分开，
当要对文本分离时(含有空格)要+spilt

python
一行就是一次输入

import string 产生各种字符
字符串不可改变修改

str.strip('指定字符')默认删除所有空格（可用于输入时去除空格）
str.lstrip('指定字符')
str.rstrip('指定字符')同理

pass

打出某部分子串直接

43
17
40
字典合并
zip
解包
random
异常

shu文件复制
str.replace（old，new，[max]）方法用于字符串的修改
！！！字符串是不可变类型，因此要重新赋值给一个变量

空集合set（{}）
对集合添加元素add（）

补零：
str.zfill(num)

python-->解释器
python IDLE -->编辑器
pip -->python自带的库管理器

pcharm-->编辑器

Anaconda -->大集成者
自带pip，解释器

for else
for正常执行else也会执行
for 中执行break后else不会执行
while else同理

访问列
ff['name']
ff.name
ff.loc[:,'name']
loc查找多列时要加[],eg['Q1','Q2']

筛选ff[ff.team=='C'].loc[ff.Q1>90]

创建新列
ff['total']=ff.Q1+ff.Q2+ff.Q3+ff.Q4

聚合
gg.groupby('team').agg({'Q1':'sum','Q2':'count','Q3':'mean','Q4':'max'})集合



#一级标题