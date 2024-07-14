> - [JavaScript typed arrays](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Typed_arrays) @ MDN > References > JavaScript > Guide > JavaScript typed arrays
> - [TypedArray or DataView](https://hacks.mozilla.org/2017/01/typedarray-or-dataview-understanding-byte-order/): Understanding byte order @ MDN Hacks
> - [Typed arrays - Binary data in the browser](https://web.dev/articles/webgl-typed-arrays): Binary Data in the Browser @ web.dev > Home > Articles


## File
File常用属性有:
`File.name`： 只读, 返回当前File对象所用引用文件名
`File.type`: 只读,返回文件的多用途互联网邮件扩展类型(MIME type)
`File.size`: 只读,返回当前File对象的大小

`FileList`: `<input type="file">`元素有个files属性, `dragenter, dragover,drop, dropleave` 事件中也有个files属性,用来存储用户所选择的文件, 当用户通过(点击或拖放）选择文件后，便可以获得选择的文件对应的`FileList`对象

```js
const fileList = document.querySelector("input[type='file']").files
console.log(fileList)
```

```js
 document.getElementById('upload-container').addEventListener('drop', (e)=>{
   const file =  e.dataTransfer.files[0]
   console.log(file)
})
```


## Blob

Blob是`Binary Large object`的缩写，

Blob常用属性和方法

```js
/**
* @param {Array} blobParts/array 一个可选的由ArrayBuffer, ArrayBufferView, Blob, DOMString 等对象构成的数组
* @param {Object} options 一个可选的BlobPropertyBag字典
*/
new Blob()
```
> options 是一个可选的BlobPropertyBag字典，它可能会指定如下两个属性：
>
>type，默认值为 ""，它代表了将会被放入到blob中的数组内容的MIME类型。
>endings，默认值为"transparent"，用于指定包含行结束符\n的字符串如何被写入。 >它是以下两个值中的一个： >"native"，代表行结束符会被更改为适合宿主操作系统文件系统的换行符，或者 >"transparent"，代表会保持blob中保存的结束符不变


`Blob.slice([start[,end[,contentType])`:返回一个新的 Blob 对象，包含了源 Blob 对象中指定范围内的数据。（由于File继承自Blob，可用此方法分割本地文件，实现分片上传）
`Blob.arrayBuffer()`

## ArrayBuffer
你从XHR、File API、Canvas等等各种地方，读取了一大串字节流

ArrayBuffer简单说就是一片内存，表示原始二进制数据缓冲区。但不能直接操作它，而是要通过View-TypedArray或DataView对象来操作它，它们会将缓冲区中的数据表示为特定的格式，并通过这些格式来读写缓冲区的内容。TypedArray给ArrayBuffer提供了一个“View”，


## TypeArray

are array-like objects that provide a mechanism for reading and writing raw binary data in memory buffers.(是类似数组的对象，它提供了一种在内存缓冲区中读取和写入原始二进制数据的机制。)


A buffer is an object representing a chunk of data; it has no format to speak of, and offers no mechanism for accessing its contents. In order to access the memory contained in a buffer, you need to use a view. A view provides a context — that is, a data type, starting offset, and number of elements.
(缓冲区是代表数据块的对象；它没有任何格式可言，也不提供访问其内容的机制。为了访问缓冲区中包含的内存，您需要使用视图。视图提供上下文，即数据类型、起始偏移量和元素数量。)

![typearrary](./assets/typed_arrays.png)

`Uint8Array` Size in bytes = 1 (以8位,1字节为单位）

`Uint16Array` Size in bytes = 2 (以16位,2字节为单位)

`Uint32Array` size in bytes = 4 (以32位, 4字节为单位)

`Float64Array` size in bytes = 8 (以64位, 8字节为单位)

## Browser APIs that use Typed Arrays

`WebGL`

`Canvas 2D`

`TextEncoder`

`FileReader`

`XMLHttpRequest2`

`File`

`Web Worker Transferable objects`

`Media Source`

`Binary WebSockets`
