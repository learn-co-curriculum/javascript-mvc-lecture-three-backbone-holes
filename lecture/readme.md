* functions in JS are not that different, except they are
* problem:functions do everything in JS

* functions can take functions as arguments
* anonymous functions are just like blocks
* similar to blocks, functions can see outside variables

build add, foreach, map, sum, reduce/inject

```javascript
function add(a,b) {
  return a + b;
}

function forEach(array, action) {
  for (var i = 0; i < array.length; i++) {
    action(array[i]);
  }
}

* functions can be defined within functions
* explicit return
function map(array, func) {
  var result = [];
  forEach(array, function (element) {
    result.push(func(element));
  });
  return result;
}

* functions defined inside of functions create a closure
* they can see the scope of the parent function
function reduce(combine, base, array) {
  forEach(array, function (element) {
    base = combine(base, element);
  });
  return base;
}

* functions defined inside of functions create a closure
* they can see the scope of the parent function
function reduce2(combine, base, array) {
  forEach(array, combiner);
  function combiner(element) {
    base = combine(base, element);
  }
  return base;
}


function sum(numbers) {
  return reduce(add, 0, numbers);
}
```

* things to point out
* - we're passing an anonymous function as an argument to a method (this is very similar to passing a block)
* - we "yield" to our anonymous function by calling/invoking the method
* - in reduce how can the anonymous function in the call to each "see" the base variable


* why is scope so confusing in javascript?
* In Ruby, the native syntax/constructs of the language impose scope upon us.  We don't really have to think about scope, it works naturally to our advantage.  It's rare variables stick around when we don't want them to.

* In javascript, there's almost no scope by default.  By default variables want to be in the global scope.  By default functions can see variables outside of their lexical scope. We have to think a lot about what can see what, whereas the rules in ruby are simple.

* function scope
* - function environments
*  - scope layers

```javascript
var aVar = "a"
function a (pass) {
     var bVar = "b"
     function b (pass) {
          var cVar = "c"
          function c (pass) {
               console.log(cVar)
               console.log(aVar)
               console.log(bVar)
               console.log(pass)
          }
          c(pass)
     }
     b(pass)
}
a("passing this down")
```

*  - shadowing
*  - operator precedence

```javascript
 function printVariable() { 
   console.log("inside printVariable, the variable holds '" +        variable + "'.");
 }
 function test() {  
   var variable = "local"; 
   var variable = "top-level";  
   console.log("inside test, the variable holds '" + variable + "'.");  
   printVariable();
 }
 test();
```

 * How about objects
 * a million ways to create objects
 * an object is just a collections of properties and values
 * values can be functions
 * we could just hang a function on an object and suddenly were doing oo
 * oo is just data and behavoir
 * essentially we want a factory that creates like things

 ```javascript
 function School () {
   this.db = {}
 }
 School.prototype.add = function (student, grade) {
   if(this.db[grade]) {
     this.db[grade].push(student);
   } else {
     this.db[grade] = [student];
   }
 }
   
 School.prototype.grade = function (level) {
   return this.db[level] ? this.db[level].sort() : [];
 }

 var school = new School()
```

* context vs scope

* context
* why is this maddening to a rubyist? context never changes.  
* methods belong to objects and there's no reason for them ever to disattach themselves.  
* BUT most of the time things work fine.
  * this works normally for constructor
  * this works normally for method calls
  * calling functions within methods screws you

```javascript
function Jane () {
  this.myName = 'Jane';    
  this.friends = ['Tarzan','Cheeta'];
}
Jane.prototype.logHiToFriends = function(){
  debugger
  this.friends.forEach(function(friend){
      // `this` is undefined here
      // show this function logging the value of "this" 
      debugger           
      console.log(this.myName+' says hi to '+friend);
  });
}
var jane = new Jane()
jane.logHiToFriends()
```

- grabbing functions off objects screws you

```javascript
var person = {
    name: 'Jane',    
    logMyName: function(){
        console.log(this.name)
    }
}
person.logMyName(); // works
var logMyName = person.logMyName;
logMyName();  //doesn't work
```

how to fix
WAY1

```javascript
function Jane () {
  this.myName = 'Jane';    
  this.friends = ['Tarzan','Cheeta'];
}
Jane.prototype.logHiToFriends = function(){
  // or var self = this;
  this.friends.forEach(function(friend){           
      console.log(this.myName+' says hi to '+friend);
  }, this);
}
var jane = new Jane();
jane.logHiToFriends()
```

WAY2

```javascript
function Jane () {
  this.myName = 'Jane';    
  this.friends = ['Tarzan','Cheeta'];
}
Jane.prototype.logHiToFriends = function(){
  var that = this;
  // or var self = this;
  this.friends.forEach(function(friend){           
      console.log(that.myName+' says hi to '+friend);
  });
}
var jane = new Jane();
jane.logHiToFriends()
```

```javascript
for (var prop in obj) { }
```  