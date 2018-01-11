# 装饰器
装饰器是用来给函数或类添加辅助功能的。它在很多场合非常有用，比如在执行函数前进行权限审核，在函数执行后进行现场清理或异常处理等。
## 装饰器基础
### 装饰器的本质
装饰器本质上是一个函数，它接收一个函数，对这个函数做出处理之后，又返回一个函数。
举个例子说明一下：假如我们需要给一个函数增加一段描述，从零开始设计一个装饰器应该是这样的。
```python
def add(x, y):
    '''Return the sum of x and y.'''
    return x + y

def decorated_by(func):
    func.__doc__ += '\nDecorated by decorated_by.'
    return func
    
add = decorated_by(add)
```
### 装饰器语法
前面这个例子虽然可以执行，但是看起来不太简洁，因此python专门设计了一个装饰器语法，语法比较简单，就是把@decorated_by放在需要修改__doc__的函数前面即可。
```python
def decorated_by(func):
    func.__doc__ += '\nDecorated by decorated_by.'
    return func
 
@decorated_by    
def add(x, y):
    '''Return the sum of x and y.'''
    return x + y

@decorated_by    
def mul(x, y):
    '''Return the multiply of x and y.'''
    return x * y
```
### 装饰器什么时候执行？
当程序执行到@decorated_by时，装饰器就执行了。
### 装饰器执行顺序
一个函数可以同时使用多个装饰器，那么它们的顺序时从下往上执行的。比如，下面这个例子的__doc__为：'Return the sum of x and y.\nDecorated by decorated_by.\nDecorated by also_decorated_by.'。相当于执行了add=also_decorated_by(decorated_by(add))
```python
def also_decorated_by(func):
    func.__doc__ += '\nDecorated by also_decorated_by.'
    return func

def decorated_by(func):
    func.__doc__ += '\nDecorated by decorated_by.'
    return func

@also_decorated_by 
@decorated_by    
def add(x, y):
    '''Return the sum of x and y.'''
    return x + y
```
## 装饰器应用场合
### 什么时候使用装饰器
在python标准库或第三方库中，有很多使用装饰器的场景。比如  
@classmethod & @staticmethod 类方法和静态方法  
@mock.patch & @mock.patch.object 单元测试  
@login_required & @permission_required Django的权限管理  
@app.route Flask的URL管理
@task Celery任务管理
### 为什么你需要写装饰器
装饰器主要用于把一些通用场景统一抽象出来，可以减少代码重复。
### 什么时候你应该写装饰器
1. __增加功能__ 。如果你想给某些函数增加一些功能，比如在执行前进行权限检查，或者讲执行结果写入日志等，这个时候最好用装饰器。
2. __参数检查__ 。如果一些函数要求输入固定类型的参数时，可以使用装饰器进行参数检查，或者将函数输出改为确定类型。
3. __增加参数__ 。比如@mock.pathc单元测试时，需要把一些测试用例作为附加参数传给被测试函数。
4. __函数注册__ 。把函数注册到其它地方是非常有用的，比如把一个task注册到task runner中。
## 写装饰器
装饰器是一个函数，通常接收一个函数，做点处理工作，然后返回一个函数。值得注意的是，程序只有碰到@，装饰器就执行了，而不是等被装饰函数执行时才执行。
