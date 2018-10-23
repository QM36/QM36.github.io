---
title: JS语言精粹读书笔记
---

## 第二章 语法
#### 1. 语句
1. `var`语句：被用在函数内部时，定义这个函数的私有变量
2. 条件语句`if`,`switch`
3. 循环语句`while`,`do`,`for`
4. 强制跳转语句`return`,`break`,`throw`
5. 在`for in`语句中，需要先通过`object.hasOwnProperty(varibale)`来确定这个属性名就是该对象的成员

#### 2. 表达式
注意三元运算符`a?b:c`，如果第一个运算数为真，则执行第二个运算数的值，否则，产生第三个运算数的值

#### 3. 字面量
字面量分为数字、字符串、对象、数组、函数、正则表达式

#### 4. 函数
函数定义了一个函数值，有一个可选的名字。函数主体包括变量定义和语句

## 第三章 对象
#### 1. 对象字面量
对象字面量提供了一种创建新对象的表示方法。一个对象字面量就是包围在花括号中的零个或多个“名/值”对。对象的属性名可以是包括空字符串在内的任何字符串。对象是可以嵌套的。

#### 2. 检索
检索就是查看对象的属性，有两种方式：一种是对象后缀`[]`，括号中为一个字符串的表达式；还有一种更常用的是`.`。如果检索的值并不存在，则会返回`undefined`。`||`运算符可以填充默认值
```javascript
 var middle = stooge["middle-name"] || "(none)";
 var status = flight.status || "unknown";
```
#### 3. 更新
对象可以扩展属性，属性的值也可以通过赋值语句重新赋予

#### 4. ==引用==
对象通过引用传递，他们永远不会拷贝

#### 5. ==原型==
每个对象都连接到一个原型对象，并且它可以从中继承属性。所有通过对象字面量创建的对象都连接到`Object.prototype`。给`Object`增加一个方法，这个方法创建一个使用原型对象作为其原型的新对象
```javascript
if (typeof Object.create !== "function") {
    Object.beget = function (o) {
        var F = function () {};
        F.prototype = o;
        return new F();
    };
}
var another_stooge = Object.create (stooge);
```

原型连接在更新时是不起作用的，对某个对象做出改变时，不会触及到该对象的原型.原型连接只有在检索值的时候有才会被用到。当我们获取某个对象的属性值时，如果改对象没有此属性名，那么JS会试着从原型对象中获取属性值，以此类推，这个过程叫委托。原型是一种动态的关系，当我们添加一个新的属性在原型中，该属性会立即对所有基于该原型创建的对象可见。

#### 6. 反射
反射就是检查对象拥有的属性，有两种情况，返回属性值或者`function`,有两种方法：`typeof`和`hasOwnProperty`,前者返回属性类型,后者返回`true`或`false`。值得注意的是`hasOwnProperty`方法不检查原型链。
```javascript
typeof flight.number   // 'number'
flight.hasOwnProperty('number')    // true
```

#### 7. 枚举
`for in`语句可以遍历一个对象中的所有属性名，但是这个枚举过程出现所有属性，包括函数和不关心的原型中的属性，而且属性名出现的顺序是不确定的，最好的办法就是把需要的属性放在数组中，用`for`语句实现循环。

#### 8. 删除
`delete`运算符可以用来删除对象中的属性，但是不会触及原型链中的任何对象。删除对象的属性可能会让来自原型链中的属性浮现出来。

#### 9. 减少全局变量污染
全局变量削弱了程序的灵活性，所以应该避免。如果把多个全局变量都整理在一个名称空间中，会显著降低与其他应用程序、组件或类库之间产生糟糕的相互影响的可能性。最小化使用全局变量的一个方法就是在应用中只创建唯一一个全局变量，在下一章中会介绍闭包来经行信息隐藏。

## 第四章 函数
#### 1. 函数对象
在JS中，函数就是对象，可以存放在变量、对象和数组中，函数可以当作参数传递给其他函数，函数也可以再返回函数，函数也可以拥有方法。函数的与众不同在于它可以被调用。每个函数在创建时附有两个附加的隐藏属性：函数上下文和实现函数行为的代码。

#### 2. 函数字面量
函数对象可以通过函数字面量创建，函数字面量包括四部分：保留字`function`，函数名（但是函数名可以省略，既为匿名函数），包围在圆括号中的参数和包围在花括号中的语句。函数字面量可以出现在任何地方，也可以出现在其他函数中，一个函数内部可以访问自己的参数和变量，也可以访问它被嵌套在其中的那个函数的参数和变量。通过函数字面量创建的函数对象包含一个连到外部上下文的连接，这称为==闭包==。

#### 3. 调用
调用一个函数就是暂停当前函数的执行，然后传递控制权和参数给新函数。除了声明时定义的形式参数，每个函数接收两个附加参数`this`和`arguments`。在JS中一共有四种调用模式：方法调用模式、函数调用模式、构造器调用模式和`apply`调用模式，这些模式在如何初始化关键参数`this`上存在差异。

调用运算符就是一对圆括号，括号内部是零个或多个用逗号隔开的表达式，每个表达式产生一个参数值，每个参数值被赋予函数声明时定义的形式参数名。当实际参数`arguments`和形式参数`parameters`个数不匹配时，不会报错。多出来的实际参数会被忽略，不足的会被当作`undefined`,任何类型的值都可以被传递给参数。

**一、方法调用模式**

当一个函数被保存为对象的属性时，我们称它为方法，当一个方法被调用时，`this`被绑定到该对象。如果一个调用表达式包含一个属性存取表达式（点表达式或下标表达式），那么它被当作一个方法调用。`this`,它能从对象中取值或修改，`this`到对象的绑定发生在调用 的时候，这个超级迟绑定使得函数对`this`高度复用。通过``this`可取得他们所属对象的上下文的方法称为公共方法。

**二、函数调用模式**

当一个函数并非一个对象的属性时，它被当作一个函数来调用。当函数以此模式调用时，`this`被绑定到全局对象，因为内部函数的`this`被绑定了错误的值，所以不能共享该方法对对象的访问权，这里有一个就解决方案：如果该方法定义一个变量并给它赋值为`this`，那么内部函数就可以通过新定义的变量访问到`this`，安装约定，新定义的变量命名为`that`
```javascript
object.double = function () {
    var that = this;
    var helper = function () {
        that.value = add(that.value,that.value);
    };
    helper();
};
object.double();
document.writeln(object.getValue());       //6
```

**三、构造器调用模式**

就是在需要调用得函数前面加`new`，这样称为构造器函数（有点类似与C++中得构造函数），这种调用用在创建对象得时候,注意此函数需要保存在大写格式命名的变量里面
```javascript
var Quo = function (string) { //创建一个名为Quo的构造器，它构造一共带有status属性的对象
    this.status = string;
};
Quo.prototype.get_stauts = function () {  //给Quo实例化出所有的对象增加一个get_status的方法
    return this.status;
};                                                                                  
var myQuo = new Quo('confused');  //构造实例创建对象
document.writeln(myQuo.get_status()); //confused
```
**四、Apply调用模式**

`apply`方法构建一个参数数组，并用其去调用函数,`apply`方法接收两个参数，一个是`this`,另一个是参数数组
```javascript
var array = [3 , 4];
var sum = add.apply(null,array) //在这之前需要写好add函数,在这里参数为数组

var statusObject = {
    status : 'A-OK';
};
var status = Quo.prototype.get_status.apply(statusObject); //在这里参数为一个对象，status值为A-OK
```

#### 4. 参数
当函数被调用时，会得到一个参数为`argument`数组，通过这个参数，函数可以访问函数被调用时所有传给它的参数列表，包括没有分配给函数声明时候定义的形式参数的多余参数，着就使得一个函数的参数个数可以不用指定。注意`argument`不是真正的数组，除了`length`的属性以外，没有其他任何数组的方法。

```javascript
var sum function () {
    var i,sum = 0;
    for (i = 0;i < argument.lenght;i++) {
        sum += arguments[i];
    }
    return sum;
};
document.writeln(sum(4,8,15,16,23,42));
```

#### 5. 返回
一个函数总会返回一个值，如果在函数前面加上`new`，则返回`this`这个新对象

#### 6. 异常
当查出异常时候，按照如下方式抛出异常：
```javascript
var add = function (a,b) {
    if(typeof a !== 'number' || typeof b !== 'number') {
        throw {
            name :'TypeError',
            message : 'add needs numbers'
        };
    }
    return a + b;
}
```
以上示例中throw执行后会抛出一个`exception`对象，这个对象的两个属性：`name`,`message`

还有一种异常处理方式就是`try-catch`示例如下：
```javascript
var try_it = function () {
    try {
        add ('asd');
    } catch (e) {
        document.writeln(e.name + ':' + e.message);
    }
}
try_it();
```
#### 7. 给类型增加方法
通过`Function.prototype`增加方法，使得这个方法对所有函数可用，举例如下：
```javascript
Function.prototype.method = function (name,func) {
    this.prototype[name] = func;
    return this;
};
```
例如给数字添加一个`integer`方法，来判断正负，使用`Math.ceiling`还是`Math.floor`给数字取整
```javascript
Number.method('integer',function() {
    return Math[this < 0 ? 'ceiling' : 'floor'](this); //this作为参数传给该方法
});
document.writeln((-10 / 3).integer());
```
通过以上方法可以给基本类增加方法，因为原型继承的动态性，新的方法被立刻赋予到所有的值（实例化对象）上，即便这个对象是在方法被创建前就创建好了。

基本类的原型是公共结构，所以在类库混用时候需要小心，一个保险的做法就是只在确定没有该方法时候再增加，用如下条件判断：
```javascript
Function.prototype.method = function (name,func) {
    if(!this.prototype[name]) {
        this.prototype[name] = func;
    }
};
```
#### 8. 递归
汉诺塔问题，但是深度递归的函数可能会因为返回栈溢出而运行失败，值得注意的是js中没有尾递归优化。

#### 9. 作用域
作用域控制变量与参数的可见性与生命周期，其减少名称冲突，并且提供了自动内存管理。因为js不支持块级作用域，所以应该在函数体的顶部声明所有函数中用到的变量。在JS中作用域只分两种，函数作用域和全局作用域，函数作用域中的变量在外部不可见，但是由花括号`{}`包围的语句作用域中的用`var`声明的变全局是可见的
```javascript
var foo = function () {
    var a = 3, b = 5;
    var bar = function () {
        var b = 7, c = 11;
        a += b + c;
    };
    bar();
};
```

#### 10. ==闭包==
在JS中内部函数可以访问定义在他们外部函数的参数和变量（除`this`和`arguments`）。而且内部函数拥有比它外部函数更长的生命周期。示例如下：
```javascript
var myObject = function () {
    var value = 0;
    return {
        increment : function (inc) {
            value += typeof inc === 'number' ? inc : 1;
        },
        getvaule: function () {
            return value;
        }
    }
}();
```
这个构造函数的作用不是赋值给`myObject`,而是把调用该函数后的返回值赋值给它，注意该函数返回一个包含两个方法的对象，并且这些方法继续享有访问`value`的特权。

所谓闭包就是该函数可以访问它被创建时所处的上下文环境。

其他示例：
```javascript
var fade = function (node) {
    var level = 1;
    var step = function () {
        var hex = level.toString(16);
        node.style.backgroundColor = '#FFFF' + hex + hex;
        if(level < 15) {
            level += 1;
            setTimeout(step, 100);//第一个参数是需要执行的程序（只执行一次），第二个参数是在执行程序前等待的时间
        }
    };
    setTimeout(step, 100);//在这里执行时候step已经返回，但是level值已经保留
};
fade(document.body);
```
以上例子中调用`fade`把`document.body`作为参数传递给它，`fade`设置的`level`为`1`，它有一个`step`函数，这个函数调用`setTimeout`，其参数为`step`函数和一个时间（100毫秒）。整个过程是十分之一秒之后`step`函数被调用，它修改`node`的背景颜色，然后查看`level`变量，如果背景色尚未变成白色，那么它增大`level`并且用`setTimeout`函数预定时间让自己再次运行。`step`函数再次执行，但这次`level`值变为`2`,`fade`在之前已经返回了，但只要`fade`内部函数的需要，它的变量就会持续保留。即内部函数能访问外部函数的变量而无须复制。

#### 11. 回调
如果有一个序列，由用户交互开始，向服务器发送请求，最终显示服务器的响应，为了避免网络上的同步请求而使客户进入假死状态，更好的方式是发起异步请求，提供一个当服务器的响应到达时被调用的回调函数，异步的函数立即返回，这样客户端不会阻塞
```javascript
request = prepare_the_request ();
send_request_asynchronously(request, function (response) {
    display(response);
});
```    
以上传递了一个函数给`send_request_asynchronously`函数，它将在收到响应时被调用

#### 12. 模块
我们可以使用函数和闭包来构造模块，模块是提供接口却隐藏状态与实现的函数或对象。模块模式是：一个定义了私有变量和函数的函数；利用闭包创建可以访问私有变量和函数的特权函数；最后返回这个特权函数，或者把它们保存到一个可以访问的地方。模块模式也可以用来产生安全的对象，例如：
```javascript
var serial_maker = function () {
    var prefix = '';
    var seq = 0;
    return {
        set_prefix: function (p) {
            prefix = String(p);
        },
        set_seq: function (s) {
            seq = s;
        },
        gensym: function {
            var result = prefix + seq;
            seq += 1;
            return result;
        }
    };
};
var seqer = serial_maker();
seqer.set_prefix('Q');
seqer.set_seq(1000);
var unique = seqer.gensym();
```
以上函数返回用来产生唯一字符串的对象，这个唯一字符串由前缀加序列号组成，该对象包含一个设置前缀的方法，一个设置序列号的方法，和一个产生唯一字符串的方法。在以上示例中产生的`seqer`对象只能用对应的方法去改变`prefix`和`seq`的值，即这个对象的函数拥有修改私有成员的特权，如果我们把`seqer.gensym`作为一个值传给第三方函数，那么这个函数能用`seqer.gensym`来产生唯一序列但是不能改变`prefix`和`seq`的值。

#### 12. 级联
有一些方法没有返回值比如修改对象的某个状态，不用返回任何值的方法，如果让这些方法返回`this`就可以启用级联。在一个级联中，我们可以在单独一条语句中依次调用同一个对象的很多方法。
```javascript
getElement('myBoxDiv').
	move(350,150).
	width(100).
	height(100).
	color('red').
	border('10px outset').
	padding('4px').
	appendText('Please stand by').
	on('mousedown', function (m) {
		this.startDrag(m, this.getNinth(m));
	}).
	on('mousemove', 'darg').
	on('mouseup', 'stopDrag').
	later(2000, function () {
		this.
			color('Yellow').
			setHTML('What hath God wraught').
			slide(400, 40, 200, 200);
	}).
	tip('This box is resizeable');
```
以上例子中`getElement`函数产生一个对应于`id = 'myBoxDiv'`的`DOM``元素并提供了其他功能的对象。这个函数允许我们移动元素，修改尺寸和样式，并添加行为。这些方法每一个都返回该对象（此处对象是哪个对象），所以调用返回结果可以被下一次调用使用。

级联可以产生出具备很强表现力的接口，它也能控制那种试图构造一次做很多事情的接口的趋势。

#### 14. 套用（柯里化）
套用允许我们将函数与传递给它的参数相结合去产生一个新的函数
```javascript
var add1 = add.curry(1);
document.writeln(add1(6));
```
`add1`是传递给`add`函数的`curry`方法之后创建的一个函数。`add1`函数把传递给它的参数的值加1，但是JS中没有`curry`方法，因此需要给`Function.prototype`添加功能来实现：
```javascript
Function.method('curry',function () {
    var args = arguments, that = this;
    return function () {
        return that.apply(null,args.concat(arguments));
    };
});
```
`curry`使用`Array`的`concat`方法连接两个参数数组。但是`arguments`数组并非真正的数组，所以它没有`conact`方法，因此我们要在两个`arguments`数组上都应用数组的`slice`方法，这样产生拥有`conact`方法的常规数组。
```javascript
Function.method('curry', function () {
    var slice = Array.prototype.slice,
    args = slice.apply(arguments),
    that = this;
    return function () {
        return that.apply(null,args.conact(slice.apply(arguments)));
    };
});
```

#### 15.记忆
函数可以用对象去记住之前操作的结果，从而避免无畏的运算，这种优化被称为记忆。如果现在需要实现一个递归的函数去生成`Fibonacci`数列，数列最前面的两个数字为0和1，之后的数字为前两个数字之和
```javascript
var fibonacci = function (n) {
    return n < 2 ? n : fibonacci(n - 1) + fibonacci(n - 2);
};
for (var i = 0; i <= 10; i++) {
    document.writeln('//' + i + ':' + fibonacci(i));
}
```
以上方法虽然可以，但是执行次数有很多都是重复之前的，如果该函数具有记忆功能，就可以显著减少它的运算量。

首先在一个名为`memo`的数组里保存结果，存储结果可以隐藏到闭包中，当这个函数被调用时，首先查看数组检查是否已经知道结果，如果知道就立即返回这个结果。
```javascript
var fibonacci = function () {
    var memo = [0, 1];
    var fib = function (n) {
        var result = memo[n];
        if(typeof result !== 'number') {
            result = fib(n - 1) + fib(n - 2);
        }
        return result;
    };
    return fib;
}();
```
经过仔细分析可以发现以上函数只是记住了0和1处的值，更高次序的值并没有存到数组中，因此还是存在冗余计算次数，而且不具有一般化。

我们可以把这种形式一般化，编写一个函数帮助我们构造带记忆功能的函数。`memoizer`函数将取得一个初始的`memo`数组和`fundamental`函数，它返回一个管理`memo`存储和在需要时调用`fundamental`函数的`shell`函数，我们传递这个`shell`函数和该函数的参数给`fundamental`函数：
```javascript
var memoizer = function (memo, fundamental) {
    var shell = function (n) {
        var result = memeo[n];
        if(typeof result !== 'number') {
            result = fundamental(shell, n);
            memo[n] = result;
        }
        return result;
    };
    return shell;
};

var fibonacci = memoizer([0, 1], function (shell, n) {
    return shell(n - 1) + shell(n - 2);
});
```
通过以上设计能产生其他函数的函数，可以极大减少我们必须要做的工作。例如：要产生一个可记忆的阶乘函数，我们只需要提供基本的阶乘公式：
```javascript
var factorial = memoizer([1, 1], function (shell, n) {
    return n * shell(n - 1);
});
```

## 第五章 继承

#### 1. 伪类
`JavaScript`不让对象直接从其他对象继承，而是插入一个多余的中间层，从而使构造器函数产生对象。当一个函数对象被创建时，`Function`构造器产生的函数对象会运行以下代码：
```javascript
this.prototype = {constructor : this};
```
新函数对象赋予一个`prototype`属性，这个属性的值是本身就是对象，该对象包含属性`constructor`，值就是函数对象，每个函数都会有一个`prototype`对象。

采用构造器调用模式，就是使用`new`前缀去调用一个函数，这将修改函数的执行方式。 
#### 2. 对象说明符
有时候构造器要接受一大串的参数，而且要记住参数的顺序，在这种情况下，我们在编写构造器实使其接受一个简单的对象说明符会更加友好，包含了将要构建的对象规格说明。
```javascript
var myObject = maker ( {
    first : f,
    last : l,
    state: s,
    city : c
});
```
如果构造器取得一个对象说明符，我们就可以简单的传递`JOSN`对象给构造器，而它将返回一个构造完全的对象

#### 3. 原型
基于原型的继承比基于类的继承更简单：一个新对象可以继承旧对象的属性。首先构造一个有用的对象，接着就可以构造更多和那个对象相似的对象，这样可以避免把一个应用拆解成一系列嵌套抽象类的分类过程。

首先用对象字面量构造一个有用的对象：
```javascript
var myMammal =  {
    name : 'herb the mammal',
    get_name : function () {
        return this.name;
    },
    says : function () {
        return this.saying || '';
    }
};
var myCat = Object.create(myMammal);
myCat.name = 'henrietta';
myCat.saying = 'meow';
myCat.purr = function (n) {
    var i, s = '';
    for(i = 0; i < n; i += 1) {
        if(s) {
            s += '-';
        }
        s += 'r';
    }
    return s;
};
myCat.get_name = function () {
    return this.says() + ' ' + this.name + ' ' + this.name();
};
```
#### 4. 函数化
构造一个生成对象的函数包含四个步骤：
1. 创建一个对象四种方法，可以构造对象字面量，或者和`new`前缀连用去调用一个构造器函数，或者使用`Object.create`方法构造一个已经存在的对象的新实例，或者它可以调用任意一个会返回对象的函数
2. 有选择定义私有变量和方法。这些就是函数中通过`var`语句定义普通变量
3. 给这个对象扩充方法，这些方法拥有特权去访问参数，以及在以上语句中通过`var`语句定义的变量
4. 返回新对象


把这个模式应用到mammal例子中：
```javascript
var mammal = function (spec) {
	var that = {};
	that.get_name = function () {
		return spec.name;
	};
	that.says = function () {
		return spec.saying || '';
	};
	return t、hat;
};
var mymammal = mammal({name: 'Herb'});
var cat = function (spec) {
	spec.saying = 'meow';
	var that = mammal(spec);
	that.purr = function (n) {
		return 'r'.padEnd(2 * n - 1, '-r');
	};
	that.get_name = function () {
		return that.says() + ' ' + spec.name + ' ' + that.says();
	};
	return that;
};
var mycat = cat({name : 'henrietta'});
```
函数化模式还提供一个处理父类方法的方法，首先构造一个`superior`方法，它取得一个方法名并返回调用那个方法的函数。该函数会调用原来的方法。
```js
Object.method('superior',function (name) {
	var that = this,
	method = that[name];
	return function () {
		return method.apply(that, arguments);
	};
});

var coolcat = function(spec) {
	var that = cat(spec);
	super_get_name = that.superior('get_name');
	that.get_name = function (n) {
		return 'like' + super_get_name() + 'baby';
	};
	return that;
};
var mycoolcat = coolcat({name:'bix'});
var name = mycoolcat.get_name();
```

#### 5. 部件
我们可以从一套部件中把对象组装出来，比如，我们可以构造一个给任何对象添加简单事件处理特性的函数，这个函数会给对象增加一个`on`方法，一个`fire`方法和一个私有的事件注册表对象：
```js
var even = function (that) {
	var regist = {};
	that.fire = function (event) {
		var array, func, handler, i,
		type = typeof event === 'string' ? event :event.type;
		if(regist.hasOwnProperty(type)) {
			array = regist[type];
			for(i = 0; i < array.length; i += 1) {
				handler = array[i];
				func = handler.method;
				if (typeof func === 'string') {
					func = this[func];
				}
				func.apply(this, handler.parameters || [event]);
			}
		} 
		return this;
	};
	that.on = function (type, method, parameters) {
		var handler = {
			method : method,
			parameters: parameters
		};
		if(regist.hasOwnProperty(type)) {
			regist[type].push(handler);
		} else {
			regist[type] = [handler];
		}
		return this;
	};
	return that;
};
```
## 第六章 数组

#### 1. 数组字面量
数组字面量提供一种非常方便的创建数组的表示方法，一个数组字面量是在一对方括号中包围多个逗号分隔符的值得表达式，数组字面量允许出现任何表达式可以出现的地方，数组从`0`开始
```js
var empty = [];
var numbers = ['zero','one','tow','three','four','five','six','seven','eight','nine'];
empty[1];
numbers[1];
empty.length;
number.length
```
对象字面量：
```js
var numbers_object =  {
    '0':'zero',
    '1':'one',
    '2':'teo'
};
```
连着产生结果相似，都是包含十个属性的对象，并且这些属性都有名字和值，但是这两个对象也有不同，`numbers`继承自`Array.prototype`,而`number_object`继承自`Object.prototype`,其次前者有个`length`属性，但是后者没有。在其他语言中一个数组中的元素要求都是相同类型，但是在`js`中允许数组包含任意混合类型的值

#### 2. 长度
每个数组都有一个`length`属性，和其他语言不同，这个`length`没有上界。就是说用大于或等于`length`的数字作为下标来存储一个元素，则`length`值会被增大以容纳新的元素，不会发生数组越界的错误。`length`的值是这个数组的最大整数属性名加`1`，不一定等于数组里属性的个数。
```js
var myArray = [];
myArray.length //0

myArray[10000] = true;
myArray.length //10001
```
`[]`后置下标运算符把它所含的表达式转换成一个字符串，如果表达式有`toString`方法就使用该方法，这个字符串将被改用做属性名。设置更大的`length`不会给数组分配更多的空间，而把`lenght`设小将导致所有下标大于等于新的`length`的属性被删除。
数组可以通过`length`属性增加元素，也可以通过`push`方法：
```js
number = ['zero','one','two'];
number.length = 3;
number[number.length] = 'three';
number.push('go');
```
#### 3. 删除
在`js`中数组其实就是对象，所以`delete`运算符可以用来从数组中移除元素；
```js
delete number[2];
```
但是这个删除会在数组中留有一个空位，这是因为排在被删除元素之后的元素都保留他们最初的属性，所以可以用`splice`方法改变数组，删除一些元素并且替换为其他元素。这个方法的第一个参数是要删除元素在数组中的序号，第二个参数是要删除元素的个数，任何额外的参数都会插入到被删除元素的位置：
```js
number.splice(2,1);
```

#### 4. 枚举
`for in`语句可以用来遍历一个数组的所有属性，但是`for in`无法保证属性的顺序，而大多数要遍历的数组的场合都期望按照阿拉伯数字顺序产生元素，而且可能从原型链中得到意外属性的问题依旧存在。但是，常规的`for`语句可以避免这样的问题
```js
number = ['zero','one','two'];
for(var index in number) {
	console.log(number[index]);
}
number.forEach(function(value) {//推荐用法
	console.log(value);
});
```
#### 5. 容易混淆的地方
为了避免一个常见的错误：在改用数组的时候用对象，改用对象时候用数组，就需要记住如下规则：当属性名是小而连续的整数时，应该用数组，否则使用对象。`js`本身对数组和对象的去呗就是混乱的，即使`typeof`运算符报告数组也是`object`,这没有任何意义。可以自定义一个函数去判断是否是数组
```js
var is_array = function (value) {
    return Object.prototype.toString.apply(value) === '[object Array]';
};
```
#### 6. 方法
数组有一套可用的方法，这些方法就是存储在`Array.prototype`中的函数，并且`Array.prototype`可以扩充。如下给`array`增加一个对数组计算的方法：
```js
Array.method('reduce',function(f,value) {
    var i;
    for(i = 0; i < this.length; i ++) {
        value = f(this[i], value);
    }
    return value;
});
```
通过给`Array.prototype`扩充一个函数，每个数组都继承了这个方法，在这个例子中，我们定义了一个函数和一个初始值作为参数，它遍历这个数组，以当前元素和该初始值为参数调用这个函数，并且计算一个新值，当完成时，返回这个值，如果传入一个把两个数组相加的函数，它就会计算相加之和，如果传入把两个数字相乘的函数，它就会计算两者乘积
```js
var data = [4, 8, 15, 16, 23, 42];
var add = function (a, b) {
    return a + b;
}
var mult = function (a, b) {
    return a * b;
}
var sum = data.reduce(add, 0);
var product = data.reduce(mult, 1);
data.total = function () {
    return this.reduce(add,0);
};
totle = data.total();
```
这里需要注意几点：
* `total`不是整数，所以给数组增加一个`total`属性不会改变它的`length`，当属性名为整数时，数组才是最有用的，但他们依旧是对象，并且可以接受任何字符串作为属性名。
* `Object.create`方法在数组中是没有意义的，因为它产生一个对象，而不是一个数组，产生的对象将继承这个数组的值和方法，但是没有特殊的`length`属性

#### 7. 制定初始值
`js`中的数组通常不会有预置值，如果你用`[]`得到一个数组，它里面就会是空的，如果你访问一个不存在的元素，得到的是`undefined`。所以在获取每个元素之前都应该设置这个值。如果有一个算法是假设每个元素都从一个值开始，那么你就要自己生命这个数组，你可以自定义一个方法`Array.dim`来帮我们完成这件事，这样就完成了数组的初始化：
```js
Array.dim = function (dimension, initial) {
    var a = [], i;
    for(i = 0; i < dimension; i++) {
        a[i] = initial;
    }
    return a;
}
var myArray = Array.dim(10,0);
```
但是`js`中没有多维数组，为了创建一个二维数组或者叫数组的数组，你必须自己创建这个二维数组
```js
//二维数组
var matrix = [
[2, 3, 4],
[3, 4, 5],
[4, 5, 6]
]

//创建一个空的二维数组
for(var i= 0; i < n; i ++) {
    my_array[i] = [];
}
//注意这里不能用Array.dim(i,[])
```
能否自定义一个方法帮我们完成创建二维数组，一下就是示例：
```js
Array.matrix = function (m, n, initial) {
    var a, i, j, mat = [];
    for(i = 0; i < m; i ++) {
        a = [];
        for(j = 0; j < n; j ++) {
            a[j] = intial;
        }
        mat[i] = a;
    }
    return mat;
} 
var mymatrix = Array.matrix(4, 4, 0);
document.writeln(mymatrix[3][3]);

Array.identity = function (n) { //构造单位矩阵
    var i ,mat = Array.matrix(n, n, 0);
    for(i = 0; i < n ; i ++) {
        mat[i][i] = 1;
    }
    return mat
}
mymatrix = Array.identity(4);
document.writeln(mymatrix[3][3]);
```

## 第七章 正则表达式
 * 正则表达式是什么：正则表达式是一门简单语言的语法规范，用于对字符串中的信息实现查找、替换和提取的操作。
 * 正则表达式存在的问题：
    * 规则非常复杂
    * 难以阅读，且修改起来充满危险
    * 不支持注释和空白，所有部分都紧密排列在一起

#### 1. 一个例子
这个例子是用来匹配`url`的
```js
var parse_url = /^(?:([A-Za-z]+):)?(\/{0,3})([0-9.\-A-Za-z]+)(?::(\d+))?(?:\/([^?#]*))?(?:\?([^#]*))?(?:#(.*))?$/;
var url = "http://www.ora.com:80/goodparts?#fragment";
var result = parse_url.exec(url);
var names = ['url','scheme','slash','host','port','path','query','hash'];
var blanks = '  ';
var i;
for(i = 0; i < names.length; i ++) {
    document.writeln(names[i] + ':' + blanks.substring(names[i].length),result[i])
}
```
以下介绍每一部分代表的含义：
* `^`: 表示此字符串的开始，它就是一个锚，指引`exec`不要跳过那些不想`url`的前缀
* `(?:([A-Za-z]+):)?`:这一部分匹配的是`url`最前面的协议名，仅当它后面有一个`:`的时候才匹配，最后面的`:`就是按照字面进行匹配
* `(?:...)`:表示一个非捕获型分组，仅仅捕获，不会放到结果集里面。后缀`?`表示这个分组是可选的。表示这个分组重复`0`次或`1`次
* `(...)`:表示一个捕获型分组，一个捕获型分组会复制它匹配到的文本，然后把他们放到结果集里面，就是`result`数组，每个捕获型分组都会被指定一个编号，第一个捕获型分组的编号是`1`，所以该分组匹配到的文本会在`result[1]`中
* `[...]`:表示一个字符类，`A-Za-z`这个字符类包含`26`个大写字母和`26`个小写字母,`(-)`:表示范围从`A`到`Z`，后缀`+`表示这个字符类会被匹配一次或多次
* `\/`:表示匹配`/`用`\`来进行转义，这样就不会被错误得解释为这个正则表达式的结束符
* `{0,3}`:表示`/`会被匹配`0`到`3`次
* `([0-9.\-A-Za-z]+)`:这一部分会匹配一个主机名，由一个或多个数字、字母以及`.`或`-`组成,`-`会被转义为`\-`以防止与表示范围的连字符混淆
* `(?::(\d+))?`:这一部分撇皮的是端口号，它由一个一个前置`:`加上一个或多个数字组成的序列。`\d`表示一个数字字符即`[0-9]`
* `(?:\/([^?#]*))?`:这一部分捕获一个或多个数字串。在里面是另外一个可选分组，这个分组以`/`开始，之后的字符类`[^?#]`以一个`^`开始，它表示这个类包含除`?`和`#`之外的所有字符，但是可能会有恶意注入的字符
* `(?:\?([^#]*))?`:这个部分匹配`?`开始的分组，可以匹配多次，且不包含`#`
* `(?:#(.*))?`: 这个部分会匹配以`#`开始的分组，除去`.`
* `$`: 表示这个字符串的结束，它保证这个`url`的尾部没有其他更多的内容了

再来看一个匹配数字的例子：
```js
var parse_number = /^-?\d+(?:\.\d*)?(?:e[+\-]?\d)?$/i;
var test = function (num) {
    document.writeln(parse_number.test(num));
};

test('1');//true
test('number');//false
test('1.1');//true
test('1.1.1.1');//false
test('123.34E-67');//true
test('123.45F-67');//false
```
再分解开来看：
* `/^ $/i`: 用`^`和`$`来框定这个正则表达式,如果没有这两个标识，只要一个字符串包含一个数字，这个正则表达式就会匹配成功（输出`true`），但是有了这两个标识，只有当一个字符串的内容仅为数字时，它才会匹配成功
* `i`:表示匹配字母时忽略大小写
* `-?`:负号后面的`?`表示这个负号是可选的，就是有没有这个负号都是可以的
* `\d+`:含义和`[0-9]`一样，后缀`+`表示可以匹配一个或多个数字
* `(?:\.\d*)?`: 表示一个可选的非捕获型分组，通常用非捕获型分组来替代少量不优美的捕获型，因为捕获型在性能上会有损失，这个分组会匹配后面跟随的`0`个或多个数字的小数点
* `(?:e[+\-]?\d)?`: 这是另外一个可选的非捕获型分组，它会匹配一个`e`或`E`，一个可选的正负号或多个数字

#### 2. 结构
创建一个`RegExp`对象有以下两种方法：
* 优先考虑的方法是使用正则表达式字面量。正则表达式字面量被包围在一对斜杠中。`RegExp`能设置三个标识符`g,i,m`，都是直接添加在`RegExp`末尾，表示的意思分别是`g`——全局的（匹配多次；不同的方法对`g`标识的处理各不相同）；`i`——忽略大小写；`m`——多行（`^`和`$`能匹配行结束符）
* 另外一个方法是使用`RegExp`构造器，这个构造器接受一个字符串，并把它编译为一个`RegExp`对象。创建这个字符串时候要多加小心，因为反斜杠在正则表达式和在字符串字面量中有一些不同的含义，通常要双写反斜杠，以引起对引号进行转义。

```js
var my_regexp = new RegExp("\"(?:\\\\.|[^\\\\\\\])*\"",'g');
```
第二个参数是指定标识的字符串。`RegExp`适用于必须在运行时动态生成正则表达式的情形。

`RegExp`对象包含的属性列表如下：

![](http://p4k6er8dp.bkt.clouddn.com/18-9-7/11960762.jpg)

用正则表达式字面量创建一个`RegExp`对象：
```js
function make_a_matcher () {
    return /a/gi; //返回的是一个正则对象
}
var x = make_a_matcher();
var y = make_a_matcher();//x和y是相同对象
x.lastIndex = 10;
document.writeln(y.lastIndex);
```

#### 3. 元素
1. 正则表达式分支

    一个正则表达式分支包含一个或多个正则表达式序列。这些序列被`|`字符分割，如果这些序列中的任何一项符合匹配条件，那么这个选择就匹配。并且匹配顺序按照书写顺序。
    ```js
    "info".match(/in|int/)
    ```
    以上例子中会在`info`中匹配`in`，但是不会匹配`int`，因为`in`已经匹配成功了
2. 正则表达式序列

    一个正则表达式包含一个或多个正则表达式因子，每个因子能选择是否跟随一个量词，这个量词决定着这个因子被允许出现的次数，如果没有指定这个量词，那么该因子只会被匹配一次
3. 正则表达式因子

    一个正则表达式因子可以是一个字符、一个由圆括号包围的组，一个字符类，或者是一个转义序列。除了控制字符和特殊字符（`\/[](){}?+-*.^$`）以外所有的字符都会按照字面处理。如果希望以上字符按照字面意思匹配就需要前缀`\`进行转义，但是`\`不能使字母或数字字面化。一个未转义的`.`会匹配除行结束符以外的任何字符。
    
    当`lastIndex`属性为`0`时，一个未转义的`^`会匹配文本的开始。当指定了`m`标识时，它也能匹配行结束符。
    
    一个未转义的`$`将匹配文本的结束。当指定了`m`标识时，它也能匹配行结束符
    
4. 正则表达式转义

    反斜杠字符在正则表达式中与其在字符串中一样均表示转义，但是在正则表达式因子中，稍微有不同。像在字符串中，`\f`是换页符，`\n`是换行符，`\r`是回车符  ，`\t`是制表符。
    
    但是在正则表达式因子中，`\b`不是退格，`\d`等同于`[0-9]`它可以匹配一个数字。`\D`则表示与其相反`[^0-9]`（`^`是‘非’的意思，即非数字）。`\s`等同于`[\f\n\r\t\u000B\u0020\u00A0\u2028\u2029]`这是空白符的一个不完全子集。`\S`则表示与其相反的`[^\f\n\r\t\u000B\u0020\u00A0\u2028\u2029]`。`\w`等同于`[0-9A-z_a-z]`即所有数字和大小写字母组成的集合。`\W`表示与其相反，`[^0-9A-z_a-z]`。`\1`是向分组`1`所捕获到的文本的一个引用，因为它能被再次匹配，依次类推
5. 正则表达式分组
    
    分组共分四组：
    * 捕获型：一个捕获型分组是一个被包围在圆括号中的正则表达式分支，任何匹配这个分组的字符都会被捕获。每个捕获型分组都被指定了一个数字，在正则表达式中第一个捕获的是分组`1`，依次类推
    * 非捕获型：改分组是在一对圆括号里面前缀`?:`，仅做简单的匹配，并不会捕获所有匹配的文本，这回带来微弱的优势，非捕获型分组不会干扰捕获型分组的编号
    * 向前正向匹配：在一对圆括号中前缀`?=`，它类似非捕获型分组，但是在这个分组匹配之后，文本会倒回到它开始的地方，实际上并不匹配任何东西，这不是一个好特性
    * 向前负向匹配：在一对圆括号中前缀`?!`,它类似于向前正向匹配，但是只有当他匹配失败时才继续向前进行匹配，这也不是一个好特性
6. 正则表达式字符集

    正则表达式字符集是一种指定一组字符的便利方式。例如，如果想匹配一个元音字母，我们就可以写做`(?:a|e|i|o|u)`,但是它可以被方便地写成一个类`[aeiou]`
    
    类提供另外两个便利，第一个是能够指定字符范围，所以一组由`32`个`ASCII`的特殊字符组成的集合可以用方括号括起组成一个简洁的类，另外一个方便之处是类的求反，如果`[`后的第一个字符是`^`那么这个类会排除这些特殊字符在。
7. 正则表达式字符转义

    字符类内部的转义规则和正则表达式因子的相比稍有不同，此处的`[\b]`是退格符。下面是在字符类中需要被转义的特殊字符：`-/[\]^`
8. 正则表达式量词：

    正则表达式因子可以用一个正则表达式量词后缀来决定这个因子应该被匹配几次。包围在一对花括号中的一个数字表示这个因子应该被匹配的次数，所以`/www/`匹配的和`/w{3}/`一样，`{3,6}`会匹配`3,4,5,6`次，`{3,}`会匹配`3`次或更多次。
    
    `?`等同于`{0,1}`，`*`等同于`{0,}`,`+`等同于`{1,}`。
    
    如果只有一个量词，表示趋向于进行贪婪性匹配，即匹配尽可能多的副本直到达到上限，如果这个量词附加一个后缀`?`则表示趋向于非贪婪匹配，即只匹配必要的副本就好，一般情况下最好坚持贪婪性匹配。
    
## 第八章 方法

#### 1. 数组 `Array`
* `array.concat(item...)`

    `concat`方法产生一个新数组，它包含一份`array`的浅复制并把一个或多个参数`item`附加在后面，如果参数是一个数组，那么它的每个元素分别添加，后面还有相同功能类似的`array.push`
    ```js
    var a = ['a','b','c'];
    var b = ['x','y','z'];
    var c = a.concat(b,true);
    ```
* `array.join(separator)`
    
    `join`方法是把一个`array`构造成一个字符串，它先把`array`中的每个元素构造成一个字符串，接着用`separator`分隔符连接，默认的`separator`是个`,`，想要做到无间隔的连接，可以用空字符串作为分隔符。如果你想把大量字符片段连接成一个字符串，就把这些片段放在数组中再用`join`方法连接起来，这种方法比用`+`运算符快

    ```js
    var a = ['a','b','c'];
    a.push('d');
    var c = a.join('');
    ```
* `array.pop()`
    
    `pop`和`push`方法使得数组`array`可以像堆栈一样工作，`pop`方法移除`array`中的最后一个元素并返回这个元素,最后数组的长度减一，如果`array`是`empty`，它会返回`undefined`

    ```js
    var a = ['a','b','c'];
    var c = a.pop();
    ```
    `pop`的实现方式如下：
    ```js
    Array.method('pop',function () {
        return this.splice(this.length - 1, 1)[0];
    })
    ```
* `array.push(item...)`
    
    `push`方法把一个或多个参数`item`附加到一个数组的尾部，和`concat`方法不同的是，它会修改`array`,如果参数是一个数组，它会吧参数数组作为单个元素整个添加到数组中，返回这个`array`的新长度值

    ```js
    var a = ['a','b','c'];
    var b = ['x','y','z'];
    var c = a.push(b, true);
    
    //push的实现：
    Array.method('push',function () {
        this.splice.apply(
        this,
        [this.length,0].concat(Array.prototype.splice.apply(arguments)));
        return this.length;
    });
    ```
* `array.reverse()`
    
    这个方法可以将数组中元素顺序反转，并返回数组本身
* `array,shift()`
    
    这个方法移除数组中第一个元素并返回该元素，如果这个数组是空的，它会返回`undefined`，`shift`通常比`pop`慢。`shift`实现方法：

    ```js
    Array.method('shift',function () {
        return this.splice(0,1)[0];
    });
    ```
* `array.slice(start,end)`
    
    该方法对数组中的一段做浅复制，首先复制`array[start]`，一直复制到`array[end]`为止,`end`参数是可选的，默认值是该数组的长度，如果两个参数中的任何一个是负数，`array.length`会和他们相加，试图让他们变成非负，如果`start`大于等于`array.length`得到的结果就是一个空数组。**注意`slice`和`splice`不一样**
* `array.sort(comparefn)`
    
    `sort`方法对数组中的内容进行排序，它不能正确的给一组数字排序:

    ```js
    var n = [4,8,15,16,23,42];
    n.sort();//n = [15,16,23,4,42,8];
    ```
    `js`的默认比较函数要把被排序元素视为字符串，它尚未足够智能到在比较这些元素之前先检测他们的类型，所以当他比较这些数字的时候会吧他们转化为字符串，于是得到一个错得离谱的结果。但是我们可以使用自己的比较函数来替换默认的比较函数，比较函数应该接受两个参数，如果这两个参数相等则返回`0`，如果第一个参数应该排列在前面，则返回一个负数，如果第二个参数应该排列在前面则返回一个正数
    ```js
    n.sort(function (a, b) {
        return a - b;
    })//n = [4,8,15,16,23,42]
    ```
    上面这个函数可以使用数字正确排序，但是它不能使字符串排序，如果我们想要给任何包含简单值得数组排序，必须要做更多工作：
    ```js
    var m = ['aa', 'bb', 'a', 4, ,8 ,15, 16, 23, 42];
    m.sort(function (a, b) {
        if(a === b) {
            return 0;
        }
        if(typeof a === typeof b) {
            return a < b ? -1 : 1;
        }
        return typeof a < typeof b ? -1 : 1;
    });//m = [4， 8， 15， 16， 23， 42， 'a', 'aa', 'bb']
    ```
    如果大小写不重要，比较哈数应该在比较值钱先将两个运算数转化为小写，如果有一个更智能的比较函数，我们也可以使用对象数组排序，为了让这个事情更满足一般情况，我们将编写一个构造比较函数的函数
    ```js
    var by = function (name) {
        return function (o, p) {
            var a, b;
            if(typeof o === 'object' && typeof p === 'object' && o && p) {
                a = o[name];
                b = p[name];
                if(a === b) {
                    return 0;
                }
                if (typeof a === typeof b) {
                    return a < b ? -1 : 1;
                }
                return typeof a < typeof b ? -1 : 1;
            } else {
                throw {
                    name: 'Error',
                    message: 'Expected an object when sorting by' + name
                };
            }
        };
    };
    var s = [
    {first:'Joe', last: 'Besser'},
    {first:'Moe', last: 'Hpward'},
    {first:'Joe', last: 'Derita'},
    {first:'Shemp', last: 'Howard'},
    {first:'Larry', last: 'Fine'},
    {first:'Curly', last: 'Howard'},
    ];
    s.sort(by('first'));
    ```
* `array.splice(start, deletecount, item...)`
    
    该方法从`array`中移除一个或多个元素，并用新的`item`替换，`start`是从数组移除元素的开始位置，`deletecount`是移除元素的个数，如果有额外的参数，就会被插入到被移除元素的位置上，该方法结果返回一个所有移除元素的数组

* `array.unshift(item)`
    
    该方法像`push`一样，用于把元素添加到数组中，但是`item`插入到`array`的开始部分而不是尾部，该方法返回最后数组的长度

#### 2. `Function`
* `function.apply(thisArg, argArray)`
    
    `apply`方法调用`function`，传递一个会绑定到`this`上的对象和可选的数组作为参数，`apple`方法被用在`apply`调用模式

    ```js
    Function.method('bind',function (that) {
        var method = this,
        slice = Array.prototype.slice,
        args = slice.apply(arguments,[1]);
        return function () {
            return method.apply(that, args.concat(slice.apply(arguments,[0])));
        };
    });
    var x = function () {
        return this.value;
    }.bind({value:666});
    alert(x());
    ```
#### 3.`Number`
* `number.toExponential(fractionDigits)`
    
    `toExponential`方法把`number`转换成一个指数形式的字符串，可选参数`fractionDigits`控制其小数点后的数字位数，它的值必须在`0-20`
* `number.toFixed(fractionDigits)`
    
    该方法把`number`转换为一个十进制数字形式的字符串，可选参数`fractionDigits`控制其小数点后的数字位数，他的值必须在`0-20`，默认为`0`
* `number.toPrecison(precision)`
    
    该方法把`number`转换为一个十进制数形式的字符串，可选参数`precision`控制数字的精度，值必须在`0-21`
* `number.toString(radix)`
    
    该方法把`number`转换为一个字符串，可选参数`radix`控制基数，它的值必须在`2-36`，默认是以`10`为基数的，最常用的是整数，但是它可以用任意的数字

#### 4. `Object`
* `object.hasOwnProperty(name)`
    
    如果这个对象包含一个名为`name`的属性，那么`hasOwnProperty`方法返回`true`,原型链中的同名是属性不会被检查，这个方法对`name`就是`hasOwnProperty`的时不起作用，此时会返回`false`:

    ```js
    var a = {member:true};
    var b = Object.create(a);
    var t = a.hasOwnProperty('member');//t = true
    var u = b.hasOwnProperty('member');//u = false
    var v = b.member;//v = true
    ```
#### 5. `RegExp`
* `regexp.exec(string)`
    
    `exec`方法是使用正则表达式的最强大和最慢的方法，如果它成功地匹配`regexp`和字符串`string`，它会返回一个数组，数字的下标为`0`的元素包含正则表达式`regexp`匹配的字符串，下标为`2`的元素是分组一捕获的文本。下标为`2`的元素是分组二捕获的文本，依次类推，如果匹配失败，它会返回`null`。
    如果一个`regexp`带有一个`g`标识，事情会变得更加复杂，查找不是从这个字符串的起始位置开始，而是从`regexp.lastIndex`（初始值为`0`）位置开始，如果匹配成功，那么`regexp.lastIndex`将被设置为该匹配后第一个字符的位置，不成功的匹配会重置`regexp.lastIndex`为`0`。这就允许你通过循环调用`exec`去查询一个匹配模式在一个字符串中发生了几次，有两件事情需要注意：如果提前退出这个循环，再次进入这个循环前必须把`regexp.lastIndex`c重置为`0`，而且`^`仅匹配`regexp.lastIndex`为`0`的情况
    ```js
    var text = '<html><body bgcolor=linen><p>' + 'This is <b>bold<\/b>!<\/p><\/body><\/html>';
    var tags = /[^<>]+|<(\/?)([A-Za-z]+)([^<>]*)>/g;
    var a,i;
    while ((a = tags.exec(text))) {
        for (i = 0;i < a.length; i++) {
            document.wirteln(('//[' + i + ']' + a[i]).entityify());
        }
        document.writeln();
    }
    ```
* `regexp.test(string)`
    
    `test`方法是使用正则表达式的最简单和最快的方法，如果该正则表达式匹配字符串，它返回`true`，否则返回`false`，不要对这个方法使用`g`标识：

    ```js
    var b = /&.+;/.test('frank &amp; beans');
    //test的实现方式：
    Regexp.method('test',function () {
        return this.exec(string) != null;
    });
    ```
#### 6. `string`
* `string.charAt(pos)`
    
    该方法返回字符串中`pos`位置处的字符，如果`pos`小于`0`或大于等于字符串的长度，它会返回空字符串，`js`没有字符类型，这个方法返回的结果是一个字符串:

    ```js
    var name = 'Curly';
    var initial = name.charAt(0);
    ```
* `string.charCodeAt(pos)`
    
    改方法同`charAt`一样，只不过它返回的不是一个字符串，而是以整数形式表示的在`string`中的`pos`位置处的字符的字符码位,如果`pos`小于`0`或大于等于字符串的长度，它返回`NaN`
* `string.concat(string)`
    
    该方法把其他的字符串连接在一起来构造一个新的字符串，它很少使用，因为`+`更方便

    ```js
    var s = 'C'.string.concat('a','t');
    ```
* `string.indexOf(searchString,postion)`
    
    该方法在字符串内查找另一个字符串`searchstring`,如果能够被找到，返回第一个匹配字符的位置，否则返回`-1`,可选参数`postion`可设置从`string`的某个指定位置开始查找
* `string.lastindexOf(searchString,postion)`
    
    该方法和`indexOf`类似，只是它从字符串的末尾开始查找而不是从开头
* `string.match(regexp)`
    
    该方法让字符串和一个正则表达式进行匹配，它依据`g`标识来决定如何进行匹配，如果没有`g`标识，那么调用该方法的结果与调用`regexp.exec`结果相同，如果，`regexp`带有`g`标识，那么它生成一个包含所有匹配（除捕获分组之外）的数组

    ```js
    var text = '<html><body bgcolor=linen><p>' + 'This is <b>bold<\/b>!<\/p><\/body><\/html>';
    var tags = /[^<>]+|<(\/?)([A-Za-z]+)([^<>]*)>/g;
    var a,i;
    a = text.match(tags);
    for(i = 0; i < a.length; i ++) {
        document.wirteln(('//[' + i ']' + a[i]).entityify());
    }
    ```
* `string.localeCompare(that)`
    
    该方法比较两个字符串，如果`string`比`that`小，结果为负数，如果它们是相等的，那么结果为`0`，这类似于`array.sort`比较函数的约定：

    ```js
    var m = ['AAA', 'A', 'aa', 'a', 'Aa', 'aaa'];
    m.sort(function (a,b) {
        return a.localeCompare(b);
    });
    ```
* `string.replace(searchValue,replaceValue)`
    
    该方法对字符串进行查找和替换，并返回一个新的字符串，参数`searchValue`可以是一个字符串或一个正则表达式对象，如果它是一个字符串，那么`searchValue`只会在第一次出现的地方被替换，如果`searchValue`是一个正则表达式并且带有`g`标识，它都会替换所有的标识，如果没有`g`标识，它仅会替换第一个匹配。`replaceValue`可以是一个字符串或一个函数，如果`replaceValue`是一个字符串，`$`拥有特别的含义，如下表
    美元符号序列 | 替换对象
    ---|---
    `$$` | `$`
    `$&` | 整个匹配的文本
    `$number` | 分组捕获的文本
    $` | 匹配之前的文本
    $' | 匹配之后的文本

    ```js
    var oldareacode = /\((\d{3})\)/g;
    var p = '(555)666-1212'.replace(oldareacode,'$1-');
    ```
    
    如果`replaceValue`是一个函数，那么美遇到一次匹配函数就会被调用一次，而函数返回的字符串将会被用做替换文本，传递给这个函数的第一个参数是整个被匹配的文本，第二个参数是分组一捕获的文本，再下一个参数是分组而捕获的文本，依次类推

* `string.search(regexp)`
    
    该方法与`indexof`方法相似，只是它接受一个正则表达式对象作为参数而不是一个字符串，如果匹配到，它返回第一个匹配的首字符的位置，如果没有匹配到，则会返回`-1`，此方法会忽略`g`，且没有`position`参数：

    ```js
    var text = 'and on it he says "any damn fool could';
    var pos = text.search(/["']/);
    ```
* `string.slice(start, end)`
    
    改方法复制字符串中的一部分构造新的字符串，如果`strat`参数是负数，它将于`string.length`相加，`end`参数是可选的，且默认值是`string.length`，如果`end`是负数，那么它将与`string.length`相加，`end`参数等于你要取得最后一个字符的位置加`1`，要想得到从位置`p`开始的`n`个字符，就用`string.slice(p,p + n)`

    ```js
    var text = 'adn in it he says "Any damn fool could';
    var a = text.slice(18);
    var b = text.slice(0, 3);
    var c = text.slice(19, 32);
    ```
* `string.split(separator,limit)`
    
    该方法把字符串分割成片段来创建一个字符串数组，可选参数`limit`可以限制被分割的片段的数量，`separator`参数可以一个字符串或一个正则表达式，如果`separator`是空字符，会返回一个单字符数组

    ```js
    var digits = '123456789';
    var a = digits.split('',5);
    ```
    否则，此方法会在字符串中查找所有`separator`出现的地方，分隔符两边的每个单元文本都会被复制到该数组中，此方法会忽略`g`标识。当`separator`是一个正则表达式时，有一些`js`的实现会排除掉空字符串，但有一些则不会
    
* `string.substring(start, end)`
    
    该方法和`slice`方法一样，只是它不能处理负数参数，因此一般都用`slice`方法去替代`substring`

* `string.toLocaleLowerCase()`和`string.toLocaleUpperCase()`

    两个方法都返回一个新的字符串，前者使用本地化的规则把这个字符串中的所有字母转换为小写格式，后者则使用本地化的规则把字符串中的所有字母转换为答谢，这个方法主要用在土耳其语上
    
* `string.toLowerCase`和`string.toUpperCase`

    两种方法都返回一个新的字符串，前者把字符串中的所有字母都转换为小写格式，后者转换为大写格式
    
* `string.fromCharCode(char...)`
    
    该方法根据一串数字返回一个字符串。

    ```js
    var a = String.fromCharCode(67,97,116);
    //a = 'Cat'
    ```
    
## 第十章 优美的特性

精简的`js`里都是好东西，包括以下内容：
1. **函数是顶级对象**，函数是有词法作用域的闭包
2. **基于原型继承的动态对象**，对象是无类别的，我们可以通过给普通的赋值给任何对象增加一个新属性，一个对象也可以从另一个对象继承成员属性
3. **对象字面量和数组字面量**，这对创建新的对象和数组来说是一种非常方便的表示法，`js`字面量是数据交换格式`JSON`的灵感之源

## 附录A 毒瘤
本附录中展示的是`js`的一些难以避免的问题特性，我们必须知道这些问题并准备好应对措施

#### 1. 全局变量
全局变量会随着程序变大变得难以管理，因为一个全局变量在程序的任何部分任意时间都可以被修改，它们使得程序的行为变得极度复杂，在程序中使用全局变量降低了程序的可靠性。全局变量使得在同一个程序中独立的程序变得更难，如果这些全局变量名称碰巧和子程序中的变量名称相同，那么它们将会发生冲突。

但是`js`的问题不仅自傲与它允许全局变量，而且在于它依赖全局变量，因为`js`没有连接器，所有的编译单元都载入一个公共的全局对象中。

有以下三种方式可定义全局变量：
1. 在任何函数之外放置一个`var`语句

```js
var foo = value;
```
2. 直接给全局对象添加一个属性，全局对象是所有全局变量的容器，在`Web`浏览器中，全局对象名为`window`

```js
window.foo = value
```
3. 直接使用未经声明的变量，被称为隐式的全局变量

```js
foo = value;
```

#### 2. 作用域
`js`的语法源于`C`，在所有`C`系语言中，一个代码块（包括一对花括号中的一组语句）会创造一个作用域，代码块中声明的变量在其外部都是不可见的，`js`采用这样的块语法，却没有提供块级作用域：**代码块中声明的变量在包含此代码块的函数的任何位置都是可见的**

一般来说，声明变量的地方就是在第一次用到它的地方，但是这种做法不适用`js`，因为它没有块级作用域，所以最好的地方是每个函数的开头声明所有变量

#### 3. 自动插入分号
`js`有一个自动恢复机制，它试图通过自动插入分号来修正有缺损的程序，但是这样只会掩盖更为严重的错误，有时它不合时宜的插入分号，如果一个`return`语句返回一个值，这个值表达式的开始部分必须和`return`位于同一行：
```js
return
{
    status: true
};
```
这看起来要返回一个包含`status`成员的对象，但是自动插入分号让它变成了返回`undefined`，自动插入分号导致程序被误解，却没有警告提醒，如果把`{`放在上一行的尾部而不是下一行，就可以避免这个问题
```js
return {
    status: true
};
```
#### 4. 保留字
在`js`被保留的单词不能被用来命名变量或参数，当保留字被用做对象字面量的键值时，它必须被引号括起来，它们不能用在点表示法中，所以有时必须用括号表示法

#### 5. Unicode
`Unicode`把一对字符视为一个单一的字符，而`js`认为一对字符是两个不同的字符

#### 6. typeof
`typeof`运算符返回一个用于识别其运算数类型的字符串，所以：`typeof 98.6`返回`number`，但是`typeof null`返回的是`object`而不是`null`，但是有更好检测`null`的方式：`my_value === null`
```js
if(my_value && typeof my_value === 'object') {
    //my_value是一个对象或数组
}
```
除此之外，`js`也不能区分正则表达式，有些会返回`object`有些会返回`function`，但是不会返回`regexp`

#### 7. parseInt
`parseInt`是一个把字符串转换为整数的函数，它遇到在非数字是会停止解析，所以`parseInt("16")`与`parseInt("16 tons")`产生结果相同，如果该函数会提醒我们出现了额外文本就好了。

如果该字符的第一个字符是`0`那么该字符串会基于八进制而不是十进制求值，在八进制中，`8`和`9`不是数字，所以`parseInt('08')`和`parseInt(09)`都会产生`0`作为结果，这个错误会导致程序解析日期和时间时出现问题，解决问题的方法就是`parseIint`可以接受一个基数作为参数，因此可以这样写`parseInt('08',10)`**现在已经修正了这个问题，但是保险起见，在后面加上参数更好**

#### 8. +运算符
`+`运算符用于加法运算或字符串连接，它究竟会如何执行取决于其参数的类型，如果其中一个运算数是空字符串，它会把另外一个运算数换成字符串并返回，如果两个运算数都是数字，它返回两者之和，否二都是把两个运算数转换为字符串并连接起来，这个复杂的行为是`bug`的来源，如果你打算用`+`做加法，请确保两个运算数都是整数

#### 9. 浮点数
二进制浮点数不能正确处理十进制的小数，因此`0.1 + 0.2`不等于`0.3`,这是`js`中最常见的问题，它是遵循二进制浮点数算术标准而有意导致的结果，这个标准对很多应用都是适合的，但它违背了中学数学认识，但是，浮点数中的整数运算是精确的，所以小数表现出来的错误可以通过指定精度避免,即先给小数乘一个较大的数使其变为整数，最后在给结果除以这个较大的数最后就能得到精确结果

#### 10. NaN
`NaN`是`IEEE754`中定义的一个特殊数量值，它表示的不是一个数字。该值可能会试图把非数字形式的字符串转换为数字时产生。如果`NaN`是数学运算中的一个运算数，那么结果就是`NaN`。

`typeof`不能辨别数字和`NaN`，而且`NaN`也不等同于它自己：
```js
NaN === NaN // flase
NaN !== NaN //true
```
`js`提供了一个`isNaN`函数，**这个函数中会涉及到强制类型转换，如果是一个只有纯数字的字符串，它仍然会返回`false`**，可以辨别数字与`NaN`:
```js
isNaN(NaN) //true
isNaN(0) //false
isNaN('oops') //ture
isNaN('0') // flase
```
判断一个值是否可用作数字的最佳方法是使用`isFinite`函数，因为它会筛选`NaN`和`Infinity`，但是`isFinite`会试图把它的运算数转换为一个数字，所以如果值事实上不是一个数字，它就不是一个好的测试，你可以这样定义自己的`isNumber`函数

#### 11. 伪数组
`js`没有真正的数组，但是`js`的数组使用非常容易，不必设置维度，也不用担心越界错误，但是它的性能比真正的数组糟糕。`typeof`运算符不能辨别数组和对象，要断一个值是否是数组，需要检查它的`constructor`属性：
```js
if(my_value && typeof my_vlaue === 'object' && my_value.constructor === Array) {
    //my_value是一个数组
}
```

#### 12. 假值
`js`拥有一组数量奇大的假值，如下表所示：

![](http://p4k6er8dp.bkt.clouddn.com/18-9-14/82063683.jpg)
这些值全部等同于假，但它们是不可互换的，例如，如果想确定一个对象是否缺少一个成员属性，这是一种错误得方式：
```js
value = myObject[name];
if (value == null) {
    alert(name + 'not found.');
}
```
`undefined`是缺失的成员属性的值，但是这段代码用`null`测试，它使用了会强制转换类型的运算符`==`，而不是更可靠的`===`运算符，有时那两个错误会被抵消，有时则不会，`undefined`和`NaN`并不是常量，但是它们是全局变量，而且你可以改变它们的值，本是不应该的

#### 13. hasOwnProperty
在第三章中，该方法被用做一个过滤器去避开`for in`语句的隐患，但是`hasOwnProperty`是一个方法，而不是一个运算符，所以在任何对象中，它可能会被一个不同的函数甚至一个非函数的值所替换：
```js
var name;
another_stooge.hasOwnProperty = null; // 被替换掉
for(name in another_stooge) {
    if(another_stooge.hasOwnProperty(name)) {
        document.writeln(name + ':' + another_stooge[name]);
    }
}
```

#### 14. 对象
`js`的对象永远不会是真的空对象，因为它们可以从原型链中取得成员属性。有时候会带来麻烦。
```js
var count = {};
console.log(count.constructor);//输出结果：[Function: Object]
```

## 附录B 糟粕
在本附录中展示的一些`js`的问题，但是很容易就能避免

#### 1. ==
`js`有两组相等运算符，`===`和`!==`，以及它们的孪生兄弟`==`和`!=`，前者一组运算符会按照你期望的工作方式，如果两个运算数类型一致且拥有相同的值，那么`===`会返回`true`，`!==`会返回`false`，但是另外一组只有在两个运算数类型一致时才会做出正确判断，如果两个运算数是不同类型的，他们就会试图强制类型转换，转换的规则复杂难以记忆
```js
'' == '0' //false
0 == '' //ture
0 == '0' // true

false == 'false' //false
false == '0' //true

false == undefined //false
false == null //false
null == undefined //true

' \t\r\n\ ' == 0 // true 
```
`==`运算符传递性的缺乏值得我们警惕，所以永远不要使用后者一对运算符，始终使用前者一对运算符，在上述例子中，如果全部使用`===`，则结果全部是`false`

#### 2. with语句
`js`提供一个`with`语句，本意是用它来快捷地访问对象的属性，但是，它的结果可能有时不可预料，所以应该尽量避免
```js
with (obj) {
    a = b;
}

//和下面代码做同样的事情
if(obj.a === undefined) {
    a = obj.b === undefined ? b : obj.b;
} else {
    obj.a = obj.b === undefined ? b : obj.b;
}

//所以，它等同于以下语句中的某一条
a = b;
a = obj.b;
obj.a = b;
obj.a = obj.b;
```
通过阅读代码，无法判断会得到哪一条结果，它可能随着程序运行到下一步时发生变化，它甚至可能在程序运行过程中发生变化，如果你不能通过阅读程序而了解它将会做什么，就不能相信它会做正确的事情。`with`语言在这门语言里存在，本身就严重影响了`js`的处理器的速度，因为它阻断了变量名的词法作用域绑定，所以应该避免使用`with`

#### 3. eval
`eval`函数传递一个字符串给`js`编译器，并且执行其结果，它是一个被滥用最多的`js`特性
```js
eval("myValue = myObject." + myKey + ";");
//等同以下写法
myvalue = myObject[myKey]
```
使用`eval`形式代码更加难以阅读，这种形式使得性能显著降低，因为它需要运行编译器。`eval`函数还减弱了应用程序的安全性，因为它的给被求值得文本授予了太多权利。

#### 4. continue语句
`continue`语句跳到循环的顶部，一段代码通过重构移除`continue`语句之后，性能都会得到改善

#### 5. switch穿越
`switch`结构中的`case`语句，默认是顺序执行，除非遇到`break`，`return`和`throw`。比如：
```js
switch(n) {
　　　　case 1:
　　　　case 2:
　　　　　　break;
　　}
```
这样写容易出错，而且难以发现。因此建议避免`switch`贯穿，凡是有`case`的地方，一律加上`break`。
```js
switch(n) {
　　　　case 1:
　　　　　　break;
　　　　case 2:
　　　　　　break;
　　}
```
#### 6. 缺少块的语句
`if`、`whiel`、`do`和`for`可以接受一个花括号的代码块，也可以接受单行语句，但是单行语句容易引起误解，虽然能节约两个字节，但是模糊了程序的结构
```js
if(ok)
    t = true;
    advance();
    
//看起来像如下语句
if(ok) {
    t = true;
    advance();
}

//但是其实是如下意思：
if(ok) {
    t = true;
}
advance();
```

#### 7. ++--
递增和递减运算符非常简洁，但是这两个运算符鼓励了一种不够谨慎的编程风格，大多数缓冲区溢出错误所造成的安全漏洞，不用最好

#### 8. 位运算符
`Javascript`完全套用了`Java`的位运算符，包括按位与`&`、按位或`|`、按位异或`^`、按位非`~`、左移`<<`、带符号的右移`>>`和用`0`补足的右移`>>>`。

这套运算符针对的是整数，所以对`Javascript`完全无用，因为`Javascript`内部，所有数字都保存为双精度浮点数。如果使用它们的话，`Javascript`不得不将运算数先转为整数，然后再进行运算，这样就降低了速度。而且"按位与运算符"`&`同"逻辑与运算符"`&&`，很容易混淆。

#### 9. function语句
在`Javascript`中定义一个函数，有两种写法：
```js
　　function foo() { }
　　
　　var foo = function () { }
```
两种写法完全等价。但是在解析的时候，前一种写法会被解析器自动提升到代码的头部，因此违背了函数应该先定义后使用的要求，所以建议定义函数时，全部采用后一种写法。

#### 10. 类型的包装对象
`js`有一套类型的包装对象，例如：
```js
new Boolean(false)
```
会返回一个对象，该对象还有一个`valueOf()`方法，会返回被包装的值,但是其实完全没有必要，因此不要使用`new Boolean`、`new Number`或`new String`，也避免使用`new Object`和`new Array`,可以使用`{}`和`[]`代替

#### 11. new
`js`的`new`运算符创建一个继承于其运算数的原型新对象，然后调用该运算数，把新创建的对象绑定给`this`，这给运算数一个机会在返回请求者之前自定义新创建的对象，但是如果你忘记`new`运算符，会得到一个普通的函数调用，并且`this`还会绑定到全局对象，而不是新创建的对象，这意味着当你的函数尝试初始化新成员的属性时将会污染全局变量,而且在编译时没有警告。
```js
var Cat = function (name) {
　　　　this.name = name;
　　　　this.saying = 'meow' ;
　　}
　　
var cat = new Cat('mimi');//生成对象的方式
```
在上述例子中如果没有加`new`，新生成的对象就不会绑定到`cat`上

按照惯例，打算与`new`结合使用的函数应该以首字母大写的形式命名，并且首字母大写的形式应该只用来命名那些构造器函数

一个更好的应对策略就是根本不使用`new`

#### 12. void
在很多语言中`void`都是一种类型，但是在`js`中，`void`是一个运算符，它接受一个运算数并返回`undefined`,这并没有什么用，反而会引起误导，应该避免使用它。

## 附录E
`js`对象表示法是一种轻量级的数据交换格式，它基于`js`的对象字面量表示法，是`js`最精华的部分之一，尽管只是`js`的一个子集，但是它与语言无关，所以现代编程语言编写的程序都可以用它来交换数据，它是一种文本格式，可以被人和机器阅读，易于实现易于使用。

#### 1. JSON语法
`JSON`有六种类型的值：对象、数组、字符串、数字、布尔值和特殊值`null`，空白（空格、制表符、回车符和换行符）可被插到任何值得前后。

`JSON`对象是一个容纳名、值对的无序对象，名字可以是任何字符串，值可以是任何类型的`JSON`值，包括数组和对象，`JSON`可以无限层嵌套，但是保持其结构的扁平式最高效的，大多是语言都有容易映射为`JSON`对象的数据类型，比如对象，结构体，字典，哈希表，属性列表或关联数组。

`JSON`数组是一个值得有序序列，其值可以是任何类型的`JSON`只，包括数组和对象，大多数语言都容易被映射为`JSON`数组的数据类型，比如数组，向量，列表或序列

`JSON`字符串被包围在一对引号中间，`\`字符用于转义，`JSON`允许`\`字符被转义，所以`JSON`可以嵌入`HTML`的`<script>`标签之中，除非是`</script>`标签，否则不允许使用`</`字符序列，但`JSON`允许使用`<\/`，它能产生同样的结果且不会和`HTML`混淆

`JSON`数字与`js`数字相似，整数的首位不允许为`0`，因为一些语言用它来标识八进制数，这种基数的混乱在数据交换格式中是不可取的，数字可以是整数，实数或科学计数

#### 2. 安全地使用JSON
`JSON`特别易于在`Web`应用中，因为`JSON`就是`js`。目前在`Web`浏览器中从服务端获取数据的最佳计数是`XMLHttpRequest`，`XMLHttpRequest`只能从生成`HTML`的同源服务器获取数据。

有漏洞的服务器并不能正确地对`JSON`进行编码，如果它能通过拼凑一些字符串而不是使用一个合适的`JSON`编码器来创建`JSON`文本，那么它可能在无意间发送了危险的数据，如果它充当的是代理的角色，并且尚未确定`JSON`文本是否格式良好就简单传递它，那么它可能再次发送危险数据

通过使用`JSON.parse`就能避免这种危险。如果文本中包含任何危险数据，那么`JSON.parse`将抛出一个异常。

在外部数据与`innerHTML`进行交互时还存在另一种危险，一种常见的`Ajax`模式把服务器端发送过来的`HTML`文本片段赋予给某个`HTML`元素的`innerHTML`属性，这是一个糟糕的习惯，如果这个`HTML`包含一个`<script>`标签或其等价物，那么恶意脚本将被执行，这可能也是因为服务器端存在漏洞。

如果这个恶意脚本在你的页面运行，它有权访问这个页面的所有状态和执行该页面的所有操作，它能与你的服务器进行交互，而你的服务器不能区分正当请求和恶意请求，恶意脚本还能访问全局对象，使得它有访问应用中除隐藏于闭包中的变量之外的数据，它可以访问`document`对象，这会使它有权访问用户所能看到的一切，它还给这个恶意脚本提供了与用户进行对话的能力，浏览器的地址栏和所有的反钓鱼程序都会告诉用户这个会话是可靠的，`document`对象还给恶意脚本授权访问网络，允许它去下载更多的恶意脚本，或者在你的防火墙之内探测站点，或者是把它已经窃取的隐私内容发送给世界的任何一个服务器。

这个危险是`js`全局变量的直接后果，并不是`Ajax`,`JSON`,`XMLHttpRequest`或`Web2.0`导致的

#### 3. JSON解析器
`JSON`解析器就是用`js`编写的，能把`JSON`文本解析成`js`数据结构的函数




## 问题
3. 原型里面的例子没看懂，原型链？
9. 同步异步请求
10. 没看懂模块里面的例子和模块有什么联系
11. 函数声明方式：var a = function () {}和function a () {} 和a: funtion () {},函数调用：var a  = b()和var a  = new a();和a();
13. 级联的例子不理解实现什么，但是级联原理好像知道一点
14. 套用的第一个例子没看懂


15. object.method是什么意思
16. 没有读懂部件里面的例子
17. 不知道判断是否是数组的例子是什么原理
18. method总是报错
19. `(?:\.\d*)?`没看懂
20. 不知道正则对象的属性的用法
21. 结构中的例子于书上结果不符
22. regexp_construction.js中两个一样的函数结果却不一样
23. 向前正向匹配分组有什么用，向前负向匹配分组
24. 匹配带元音的单词
25. replace函数的replacevalue是函数的时候参数是什么意思？
26. 为什么一个语句不能以一个函数表达式开头，为什么要把函数调用括在一个圆括号中