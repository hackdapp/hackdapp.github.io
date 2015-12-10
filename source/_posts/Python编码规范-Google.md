title: Python编码规范(Google)
date: 2015-10-30 11:13:04
tags:
---
导航目录:  

- [命名规范](#命名规范)
- [语句/方法体](#分号)  
- [注释](#注释)   
- [类定义](#类定义)   
- [引用](#引用)  
- [访问控制](#访问控制)  


----

### 分号

1. 不要在行尾加分号
2. 不要用分号将两条命令放在同一行

### 行长度

1. 每行不超过80个字符(`长的导入模块语句`/`注释里的URL`)
. 了2. 不要使用反斜杠连接行  


**正确使用:**
  
    /*Python会将**圆括号, 中括号和花括号**中的行隐式的连接, 所以可以在表达式外添加一对括号*/
    
    /*1. 函数*/
    foo_bar(self, width, height, color='black', design=None, x='foo', 
            emphasis=None, highlight=0)
    if (width == 0 and height == 0 and
        color == 'red' and emphasis == 'strong'):

    /*2. 字符串*/
    x = ('这是一个非常长非常长非常长非常长 '
     '非常长非常长非常长非常长非常长非常长的字符串')


### 括号
- 用于行连接
- 元组

**正确使用:**
        
        if foo:
             bar()
        while x:
             x = bar()
        if x and y:
             bar()
        if not x:
             bar()
        return foo
        
        for (x, y) in dict.items(): ...



### 缩进
---
- 用4个空格来缩进代码(`绝对不要用tab`)


---
转载:(http://www.runoob.com/w3cnote/google-python-styleguide.html)



