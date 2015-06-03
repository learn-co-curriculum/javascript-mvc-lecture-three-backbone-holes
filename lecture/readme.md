* What is the arguments object?
  * an array like object

* What is an array like object?

Take this plain object as an example.
```javascript
var my_object = {
    '0': 'zero',
    '1': 'one',
    '2': 'two',
    '3': 'three',
    '4': 'four',
    length: 5
};


// the this object must have a length method, and []
Array.prototype.slice = function(begin, end) {
  var new_array = [];
  var i = begin;
  var real_end = end ? end : this.length
  for (i = 0; i < real_end; i++) {
    new_array.push(this[i]);
  }
  return new_array;
}

function test() {
  return [].slice.call(arguments, 0, 2)
}

test("a", "b", "c")

```




