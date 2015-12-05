# Check global object and functions in JS

In order to know if  an object was loaded the easiest way is to ask  direct for it. The problem with  that,  it's if   the  object is  not   loaded,  you'll get an error.

```javascript
if(jQuery){
	//Do stuff with jquery
}else{
    //Don't work with jquery
    }
//Uncaught ReferenceError: jQuery is not defined    
```
To resolve this, the easiest  way is to see if the window object has a "property" called as  the object which  is being verifying.

```javascript
function checkGlobal(globalObject){
  if(window[globalObject]!== undefined ){
      return true;
  }else{
      return false;
  }
}
console.info(checkGlobal("jQuery"));
```

Checking  functions  is a little bit  tricky when you want to check   a function which has dependencies, for instance  if you want to use    a popover of Bootstrap.
In this case the dependency is  Jquery, so  you have to load it first in order to use  a  popover.
Obviously you can't check <b>$.fn.popover</b> directly because you don't know if Jquery has already been loaded. 
You can't   check   <b>var functionToChec = "$.fn.popover"</b>   as a varialbe either, because typeof will return  is a string.

```javascript
function checkFunction(isFunction){
	if(typeof (isFunction) === "function" ){
    	return true;
    }else{
    	return false;
    }
}
console.info(checkFunction("$.fn.popover")); //string
```

In order to check  the  function,  you can  check  Jquery and then  check  he popover function.


```javascript
if(checkGlobal("jQuery") && checkFunction($.fn.popover)){
        console.info("Jquery is loaded.","Bootstrap is loaded.");
    }else{
        console.info("Jquery is  not loaded.","Bootstrap is not loaded(possibly).");
    }
```

 This works  because  if jQuery is not loaded, the second part of the if statement  won't be executed.
 
 
If you want to check  just   the function without checking Jquery in this case, you can use  <b>eval()</b>

```javascript
function checkFunctionEval(isFunction){
        eval("var exist=false; try{ exist = typeof ("+isFunction+")==='function' ; }catch(e){ exist=false } ");
        return exist;
}
if(checkFunctionEval("$.fn.popover")){
	console.info("Bootstrap is loaded.");
}else{
	console.info("Bootstrap is not loaded.");
}
```

Now It doesn't matter if  Jquery is not loaded, checkFunctionEval can check if the function exists.



