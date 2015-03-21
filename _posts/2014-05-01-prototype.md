---
layout: default
title: "JavaScript中的原型与原型链"
author: itsmikej
---

JavaScript没有类的概念，但几乎所有的东西又是基于对象的，同时也能实现继承，这就是js跟其他OOP语言最大的不同之处，这也是js最难理解的一块。下面我来说说我个人的理解。

首先从创建对象说起，一般会有下面几种方法:

1.创建一个Object实例，然后给它添加属性和方法。
{% highlight javascript %}
var person() = new Object();
person.name = 'mikej';
person.sayName = function(){
  alert(this.name);
}
{% endhighlight %}
2.也可以这样写：
{% highlight javascript %}
var parson = {
  name : 'mikej',
  sayName : function(){
    alert(this.name);
  }
}
{% endhighlight %}
3.这两种创建对象的方法很简单，但都有缺陷，使用同一个模式创建对象的时候，会产生大量重复代码。于是就有了工厂模式：
{% highlight javascript %}
function createPerson(name){
  var p = new Object();
  p.name=name;
  p.sayName = function(){
    alert(this.name);
  };
  return p;
}
var p1 = createPerson('mikej');
var p2 = createPerson('tom');
{% endhighlight %}
这样就可以无限创建对象了。

4.还有一种方法，跟工厂模式异曲同工，叫做构造函数模式：
{% highlight javascript %}
function Person(name){
  this.name=name
  this.sayName = function(){
   alert(this.name);
  }
  this.say = function(){
    alert('hi');
  }
}
var p1 = new Person('mikej');
var p2 = new Person('tom');
{% endhighlight %}

这里有几个值得关注的地方：没有显示的创建对象、函数名Person使用的是大写字母P（这是必须的）、p1和p2中都有一个constructor（构造函数）属性，指向Person。同时p1和p2既是Object的实例，也是Person的实例。
{% highlight javascript %}
alert(p1.constructor == Person); //true
alert(p1 instanceof Object); //true
alert(p1 instanceof Person); //true
{% endhighlight %}

//5.11更新：以一个phper的角度看的话，之前很容易将创建对象的流程想成这样，Person就是一个“类”，然后用`new Person('mikej')`实例化了这个类，并且传入参数。但实际上并不是这样的，创建的流程应该是这样：首先，创建一个空对象，然后用apply方法，第一个参数是这个空对象，第二个参数是上下文的参数，这样Person中的this就会指向这个对象，也就是p1。

{% highlight javascript %}
var p1 = new Person('mikej');
//上面代码就相当于
var p1 = {};
Person.apply(p1, ['mikej']);
{% endhighlight %}

构造函数模式看上去很好，但是它有一个弊端就是浪费内存，接上例
{% highlight javascript %}
alert(p1.say == p2.say) //false
{% endhighlight %}
5.为了避免这个缺陷，可是使用**原型模式**来创建对象，js中的每个对象都有一个prototype属性用来指向另外一个对象，这个对象的所有属性和方法都会被构造函数的实例继承，是共享的，这就意味着，我们可以把那些不变的属性和方法，定义到prototype对象上。
{% highlight javascript %}
function Person(name){
  this.name = name;
}
//Person的原型对象
Person.prototype = {
  say: function(){
    alert('hi');
  },
  sayName: function(){
    alert(this.name);
  }
};
var p1 = new Person("mikej");
var p2 = new Person("tom");
p1.sayName();
p2.sayName();
//下面就可以看出方法实现了共享
alert(P1.say == P2.say) //true
alert(P1.sayName == P2.sayName) //true
{% endhighlight %}
再来扩展一下上面的例子，使用原型来实现继承。
{% highlight javascript %}
function Person(name){
  this.name = name;
}

Person.prototype = {
  say: function(){
    alert('hi');
  },
  sayName: function(){
    alert(this.name);
  }
};

function Programmer(){
  this.say = function(){
    alert('im Programmer, my name is ' + this.name);
  }
}

Programmer.prototype = new Person('mikej');
//手动修正构造函数
Programmer.prototype.constructor = Programmer;
var p1 = new Programmer();

console.dir(Programmer.prototype.constructor);//Programmer
console.dir(p1.constructor);//Programmer
console.dir(p1);
{% endhighlight %}
Programmer的原型指向了Person的一个实例，那么所有的Programmer的实例都能继承Person和Person的原型了。

这里会有一个问题。

默认原型对象里有一个constructor属性，指向它的构造函数。而每一个实例也有一个constructor属性，会默认调用prototype对象的constructor属性。

假设没有`Programmer.prototype = new Person('mikej');`

Programmer.prototype.constructor是指向Programmer的。p1的构造也指向Programmer
{% highlight javascript %}
alert(Programmer.prototype.constructor == Programmer) //true
alert(p1.constructor == Programmer) //true
{% endhighlight %}
但有了这句`Programmer.prototype = new Person('mikej');`之后，

Programmer.prototype.constructor就指向了Object，也就是Person.prototype指向的对象的构造。p1.constructor也指向了Object。但p1明明是构造函数Programmer生成的，这就造成了继承的混乱，所以我们必须手动修正构造函数，也就是下面这代码。
{% highlight javascript %}
Programmer.prototype.constructor = Programmer;
{% endhighlight %}

好了，现在我们再来看看原型链：
{% highlight javascript %}
console.dir(p1);
{% endhighlight %}
这句代码的结果是

![prototype](/image/prototype.png)

可以看出，p1是Programmer的实例，Programmer的原型是Person，Person的原型是Object，再网上就是JS的对象了。这就是原型链，也就是说，JavaScript的继承是基于原型链来实现的。
