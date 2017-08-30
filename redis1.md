###数据类型
- string
- hash
- list 类似java queue
- set
- zset


###如何设计  
id | name     |  age  
<- - -  - - - - - ->     
1  | zhangsan | 16 

策略一：  
key : 对应一个json对象，所有属性；      
策略二：  
一个hash，对应一个对象；这样可以取单个属性；  
策略三：  
当做mq使用；  
  
###list
lpush 头部加 先进后出 栈  
rpush 尾部加 先进先出 队列  

lpop 头部取   
rpop 尾部取  

lrange key 0 -1  查看所有  

rpoplpush 尾部取删，头部添加  


###set
string类型的无序的集合。  
类似java中的list。  
提供取交集，并集操作。


###zset
加序列号，有序的。   
zadd zset1 1 val1  
zadd zset1 3 val3   
zrange zset1 0 -1 withscores  带索引  
  


