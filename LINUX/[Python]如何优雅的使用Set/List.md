# Python 如何优雅的使用Set和List
**前言**: 最近使用python(简称 py)搞了一个小脚本,把git仓库中的所有commit记录统计下来放入到一个excle表中,统计自己的提交时间;其中遇到了一些问题,找了很久,终于在一个偏僻的网站找到答案,现在给大家总结一下,方便大家,让它更容易被发现;  
-------  

### ***先说结论:***


### python中想要自定类能被set容纳  
> 重载实现 __hash__方法和__eq__方法  
> 函数原型:
> ```python
> def __hash__(self):
>   return [some hash value]
> def __eq__(self,other):    #equality  
>   if isinstance(other, [classname]): #judge type is legal
>       return [True or False]  # do your judge there are equality ?
>   else:
>       return False
> ```

### python总想要调用sort方法
> 重载实现 __cmp__方法  
> 函数原型  
> ```python
> def __cmp__(self ,other):
>   if isinstance(other, [classname]):
>       # do your judge and return -1 or 0 or 1
>   else:
>       raise Exception("type error")
>       return 0
> ```
>


## 自定义类能够被插入到Set中  
数据结构中有很多使用的很频繁的数据结构,其中 `Set` 和 `List` 是及其常用的两个结构,
py中定义一个 set 语句如下
```python
numset = set()
```  
set 就是集合的意思,普通set是不允许有重复的元素出现,set的这一特点用来去除重复;例如我想统计今天我博客上到底有多少人来看过这一篇博客
```python
class visitor(object):
    def __init__(self,nam = "",idnum = 0,emal="dog@qq.com"):
        self.name = nam
        self.id = idnum
        self.email = emal
```  
然后我们只需要将对应的每个访客的ID/昵称/邮箱填好,然后在网站上一拉~出现了一个 `visitor` 数组  
**但是**,有些人对我特别喜欢,一天来看我七八回~比如我自己,那这个时候数组里就会有七八个我的访问记录,可我想要的记录是今天来过的人,这个时候就可以使用set来做