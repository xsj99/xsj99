### 1.面向对象补充
- 删除对象属性使用delete 或者直接赋值undefined  用法          
  `delete dog1.name;`               
 ` dog1.name = undefined;`                 
区别是使用delete之后打印对象是没有对应的属性名的，而直接赋值会显示 为 `dog1.name = undefined;`
- 枚举（相当于循环遍历）


		for (var key in dog1) {
		        console.log(dog1[key]);
		    }

- 设置不可枚举

		
		Object.defineProperty(dog1,'name',{
		        enumerable:false
		    })
不可枚举,和删除不同。只是在进行for..in 操作时，会过滤指定的属性名
- 设置对象的读写
  
		    var rabbit = {
		        name:'蓝兔',
		        color:'蓝色',
		        age:0,
		        get dage(){//读取，只读
		            return this.age;
		        },
		        set dage(val){//写入，只写
		            // 对数据进行过滤操作
		            val = val -5;
		            this.age = val
		        }
		    }


### 2. 面向对象的多态
- 定义： 多态是同一个行为具有多个不同的表现形式或者形态的能力。多态实际上是不同对象作用于同一个操作产生不同的效果。多态的思想实际上是把'想做什么'和'谁去做'分开。
- 实现基础：重载是多态的基础，js函数不支持多态，因为js函数是无态的。支持任意长度，类型的参数列表。js不支持重载，不会报错，但是后面的同名函数会把前面的函数覆盖。如果同时定义多个同名函数，则最后一个函数生效。
- 函数重载的应用场景：
            函数功能相同，仅仅是参数不同的情况下进行重载，可以减少开发的重复命名问题。
            重载的本质就是将多个功能相近的函数合并为同一个函数


- js重载

	js实现重载，通过判断参数的个数来实现重载

		function Person(){
	        this.demo = function (){
	            if(arguments.length == 1){
	                this.show1(arguments[0]);
	            }else if(arguments.length == 2){
	                this.show2(arguments[0],arguments[1]);
	            }else if(arguments.length == 3){
	                this.show3(arguments[0],arguments[1],arguments[2]);
	            }
	        }
	
	        this.show1 = function(a){
	            alert('我被调用啦!'+a);
	        }
	
	        this.show2 = function(a,b){
	            alert('我被调用啦!'+a +'----'+b);
	        }
	
	        this.show3 = function(a,b,c){
	            alert('我被调用啦!三个参数');
	        }
	
	    }

- js多态实现方法

	为具体类型的原型定义上具体的方法就可以啦，例如：
	function Duck(){}
    Duck.prototype.say = function(){
        console.log('嘎嘎嘎');
    }

    function Chicken(){}
    Chicken.prototype.say = function(){
        console.log('叽叽叽');
    }

    function Goose(){}
    Goose.prototype.say = function(){
        console.log('呃呃呃');
    }

    function Animal(am){
        am.say();
    }


### 3.面向对象实例

- html


		<ul>
	        <li data-value="1">冰冰：</li>
	        
	        <li data-value="2">杰哥：</li>
	    </ul>
点击文本之后动态添加输入框和两个按钮，点击保存显示对应的值，点击取消返回之前的值

首先遍历页面元素为每一个元素都生成一个编辑对象

	
	<script>
	        $(function(){
	            // 遍历页面元素
	            $('ul li').each(function(){
	                // 为每一个元素都生成一个编辑对象
	                new PlaceFieldEditor('请输入尺寸...',$(this));
	            })
	        })
	    </script>


然后定义对应的PlaceFieldEditor方法               
创建一个抽象类(函数)    搭建骨架，相当于指定了对应的你有的功能，但是怎么实现需要自己来定义                    
先定义好类生成的对象要做些什么？就想做模型的骨架一样                   
	
	function PlaceFieldEditor(value, $parentEle) {
	    this.value = value;     //当前对象输入的值
	
	    this.$parentEle = $parentEle;   //父级的DOM元素
	
	    this.initValue = value;     //初始化后的默认值
	
	    /* 
	        接下来实例化对象马上用到的方法
	    */
	    this.initElements();     //页面初始化元素的方法
	
	    this.initEvents();      //初始化页面事件的方法
	}


再然后把需要具体执行的动作放在原型里面用对象进行接收存储，开始进行拼接来形成完整的个体，把具体的内容行为添加进去



		PlaceFieldEditor.prototype = {
    constructor: PlaceFieldEditor,   //定义构造函数来自哪里

    /* 
        页面初始化元素的方法
    */
    initElements: function () {
        this.$spanEle = $('<span />');      //使用jq创建一个span元素
        this.$spanEle.text(this.getVal());     //往span里面加入当前的值，因为$spanEle是使用jq创建的对象，所以可以直接使用jq的方法

        this.$textEle = $('<input type="text">');   //使用jq创建一个input普通文件输入框
        this.$textEle.val(this.getVal());              //往input里面加入值

        this.$btnBox = $('<div style="display:inline" />');         //使用jq创建一个div对象
        this.$saveBtn = $('<input type="button" value="保存">');    //使用jq创建一个DIV下面的保存按钮
        this.$cancelBtn = $('<input type="button" value="取消">');    //使用jq创建一个DIV下面的取消按钮

        this.$btnBox.append(this.$saveBtn).append(this.$cancelBtn);

        this.$parentEle.append(this.$spanEle).append(this.$textEle).append(this.$btnBox);   //组装好的所有页面元素添加到li中

        // 进入只读模式
        this.readModel();
    },

    /* 
        初始化页面事件的方法
    */
    initEvents: function () { 
        var that = this;    //使用另一个变量进行保存对象，防止用于域污染

        // 绑定文本区域的点击事件
        this.$spanEle.on('click',function(){
            // 转到编辑模式
            that.convertToEditable();
        });

        // 绑定保存的事件
        this.$saveBtn.on('click',function(){
            // 执行保存的方法
            that.save();
        })

        // 绑定取消事件
        this.$cancelBtn.on('click',function(){
            // 执行取消方法
            that.cancel();
        })
    },

    // 只读模式
    readModel:function(){
        this.$spanEle.show();       //显示文本元素

        this.$textEle.hide();       //隐藏输入框

        this.$btnBox.hide();        //隐藏按钮元素
    },

    // 编辑模式
    convertToEditable:function(){
        this.$spanEle.hide();       //隐藏文本元素

        this.$textEle.show();       //显示输入框

        this.$textEle.focus();      //自动获取焦点，提升用户体验

        this.$btnBox.show();        //显示按钮元素

        // 如果当前的值等于初始值，我们就进行清空
        if(this.getVal() == this.initValue) this.$textEle.val('');
    },
    // 保存的方法
    save:function(){
        // this.value = this.$textEle.val();
        this.setVal(this.$textEle.val());

        this.$spanEle.html(this.getVal());

        // 保存后进入只读模式
        this.readModel();
    },
    // 取消的方法
    cancel:function(){
        // 获取之前的值，写入到input框中
        this.$textEle.val(this.getVal());

        this.readModel();   //取消后进入只读模式
    },

    // 定义一个设置当前值得方法
    setVal:function(value){
        // 设置数据之前，在这里可以做过滤处理

        this.value = value;
    },
    // 获取当前输入值得方法
    getVal:function(){
        // 获取数据之前可以做一些处理

        return this.value;
    }
}










