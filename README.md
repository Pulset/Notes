# Notes
学习心得及项目中的坑

### 判断一个数组是否为数组
1. arr instanceof Array
2. Array.isArray(arr)
### 判断一个对象是否为空对象
1. $.isEmptyObject(obj) 

### this的四种调用模式
1. 方法调用模式：</br>
如果调用表达式包含一个提取属性的动作，那么他就是方法调用
2. 函数调用模式：</br>
当一个函数并非一个对象的属性时，那么它就是当做一个函数调用
3. 构造器调用模式：</br>
当一个函数前面带上new来调用，那么背地里将会创建一个连接到该函数的prototype成员的新对象，同时this会被绑定到那个新对象上
4. apply调用模式：</br>
apply方法接受两个参数，第一个是this指向的内容，第二个就是一个参数数组

### 作用域 
　　js有函数作用域，一个函数内部任何位置定义的变量，在该函数内部任何地方都可见。最好的做法就是在函数体的顶部声明函数中可能用到的所有变量
  
### document.write与document.writeln的区别：
- document.writeln()会自动换行

### label与input的问题
- 点击label，input会自动打钩，把绑定事件放在label上。label的for与input的id相关联起来
```
<label for="male">Male</label>
<input type="radio" name="sex" id="male" />
<label for="female">Female</label>
<input type="radio" name="sex" id="female" />
```

### ios与Android的图片问题
- ios中的图片要加@2x、@3X后缀，才会清晰显示，Android中则不需要

-------------
2017/6/11
### 构造函数
如果不写new，这就是一个普通函数，它返回undefined。但是，如果写了new，它就变成了一个构造函数，它绑定的this指向新创建的对象，并默认返回this，也就是说，不需要在最后写return this;。
```
function Student(name) {
    this.name = name;
    this.hello = function () {
        alert('Hello, ' + this.name + '!');
    }
}

var xiaoming = new Student('小明');
xiaoming.name; // '小明'
xiaoming.hello(); // Hello, 小明!
```
-------------
2017/6/24
### 数组的长度
length属性的值是这个数组的最大整数属性名加上1，它不一定等于数组里的属性的个数。
```
var arr = [];
arr.length  //0
arr[100] = true;
arr.length  //101
//arr 只包含一个属性。
```
