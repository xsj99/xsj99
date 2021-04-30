### 1.高阶函数
- 高阶函数就是那种输入参数里面有一个或者多个函数，输出也是函数的函数在js中主要是利用闭包的形式实现, 高阶函数，其实本质就是函数操作另一个函数（说白了就是函数调用时再传函数）
- ####1.1最简单的高阶函数

		var demo = function () {
        	return function () { }
    	}
- 小实例


		// 使用闭包形式做私有化
		    // 使用闭包保持作用域
		    // 每次运行add函数后返回的都是不同的匿名函数，执行add函数时会从上往下执行代码，将num值初始化为0;
		    var add = function () {
		        var num = 0;
		        return function (a) {
		            return num = num + a;
		        }
		
		    }

- 模拟foreach实例

		function forEach(array, fun) {
		        for (var i = 0; i < array.length; i++) {
		            fun(array[i]);
		        }
		    }
		
		    // params1: 要遍历的数据
		    // params2: 使用匿名函数接收遍历出来的值
		    forEach(arr, function (val) {
		        // 可以对数据进行处理
		        console.log(val);
		    })

- 模拟过滤实例，注意以下代码里的foreach都是上面自定义的

		var arr = [2,3,4,5,6,7];//要过滤的数组
		    function filter(array, fun) {
		        var brr = [];
		        forEach(array, function (val) {
		            if (fun(val)) brr.push(val);//如果条件满足调用时传进来的条，就插入到新数组
		        })
		
		        return brr;
		    }
		
		    console.log(filter(arr,function(val){
		        return val >3;
		    }));

- #### 1.2规约函数
- 规约函数,通过一个函数调用另一个函数，将数组转换为单一的值
- 模拟Reduce -> 将数组转换为单一的值(相加功能),返回一个结果将初始值相加后，再次传递，加上下一个下标的值。

		var arr = [1, 2, 3, 4, 5];
		    /*
		     * 
		     * @params1: 函数作用是参数相加
		     * @params2: 要相加的值
		     * @params3: 数组
		     */
		    function Reduce(addFun, num, array) {
		        // console.log(array);
		
		        // 遍历
		        forEach(array, function (val) {
		            // console.log(val);
		            num = addFun(num, val)
		            // console.log(num);
		        })
		
		        return num;
		    }
		
		    function add1(num1, num2) {
		        return num1 + num2;
		    }


- #### 1.3映射函数
- 遍历数组，针对数组的每一个元素，调用指定的操作，然后将操作得出的值存储在另一个数组中，返回新的数组(就是一 一对应，相应的元素做相应的事)
- 数组每个值都进行四舍五入返回一个新数组

		
		function map(fun, array) {
		        var brr = [];
		        forEach(array, function (val) {
		            brr.push(fun(val));
		        })
		
		        return brr;
		    }
		
		    var roundArr = map(Math.ceil, [1.4, 2.7, 7.2, 9.5, 10.3]);
		    console.log(roundArr);

### 2.函数设计模式

- #### 2.1单例模式

- 理论： 单例(单体)模式是提供一种代码阻止为一个逻辑单元的手段。这个逻辑单元中的代码可以通过单一的变量进行访问
- 优点：            
            1. 可以划分命名空间                 
            2. 使用代码具有阅读性，维护性更好            
            3. 可以实例化(new),但是只能实例化一次  
- 实例


// 单例模式

	    /* function obj(){
	        this.name = '杰哥';
	    }
	
	    obj.prototype.getName = function(){
	        return this.name;
	    }
	
	    obj.prototype.setName = function(name){
	        return this.name = name;
	    }
	
	    var getRun = (function(){
	        var run;
	        return function(){
	            if(!run) run =  new obj();//判断对象是否被new了，new了就不再new了
	            return run;
	        }
	    })();
	
	    var a = getRun();//返回的是自调函数，再加个括号就可以调用返回的函数了
	    var b = getRun();
	    console.log(b.setName('彬彬'));
	    console.log(a.name); */   
单例模式运用，使得我们只能new 一个对象，而不会重复的去new它


- #### 2.2策略模式


- 1 定义： 定义一系列的算法，把它们一个一个封装起来，并且使它们可以互相替换
- 2 核心： 将算法的使用和算法的实现分离开来,一个基于策略模式的程序至少由两部分组成                
                第一部分：一组策略类，策略类封装了具体的算法，并负责计算过程                    
                第二部分：环境类Context,Context接收用户的请求，随后把请求委托给某一个策略类，                      
                要做到这点，Context中要维持对某个策略对象的引用
- 3. 实现：

 // 加权映射关系
    var levelMap = {
        A: 8,
        B: 6,
        C: 4,
        D: 2,
    }

    // 组策略，封装具体的算法，并负责计算过程
    var scoreLevel = {
        A:function(basicScore){
            return basicScore + levelMap['A'];
        },
        B:function(basicScore){
            return basicScore + levelMap['B'];
        },
        C:function(basicScore){
            return basicScore + levelMap['C'];
        },
        D:function(basicScore){
            return basicScore + levelMap['D'];
        },
    }

    // console.log(scoreLevel.A(66));

    // 调用，环境类，接收用户请求，随后委托给一个策略
    function getScore(level){
        return scoreLevel[level]? scoreLevel[level]:function(){return 0};
    }


    console.log(getScore('A')(66));
    console.log(getScore('B')(66));
    console.log(getScore('C')(66));
    console.log(getScore('D')(66));
    console.log(getScore('S')(66));
- 4 策略模式验证信息


		/* 映射关系,错误提示 */
		    var errorMsgs = {
		        default: '输入的数据不正确',
		        minLength: '输入数据的长度不足',
		        isNumber: '输入的不是纯数字',
		        isNull: '内容不能为空'
		    }
		
		    // 规则集
		    var rules = {
		        minLength: function (val, length, errorMsg) {
		            if (val.length < length) return errorMsg || errorMsgs['minLength'];
		        },
		        isNumber: function (val, errorMsg) {
		            if (!/\d+/.test(val)) return errorMsg || errorMsgs['isNumber'];
		        },
		        isNull: function (val, errorMsg) {
		            if (val === '') return errorMsg || errorMsgs['isNull']
		        }
		    }
		    // 检验器
		    // minLength('123456', 6, '你太短了')
		    function Validator() {
		        this.items = [];
		    }
		
		    Validator.prototype = {
		        // 添加校验规则
		        add: function (val, rule, errorMsg) {
		            var arg = [val];
		            // 检测规则中的长度是否有设置
		            if (rule.indexOf('minLength') != -1) {
		                var temp = rule.split(':');
		                // console.log(temp);
		                arg.push(temp[1]);  //6
		                rule = temp[0]; //minLength
		            }
		            arg.push(errorMsg); //自定提示信息压入数组
		            this.items.push(
		                function () {
		                    return rules[rule].apply(this, arg);     //进行校验
		                }
		            );
		        },
		
		        // 开始验证
		        start: function () {
		            // 压入新的错误后的规则列表
		            var ret = []
		            for (let i = 0; i < this.items.length; i++) {
		                // console.log(this.items[i]);
		                ret.push(this.items[i]());
		            }
		
		            // 返回错误信息的数组
		            return ret;
		        }
		    }
		
		    var validator = new Validator();
		    // @params1 获取用户输入框的值
		    validator.add('1234', 'minLength:6', '最少要输入大于6位数哦');
		    validator.add('asdad', 'isNumber');
		    validator.add('', 'isNull', '沙雕，你要写点东西');
		    console.log(validator.start());