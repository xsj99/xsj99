### 1.oop的封装
- 什么是面向对象可以这样的理解，面向对象就是把所有事物都进行了对象化。我们只需要对这个对象进行操作，就可以实现需要的功能。
- 面向对象的三大特性
- 1、封装：可以认为把某些属性与方法放到一起才会变成一个对象。
- 2、继承：所有事物都是有祖辈的，只要把祖辈的东西继承下来再生成新的东西才事物才会有发展。
- 3、多态：对象可以在不同的情况下实现不一样的功能。
- 最简单的对象封装其实就是定义一个对象类型的数据 
- 通过定义一个函数来生成对象


		function Dog(dog_name, dog_color) {
	        return {
	            name: dog_name,
	            color: dog_color
	        }
	    }
		var dog3 = Dog('来福', '黄色');

- 使用构造函数来生成对象

		function getDog(name,color){
        	this.name = name;
        	this.color = color;
    	}
		var dog5 = new getDog('小白','黑色');   //使用构造方法的对象必须要new
		console.log(dog3.constructor == man1.constructor );
- 对象的constructor指向的是定义对象的构造函数，通过  new 函数名    来实例化对象的函数叫构造函数。任何的函数都可以作为构造函数存在。之所以有构造函数与普通函数之分，主要从功能上进行区别的，构造函数的主要 功能为 初始化对象，特点是和new 一起使用。new就是在创建对象，从无到有，构造函数就是在为初始化的对象添加属性和方法。构造函数定义时首字母大写 对new理解：new 申请内存, 创建对象,当调用new时，后台会隐式执行new Object()创建对象。所以，通过new创建的字符串、数字是引用类型，而是非值类型。
    

### 2.对象的封装
- 使用构造函数来生成对象

		//构造函数
		 function GetDog(name, color) {
        	this.name = name;
        	this.color = color;
        	this.type = '犬科动物';
        	this.eat = function () {
            	alert('爱吃大骨头');
        	}
    	}
		//对象实例化
		var dog1 = new GetDog('大黄', '黄色');
- 在例子中构造函数里面的固定的属性或者方法，通过new来生成对象的话，那么相同的地方也进行了new。这样的话在内存中会生成相同的东西，效率相对会降低。使用构造方法的对象必须要new


		function GetDog2(name,color){
	        this.name = name;
	        this.color = color;
	    }
	
	    GetDog2.prototype.type = '犬科';
	    GetDog2.prototype.eat = function(){
	        alert('爱吃粑粑！！');
	    }
所有new出来的对象通过prototype的方法，他们都指向同一个内存地址，就是说他们是共用一个构造函数的
- `GetDog2.prototype.isPrototypeOf(dog2)` 用于判断，某个prototype对象和某个实列之间的关系
- `dog3.hasOwnProperty('name')` 是否是一个本地属性，如果是为true
- `dog3.hasOwnProperty('type')`  是否是一个本地属性，不是为false
-  `alert('name' in dog3);` in 运算符 , 可以用于判断，某个实列是否包含某个属性，不管是不是本地属性              
若是则返回true，若不是，则返回false
- 使用for in形式遍历对象里的所有属性和它的属性值

		for(var key in dog3){
	        console.log('dog3['+key+']=>'+ dog3[key]);  //这里有点像php数组的方法
	    }

### 3.常规继承



		// 定义一个父级的构造函数
    function Movie(){
        this.category = '电影'
    }


    // 定义一个子级的构造函数
    function Kongfu(name,director){

        // arguments所有形参的集合
        // 使用call将当前this指向，指向为Movie
        Movie.call(this,arguments);

        this.name = name;
        this.director = director;
        this.type = '功夫片';
        this.action = function(){
            alert('降龙十八掌');
        }
    }

    var kongfu = new Kongfu('功夫之王','强强');
    console.log(kongfu.name);
    console.log(kongfu.type);
    console.log(kongfu.category);
- 使用call来进行常规继承，使用了call之后this指向会发生变化，this不再指向当前对象，而是指向被继承的对象

### 4.构造函数继承父类方法
- 1.直接使用call或者apply来继承
- 2.通过prototype来进行继承


		function Movie() {
	         this.category = '电影'
	    }
	
		function loveAction(name, director, man, girl, action) {
	        this.name = name;
	        this.director = director;
	        this.man = man;
	        this.girl = girl;
	        this.action = action;
	        this.type = '动作片';
	    }
		loveAction.prototype = new Movie(); //我们将loveAction的prototype对象指向一个Movie实列
		loveAction.prototype.constructor = loveAction;//任何一个prototype对象都有constructor属性，指向它的构造函数因此，在运行loveAction.prototype = new Movie()这一行之后，mv2.constructor也指向了Movie!所以我们需要手动纠正，将mv2.prototype对象的constructor值改为loveAction
		var mv2 = new loveAction();
    	console.log(mv2.category);
    	console.log(mv2.constructor); */
- 3.直接继承prototype           
这种方法的好处显示是很方便，而且不需要new Movie() 对象，所以很省内存，也就是说效率会比较高。但是也有非常不好的地方就是父级的构造函数会被更改为子级


		function Movie() {
        	
    	}
		Movie.prototype.category = '电影';
		function loveAction(name, director, man, girl, action) {
        	this.name = name;
        	this.director = director;
        	this.man = man;
        	this.girl = girl;
        	this.action = action;
        	this.type = '动作片';
    	}
		loveAction.prototype = Movie.prototype;
		loveAction.prototype.constructor = loveAction;
		var mv2 = new loveAction();
     	console.log(mv2.category);
     	console.log(mv2.constructor);
     	console.log(Movie.prototype.constructor); */
- 4.把方法2和3合并                 
F是空对象，所以几乎不占内存，这时，修改子级的prototype对象，就不会影响到Movie的prototype对象.

		function Movie() {
        	
    	}
		Movie.prototype.category = '电影';
		function loveAction(name, director, man, girl, action) {
        	this.name = name;
        	this.director = director;
        	this.man = man;
        	this.girl = girl;
        	this.action = action;
        	this.type = '动作片';
    	}
		// 创建一个用于继承的函数
    	function extend(Parent,Child) {
        	var F = function(){};
        	F.prototype = Parent.prototype;
        	Child.prototype = new F();
        	Child.prototype.constructor = Child;
        	Child.uber = Parent.prototype;  //上级，作为备用
    	}
		extend(Movie,loveAction);
		var mv2 = new loveAction();
- 5. 对象拷贝


			function Movie() {
	        	
	    	}
			Movie.prototype.category = '电影';
			function extend2(Parent,Child){
        		var p = Parent.prototype;
        		var c = Child.prototype;
        		for(var i in p){
            	// console.log(i);
            		c[i] = p[i];//相当于把这个对象复制了一份给新的对象
        		}
    		}
			extend2(Movie,XiJu);
    		var xj2 = new XiJu();


### 5. 非构造函数继承 
- 浅拷贝方法：jq早期使用的对象继承方法


			// 创建一个父级对象
	    var Car = {
	        category:'汽车',
	        banner:'五菱宏光',
	        color:'银白色',
	        part:['方向盘','车胎','车门','刹车片'],
	        config:{
	            gps:1,
	            gang:32,
	            ls:[1,2,3,5,5,7,8,9,0]
	        }
	    }
		function extend(Parent){
		        var c = {};
		        for(var key in Parent){
		            c[key] = Parent[key];
		        }
		        return c;
		    }
		var my_car = extend(Car,{});
- 深拷贝方法


			// 创建一个父级对象
	    var Car = {
	        category:'汽车',
	        banner:'五菱宏光',
	        color:'银白色',
	        part:['方向盘','车胎','车门','刹车片'],
	        config:{
	            gps:1,
	            gang:32,
	            ls:[1,2,3,5,5,7,8,9,0]
	        }
	    }
			// 深拷贝方法：现在的jq使用的对象继承方法,这是可以无限扒下的，直到把里面的对象或数组全都扒光为止，里面进行了递归的运用，不断的判断，不断的赋值，以达到到所有的属性都是传递赋值而不是引用赋值
	    function copyExtend(Parent,Child){
	        var c = Child || {};
	        for(var key in Parent){
	            // console.log(key);
	            // c[key] = Parent[key];
	            // console.log(typeof Parent[key]);
	            if(typeof Parent[key] == 'object'){
	                // 判断如果当前属性值取得为数组或对象，则会进入该区间
	                // 通过判断构造函数等于数组时，则赋值一个空数组，否则赋值一个空对象
	                // console.log(Parent[key].constructor);
	                // console.log(key);
	                c[key] = (Parent[key].constructor === Array)?[]:{};
	                // part config  ,  part = [] ,config = {}
	                copyExtend(Parent[key],c[key]);
	            }else{
	                c[key] = Parent[key];
	            }
	        }
	
	        return c;
	    }
		
