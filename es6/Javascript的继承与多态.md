ES6之前的继承

``` 
function Person(name){
 this.name=name;
 this.className="person" 
}
Person.prototype.getClassName=function(){
 console.log(this.className)
}

function Man(){
}

Man.prototype=new Person();//1
//Man.prototype=new Person("Davin");//2
var man=new Man;
>man.getClassName()
>"person"
>man instanceof Person
>true

```

调用构造函数方式

``` 
function Person(name){
 this.name=name;
 this.className="person" 
}
Person.prototype.getName=function(){
 console.log(this.name)
}
function Man(name){
  Person.apply(this,arguments)
}
var man1=new Man("Davin");
var man2=new Man("Jack");
>man1.name
>"Davin"
>man2.name
>"Jack"
>man1.getName() //1 报错
>man1 instanceof Person
>true
```

组合继承


``` 
function Person(name){
 this.name=name||"default name"; //1
 this.className="person" 
}
Person.prototype.getName=function(){
 console.log(this.name)
}
function Man(name){
  Person.apply(this,arguments)
}
//继承原型
Man.prototype = new Person();
var man1=new Man("Davin");
> man1.name
>"Davin"
> man1.getName()
>"Davin"

```

分离组合继承


``` 
function Person(name){
 this.name=name; //1
 this.className="person" 
}
Person.prototype.getName=function(){
 console.log(this.name)
}
function Man(name){
  Person.apply(this,arguments)
}
//注意此处
Man.prototype = Object.create(Person.prototype);
var man1=new Man("Davin");
> man1.name
>"Davin"
> man1.getName()
>"Davin"
```
es6的继承方式


``` 

```
class Person{
  //static sCount=0 //1
  constructor(name){
     this.name=name; 
     this.sCount++;
  }
  //实例方法 //2
  getName(){
   console.log(this.name)
  }
  static sTest(){
    console.log("static method test")
  }
}

class Man extends Person{
  constructor(name){
    super(name)//3
    this.sex="male"
  }
}
var man=new Man("Davin")
man.getName()
//man.sTest()
Man.sTest()//4
输出结果：
Davin
static method test
``` 

es6和es5继承的区别

class A {
}

class B extends A {
}

B.__proto__ === A // true
B.prototype.__proto__ === A.prototype // true
```
子类B的__proto__属性指向父类A，子类B的prototype属性的__proto__属性指向父类A的prototype属性


多态


``` 
function Person(name,age){
 this.name=name
 this.age=age
}
Person.prototype.toString=function(){
 return "I am a Person, my name is "+ this.name
}
function Man(name,age){
  Person.apply(this,arguments)
}
Man.prototype = Object.create(Person.prototype);
Man.prototype.toString=function(){
  return "I am a Man, my name is"+this.name;
}
var person=new Person("Neo",19)
var man1=new Man("Davin",18)
var man2=new Man("Jack",19)
> person+""
> "I am a Person, my name is Neo"
> man1+""
> "I am a Man, my name isDavin"
> man1<man2 //期望比较年龄大小 1
> false
```


``` 
function Person(name,age){
 this.name=name
 this.age=age
}
Person.prototype.valueOf=function(){
 return this.age
}
function Man(name,age){
  Person.apply(this,arguments)
}

Man.prototype = Object.create(Person.prototype);
var person=new Person("Neo",19)
var man1=new Man("Davin",18)
var man2=new Man("Jack",19)
var man3=new Man("Joe",19)

>man1<19//1
>true
>person==19//2
>true
>man1<man2//3
>true
>man2==man3 //4 注意
>true
>person==man2//5
>false
```