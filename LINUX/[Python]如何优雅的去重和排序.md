# Python 如何优雅的排序与去重
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

### python 中想要调用sort方法
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
  
</br>  

## 简单示例
```python
import random


class stu(object):
    def __init__(self, stid=0, stname=""):
        self.id = stid
        self.name = stname

    def __eq__(self, other):
        if isinstance(other, stu):
            # do your judge and return -1 or 0 or 1
            return self.id == other.id
        else:
            return False

    def __hash__(self):
        return hash(self.id)

    def __cmp__(self, other):
        if isinstance(other, stu):
            # do your judge and return -1 or 0 or 1
            return self.id.__cmp__(other.id)
        else:
            raise Exception("type error")
            return 0

    def __repr__(self):
        return self.name + "\'s ID was:" + str(self.id)+"\n"


stuSet = set()
stuList = []

for i in range(0, 10):  # if you want use random plase import random first
    temStu = stu(random.randint(1, 6), "bob"+str(random.randint(1, 4)))
    stuSet.add(temStu)
    stuList.append(temStu)

print("the set was")
print(stuSet)
print("\n\nbefore sort list was:")
print (stuList)
stuList.sort()
print("after sort list was:")
print(stuList)

```
set的作用是去重,我将10个stu的id设置为1~6的随机数,那么肯定有重复的,因此set里的元素个数 一定 `<=6` ,list里面就一定是10个
下面是运行结果:  
```bash
set([bob3's ID was:1
, bob2's ID was:4
, bob4's ID was:5
, bob1's ID was:6
])


before sort list was:
[bob2's ID was:4
, bob3's ID was:1
, bob4's ID was:1
, bob1's ID was:6
, bob2's ID was:6
, bob3's ID was:4
, bob2's ID was:6
, bob2's ID was:6
, bob4's ID was:5
, bob3's ID was:1
]
after sort list was:
[bob3's ID was:1
, bob4's ID was:1
, bob3's ID was:1
, bob2's ID was:4
, bob3's ID was:4
, bob4's ID was:5
, bob1's ID was:6
, bob2's ID was:6
, bob2's ID was:6
, bob2's ID was:6
]
```

