---
layout: post
title:  "JavaScript中的this"
---

在类似PHP的传统OO语言中，this关键字的含义很明确，就是指当前对象，但javascript中的this却不是这样，它有很强的灵活性。它可以是全局对象，当前对象，也可以是任意对象。

1.在全局作用域下（指向全局）：
{% highlight javascript %}
console.log(this); //Window
{% endhighlight %}
2.在函数内调用（指向全局）：
{% highlight javascript %}
function foo(){
	console.log(this);//window 调用一个函数时
}
foo();  //Window
{% endhighlight %}
3.调用一个对象的一个方法（指向当前对象）：
{% highlight javascript %}
var test = {
	MyName:"mikej",
	foo: function(){
		console.log(this.MyName);
	}
}
test.foo();  //mikej
{% endhighlight %}
4.实例化一个对象（指向新创建的对象）：
{% highlight javascript %}
function Person(name){
	this.sayName = function(){
		console.log(this);
	}
}
var p1 = new Person("mikej");
p1.sayName();  //mikej
{% endhighlight %}
实际上js（通过new）创建一个新的对象的过程应该是这样：
{% highlight javascript %}
p1 = {};
Person.apply(p1, ["mikej"]);
{% endhighlight %}
apply方法的作用是改变函数执行的上下文，这里将函数执行的上下文改为了p1，所以，this当然指向p1了。

5.不用说，当然就是通过`apply`或者`call`显示的设置`this`了，call和apply通常用来修改函数的上下文，函数中的this指针将被替换为call或者apply的第一个参数，事实上上述的几种方式都是以apply或者call做为基础演变而来的。

有两个值得注意的问题：

a.直接调用函数时，this指向全局对象
{% highlight javascript %}
Foo.method = function() {
	function test() {
		//这里的this会指向全局
	}
}
{% endhighlight %}
显然，test里的this要指向Foo对象才对，解决方案如下：
{% highlight javascript %}
Foo.method = function() {
	var that = this;
	function test() {
		//这里使用that来指向Foo对象
	}
}
{% endhighlight %}
b.将一个方法赋值给一个变量
{% highlight javascript %}
var test = obj.method;
test();
{% endhighlight %}
test中的this不会指向obj，而是全局，因为test会被当成一个普通的函数执行。