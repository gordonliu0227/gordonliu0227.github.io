## Javescript notes

This blog is used for record some js related notes.

### JS

#### when the js not working well in a form

Sometimes, Jquery cannot get the value from a form, need do more research about the reason, 
even that input has an id. usually, we can use 
```js
var str = document.getElementById("test").value;
```
BUT, sometimes it will has an error shows cannot get value from null
use
```js 
$('form').serializeArray() 
```
to check the value from the form, and make sure the name is correct and then use

```js
var inpuValue = document.getElementById('formId').elements['name'].value;

```
to get the value.


### form steps
http://www.jquery-steps.com/Examples


### make the html content cant click
```js
  document.getElementById("trial").style.pointerEvents = "none";

```

