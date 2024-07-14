## keyboard interaction

```js
// document.addEventListener('keypress', onKeyPress)
// deprecated for don't produce a character value are modifier keys such as Alt, Shift, Ctrl, or Meta.

document.addEventListener('keydown', onKeyDown)

function onKeyDown(event) {

  switch (event.key) {
    case 'w': goForward(); break;
    case 's': goBackward(); break;
    case 'a': goLeft(); break;
    case 'd': goRight(); break;
  }

}
```

```js
//const keyboard_cbs = [];
//const keyboard_cbs_docs = [];
 
const keybindings = []
const keybindingsDocs = []

TW.setKeyboardCallback = function(key,callback,doc,shared) {
	//keyboard_cbs[key] = callback
  // keyboard_cbs_docs[key] = doc
  keybindings[key] = callback
  keybindingsDocs = doc
}
 
document.addEventListener('keypress', onKeyPress)

function onKeyPress(event) {
	var key = event.key
 	if(keybindings[key] !== 'function') {
			return 
  }
	keybindings[key](event)
}
```

## mouse Coordinates

![calculateCoords](images/calculateCoords.png)

```js
document.addEventListener('click', onMouseClick)

function onMouseClick(event) {
	const mx = event.clientX
  const my = event.clientY
  
  const target = event.target
  if (target === c1) {
    var rect = target.getBoundingClientRect()
    const cx = mx - rect.left
    const cy = my - rect.top
  }
}
```

## Mouse Movement

```js
document.addEventListener('mousemove', onMouseMove);
document.addEventListener('mousedown', onMouseDown);
document.addEventListener('mouseup', onMouseUp);



var isMouseDown = false;
var prevMouse = {x: 0, y: 0}

function onMouseDown (event) {
    isMouseDown = true;
}
    
function onMouseUp (event) {
    isMouseDown = false;
}

function onMouseMove (event) {
  
}
```







steve move



```js
function move() {
  if (directionVec.x !== 0 || if (directionVec.z !== 0) {
    walk() // rotate arms and legs
    rotate()  // rotate body
    move(); // update position
    return true
  }
}
```





```js
function walk() {
  
  
  legL.rotation.x = rotationX;
  legR.rotation.x = -rotationX;
  armR.rotation.x = rotationX;
 	armL.rotation.x = -rotationX;
}
```



## scroll                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       





## parallax effect











