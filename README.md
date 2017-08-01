# Notes
学习心得及项目中的坑

### 判断一个数组是否为数组
1. arr instanceof Array
2. Array.isArray(arr)
### 判断一个对象是否为空对象
1. $.isEmptyObject(obj) 
2. JSON.stringify(record)=='{}'

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
如果不写new，这就是一个普通函数，它返回undefined。但是，如果写了new，它就变成了一个构造函数，它绑定的this指向新创建的对象，并默认返回this，也就是说，不需要在最后写return this。当一个函数对象被创建时，Function构造器产生的函数对象会运行类似这样的代码：
```
this.prototype = {constructor:this};
```
新函数对象被赋予一个prototype属性，他的值是一个包含constructor属性且值为该新函数的对象。
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
-------------
2017/7/3
### 深浅拷贝与堆栈的关系
1. stack为自动分配的内存空间，它由系统自动释放；而heap则是动态分配的内存，大小不定也不会自动释放。　
2. 基本类型：5种基本数据类型Undefined、Null、Boolean、Number 和 String，他们数据大小确定，是直接按值存放的，所以可以直接访问。  
引用类型：存放在堆内存中的对象（如对象，数组，函数等），变量实际保存的是一个指针，这个指针指向另一个位置，每个空间大小不一样。读取时首先从栈中获得该对象的地址指针，然后再从堆内存中取得所需的数据。
3. 浅拷贝：只是复制了一个指向对象的指针放在栈里，他和父对象指向同一个堆里的内容，所以他们两者之间只要其中一个改动，其他一个对象也会变着改动。
4. 深拷贝：重新再堆里生成一个对象，与父对象不同。这样父子对象之间就不会产生关联。
5. jQuery：$.extend([deep], target, object1, [objectN])  
JS:JSON.parse(JSON.stringify(oldObject))  
#### mui地图的坑
1. mui中的地图是原生控件，不受css的zindex控制。所以在层级上属于最高层，如果页面要滑动的话，就要考虑把地图放在子页面中，这样才不会覆盖父页面。如果要在地图上放按钮、搜索框等等，必须新建webview。
2. 地图覆盖问题。当打开的几个页面中都有地图时，这几个地图会出现覆盖问题。解决方案：
```
var sub = plus.webview.getWebviewById("nearbyCustomerList");
if(sub != null) {
    sub.setStyle({
        top: '100%'
    });
}
```
通过改变位置，把地图隐藏了，回到之前的地图时，再把地图显示出来。这里用map.hide()好像没效果。
```
var sub = plus.webview.getWebviewById("nearbyCustomerList");
if(sub != null) {
    sub.setStyle({
        top: '0px', 
	bottom: '0px',
    });
}
```
3. 地图花屏问题。从其他页面返回回来会出现花屏现象。
解决方案：在返回来之前，执行reload方法，使地图再加载一次。
4. H5地图在iOS与Android中的不一致性
      - ios中地图没有onclick事件，在本地调试的时候，可以触发onclick事件，打包之后，地图完全点不动。所以只能采用onstatuschanged事件
      - map.setCenter()函数存在异步问题.执行需要一定的时间。所以createMarker()必须要延迟执行。在Android中则没有这样的问题，ios也只能在打包之后才会出现这个问题。
      - ios中滑动地图在灵敏了，每滑动一次，地图会请求2次，会出现闪2次的现象。解决方式：采取flag锁
```
map.onstatuschanged=function(event){
	if(over){
		over=false;
		map.setCenter(event.center);
		poi=event.center;
		setTimeout(function(){
			createMarker(event.center);
			loadPlaceAround(event.center,"","","",true);
		},300);
	}
	isStopPull=false;
	poiKey="";
	page=1;							
	mui('#refreshContainer').pullRefresh().refresh(true);	//重置上拉加载
}
```
