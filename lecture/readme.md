* why not define things like this?

```javascript
function School() {
  this.somefunc () {

  }
}

* for more see professional js for web developers pg 182-4
rather than 
function School() {}
School.prototype.somefunc = function() {

}
```

In the first case were creating an instance of the function for each object
The second case all objects share the same instance of the function
In ruby we do it the first way, but the second way is just more "correct" and memory efficient

defining functions within functions, why?

* differences in JS
  * functions can be defined within functions
  * why? we want to create a way to keep track of state, and share that state with functions who implement behavior.  encapsulate an idea of some work

```javascript
paragraphs = [{ type: "footnote",
                content: "When a programming language is created, it is given syntax and semantics."
                }, 
              { type: "body",
                content: "The syntax describes the form of the program, the semantics describe the function."
              } ]

function extractFootnotes(paragraphs) {
  var footnotes = [];
  var currentNote = 0;

  function replaceFootnote(fragment) {
    if (fragment.type == "footnote") {
      currentNote++;
      footnotes.push(fragment);
      fragment.number = currentNote;
      return fragment;
    } else {
      return fragment;
    }
  }

  paragraphs.forEach(replaceFootnote)

  return footnotes;
} 
extractFootnotes(paragraphs)
```

* call/apply

```javascript
function hello(thing) {  
  console.log("Hello " + thing);
}

* this:
hello("world") 

* desugars to:
hello.call(window, "world");
```

object example

```javascript
var person = {  name: "Brendan Eich",
                hello: function(thing) {
                    console.log(this + " says hello " + thing);
                }
              }

* this:
person.hello("world") 

* desugars to this:
person.hello.apply(person, []);
```

* private variables/functions
function School () {
  this.db = {}
}
School.prototype.grade(level) {
  return db[level] ? clone(db[level]).sort() : [];
}
var school = new School()
* the example above has a public interface where i can call obj.db
* the example below has a private "instance variable"
* closures (we create one when we define these functions and variables at the same time)
* were explicity returning an object to add clarity
```javascript
function School() {

  var db = {};

  function add(student, grade) {
    if(db[grade]) {
      db[grade].push(student);
    } else {
      db[grade] = [student];
    }
  }

  function grade(level) {
    return db[level] ? clone(db[level]).sort() : [];
  }

  return {
    add: add
  };

};
var school = new School()
```
*in this example were not returning an object but using this to make a function "public", we still have grade private and db private
*if we want access to db we have to define a method on the object that can "see" the db variable.

```javascript
function School() {

  var db = {};

  this.add = function (student, grade) {
    if(db[grade]) {
      db[grade].push(student);
    } else {
      db[grade] = [student];
    }
  }

  function grade(level) {
    return db[level] ? clone(db[level]).sort() : [];
  }
  // this.db = function() {
  //   return db;
  // }
};
var school = new School()
```
* arguments object, how does that work? 
