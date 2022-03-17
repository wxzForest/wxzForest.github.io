# Python嵌套函数、闭包、装饰器


##### 嵌套函数的调用示例

```
def func1(a1):
    def func2(a2):
        def func3(a3):
            print(f'{a1},{a2},{a3}')
        return func3
    return func2

func1(1)(2)(3)
# 1,2,3
```

##### 闭包示例

```
def func1():
    def func2(a2):
        def func3(a3):
            print(f'{a2},{a3}')
        print("func3")
        print(func3.__closure__)
        return func3
    print("func2")
    print(func2.__closure__)
    return func2
print(func1.__closure__)

func1()(2)(3)
# None
# func2
# None
# func3
# (<cell at 0x000001FCA4B03AC8: int object at 0x00007FFD0F3A6690>,)
# 2,3
```

`函数名.__closure__` 在函数是闭包函数时，返回一个cell元素；不是闭包时，返回None。上例中func1、func2都不是闭包，func3是闭包，区别在于func3中引入了func2中的变量，**可见闭包的特点就是内部函数引用了外部函数中的变量。**

另可知，闭包及嵌套函数执行顺序为，内部函数只会被定义，只有当内部函数被调用时才执行其内部代码。

##### 装饰器示例

```
def add_tag(func):
    def add_func(*args,**kwargs):
        return f'<sb> {func(*args,**kwargs)}'
    return add_func

def print_text(name):
    return f'{name}'

@add_tag
def print_text2(name):
    return f'{name}'

print(add_tag(print_text)('wxz'))
print(print_text2('wxz'))
# <sb> wxz
# <sb> wxz

print(print_text3.__name__) 
# add_func
```

add_tag(print_text)('wxz')和print_text2('wxz')是等价的，装饰器实际就是嵌套函数（吗？其实还是有些不一样，见下面）。

另外可以看到执行func.__name__返回的是装饰器内的add_func方法名，**原因是 add_func 覆写了 print_text3 函数的 __name__、__doc__、__modual__ 三个属性。改回来的话可以使用Python 中的 `functools.wraps` 装饰器**，如下面的代码。

##### 带参数的装饰器示例

```
def add_anytag(tagname):
    def add_tag(func):

        @wraps(func)
        def add_func(*args, **kwargs):
            return f'<{tagname}> {func(*args, **kwargs)} </{tagname}>'
        return add_func
    return add_tag

def print_text33(name):
    return f'{name}'

@add_anytag(tagname='div')
def print_text3(name):
    return f'{name}'

print(add_anytag('div')(print_text33)('baba'))
print(print_text3('wxz'))
# <div> baba </div>
# <div> wxz </div>

print(print_text3.__name__) 
# print_text3
```

add_anytag('div')(print_text33)('baba')和print_text3('wxz')效果也是等价的。

##### [如果想直接访问原始的无装饰器的函数怎么办？](https://python3-cookbook.readthedocs.io/zh_CN/latest/c09/p03_unwrapping_decorator.html)

##### 装饰器到底是什么

```
def real_decorator(func):
    print('【装饰器】：当装饰器被定义时我就会被加载。')
    def inner_decorator(*args,  **kwargs):
        print('【装饰器】：func执行前调用')
        func()
        print('【装饰器】：func执行后调用')
    return inner_decorator

print('func定义前我会打印出来')

@real_decorator
def test_decorator():
    print('【func】：我是func')

print('func定义后我会打印出来')

test_decorator()

print('func执行后我会打印出来')

# func定义前我会打印出来
# 【装饰器】：当装饰器被定义时我就会被加载。
# func定义后我会打印出来
# 【装饰器】：func执行前调用
# 【func】：我是func
# 【装饰器】：func执行后调用
# func执行后我会打印出来
```

**由上面可见，装饰器real_decorator在被定义到test_decorator头上时就被执行了。它的本质相当于执行下面这行代码**

```
# 定义一个变量do_real_decorator
do_real_decorator = real_decorator(test_decorator)
```

所以real_decorator里面的print在test_decorator执行前就执行了。下面看下多个装饰器的情况。

##### 多个装饰器

```
def set_func1(func):
    print('外壁巴布1')
    def wrapper1(*args,**kwargs):
        print('装饰内容开始1')
        func(*args, **kwargs)
        print('装饰内容结束1')
    print('外壁巴布11')
    return wrapper1


# 定义第二个装饰器
def set_func2(func):
    print('外壁巴布2')
    def wrapper2(*args,**kwargs):
        print('装饰内容开始2')
        func(*args, **kwargs)
        print('装饰内容结束2')
    print('外壁巴布22')
    return wrapper2

# 定义第二个装饰器
def set_func3(func):
    print('外壁巴布3')
    def wrapper3(*args,**kwargs):
        print('装饰内容开始3')
        func(*args, **kwargs)
        print('装饰内容结束3')
    print('外壁巴布33')
    return wrapper3

@set_func1
@set_func2
@set_func3
def show():
    print('Show Run....')

def show2():
    print('show run2')

print('》》》》》》》》》》》》》》》》》我是分割线，免得你看懵逼了')

show()

# 外壁巴布3
# 外壁巴布33
# 外壁巴布2
# 外壁巴布22
# 外壁巴布1
# 外壁巴布11
# 》》》》》》》》》》》》》》》》》我是分割线，免得你看懵逼了
# 装饰内容开始1
# 装饰内容开始2
# 装饰内容开始3
# Show Run....
# 装饰内容结束3
# 装饰内容结束2
# 装饰内容结束1
```

可见，**多个装饰器被定义的顺序是从下到上，转换成嵌套函数则是，最下层是最内层**。

set_func1(set_func2(set_func3(show2)))() 和 show() 是等价的。

```
def show2():
    print('show run2')

set_func1(set_func2(set_func3(show2)))()

# 外壁巴布3
# 外壁巴布33
# 外壁巴布2
# 外壁巴布22
# 外壁巴布1
# 外壁巴布11
# 装饰内容开始1
# 装饰内容开始2
# 装饰内容开始3
# show run2
# 装饰内容结束3
# 装饰内容结束2
# 装饰内容结束1
```
