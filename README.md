# JavaScript-An-Easier-Way
It contains the basic concepts of JavaScript in a more simplified and understandable way. 


### 1. Prototype
Every function has a property called prototype that is blank/empty by default that contains methods and properties. Any object created from that function will inherit those properties and methods as that of the prototype's.
e.g.- if x is a function having its own methods and properties, then if we create an object x1 from that function, x1 will inherit all the properties and methods that's defining x's prototype.

#### 1.1 Constructor 
Every function or expression is a constructor in JavaScript. 

```
example 1.1- 
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
example 1.2- 
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
example 1.3- 
var X = function() {
    
}

console.dir(x);

console.log(x.toString());
```

Here dir() is a function that gives out the details about any object passed through it. The toString method works for serializing the input and give out the output. Although the method isn't defined in the code, but the object can inherit the method toString() from the object containing it i.e. the master object. 

#### 1.3 Prototype Inheritance
JavaScript doesn't have any class definition so instead of creating classes,we create constructors. Base class constructors can be created that further, can create sub class constructors and using those sub class constructors we can create objects. In simple words, this is called prototype inheritance. Let's look at an example related to it.
```
example 1.4- 

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
example 1.5-
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
example 1.6-
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
example 1.7- 

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
### 2. call() | apply() | bind()

In JavaScript, call, Apply and Bind have the same functioning as they seem to do exactly the same thing. That's one of the reasons why there is confusion in their usage into the code and how it works. 
Usually there can be several objects in a code with their respective properties and methods. Let's say for an example, Object1 has its own properties and methods and Object2 has its properties and methods and so on. But in JavaScript, you can have different objects with their properties and still can apply a common method to various objects by call(), apply() or bind(). This is how we can apply common functions. 


#### 2.1 call()

 call() attaches a function to the object temporarily and then runs and gives the result back as if it's a part of that object itself. Let's see what I mean by this exapmle.

```
example 2.1-
    var obj = {num:2}; //Creating an object 'obj'

    var addToThis = function(a){  //creating a method 
        return this.num + a;
    }
```
***
```
Output:
```
In the above 'obj' is an object that has a property 'num' which is set to the value 2. Then there is another object that is a method that returns the sum of the parameter passed through it i.e. 'a' and the property 'num'. So, if num is set to 2 and we pass 3 through the method then it should return us the sum i.e. 5. But, the above code surely didn't give us any output because neither the object 'obj' is not associated to the method 'addToThis' nor the method has any property 'num', however, it's returning 'this.num'. 
So if we want to take an action on the property 'num' of the object 'obj' by the method 'addToThis', we can use call(). Let's go back to the above example and see how it works. 

```
example 2.2-
    var obj = {num:2}; //Creating an object 'obj'

    var addToThis = function(a){  //creating a method 
        return this.num + a;
    }

    addToThis.call(obj, 4); //Linking object to method by call()
```
In the above program, we used call() to link object 'obj' with the method 'addToThis' where we included first the function that we have to apply on an object then use call function and pass two parameters through it:
1. The first parameter would be the object we want to apply the method on. In this example it's 'obj';
2. The second parameter would be the method's very own parameter that is, the argument. Here, it's 'a' that has been given a value of '4' in the call function.

By this syntax for call() would be:
```
functionName.call(object, functionArgument);
```
So, let's see the output in console.

```
example 2.3-
    var obj = {num:2}; //Creating an object 'obj'

    var addToThis = function(a){  //creating a method 
        return this.num + a;
    }

    console.log(addToThis.call(obj, 4)); //Linking object to method by call()
```
***
```
Output:
6
```
What exactly happened here is, the function called for the object 'obj' and its property 'num' is accessed by the function by 'this.num' and the argument '4' was added to the value of num i.e. 2. Therefore, the sum of 4 and 2 would give you an output of 6.

If we have a function with **multiple arguments**, then we'll be using the call() similarly by taking the first parameter as the object that we want to apply the function on, and rest of the function arguments are passed as it is.

```
example 2.4-
    var obj = {num:2}; //Creating an object 'obj'

    var addToThis = function(a, b, c){  //function with multiple arguments 
        return this.num + a + b + c;
    }

    console.log(addToThis.call(obj, 4, 5, 10)); //Linking object to method by call()
```
***
```
Output: 21

```
Here, the value of 'num' is 2 and the arguments passed via the function i.e. 'a', 'b' and 'c' and their respective values are 4, 5 and 10 that returns a value of their sum wich is 21 in the console as the output.

This is what call() did. It attached the function 'addToThis()' to the object 'obj' temporarily and then ran and gave the result back as if it was a part of that object itself.

#### 2.2 apply()

apply() is used in the case where we combine the arguments of a function into a single array and then pass it as an argument along with the object that we're linking the function to. There can be multiple arguments in a function. For a small number of arguments, call() can be used. But, what if we want to pass a large number of arguments. Don't you think it would make the code look real clumsy? Let's see what I meant by this point.

```
example 2.5-
    var obj = {num:2}; //Creating an object 'obj'

    var addToThis = function(a, b, c, d, e, f, g, h, i, j){  //function with multiple arguments 
        return this.num + a + b + c + d + e + f + g + h + i + j;
    }

    console.log(addToThis.call(obj, 2, 3, 4, 5, 1, 6, 7, 8, 9, 10)); //Linking object to method by call()
```
***
```
Output: 57

```

By the previous topic, you must have got the logic of the program. But, just look at the 'console.log' line. So many arguments are being passed. What if all those large number of aguments can be combined into an array? This can make the code look a lesser clumsy too. Let's modify the code a bit. 

```
example 2.6-
    var obj = {num:2}; //Creating an object 'obj'

    var addToThis = function(a, b, c, d, e, f, g, h, i, j){  //function with multiple arguments 
        return this.num + a + b + c + d + e + f + g + h + i + j;
    }

    var arr = [2, 3, 4, 5, 1, 6, 7, 8, 9, 10];  //creating an array having all the arguments of function 'addToThis'

    console.log(addToThis.apply(obj, arr)); //Linking object to method by call()
```
***
```
Output: 57

```
This code contains apply() where the first parameter passed is the object itself and the second argument is the array 'arr' that is the combination of all the function argument i.e. 'a','b','c','d','e','f','g','h','i', and 'j'.
The apply() works similarly as call(). The only difference is that it takes all the function arguments in an array and the object as parameters.

#### 2.3 bind()

So, in bind(), we first pass the object itself that will return a function (bound) that can execute the code and hence, we execute this returned function by passing arguments directly through it. To understand bind(), let's take a reference of previous example we did.

```
example 2.7-
    var obj = {num:2}; //Creating an object 'obj'

    var addToThis = function(a, b, c){  //function with multiple arguments 
        return this.num + a + b + c;
    }

    var arr = [4, 5, 10]

    console.log(addToThis.bind(obj, arr)) //Linking object to method by call()
```
***
```
Output:

function bound() {[native code]}

```

In the above exapmle, we have used bind() instead of apply. And we get a function as an output instead of the sum of the values passed. This function has the ability to execute the code. This is how, bind() is different from both call() and apply() as it attaches the object 'obj' and then makes the method 'addToThis' a part of as a method of that object and gives us back the object in a different way. Let's understand how bind() works in this modified code.

```
example 2.8-
    var obj = {num:2}; //Creating an object 'obj'

    var addToThis = function(a, b, c){  //function with multiple arguments 
        return this.num + a + b + c;
    }

    var arr = [4, 5, 10];

    var bound = addToThis.bind(obj); //Binding with the function 'addToThis'

    bound(4, 5, 10); //Passing the argument itself as the function is already linked
    console.log(bound(4, 5, 10));
```
***
```
Output:
21

```

So we, passed the object 'obj' itself that returned a function 'bound'. Then we passed the argument directly through it. 

syntax for bind will be:

```
var bound = functionName.bind(object);
bound(argument1, argument2, argument3);
```  



