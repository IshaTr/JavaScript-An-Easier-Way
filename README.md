# JavaScript-An-Easier-Way
It contains the basic concepts of JavaScript in a more simplified and understandable way. 


### 1. Prototype
Every function has a property called prototype that is blank/empty by default that contains methods and properties. Any object created from that function will inherit those properties and methods as that of the prototype's.
e.g.- if x is a function having its own methods and properties, then if we create an object x1 from that function, x1 will inherit all the properties and methods that's defining x's prototype.

#### 1.1 Constructor 
Every function or expression is a constructor in Javascript. 

```e.g.- 
    var X = function(j){
    this.i = 0; //setting default value to the property
    this.j = j; //defining another property

    this.getJ = function() { // Also a method can be added 
        return this.j;
    };
};

// creating instances of the constructor X
var x1 = new X(1);
var x2 = new X(21);

console.log(x1.getJ());
console.log(x2.getJ());
```
***
```
Output:
1
21
```

So, two objects x1 and x2 are created where first object value of property j i.e. x1 as 1 and similarly second object value of property j i.e. is 21. 
Therefore, x1 and x2 are two distinct objects of the constructor X where they inherit properties and methods of X.Also, x1 and x2 will be having their own method getJ(). 

This implies that if we create n objects of X each of them will be having their own distinct method that will create the issue of redundancy.

The solution of this problem is prototype.
```
e.g.- 
    var X = function(j){
    this.i = 0; //setting default value to the property
    this.j = j; //defining another property
};

x.prototype.getJ = function() {     //{ojectName}.prototype.{method}
    return this.j;
};

var x1 = new X(1);
var x2 = new X(21);

console.log(x1.getJ());
console.log(x2.getJ());
```
***
```
Output:
1
21
```

Since, by the definition that every function has a property called prototype that is empty by default, the method getJ() has been attached to that property. That means getJ() is automatically available to both the objects even though it's not present in the constructor X. But, it will behave exactly like the previous code, the only difference is now, x1 and x2 doesn't have their own methods, instead the objects just look up for the parent if there is any such method, finds it in the prototype and provides output accordingly.

#### 1.2 Master Object
Every object in Js is created from this master object i.e. when you run the code for knowing about the details where a object is created in a code, you'll get to see in the console another parent object, containing all the details, through which the child object has been nested. 

So, if we create an instance x1 of the constructor/function X and call for a method or property, x1 will look up to the constructor X for those methods and properties, if they're not there, then X will look up into the master object for the required methods and properties. If no such property or method is found, then the called method by x1 is said to be undefined in the output.
```
e.g.- 
var X = function() {
    
}

console.dir(x);

console.log(x.toString());
```

Here dir() is a function that gives out the details about any object passed through it. The toString method works for serializing the input and give out the output. Although the method isn't defined in the code, but the object can inherit the method toString() from the object containing it i.e. the master object. 

#### 1.3 Prototype Inheritance
Javascript doesn't have any class definition so instead of creating classes,we create constructors. Base class constructors can be created that further, can create sub class constructors and using those sub class constructors we can create objects. In simple words, this is called prototype inheritance. Let's look at an example related to it.
```
e.g.- 

var Job = function(){ //parent constructor Job
    this.pays = true;
}

job.prototype.print = function() { // Defining prototype for Job
    console.log(this.pays ? 'please hire me' : 'no thanks');
}

var TechJob = function(titlle, pays){ // Defining child constructor
    this.title = title;
    this.pays = pays;
}
```
***
In the above code, we've tried to inherit the properties of Job constructor into child constructor TechJob where Job has a property 'pays' that will be true by default and contains a prototype 'print' that says if the property 'pays' is true, it will return a string of 'please hire me' else it will return 'no thanks'.

Now when it comes to constructor TechJob, it's taking two properties as parameter i.e. 'title' and 'pays'. Obviously, 'pays' has to be inherited from the parent constructor Job. 

As of now, all we have done is just defined the constructors, properties and methods but not done the inheritance yet. Hence, the above code wouldn't work as we still have to apply inheritance to it. Let's see how it's done. 
```
var Job = function(){ 
    this.pays = true;
}

job.prototype.print = function() { 
    console.log(this.pays ? 'please hire me' : 'no thanks');
}

var TechJob = function(titlle, pays){
    
    Job.call(this); //inheriting the properties of the method Job except the prototype

    this.title = title;
    this.pays = pays;
}

TechJob.prototype = Object.create(Job.prototype); //inheriting from prototype of Job
TechJob.prototype.constructor = TechJob; //Set a constructor for TechJob

//Create Objects from the sub class i.e. TechJob

var softwarePosition = new TechJob('JavaScript Programmer', true);

console.log(softwarePosition.print()); // object will inherit from Base class Job
```
***
```
Output:
please hire me
```

Here, the call() works for inheriting all the methods and properties inside the base class into the sub class (by inside we clearly don't mean the prototype). 

So, to inherit the prototype from the base class i.e. Job, we clearly create an object for the TechJob but, the problem here is that, we get lost constructor of TechJob when we write 

'TechJob.prototype = Object.create(Job.prototype);'

So, we add it back by the third addition has been done i.e. setting the constructor for TechJob again that finally links the prototype object to the sub class.

#### 1.4 Prototype Overwriting
```
var Job = function(){ 
    this.pays = true;
}

job.prototype.print = function() { 
    console.log(this.pays ? 'please hire me' : 'no thanks');
}

var TechJob = function(titlle, pays){
    
    Job.call(this); //inheriting the properties of the method Job except the prototype

    this.title = title;
    this.pays = pays;
}

TechJob.prototype.print = function() { // Over writing the print method
    console.log(this.pays ? 'I love JavaScript' : 'Let us just stick for vb');
}

TechJob.prototype = Object.create(Job.prototype); //inheriting from prototype of Job
TechJob.prototype.constructor = TechJob; //Set a constructor for TechJob

//Create Objects from the sub class i.e. TechJob

var softwarePosition = new TechJob('JavaScript Programmer', true);

console.log(softwarePosition.print()); // object will inherit from Base class Job
```
***
```
Output:
I love JavaScript
```

In the above code the prototype method of base class Job has been over written by the prototype method of the sub class TehJob. And hence, the output of the object 'softwarePosition' calling for method 'print' i.e. 'softwarePosition.print()' will give the above output. Implies the constructor has been over written.

#### 1.5 Inserting user defined Method/Properties into Master object

As per prototype chaining, an sub class object in JavaScript when calls for any method, it will first look up for that function into it's own prototype. If the required function is absent, then the object will look up for it into the its parent object, and so on until the base object and then the Master object. What we can also do is add a method into the master object. Let's see how it's done.
```
e.g.- 

var  Job = function(){
    this.pay = true;
}

var TechJob = function(title, pay){
    Job.call(this);

    this.title = title;
    this.pay = pay;
}

TechJob.prototype = Object.create(Job.prototype);
TechJob.constructor = TechJob;

Object.prototype.print = function() { //Object stands for master Object
    console.log('I'm in the Master Object');
}

var softwarePosition = new TechJob();

console.log(softwarePosition.print());
```
***
```
Output:

I'm in the Master Object
```









    



