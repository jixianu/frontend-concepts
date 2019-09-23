# frontend-concepts
根据自己的理解描述前端每个重要的知识点
#this指向
this只管定义时的，跟调用无关是错误的。
this总是指向调用该函数的对象。除了箭头函数
是根据外层（函数或者全局）作用域（词法作用域）来决定this

1. 箭头函数不绑定this，箭头函数中的this相当于普通变量。
2. 箭头函数的this寻值行为与普通变量相同，在作用域中逐级寻找。
3. 箭头函数的this无法通过bind，call，apply来直接修改（可以间接修改）。
4. 改变作用域中this的指向可以改变箭头函数的this。
5. eg. function closure(){()=>{//code }}，在此例中，我们通过改变封包环境closure.bind(another)()，来改变箭头函数this的指向。


		var name = 'window'
		
		var person1 = {
		  name: 'person1',
		  show1: function () {
		    console.log(this.name)
		  },
		  show2: () => console.log(this.name),
		  show3: function () {
		    return function () {
		      console.log(this.name)
		    }
		  },
		  show4: function () {
		    return () => console.log(this.name)
		  }
		}
		var person2 = { name: 'person2' }
		
		// 普通函数调用使用call改变this指向
		person1.show1() // 'person1' 
		person1.show1.call(person2) // 'person2'
	
		// 箭头函数this指向上一个执行上下午这时指向window，箭头函数的this无法使用bind call apply改变
		person1.show2() // window
		person1.show2.call(person2) // window
	
		// function closure(){return function(){}}
		// 匿名函数function(){}的执行环境是全局性的,不包括属性方法调用
		person1.show3()() //window 非严格模式下this绑定window 严格模式下this是undefined
		person1.show3().call(person2) // person2 ==> function () {this.name}.call(person2)
		person1.show3.call(person2)() // window (function(){this.name})() 
		
		// function closure(){return ()=>{}}
		// 闭包正常调用时this往往指向外层作用域 当使用closure.bind(obj)()来改变闭包中this指向
		person1.show4()() //person1 person1.show4()运行时箭头函数中的this已经绑定到了person1 箭头函数绑定，this指向外层作用域
		person1.show4().call(person2) //person1 箭头函数的this无法使用bind call apply改变
		person1.show4.call(person2)() //person2 改变闭包中this指向
