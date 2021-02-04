

# 缓冲

> **系统自动的在内存中为每一个正在使用的文件开辟一个缓冲区，从内存向磁盘输出数据必须先送到内存缓冲区，再由缓冲区送到磁盘中去。从磁盘中读数据，则一次从磁盘文件将一批数据读入到内存缓冲区中，然后再从缓冲区将数据送到程序的数据区。**

# 刷新缓冲区条件

**1.缓冲区被写满**

**2.程序执行结束或者文件对象被关闭。**

**3.行缓冲遇到换行**

**4.程序中调用flush()函数**

实例：

```python
import sys
from time import sleep

def printStar(n):
    for i in range(n):
        print('*',end=' ')
        sys.stdout.flush()
        sleep(0.5)

if __name__ == '__main__':
    printStar(5)
```

> **在如上实例中，如果将sys.stdout.flush()注释掉，
> 则5颗星会一起打印，否则会一个一个打印**

另一个例子：

```python
f = open('C:/Users/Administrator/Desktop/1.txt','w')

while True:
    data = input('>>')
    if not data:
        break
    f.write(data)

f.close()
```

当终端打印>>时输入Hello world回车，此时打开1.txt会发现里面并没有内容，说明此时Hello world还在缓冲区中，再输入一个回车，程序执行结束，此时可以看到1.txt里出现了Hello world。

更改代码如下：

```python
f = open('C:/Users/Administrator/Desktop/1.txt','w',1) # 行缓冲，换行刷新文件缓冲区

while True:
    data = input('>>')
    if not data:
        break
    f.write(data + '\n')

f.close()
```

此时每输入一个字符串都会保存进文件里

另一种方法：

```python
f = open('C:/Users/Administrator/Desktop/1.txt','w')

while True:
    data = input('>>')
    if not data:
        break
    f.write(data + '\n')
    f.flush() # 人为刷新文件缓冲区

f.close() # 关闭文件会刷新缓冲
```

此时效果和上面一样
