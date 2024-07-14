



## Double Buffering 



The graphics system is constantly erasing and redrawing the scene. The monitor is constantly refreshing the screen. (Most modern monitors refresh between 50-100 times per second, so every 10 to 20 milliseconds.) If the screen is refreshed when the new image is only partly drawn (this includes filling areas in the framebuffer), you'll see, briefly, that partial image. That's what causes the flicker.





"front" and "back" buffer are conventional: the front buffer is the one that is "on stage" and the back buffer is the one that is being prepared for the next scene.





- invoke the `requestAnimationFrame()` function, built into modern browsers





## animation

- reset the animation to the initial state
- advance the animation by one step
- start the animation going (continuously looping)
- stop the animation (freezing it at the current state).



Bouncing Ball with Controls

Spinning Cube with Controls

```js
var guiParams = {
  vx: 0.01,
  vy: 0.02,
  vz: 0.04
}


var animationState


//  set animationState to its initial State

function resetAnimationState() {
  animationState = {
    time: 0,
    rx: 0,
    ry: 0,
    rz: 0
  }
}

resetAnimationState()

function updateState() {
  	animationState.time += 1;
   // increase the total rotations by the user-specified velocity
    animationState.rx += guiParams.vx;
    animationState.ry += guiParams.vy;
    animationState.rz += guiParams.vz;
   // transfer the state info to the cube
    cube.rotation.x = animationState.rx;
    cube.rotation.y = animationState.ry;
    cube.rotation.z = animationState.rz;
}
```





