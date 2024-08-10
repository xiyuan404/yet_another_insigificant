- [代码仓库地址](https://github.com/xiyuan404/bigfiles-upload)
- [演示地址](https://bfiles-upload-244ezz4bu-ayaos-projects.vercel.app/)

## 2.大文件上传面试题
1. 基础概念
   - 描述实现大文件上传的基本流程
   - 解释为什么大文件上传比普通文件上传更具有挑战性
   - 什么是分片上传，它如何帮助前端处理大文件上传
2. 技术实现
   - 如何在前端实现大文件的切片上传？
   - 描述使用HTML5的File API来处理大文件上传的过程
   - 在处理大文件上传时，如何在前端进行文件类型和大小的验证？
3. 性能优化
   1. 在大文件上传时，如何优化前端性能以提高用户体验
   2. 如何在用户上传过程中暂停或断网，你将如何处理已上传的数据
   3. 如何在前端实现上传进度的可视化反馈
4. 错误处理和安全性
   1. 在大文件上传过程中，如果遇到网络错误或服务器错误，前端应该如何处理优化
   2. 描述在大文件上传中常见的安全问题及其解决方案
   3. 如何在前端实现大文件上传的加密处理
5. 后端交互
   1. 大文件上传中的前后端协作通常是怎样的？请描述整个过程
   2. 如果后端API在上传过程中发生更改，前端应该如何灵活适应
   3. 在大文件上传中，前端和后端如何协作处理上传进度和错误恢复？
6. 用户体验
   1. 如何设计一个用户友好的大文件上传界面
   2. 在上传大文件时， 如何减少对用户操作其他网站的影响
   3. 如果需要上传多个大文件，你将如何设计这个上传流程
7. 现代技术和框架
   1. 使用React/Vue等现代前端框架，如何实现大文件上传的功能
   2. 在大文件上传中， web Workers能发挥什么作用
   3. 描述使用云服务（如AWS S3） 进行大文件上传的优势和挑战

### 2.1 描述在前端实现大文件上传的基本流程

1. 文件选择：
   1. 用户通过拖放文件拖放区域或上传按钮
   2. 使用HTML5的`File API` 来获取用户选择的文件信息
2. 文件切片：
   1. 对于大文件， 直接上传整个文件可能导致问题（如网络不稳定导致上传失败、浏览器崩溃）因此将文件切分成多个小块是常见做法
   2. 使用JavaScript将文件切分成多个小块， 每个块可以是固定大小（如1MB）
3. 上传前准备
   1. 可以向服务器发送一个预请求，告知服务器将要上传的文件信息（文件名、文件大小、切片数量、切片大小）
   2. 服务器可以在此时进行一些准备工作，如检查文件是否已部分上传，为即将上传的文件分配资源
4. 并行或顺序上传切片：
   1. 切片可以并行上传，以提高上传效率，也可以顺序上传，以降低服务器压力
   2. 每个切片作为一个单独的请求发送到服务器，这些请求通常包含一些元数据（切片索引、总切片数）
5. 错误处理和重试机制：
   1. 在上传过程中，一些切片可能因网络问题或服务器问题上传失败，前端应有机制检测这些失败，并且能够重传这些切片
   2. 可以设置重试次数限制，以避免无限重试
6. 上传进度反馈
   1. 在上传过程中，前端应提供实时的上传进度反馈，这可以通过监听每个切片的上传请求的进度事件事件来实现
7. 完成上传：
   1. 所有切片上传完成后，前端向服务器发送一个完成上传的信号
   2. 服务器收到完成信号，开始将所有切片组合成原始文件
8. 文件验证和清理
   1. 服务器组合文件后，可以对文件进行验证。确保文件的完整是和正确性
   2. 验证完成后，前端和后端可以进行必要的清理的工作，如删除以上传的切片文件
9. 错误处理和用户反馈
   1. 如果在任何上传阶段发生错误，应即使将错误信息反馈给用户
   2. 同时，前端应提供用户友好的错误信息和可能的解决方案

### 2.2 解释为什么大文件上传比普通文件上传更具挑战性

1. **网络稳定性问题**：大文件上传需要更长时间，这增加了网络连接中断的风险。这增加了网络连接中断的风险，如果在上传过程中

2. 资源消耗： 上传大文件会占用更多的网络带宽和服务器资源.这可能导致服务器性能下来，影响其他用户的使用体验

3. 浏览器和服务器限制： 某些浏览器和服务器对上传文件的大小有限制。超出这个大小限制的文件，需要特殊的配置或技术来处理

4. 数据完整性和安全性：在长时间的传输过程中，确保数据的完整性和安全性是一大挑战。需要额外的措施来保证文件在传输过程中不被损坏或篡改

   1. 用户体验问题： 大文件上传可能需要很长时间，可能导致用户体验不佳，用户可能不清楚上传进度，或者在等待过程中感到沮丧

   为了解决这些挑战，通常会采用一些策略，如文件分割上传（将大文件分割成小块，分别上传), 断点续传（在连接中断后能从断点重新开始上传），以及优化网络和服务器配置

### 2.3 什么是分片上传， 他如何帮助亲端处理大文件上传？

分别上传是一种处理大文件上传的技术， 他将大文件分割成多个片段

1. 支持断点续传：如果上传过程中断，可以在网络恢复后继续上传未完成的分片。这对移动设备用户特别重要，因为它们可能在不稳定的网络环境下上传文件
2. 优化上传速度： 分片允许并行上传，可以同时上传多个分片，这可以有效利用网络带宽，从而加快上传速度
3. 灵活的错误处理：分配上传中，可以针对单个分片进行错误检测和恢复，而不是对整个文件进行处理。这提高了上传过程的效率
4. 进程控制和管理： 分片上传使得可以精准展示上传进度，提高用户体验
5. 绕过文件大小限制： 某些浏览器或服务器对单次上传的文件大小有限制。分片上传可以绕过这些限制，因为分片都小于这个限制

### 2.4 如何在前端使用JavaScript实现大文件的切片上传？

1. 选择文件

```js
<input type="file" id="fileInput"/>
```

2. 切割文件

```js
function sliceFile(file, chunkSize) {
  let chunks = []
  let size = file.size;

  for (let start = 0; start < size; start += chunkSize) {
    let end = Math.min(start + chunkSize, size)
    chunks.push(file.slice(start, end))
  }
  return chunks
}
```

3. 上传分片

```js
async function uploadChunk(chunk , index) {
  let formData = new FormData()
  formData.append('file', chunk)
  formData.append('index', index)
  let response = await fetch('/upload', {
    method:"POST",
    body: formData
  })

  return response.ok // 后端约定， 也可以是response.errCode
}
```

4. 合并分片 一旦所有分片都成功上传，服务器需要一个机制重新组合这些分片。这通常在服务端的上传脚本中处理
5. 完整的上传的过程

```js
document.getElmentById('fileInput').addEventListener('change', async (event) => {
  let file = event.target.files[0]
  let chunkSize = 1024 * 1024 // 1MB
  let chunks = sliceFile(file, chunkSize)
  for (let i = 0; i < chunks.length; i++) {
    let success = await uploadChunk(chunks[i], i)
    if (!success) {
			console.error('Upload failed for chunk' + i)
      return;
    }
  }
  console.log('File upload successfully')
})
```

**注意事项**

- 错误处理： 实际应用中需要考虑到错误处理和重试机制
- 安全性： 确保服务端安全地处理文件上传
- 用户体验：提供上传进度指示和必要的用户反馈
- 并发和性能： 适当控制并发上传的分片数量，以平衡上传数量和服务器负载

### 2.6 在处理大文件上传时，如何在前端进行文件类型和大小的验证？

1. 获取文件引用

```html
<input type="file" id="fileInput" />
```

监听文件输入的的变化来获取文件

```js
document.getElementById('fileInput').addEventListener('change', (event) => {
  const file = event.target.files[0]

})
```

2. 验证文件大小

```js
const MAX_SIZE = 10 * 1024 * 1024 // 10MB
if (file.size > MAX_SIZE) {
	alert('File is too large. Maximum size is 10MB')
  return;
}
```

3. 验证文件类型

验证文件类型通常是通过验证文件的MIME

```js
const ALLOWED_TYPES = ['image/jpeg', 'image/png']

if (!ALLOWED_TYPES.includes(file.type)) {
	alert('Invalid file type. Only JPEG and PNG are allower')
  return
}
```

或者通过文件扩展名验证：

```js
const ALLOWED_EXTENSIONS = ['.jpeg', '.jpg', '.png']

if (!ALLOWED_EXTENSIONS.some(exetension => file.name.endWith(extension))) {
	alert('Invalid file type. Only JPEG and PNG are allowed')
  return;
}
```

4. 处理验证结果 如果文件不符合要求，你可以向用户显示一个错误消息， 并停止进一步/后续的上传处理。如果文件通过验证，你可以继续上传处理
5. 综合



```js
document.getElementById('fileInput').addEventListener('change', function(event) {
    var file = event.target.files[0];

    // 验证文件大小
    const MAX_SIZE = 10 * 1024 * 1024; // 10MB
    if (file.size > MAX_SIZE) {
        alert('File is too large. Maximum size is 10MB.');
        return;
    }

    // 验证文件类型
    const ALLOWED_TYPES = ['image/jpeg', 'image/png'];
    if (!ALLOWED_TYPES.includes(file.type)) {
        alert('Invalid file type. Only JPEG and PNG are allowed.');
        return;
    }

    // 文件通过验证，继续处理上传
    // ...
});
```

*注意事项*

- 前端验证可以被绕过，因此应该始终在服务器端再次验证文件类型和大小
- 考虑到用户体验，应该在文件输入字段旁边或在文档中说明文件的类型和大小限制

### 2.7 大文件上传时，如何优化前端性能以提高用户体验

1. 分片上传
2. 断行续传
3. 前端文件验证
   1. 在上传之前验证文件大小和类型，防止不必要的上传尝试，节省带宽和时间
   2. 提供错误反馈和解决方案，帮助用户理解为什么文件不能上传
4. 显示上传进度
   1. 查看当前状态和剩余时间
5. 优化内存使用
   1. 在不需要时释放资源
6. 保持界面响应性
   1. 保持界面响应性，即使在上传大文件时，可以考虑用web worker处理文件上传操作，以避免阻塞主线程
   2. 提供暂停，取消上传的选项，给用户控制上传过程的能力
7. 安全性和隐私
   1. 确保上传过程符合安全标准，比如使用HTTPS加密上传
   2. 对于明暗文件，提供额外的安全措施，如加密文件
8. 反馈和支持
   1. 提供有用的反馈机制，比如上传成功或失败的通知
   2. 对于上传问题，提供用户支持选项，比如帮助文档或客服联系方式

### 2.8 如果用户在上传过程中暂停和断网，你将如何处理已上传的数据？

1. 启用分片上传
2. 跟踪上传进度
   1. 在前端，记录每个分片上传状态，例如用一个布尔数组或对象来标记每个分片是否成功上传
   2. 当用户暂停上传时，保存这些进度信息
3. 实现断点续传功能
   1. 当用户重新开始上传时，使用之前保存的进度信息来决定从那个分片开始上传
   2. 只上那些尚未上传的分片，避免重复上传已经成功上传的分片

4. 在服务端处理分片
   1. 确保服务器接收和存储分片，并在上传完成后组合这些分片
   2. 服务器端可能需要实现一种机制来识别和关联同一文件的不同分片
5. 提供恢复上传的选项
   1. 提供一继续上传选项
   2. 恢复上传之前，可以先检查已上传的分片的完整性和有效性
6. 处理长时间断开的情况
   1. 如果用户在很长时间后才返回来继续上传，考徐实现机制检测上传分片是否仍然有效，服务器上为每个分片设置一个“过期时间”
7. 安全性考虑
   1. 处理分片和恢复上传时，确保遵守安全最佳实践，防止数据泄露或未授权访问

### 2.9 如何在前端实现上传进度的可视化反馈？

追踪上传进度和在用户界面显示这些进度

1. 设计进度显示界面

```js
<div id="progressContainer">
  	<div id="progressBar"></div>
 </div>
```

```css
#progressContainer {
  width: 100%;
  background-color:#ddd;
}

#progressBar {
	width: 0%;
  height: 30px;
  background-color:#4caf50;
  text-align:center;
  line-height: 30px;
  color: white;
}
```

2. 追踪上传进度并更新进度条

```js
var xhr = new XMLHttpRequest()

xhr.upload.onprogress = function(event) {
  if (event.lengthComputable) {
    const  percentComplete = (event.loaded / event.total ) * 100
    // 更新进度条
    document.getElementById('progressBar').style.width = percentCompleted + '%'
    document.getElementById('progressBar').textContent = Math.round(percentCompleted) + '%'
  }
}

xhr.open('POST', '/upload', true)
```

3. 处理上传完成和错误
   1. 当上传完成时，确保进度条为100%
   2. 当上传过程出错，提供视觉反馈（如更改进度条颜色或显示错误）

### 在大文件上传过程中，如果遇到网络中断或服务器错误，前端应该如何优雅处理

1. 捕获和识别错误
   1. 捕获上传过程中的错误
   2. 根据错误的类型（如网络中断， 服务器响应错误）分类处理
2. 友好的错误消息
   1. 告诉用户发生了什么， 同时避免让用户感到困惑和沮丧
3. 自动重试和断点续传
   1. 对于临时性的网络错误，可以尝试重试上传，设置重试次数和重试间隔，避免无限重试
   2. 对于已经上传的文件部分，实现断点续传功能，这样在网络连接恢复后，从中断的地方开始，而不是从头再来
4. 提供手动重试选项
   1. 在显示错误信息的同时，提供一个“重试”按钮，允许用户在解决了问题（重新连接到网络后）手动启动重试
5. 日志记录
   1. 记录错误的详细信息，对于调试和改进上传过程很有帮助



### 2.11 描述在大文件上传中常见的安全问题及其解决方案

1. 未授权的大文件上传
   1. 问题如果未正确限制，攻击者可能上传恶意文件，如脚本或可执行文件，这可能导致服务器被攻击或数据泄露
   2. 解决方案： 严格限制可上传文件的类新，例如只允许特定的文件扩展名或MIME类型。在服务端也验证文件类型防止前端验证被绕过
2. 服务拒绝攻击（DOS)
   1. 问题：如果上传大量的大文件，攻击者尝试消耗服务器资源，导致服务不可用
   2. 解决方案： 限制文件大小，实现速率限制和并发连接限制。还可以使用CAPTCHA来区分人类用户和机器人
3. 敏感数据泄露
   1. 问题： 如果上传到的文件包含敏感信息，如个人数据或机密文件，数据未得到适当保护，可能会泄露
   2. 实现短刀端的加密，确保数据在传输恩好存储过程中均被加密，
4. 跨站脚本攻击（XSS）
   1. 问题： 如果文件上传功能允许上传包含可执行代码的文件，并且这些脚本可以在浏览器中其他网页中执行，可能会导致XSS
   2. 解决方案： 对上传的文件内容进行适当的清理和验证，确保文件不执行任何脚本代码
5. 文件完整性问题
   1. 问题： 在上传过程中，文件可能会因为网络问题或恶意攻击而损坏或篡改
   2. 解决方案： 使用校验和（如MD5或SHA-256)来验证文件完整性。
6. 身份验证和授权
   1. 问题： 未授权的用户可能尝试上传文件
   2. 解决方案： 实施强制身份验证和授权检查，确保只有具有适当权限的用户才能上传文件
7. 服务端执行代码
   1. 问题：上传的文件可能被误认为是服务器上可执行代码，
   2. 解决方案：在服务器端设置适当的执行权限，确保上传的文件不会被执行，将上传的文件存储隔离在不可执行目录上



### 2.12 如何在前端实现大文件上传的加密处理？

1. 选择加密算法
   1. 选择一个适合的加密算法
2. 生成密钥
   1. 使用`Web Crypto API` 生成密钥，这是一个内置于现代浏览器中的加密API
   2. 密钥管理非常重要，如果密钥丢失，加密后的文件将无法解密
3. 文件分片加密
   1. 受限于浏览器内存限制，避免一次整个大文件加密，分割成小块加密
4. 处理加密数据
   1. 加密每个分片后，将其添加到FormData对象中

```js
async function encryptData(data, key) {

	let encrypted  = await window.crypto.subtle.encrypt(
  	{
			name: "AES-GCM",
      iv: window.crypto.getRandomValues(new Unit8Array(12))

    },
    key,
    data
  )
  return encrypted
}

async function encryptFileChunk(chunk) {
	let key = await window.crypto.subtle.generateKey(
  		{
				name: 'AES-GCM',
        length: 256
      }
  	),
    true,
    ["encrypt", "decrypt"]

  	return await encryptData(chunk, key)
}
```

### 大文件上传中的前后端协作通常是怎样的？ 请描述整个流程

1. 用户选择文件
   1. 拖放或点击选择要上传的文件
2. 前端准备文件上传
   1. 文件分片：
   2. 文件验证：限制上传文件类型，避免上传恶意文件，如脚本或可执行文件，
3. 初始化上传
   1. 向后端发送初始化上传的请求，请求可能包含文件的大小，文件名，预期分片数
   2. 后端接受初始化请求，然后创建一个文件上传到绘画，返回唯一会话标识符或上传URL
4. 分片上传
   1. 上传分片： 前端上传的每个分片都会附带标记或序号，以便后端可以正确重组文件
   2. 进度反馈： 显示上传进度，并允许用户暂停，取消或重试上传
5. 后端处理
   1. 接收分片：后端接收每个上传分片，保存到`tmp`临时文件夹中
   2. 分片重组：所有分片上传完成后，后端将这些重组为原始文件
6. 上传完成
   1. 上传完成后，后端发送一个确认响应给前端，
   2. 前端收到确认响应，显示上传成功的消息
7. 安全和验证
8. 清理和维护
   1. 后端可能需要定期清理未完成的上传会话，以释放服务器资源
   2. 前端在上传过程中也应管理资源使用，如适时释放不再需要的内存

### 2.14 如果后端API在上传过程中发生更改，前端应该如何应对

1. 版本控制
   1. 保证后端API具有版本控制，这样即使API有更新，也可以保持旧版本API一段时间，直到前端也更新
   2. 前端可以指定使用特定版本API，确保在过渡期间仍能正常工作
2. 及时沟通
   1. 后端团队应该提前通知前端团队关于即将法的API更改，包括新的API文档，更改内容，上线时间
3. 灵活配置
   1. 前端代码应该设计成可以通过配置文件或环境变量轻松切换API 端点或调整请求参数

4. 错误处理和回退机制

- 强化前端的错误处理逻辑，以应对后端API更改可能带来的问题。
- 实现回退机制，当新API出现问题时，可以临时回退到旧API。



## 3. 大文件上传的业务背景和挑战

3.1 业务背景

	1. 媒体处理： 视频编辑平台，音频处理软件、图像库等需要上传大量媒体文件
	1. 数据备份与迁移： 企业需要备份或迁移大量数据，包括数据库文件、系统镜像等
	1. 内容分发网络： 在CDN中上传大文件以便更快在全球范围内分发
	1. 科学与研究： 上传大型数据集， 例如基因组序列、气象模型数据
	1. 教育和在线学习： 上传高质量的教学视频和教材
	1. 法律和财务： 共享大量的法律文档或财务报表









## 4. 大文件上传实现(React)

### 4.1 创建项目

```bash
mkdir uploadfile && cd $_
npx create-react-app client
npm install @ant-design/icons antd axios
npm start
```

### 4.2 绘制页面

`src\index.js`

```js
import React from 'react'
import ReactDOM from 'react-dom/client'
import FileUploader from './FileUploader'
const root = ReactDOM.createRoot(document.getElementById('root'))
root.render(
	<FileUploader/>
)
```

`src\FileUploader.js`

```js
import { InboxOutlined } from '@ant-design/icons'
import './FileUploader.css'

const FileUploader = () => {
  return (
  	<div className="upload-container">
    	<InboxOutlined />
    </div>
  )
}
export default FileUploader
```

`src/FileUploader.css`

```css
.upload-container {
  width: 100%;
  height: 200px;
  display: flex;
  align-items: center;
  border: 1px dashed #d9d9d9;
  background: #fafafa;
}

.upload-contianer:hover {
	border-color: #40a9ff;
}
.upload-container span {
  font-size: 60px;
}
```

### 4.3 拖入文件

`src/FileUploader.js`

```diff
+ import {useRef} from 'react'
import { InboxOutlined } from '@ant-design/icons'
import './FileUploader.css'
+ import useDrag from "./useDrag"
const FileUploader = () => {
+ const uploadContainerRef = useRef(null)
+ useDrag(uploadContainerRef)
  return (
+  	<div className="upload-container" ref={uploadContainerRef}>
    	<InboxOutlined />
    </div>
  )
}
export default FileUploader
```

`src\useDrag.js`

```js
import { useState, useEffect } from 'react'

  const handleDrag = (e) => {
    e.prevetnDefault()
    e.stopPropagation()
    const { files } = e.dataTransfer;
    console.log("files", files)
  }


function useDrag(uploadContainerRef) {
  const [selectedFile, setSelectedFile] = useState(null)
	useEffect(() => {
   const uploadContianer = uploadContainerRef.current
   uploadContainer.addEventListener('dragenter', handleDrag)
   uploadContainer.addEventListener('dragover', handleDrag)
   uploadContainer.addEventListener('drop', handleDrag)
   uploadContainer.addEventListener('dragleave', handleDrag)
  },[])

  return {selectedFile}
}

export default useDrag
```












### 4.4 检查并预览文件

`src/FileUploader.js`

```diff
import {useRef} from 'react'
import { InboxOutlined } from '@ant-design/icons'
import './FileUploader.css'
import useDrag from "./useDrag"
const FileUploader = () => {
  const uploadContainerRef = useRef(null)
+  const { filePreview } = useDrag(uploadContainerRef)
useDrag(uploadContainerRef)
  return (
  	<div className="upload-container" ref={uploadContainerRef}>
+   { renderFilePreview(filePreview}
    </div>
  )
}

+ function renderFilePreview(filePreview){
+  if (filePreview.url) {
+    if (filePreview.type.startsWith("video/")) {
+      return <video src={filePreview.url} alt="preview" controls />
+    } else if (filePreview.type.startsWith("image/") {
+      return <img src={filePreview.url} alt="preview" />
+    } else {
+      return filePreview.url
+    }
+  } else {
+   	  return <InboxOutlined />
+  }
+}


export default FileUploader

```

`src/FileUploader.css`

```diff
.upload-container {
  width: 100%;
  height: 200px;
  display: flex;
  align-items: center;
  border: 1px dashed #d9d9d9;
  background: #fafafa;
}

.upload-contianer:hover {
	border-color: #40a9ff;
}
.upload-container span {
  font-size: 60px;
}
+.upload-container video,img {
+    height: 100%
+}

```

`src/constant.js`

```js
export const MAX_FILE_SIZE = 2 * 1024 * 1024
```

`src/useDrag.js`

```js

import {useState, useEffect } from 'react'
import { InboxOutlined } from '@ant-design/icons'
import {MAX_FILE_SIZE from './constant'

const checkFile = files => {
  const file = files[0]
  if (!file) {
    message.error("没有选择任何文件")
    return
  }
  if (file.size > MAX_FILE_SIZE) {
    message.error("文件大小不能超过2GB")
    return
  }
  if (!file.type.startsWith("image/") && !file.type.startsWith("video/") ) {
    message.error("文件类型必须是图片或视频")
    return
  }

  setSelectedFile(file)
 }


const handleDrag = (e) => {
  e.preventDefault()
  e.stopPropagation()
}
const handleDrop = (e) => {
  e.preventDefault()
  e.stopPropagation()
  checkFile(e.dataTransfer.files)
}


function useDrag(uploadContainerRef) {
  const [selectedFile, setSelectedFIle] = useState(null)
  const [filePreview, setFilePreview] = useState({
    url: null,
    type: null
  })

  useEffect(()=>{
    if (!selectedFile) return
    const url = URL.createObjectURL(selectedFile)
    setFilePreview({url, selectedFile.type})
    return () => {
      URL.revokeObjectURL(url)
    }
  },[selectedFile])


  useEffect(()=>{
    const uploadContainer = uploadContainerRef.current
    uploadContainer.addEventListener("dragenter", handleDrag)
    uploadContainer.addEventListener("dragover", handleDrag)
    uploadContainer.addEventListener("drop", handleDrop)
    uploadContainer.addEventListener("dropleave", handleDrop)

  },[])
  return { filePreview }
}


```



```ts

enum TodoStatus = {
  NOTSTART = 0,
  WORKINPROGRESS = 1,
	PENDING = 2,
  DREPRECREATED = 3,
  COMPLETED = 4
}
```


### 4.5 分片上传

`src/FileUploader.js`

```js

import {useRef} from 'react'
import { CHUNK_SIZE } from './constant'
import axiosInstance from './axiosInstance'

const FileUploader =  () => {
  const uploadContainerRef = useRef(null)
  const { selectedFile, filePreview } =  useDrag(uploadContainerRef)

  const handleUpload = async () => {
    if (!selectedFile) {
      message.error("请先选择一个文件")
      return
    }

    const filename = await getFileName(selectedFile)
    await uploadFile(selectedFile, filename)
  }

  async function uploadFile(selectedFile, filename) {
    const chunks = createFileChunks(file,filename)
    const request = chunks.map( {chunk, chunkFileName} => createRequest(filename, chunkFileName, chunk))
    try {
      await Promise.all(requests)
      await axiosInstance.get(`/merge/${filename}`);
      message.success("上传成功”)
    } catch (error) {
      message.error("上传出错”， error)
    }
  }




  function createFileChunks(file, filename) {
    const chunks = [];
    // for(let start = 0; start < file.size; start + CHUNK_SIZE ) {
    //    let end = Math.min(start + CHUNK_SIZE, file.size)
    //    chunks.push({
    //       file.slice(start, end),
    //       chunkFileName: `${filename}-${start}`
    //    })
    // }
    const count = Math.ceil(file.size / CHUNK_SIZE)
    for (let i = 0; i < count; i++) {
      chunks.push({
        file.slice(i * CHUNK_SIZE, Math.min( (i + 1) * CHUNK_SIZE, file.size )),
        chunkFileName: `${filename}-${i}`
      })
    }
    return chunks
  }

  function createRequest(filename, chunkFileName, chunk) {
  return axiosInstance.post(`/upload/${filename}`, chunk, {
    headers: {
      "Content-Type": "application/octet-stream"
    },
    params: {
      chuknFileName
    }
  })
  }


  // ============================
  async function getFileName(file) {
    const fileHash = await calculateHash(file)
    const fileExtension = file.name.split(".").pop()
    return `${fileHash}${fileExtension}`
  }

  async function calculateHash(file){
    const arrayBuffer = await file.arrayBuffer();
    const hashBuffer = await crypto.subtle.digest("SHA-256", arrayBuffer)
    return bufferToHex(hashBuffer)
  }

  function bufferToHex(buffer) {
    return Array.from(new Uint8Array(buffer))
      .map(b => b.toString(16).padStart(2, '0')
      .join('')
  }


  const renderButton() => {
    return <Button onClick={handleUpload}>上传</Button>
  }


    return (
      <div className="upload-container" ref={uploadContianerRef}>
        {renderFilePreview(filePreview)}
      </div>
      {renderButton()}
    )
}


function renderFilePreview(filePreview) {
  if (filePreview.url) {
    if(filePreview.type.startsWith("video/")) {
      return <video src={filePreview.url} alt="preview" />
    } else if (filePreview.type.startsWith("images/") {
      return <img src={filePreview.url} alt="priview" />
    } else {
      return filePreview.url
    }
  } else {
    return <InboxOutline/>
  }
}

```

`src/constant.js`

```js
export const MAX_FILE_SIZE = 2 * 1024 * 1024 * 1024
export const CHUNK_SIZE = 100 * 1024 * 1024
```

`src/axiosIntance.js`
```js
import axois from 'axios'
const axiosInstance = axios.create({
  baseURL: "http://localhost:8000"
})

axiosInstance.interceptors.response.use(request => {
  if (response.data && response.data.success) {
    return response.data
  } else {
    throw new Error(response.data.message || "服务器响应错误")
  }
}, error => {
  throw error;
})

export default axiosInstance
```


### 4.6 上传进度

`src/useDrag.js`

```js
import {useState, useEffect } from 'react'
import {MAX_FILE_SIZE} from './constant'


function useDrag(uploadContainerRef) {
  const [selectedFile, setSelectedFile] = useState(null)

  const checkFiles = (files) => {
    // fetch file from FileList
    const file = files[0]
    // process file
    if (!file) {
      message.error("没有选择任何文件")
      return
    }
    if (file.size > MAX_FILE_SIZE) {
      message.error("文件大小不能超过2GB")
      return
    }
    if (!file.type.startsWith("image/") && !file.type.startsWith("video/") {
      message.error("文件类型必须是图片或视频")
      return
    }
    setSelectedFile(file)
  }



  const handleDrag = (e) => {
    e.preventDefault()
    e.stopProgation()
  }
  const handleDrop = (e) => {
    e.preventDefault()
    e.stopProgation()

    checkFiles(e.dataTransfer.files)
  }

  useEffect(()=> {
    const uploadContainer = uploadContainer.current
    uploadContainer.addEventListener("dragenter", handleDrag)
    uploadContainer.addEventListener("dragover", handleDrag)
    uploadContainer.addEventListener("drop", handleDrop)
    uploadContainer.addEventListener("dropleave", handleDrop)
  }, [])

  const resetFileStatus = () => {
    setSelectedFile(null)
    setFilePreview({
      url: null,
      type: null,
    })
  }
  return { seletcedFile, filePreview, resetFileStatus }
}
export default useDrag
```

`src/FIleUploader.js`

```js
import { useRef, useState } from 'react'
import { InboxOutlined } from '@ant-design/icons"
import useDrag from "./useDrag"
import {CHUNK_SIZE} from './constants'
import axiosInstance from './axiosInstance'
import {Progress} from 'antd'

const fileUploader = () => {
  const uploadContainerRef = useRef(null)
  const { Button, message, Progress } from 'antd'
  const { selectedFile, filePreview, resetFileStatus } from useDrag(uploadContainerRef)
  const [uploadProgress, setUploadProgress] = useState({})
  const resetAllStatus = () => {
    resetFileStatus()
    setUploadProgress()
  }


  const handleUpload = async() => {
    if (!selectedFile) {
      message.error("请先选择一个文件")
      return
    }
    const filename = await getFileName(selectedFile)
    await uploadFile(selectedFile, filename, setUploadProgress, resetAllStatus)
  }


  const renderButton = () => {
    return <Button onClick={handleUpload}> 上传</Button>
  }

  const renderProgress = () => {
  return Object.keys(uploadProgress).map((chunkname, index) =>( <div key={chunkname}>
  <span>切片{index}:</span>
    <Progress precent={uploadProgress[chunkname]} />
  </div>)
  }


  return (
    <div className="upload-contianer" ref={uploadContainerRef} >
      { renderFilePreview(filePreview)}
      { renderButtton }
      { renderProgress }
    </div>
  )
}


async function uploadfile(file, filename, setUploadProgress, resetAllStatus){
const chunks = createChunks(file, filename)
const requests = chunks.map({chunk, chunkname} => createRequest(filename, chunk, chunkname, setUploadProgress)
try {
  await Prmosie.all(requests)
  await axiosInstance.get('/merge/${filename}')
  message.success("上传成功")
  resetAllStatus()
} catch (error) {
  message.error("上传出错")
}
}


function createRequest(filename, chunk, chunkname, setUploadProgress) {
  return axoisInstance.post('/upload/${filename}', chunk, {
    headers: {
    "Content-Type": "application/octet-stream",
    },
    onUploadProgress: progressEvent => {
      const percentCompleted = Math.round(progressEvent.loaded *100 / progressEvent.total )
      setUploadProgress(prevProgress => ({
        ...prevProgress
          [chunkname]: percentCompleted
      }))
    },
    params:{
      chunkname
    }
  }
  )
}


function createChunks(file, filename) {
  const chunks = []
  const count = Math.ceil(file.size / CHUNK_SIZE)

  for (let i = 0; i < count; i++) {
    chunks.push({
      chunk:file.slice( i * CHUNK_SIZE, Math.min( (i+1) * CHUNK_SIZE, file.size)
      chunkname: `${filename}-${i}`
    })
  }
  return chunks
}


async function getFileName(file) {
  const fileHash = await calculateHash(file)
  const fileExtension = file.name.split(".").pop()
  return `${fileHash}.${fileExtension}`
}


async function calculateHash(file) {
  const arrayBuffer = await file.arrayBuffer()
  const hashBuffer = await crypto.subtle.digest("SHA-256", arraryBuffer)
  return bufferToHex(hashBuffer)
}

function bufferToHex(buffer) {
  return Array.from(new Uint8Array(buffer))
    .map(b => b.toString(16).padStart(2, '0'))
    .join('')
}



function renderFilePreview(filePreview) {
  if (filePreview.url) {
    if (filePreview.type.startsWith("images/")) {
      return <video src={filePreview.url} alt="preview" />
    } else if (filePreview.type.startsWith("video/") {
      return <img src={filePreview.url} alt="preview" />
    } else {
      return filePreview.url
    }
  } else {
    return <InboxOutlined />
  }
}

export default

```


### 4.7 秒传

`src/FileUploader.js`

```js

function uploadFile(file, filename ,setUploadProgress) {
  const {needUpload} = axiosInstance.get(`/verify/${filename}`)
  if (!needUpload) {
    message.success("文件已存在")
    return resetAllStatus()
  }

  const chunks = createChunks(file, filename)
  const requests = chunks.map({chunk, chunkname} => createRequest(filename, chunkname, chunk, setUploadProgress)
  try {
    await Promise.all(requests)
    await axiosInstance.get(`merge/${filename}`)
    resetAllStatus()
  } catch(error) {
    console.error("上传出错", error)
    message.error
  }

}

```


### 4.8 暂停上传

```js
import {useRef} from 'react'
import {useDrag} from './useDrag'
import {InboxOutlined} from '@ant-design/icons'
import {Button, message, Progress} from 'antd'
import {CHUNK_SIZE} from './constant'
import axiosInstance from './axiosInstance'

const UploadStatus = {
  NOT_START: "NOT_START",
  UPLOADING: "UPLOADING",
  PAUSED: "PAUSED"
}


const FileUploader = () => {
  const uploadContainerRef = useRef(null)
  const {filePreview,selectedFile, resetFileStatus} = useDrag(uploadContainerRef)
  const [uploadProgress, setUploadProgress] = useState({})
  const [uploadStatus, setUploadStatus] = useState(UploadStatus.NOT_STARTED)
  const [cancelTokens, setCancelTokens] = useState([])

  cosnt resetAllStatus() => {
    resetFileStatus()
    setUploadProgress({})
    setUploadStatus(UploadStatus.NOT_STARTED)
  }

  const handleUpload = () => {
    if (!selectedFile) {
      message.error("请先选择一个文件")
      return
    }
    setUploadStatus(UploadStatus.UPLOADING)
    const filename = await getFileName(selectedFile)
    await uploadFile(file, filename, resetAllStatus,setUploadProgress)
  }

  const pauseUpload = () => {
    setUploadStatus(UploadStatus.PAUSED)
    cancelTokens.forEach(cancelToken.cancel("用户取消上传"))
  }
  const resumeUpload = () => {
    setUploadStatus(UploadStatus.UPLOADING)
    handleUpload()
  }

  const renderButton = () => {
    switch (uploadStatus) {
      case UploadStatus.NOT_STARTED:
        return <Button onClick={handleUpload}>上传</Button>
      case UploadStatus.UPLOADING:
        return <Button icon={<PauseCircleOutlined />} onClick={pauseUpload}>暂停</Button>
      case UploadStatus.PAUSED:
        return <Button icon={<PlayCircleOutlined />} onClick={resumeUpload}>恢复上传</Button>
      default:
        return null
    }
  }

  const renderProgress = () => {
  return Object.keys(uploadProgress).map((chunkname,index) => (
    <div >
      <span> 切片{index}：</span>
      <Progress percent={uploadProgress[chunkname] />
    </div>
  ))
  }

  return (
    <div className="upload-container" ref={uploadContainerRef}>
      { renderFilePreview(filePreview)}
    </div>
    { renderButton()}

  )
}


function createRequest(filename,chunk,chunkname) {
  return axiosInstance.post(`/upload/${filename}`, chunk, {
    headers: {
      "Content-Type":  "applicaton/octet-stream"
    },
    onUploadProgress: progressEvent => {
      const percentCompleted = (progressEvent.loaded * 100 / progressEvent.total)
      setUploadProgress(prevProgress => ({
        ...prevProgress,
        [chunkname]: percentCompleted
      }))
    }
    params: {
      chunkname
    },
    cancelToken: cancelToken.token
  })
}

function createChukns(file, filename) {
  const chunks = []
  const count = Math.ceil(file.size / CHUNK_SIZE)
  for (let i =0; i < count; i++) {
    chunks.push({
      chunk: file.slice(i*CHUNK_SIZE, Math.min((i+1) * CHUNK_SIZE), file.size))
      chunkname: `${filename}-${i}`
    })
  }
  return chunks
}


async function uploadFile (file, filename, resetAllStatus) {
  const chunks = createChunks(file,filename)
  const newCancelTokens = []
  const requests = chunks.map({chunk,chunkname} =>{
    const cancelToken = axios.CancelToken.source()
    newCancelTokens.push(cancelToken)
    return createRequest(filename,chunk,chunkname,setUploadProgress,cancelToken)
  }
  try{
    setCancelTokens(newCancelTokens)
    await Promise.all(requests)
    await axiosInstance.get(`/merge/${filename}`)
    resetAllStatus()
  } catch (err) {
    if (axios.isCancel(error)) {
      console.log("上传暂停")
      message.warn("上传暂停")
    } else {
      console.log("上传出错", error)
      message.error("上传出错")
    }
  }
}




function bufferToHex(buffer) {
  return Array.from(new Uint8Array(buffer))
    .map(b => b.toString(16).padStart('0'))
    .join('')
}

async function calculateFileHash(file) {
  const fileBuffer = file.arrayBuffer()
  const hashBuffer = await crypto.subtle.digest.('SHA-256', fileBuffer)
  return bufferToHex(hashBuffer)
}

async function getFileName(file) {
  const fileHash = await calculateFileHash(file)
  const fileExtension = file.name.split('.').pop()
  return `${fileHash}.${fileExtension}`
}

function renderFilePreview(filePreview) {
  if (filePreview.url) {
    if (filePreview.type.startsWith('image/')) {
      return <img src={filePreview.url}  alt="preview" />
    } else if(ilePreview.type.startsWith('video/')) {
      return <video src={filePreview.url} alt="preview" />
    } else {
      return filePreview.url
    }
  } else {
    return <InboxOutlined />
  }
}
```

`src/useDrag.js

```js
import {useState,useEffect} from 'react'
import {MAX_FILE_SIZE} from './constant'


const useDrag =  (uploadContainerRef) => {
  const [selectedFile, setSelectedFile] =  useState(null)
  const [filePreview, setFilePreview] = useState({
    url: '',
    type: ''
  })
  const checkFiles = (files) => {
    const file = files[0]
    if (!file) {
      message.error("没有选择任何文件")
      return
    }
    if(!file.type.startsWith('image/') && !file.type.startsWith('video/')) {
      message.error("文件类型必须是图片或视频")
      return
    }

    if(!file.size > MAX_FILE_SIZE) {
      message.error(`文件大小不能超过${MAX_FILE_SIZE}`)
      return
    }
    setSelectedFile(file)
  }

  const handleDrop = (e) => {
    e.preventDefault()
    e.stopProgation()
     checkFiles(e.transfer.files)
  }

  useEffect(()=>{
    const uploadContainer = uploadContainerRef.current
    uploadContainer.addEventListener('dragenter',handleDrag)
    uploadContainer.addEventListener('dragover',handleDrag)
    uploadContainer.addEventListener('drop',handleDrop)
    uploadContainer.addEventListener('dropleave',handleDrop)

  }, [])

  useEffect((e)=>{
    if(!selectedFile) return
    const url = URL.createObjectURL(selectedFile)
    setFilePreview({url, type: selectedFile.type})
    return () => {
      URL.revokeObjectURL(url)
    }
  },selectedFile)

  const resetFileStatus = () => {
    setSelectedFile(null)
    setFilePreview({
      url: null
      type: null
    })
  }
  return {filePreview, selectedFile}
}


export default useDrag

```



### 4.9 端点续传


```js
async function uploadFile(file, filename, setUploadProgress, resetAllStatus, setCancelTokens)
const { needUpload, uploadList } = await axiosInstance.get(`/verify/${filename}`)
if(!needUpload) {
  message.success("文件已存在")
  return resetAllStatus()
}

const chunks = createChunks(file, filename)
const newCancelTokens = []
const requests = chunks.map({chunk, chunkname} => {
  const cancelToken = axios.CancelToken.source()
  newCancelTokens.push(cancelToken)
  const existingChunks = uploadList.find(item => item.chunkname === chunkname)
    if (existingChunks) {
      const uploadSize = existingChunks.size
        const remaingChunk = chunk.slice(uploadSize)
          if (remaingChunk.size === 0) {
            return Promise.resolve()
          }
          return createRequeset(filename, chunkFileName, remainingChunk, setUploadProgress,cancelToken,uploadedSize)
    } else {
    return createRequest(filename, chunkFileName, chunk, setUploadProgress,cancelToken, index * CHUNK_SIZE);
    }
})
```

### 5.1 Web Workers
`src/FileUploader.js`

```js
const [filenameWorker, setFilenameWoker] = useState(null)


useEffect(()=> {
    const filenameWorker = new Worker('filenamWorker.js'')
    setFilenameWorker(filenameWorker)
    return () => filenameWorker.terminate()
},[])


const handleUpload = () => {
  if (!selectedFile) {
    message.error("请先选择一个文件")
    return;
  }
  setUploadStatus(UploadStatus.UPLOADING)
  filenameWorker.onmessage = async(event) => {
    setCalculatingFilename(false)
    await uploadFile(selectedFile, event.data, setUploadProgress, resetAllStatus, setCancelToken)
  }
}


{calculatingFilename && <Spin tip={<span>计算文件名...</span>} />
```

`public/filenameWorker.js`


```js
self.addEventListener('message', async (event) => {
  const file = event.data
  const filename = await getFilename(file)
  self.postMessage(filename)
})



async function getFilename(file) {
  const arrayBuffer = file.arrayBuffer()
  const hashBuffer = await crypto.subtle.digest("SHA-256", arrayBuffer)
  const bufferToHex(arrayBuffer)



  const fileHash =
  const fileExtension = file.name.split('.').pop()
  return `${fileHash}.${fileExtension}`
}

function bufferToHex(buffer){
  return Arrary.from(new Uint8Array(buffer))
    .map(b => b.toString(16).padStart(2,'0'))
    .join('')
}
```


### 5.2 重试机制

```js
import {CHUNK_SIZE, MAX_RETRIES} from './constant'



  if(retryCount < MAX_RETRIES) {
    console.log("上传出错,重试中", error)
     setTimeout(()=> {
    return  await uploadFile(file, filename, setUploadProgress, resetAllStatus, setCancelTokens, retryCount + 1)
    },1500)
  }

```

5.3 点击选择文件上传

合适的并发上传数量
1. 网络带宽： 过多的并发请求可能会导致拥堵和延迟
2. 服务器容量： 太多的并发请求可能会导致服务器响应变慢或者崩溃
3. 用户设备： 多个上传请求会占用用户设备较多的计算资源


控制并发上传数量

1. `Promise.all`处理多个上传任务
2. 队列管理：创建一个上传队列，同时只处理队列中一定数量的上传任务，完成一个任务后，从队列中取出下一个任务开始上传
3. 动态调整并发数： 根据上传过程的成功率和速度动态调整并发数，如果上传错误率增加或速度下降，可以减少并发数

```js

async function uploadFileInChunks(file, chunkSize, maxConcurrentUploads) {
  const chunks = createChunks(file, chunkSize)
  const activeUploads = []
  let completedUploads = 0;
  async function uploadNextChunk() {
    if (completedUploads === chunks.length ){
      console.log("上传完成")
      return
    }

    while (activeUploads.length < maxConcurrentUploads && chunks.length > 0 ) {
      let chunk = chunks.shift()
      let uploadPromise = uploadChunk(chunk).then(()=>{ completedUploads++}).catch(error => console.log("上传失败", error)
        .finally(()=>{
          activeUploads.splice(actvieUploads.indexOf(uploadPromise), 1)
        })
        activeUploads.push(uploadPromise)
    }
  }
  await uploadNextChunk()

}

```


### 5.6 文件校验
为了确保上传的文件内容未被篡改， 可以在客户端和服务端实施一系列校验机制

客户端
1.计算文件哈希： 在上传前， 使用`Web Worker`来计算文件的哈希值，这个哈希值作为文件的唯一标识符
2. 发送哈希值到服务器： 在开始上传文件前，将计算出的哈希值发送到服务器进行校验，服务器将检查次哈希值是否已经存在，从而避免重复上传
3. 在上传每个分片将该分片对应的哈希一起上传到服务器

服务器端
1.验证文件哈希： 在接受文件哈希时，服务器应该验证是否有对应的文件已经存在
2.存储分片哈希： 在每个分片上传后，存储器其哈希
3. 所有分片上传并合并后，计算整个文件的哈希值，确保与客户端发送的初始哈希值匹配
4. 如果哈希值不匹配，服务器应删除已上传的文件，并向客户端报错

```js

async function calculateHashForFile(filepath) {
  return new Promise((resolve, reject) => {
    const hash = crypto.createHash('sha256')
    const stream = fs.createReadStream(filepath)
    stream.on('data', (chunk) => {
      hash.update(chunk)
    })
    stream.on('end', () => {
      const hashValue = hash.digext('hex')
      resolve(hashValue)
    })
  }
}

async function validateFile(filename, originalFileHash){
  const filepath = path.resolve(PUBLIC_DIR, filename)
  const calculatedFileHash = await calculateHashForFile(filepath)
  if( originalFileHash  !== calculatedFileHash) {
    await fs.unlink(filepath)
  }
}

```


### 5.7 文件加密

为防止文件被窥探，客户端加密文件切片，然后在服务端解密

（客户端）
```js
async function encryptChunk(chunk, key) {
  const algorithm = {
    name: "AES-GCM",
    iv: window.crypto.getRandomValues(new Uint8Array(12))
  }

  const encryptedChunk = await window.crypto.subtle.encrypt(algorithm, key, chunk)
  return { encryptedChunk, iv: alogorith.iv }
}
```

将密钥安全地导出并提供给服务端开发人员
密钥导出格式(通常是`raw`或`jwk`)

JWK（[JSON Web Key](https://auth0.com/docs/secure/tokens/json-web-tokens/json-web-key-sets) 格式: A JSON object that represents a cryptographic key


通过 Web Cryptography API中的`generateKey`方法来生成密钥，`exportKey`方法来导出密钥，
```js
async function generateKey(){
  // f(alogrithm, extractable, keyUsages)
  const key = await window.crypto.subtle.generateKey(
    {
      nmae:"AES-GCM",
      length: 256
    },
    true //是否导出,
    ["encrypt","decrypt"]
  )
}


async function downloadKeyAsFile(key, filename) {
  const expotedKey = await crypto.subtle.exportKey("raw", key)
  const blob = new Blob([exportedKey], {type:"application/octet-stream")
  const url = URL.createObjectURL(blob)
  const a = document.createElement("a")
  a.href = url
  a.download = filename
  document.body.appendChild(a)
  a.click()
  window.URL.revokeObjectURL(url)
  document.body.removeChild(a)
}
```
将密钥导出为`jwk`格式
```js
async function downloadKeyAsFile(key, fileName) {
    try {
        const exportedKey = await window.crypto.subtle.exportKey("jwk", key);
        // f(value, ?replaceer, ?space)
        const jsonStr = JSON.stringify(exportedKey, null, 4);
        const blob = new Blob([jsonStr], {type: "application/json"});
        const url = URL.createObjectURL(blob);
        const a = document.createElement("a");
        a.href = url;
        a.download = fileName;
        document.body.appendChild(a);
        a.click();
        window.URL.revokeObjectURL(url);
        document.body.removeChild(a);
    } catch (e) {
        console.error("密钥导出错误: ", e);
    }
}

```


### 安全传输密钥


### 5.8 上传阿里云的OSS云服务



## 6.后端实现

### 6.1 创建项目
```bash
npm init -y
npm install express morgan http-status-codes http-errors cors fs-extra

```


`src/index.js`

```js
// 引入 Express 模块
const express = require("express");
// 引入 Morgan 日志记录模块
const logger = require("morgan");
// 引入 HTTP 状态码
const { StatusCodes } = require("http-status-codes");
// 引入 CORS 跨域资源共享模块
const cors = require("cors");
// 引入 path 模块处理文件路径
const path = require("path");
// 引入 fs-extra 模块处理文件系统
const fs = require("fs-extra");
// 引入创建 HTTP 错误的模块
const createError = require('http-errors');
const CHUNK_SIZE = 100 * 1024 * 1024

const TEMP_DIR = path.resolve(__dirname, "temp")


const app = express()
app.use(logger("dev"))
app.use(express.json())
app.use(express.urlencoded({extended: true))
app.use(cors())
app.use(express.static(path.resolve(__dirname, "public"))


  // 处理上传文件的请求
app.post("/upload/:filename", async (req, res, next) => {
  try{
  // 从路径参数中获取文件名
  const {filename} = req.params;
  // 从查询参数中获取块文件名
  const chunkname = req.query.chunkname
  // 从查询参数中获取起始位置
  const start = isNaN(req.query.start) ? 0 : parseInt(req.query.start, 10)
  // 定义块文件存放路径
  const chunkDir = path.resolve(TEMP_DIR, filename)
  const chunkPath = path.resolve(chunkDir,chunkname)
  // 确保块文件夹存在
  await fs.ensureDir(chunkDir)
  // 创建写入流
  const ws = fs.createWriteStream(chunkPath, {start, flags:"a"})
  // 如果请求中断， 关闭写入流
  req.on("aborted", () => {
    ws.close()
  })
  await pipeStream(req,ws)
  res.json({success: true})
  } catch (error) {
    next(error)
  }
})


app.get("/merge/:filename", async (req, res, next) => {
  // 从请求url中的路径参数获取文件名
  const {filename} = req.params
  // 检查文件是否已在公共目录中存在
  const filepath  = path.resolve(PUBLIC_DIR, filename)
  try {
    await mergeChunks(filename)
    res.json({success: true})
  } catch(error) {
    next(error)
  }

})

app.get("/verify/:filename", async (req, res, next) => {
  // 从请求url中的路径参数获取文件名
  const {filename} = req.params
  // 检查文件是否已经在公共目录存在
  const filepath = path.resolve(PUBLIC_DIR, filename)
  const existFile = await fs.pathExists(filepath)
  if (existFile) {
    return res.json({ success: true, needUpload: false})
  }

  // 检查临时目录是否存在
  const tempDir = path.resolve(TEMP_DIR, filename)
  const exist = await fs.pathExists(tempDir)
  // 初始化上传列表
  const uploadList = []
  if (exist) {
    // 获取临时目录中的所有分片
    const chunksname = await fs.readdir(temDir)
    // 生成上传列表
    uploadList  = Promise.all(
      chunksname.map(async (chunkname) => {
        const stat = await fs.stat(path.resolve(tempDir,chunkname)
        return {chunkname, size: stat.size}
      })
    )
  }
  // 返回上传的分片列表
  res.json({success: true, needUpload: true, uploadList})
})

app.use((req, res, next) => {
  next(createError(StatusCodes.NOT_FOUND))
})

app.use((error, req, res, next) => {
  console.error(error)
  res.status(error.status || StatusCode.INTERNAL_SERVER_ERROR);
  res.json({success: false, error: error.message})
})


function pipeStream(rs, ws) {
      return new Promise((resolve, reject) => {
        re.pipe(ws).on("finish", resolve).on("error", reject)
      }
}

function mergerChunks(filename) {
  const filePath = path.resolve(PUBLIC_DIR, filename)
  const chunksDir = path.resolve(TEMP_DIR)
  const chunksName = await fs.readdir(chunkDir)
  // 对块文件排序
  chunksName.sort((a, b) => Number(a.split('-').pop()) - Number(b.split('-').pop()))

  // 合并所有块文件
  await Promise.all(
  chunksname.map((chunkname, idnex) => {
    pipeStream(
      fs.createReadStream(path.resolve(chunksDir, chunkname), {autoClose: true})
      fs.createWriteStream(filePath, {start: index * CHUNK_SIZE })
    )
  })
  )
  // 删除临时文件夹
  await fs.rmdir(chunksDir, { recursive: true} )
}

// 启动服务器监听8080端口
app.listen(8080, ()=> console.log('Server started on prot 8080')
```


`src/package.json`

```js
{
  "scripts": {
    "start": "nodemon index.js"
  }
}
```


## 7.参考

### 7.1 nodejs调试

- [debugger](https://nodejs.org/api/debugger.html) @ Node.js v22.4.1 documentation >  Table of contents > Index  Other versions > Options


`NODE_INSPECT_RESUME_ON_START=1 node inspect index.js`： node debug
`node --inpsect index.js`: open v8 inpsector
`repl`: 打印变量
`cont` c: Continue execution
`next` n: Step next
`step` s: Step in
### 7.3 flags

`r`：以只读方式打开文件。如果文件不存在，则抛出异常。

`w`：以写入模式打开文件。如果文件不存在则创建文件，如果文件存在则截断（清空）文件。

`wx`：类似于 w，但如果文件已存在则失败。

`a`：以追加模式打开文件。如果文件不存在则创建文件。

`ax`：类似于 a，但如果文件已存在则失败。

### 7.4 Web Workers

与UI线程分离的后台线程


1. 创建Woker文件， 包含在Worker线程中运行的代码

```js
self.addEventListener('message', function(e){
  // 接受主线程消息
  const result = e.data
})
```

2. 在主线程中使用Worker，再主线程创建worker的实例并与之通信

```js
// 创建 Worker 实例
const getFileNameWorker = new Worker('./worker.js')
// 向 Worker 发送数据
getFileNameWorker.postMessage(file)
// 监听 Worker 发回来的消息
getFileNameWorker.addEventListener('message', e => {
  const filename = e.data
})
```



应用场景 Web Workers 非常适合用于那些需要大量计算且可能会阻塞 UI 的任务，如图像处理、大数据计算、复杂的排序或搜索操作等。通过将这些操作移到后台线程，可以保持前端界面的流畅和响应。

### 7.7 二进制对象



![browserbinary](./assets/browserbinary.png)

这张图是前端 JavaScript 处理二进制数据流和图像的流程图，它详细地展示了不同的 Web API 之间的关系及其用途
左上角: Canvas操作
- canvas: 代表HTML中的`<canvas>`元素, 用于图像绘制
- canvas.getImageData(): 从canvas中提取图像数据
- canvas.toDataURL(): 将canvas中的内容转换为一个 DATA URL (base64编码的字符串)

右上角: 图像
- image: 代表HTML的`<img>`元素
- image.src = dataURL 将Data URL设置为图像的源
- image.src = objectURL 将Object URL设置为图像的源,指向内存中的blob对象

中间: Object URL
- ObjectURL: 通过`URL.createObjectURL(blob)` 创建临时URL，指向传入的Blob或File对象
