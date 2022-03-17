# JS变量提升、函数提升


# js 代码执行过程分为两个阶段
词法分析：词法分析主要包括：分析变量声明、分析函数声明、分析形参三个部分。
执行阶段
引擎通过词法分析将我们写的js代码转成可以执行的代码，接下来才是执行。

## var定义的变量提升
```javascript
    console.log(a);  //undefined
    var a = 123;
```
因为变量a的声明被提到了作用域顶端。上面代码编译后应该是下面这个样子
```javascript
    var a;
    console.log(a)
    a = 123
    //所以输出内容为 undeifend
```
看一道经典面试题
```javascript
	console.log(v1);
	var v1 = 100;
	function foo() {
		console.log(v1);
		var v1 = 200;
		console.log(v1);
	}
	foo();
	console.log(v1);
	// undefined  undefined 200  100
```
他的分析过程如下：
 ```javascript
	//他的词法分析结果如下
	 var foo = ()=>console.log(1)
	 foo()
	 var foo = null
	 foo = ()=> console.log(2)
	 foo()
	// 所以执行结果如下。
 ```

## 函数提升
具名函数的声明有两种方式：1. 函数声明式 2. 函数字面量式
```javascript
	//函数声明式
	function bar () {}
	//变量形式声明； 
	var foo = function () {}
```
函数 变量形式声明 和普通变量一样 提升的 只是一个没有值的变量。

函数声明式的提升现象和变量提升略有不同，函数声明式会提升到作用域最前边，并且将声明内容一起提升到最上边。

```javascript
	bar()
	var bar = function() {
	  console.log(1);
	}
	// 报错：TypeError: bar is not a function
```

```javascript
	bar()
	function bar() {
	  console.log(1);
	}
	//输出结果1
```

```javascript
   function a(){}
   var a
   console.log(typeof a) //function
```

```javascript
	function a(){}
	var a = 'wxz'
	console.log(typeof a)//string
```

## 知识点总结
所有的声明都会提升到作用域的最顶上去。
同一个变量只会声明一次，其他的会被忽略掉或者覆盖掉。
**变量声明的优先级高于函数声明的优先级。**
函数声明和函数定义的部分一起被提升。

