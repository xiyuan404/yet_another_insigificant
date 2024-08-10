
# css workflow
- [x] spacing
- [x] sizing
- [x] alignment
- [x] color


## white-space bahaviour

css white space property

|            | line breaks char | extra while space | Text wrapping |
| :--------- | :--------------- | :-------------- | :------------ |
| `normal`   | Collapse         | Collapse        | Wrap          |
| `pre`      | Preserve         | Preserve        | No wrap       |
| `nowrap`   | Collapse         | Collapse        | No wrap       |
| `pre-wrap` | Preserve         | Preserve        | Wrap          |
| `pre-line` | Preserve         | Collapse        | Wrap          |



### spacing

```css 
  item:not(:last-child) {
    margin-right: .06rem;
  }
```

<h2  style="font-size: 22px; display: inline-block; padding-left: 10px; border-left: 5px solid rgb(145, 109, 213); font-weight:bold;">viewports</h2>

<h3 style="text-align:center;  font-size: 20px;font-weight: bold;"><span style="border-bottom: 2px solid rgb(145, 109,213);">resize content to the viewport</span></h3>

user are used to scroll website vertically on both desktop and mobile deviecs - but not horizontally!

so, if the user is forced to scroll horizontally, or zoom out, to see the whole web page, which results in a poor user experience



<h3 style="text-align:center;  font-size: 20px;font-weight: bold;"><span style="border-bottom: 2px solid rgb(145, 109,213);">a tale of  two viewport</span></h3>



The **layout viewport** is the viewport into which the browser draws a web page.

 [visual viewport](https://developer.mozilla.org/en-US/docs/Glossary/Visual_Viewport) represents what is currently visible on the user's display device.

 pinching(捏合) gesture can usually be used to zoom in and out on a site's contents



> Imagine the layout viewport as being a large image which does not change size or shape. Now image you have a smaller frame through which you look at the large image. The small frame is surrounded by opaque material which obscures your view of all but a portion of the large image. The portion of the large image that you can see through the frame is the visual viewport. You can back away from the large image while holding your frame (zoom out) to see the entire image at once, or you can move closer (zoom in) to see only a portion. You can also change the orientation of the frame, but the size and shape of the large image (layout viewport) never changes.
>
> George Cummins explains the basic concept best [here](http://stackoverflow.com/questions/6333927/difference-between-visual-viewport-and-layout-viewport) at Stack Overflow:



## viewport , window, screen

![comp](images/viewport_term-comp.png)

To get the size of viewport

```js
const size = {
  width: window.innerWidth,
  height: window.innerHeight
}
```





### device pixels vs css pixels

`pixel` : represented by a square on computer monitor screen

`device pixels` : could be read from `screen.width` and `screent.height`

`pixel ratio` `device pixels for one pixel unit on the software part`

```js
window.devicePixelRatio //2
```

## resize 
```js

document.documentElement.style.fontSize = document.documentElement.clientWidth / 375 * 100 + 'px'
window.addEventListener('resize',() => {
  document.documentElement.style.fontSize = document.documentElement.clientWidth / 375 * 100 + 'px'
})

```

## Full screen

fit the whole scrren to get immersive experience

- [`Fullscreen API`](https://developer.mozilla.org/en-US/docs/Web/API/Fullscreen_API)
- [`Element.requestFullscreen()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/requestFullscreen)
- [`Document.exitFullscreen()`](https://developer.mozilla.org/en-US/docs/Web/API/Document/exitFullscreen)
- [`Document.fullscreenElement`](https://developer.mozilla.org/en-US/docs/Web/API/Document/fullscreenElement)
- [`:fullscreen`](https://developer.mozilla.org/en-US/docs/Web/CSS/:fullscreen), [`::backdrop`](https://developer.mozilla.org/en-US/docs/Web/CSS/::backdrop)
- [`allowfullscreen`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe#allowfullscreen)



```js
window.addEventListener('dblclick', () => {
  const fullscreenEl =
    document.fullscreenElement || document.webkitFullscreenElement

  if (!fullscreenEl) {
    canvas.requestFullscreen && canvas.requestFullscreen()
    canvas.webkitRequestFullscreen && canvas.webkitRequestFullscreen()
  } else {
    document.exitFullscreen && document.exitFullscreen()
    document.webkitExitFullscreen && document.webkitExitFullscreen()
  }
})

```





```css
.el:fullscreen {
  background-color: 'teal'
}
```





`zoom level` : at zoom level 1, one css pixel is exacty equal to one device pixel



__Screen Size__

measured in device pixel. they are  a feature of the monitor, and not of the browser

![screen_size](images/screen_size.jpg)



__Window size__



### __scrolling offset__

![scrolling_offset](images/scrolling_offset.jpg)

 find out how much the user has scrolled, `vertically, window.pageYOffset ` or `horziontally, window.pageXOffset`





> Suppose you have a liquid layout and one of your sidebars has `width: 10%`. Now the sidebar neatly grows and shrinks as you resize the browser window. But exactly how does that work?
>
>Technically, what happens is that the sidebar gets 10% of the width of its parent. Let’s say that’s the `<body>` (and that you haven’t given it a `width`). So the question becomes which width the `<body>` has.
>
>Normally, all block-level elements take 100% of the width of their parent (there are exceptions, but let’s ignore them for now). So the `<body>` is as wide as its parent, the `<html>` element, which is the uppermost containing block of your site.
>
>And how wide is the `<html>` element? Why, it’s as wide as the browser window. That’s why your sidebar with `width: 10%` will span 10% of the entire browser window. All web developers intuitively know and use this fact.





<h3 style="text-align:center;  font-size: 20px;font-weight: bold;"><span style="border-bottom: 2px solid rgb(145, 109,213);">all about height </span></h3>

![alt text](images/*height.png)

- offsetHeight 
  `VISIBLE content & padding & scrollbar `

- clientHeight:
  `VISIBLE content & padding` only

- scollHeihgt
  `ENTIRE content & padding (visible or not)`

- Element.getBoundingClientRect()


## 
