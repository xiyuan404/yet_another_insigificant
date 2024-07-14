# event model

```
                 | |  / \
-----------------| |--| |-----------------
| element1       | |  | |                |
|   -------------| |--| |-----------     |
|   |element2    \ /  | |          |     |
|   --------------------------------     |
|        W3C event model                 |
------------------------------------------
```



```js
addEventListener(
  type, 
  listener, 
  useCapture // default to false
)
```

```js
addEventListener(
  type, 
  listener, 
  options
)

otpions = {
	capture: false,
  once: false
  singal: 
}
```



- event delegation
- event bubbling
- event capturing
- custom events



__bubble__

- target is the element that triggered the event (e.g., the user clicked on)

- currentTarget is the element that the event listener is attached to.