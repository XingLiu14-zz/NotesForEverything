## python基础

### 数据结构
字符串可以用“”或者‘’包起来，当字符串本身有‘’那就用“”包，如果都有那就用\转义  
r''里面的字符串默认不转义；'''...'''可以包含多行内容  
同一个变量可以被赋予不同的数据类型，所以python是动态语言  
python中，全部大写的变量名表示常量  
//是地板除，只返回整数

### List
`初始化`：classmates = ['Michael', 'Bob', 'Tracy']  
用len(classmates)获得长度，classmates[1]获得元素，用-1等索引可以获得倒数第几个元素  
LIST.append('james')，LIST.insert(position, 'james')；用pop()删除最后一个元素或指定位置的元素  
直接赋值给对应索引位置可以更换元素值；list中元素数据类型可以不相同（比如另一个list）  

### Tuple
也是一种有序集合，用()包起来，创建之后不能修改：classmates = ('Michael', 'Bob', 'Tracy')，没append、insert这些方法  
创建一个只有一个元素的tuple时，为了消除歧义，加一个逗号：t = (1,)  
当tuple有一个list元素的时候，它似乎变成了“可变”的

### 条件判断
elif是else if的缩写  
*if x:*，只要x不是非零数值，非空字符串，非空list等，都判断true  
input()返回的是一个字符串，若需要比较用int()等函数转换  

### 循环
`for...in...`：常常搭配range(INT)来循环  
`while...`：

### dict
其他语言的map，`初始化`：d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}  
可以通过key放入：d['Adam'] = 67；如果key不存在则会报错  
可以用`'Thomas' in d`来判断key存不存在；或者用dict自带的get()函数（不存在则返回None）  
使用pop(key)把对应的值删除  
因为dict的key需要是不可变量，所以不能使用list，可以使用数字和字符串

### set
和dict类似，但没有value，只储存key，没有重复的key  
`初始化（用一个list）`：s = set([1, 2, 3])  
用SET.add(key)来添加元素，用SET.remove()来删除元素  
两个set可以做数学意义上的交集（&）和并集（|）等