# 装饰器
装饰器是用来给函数或类添加辅助功能的。它在很多场合非常有用，比如在执行函数前进行权限审核，在函数执行后进行现场清理或异常处理等。
## 装饰器的本质
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
## 装饰器语法
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
## 装饰器什么时候执行？
当程序执行到@decorated_by时，装饰器就执行了。
## 装饰器执行顺序
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
