## 异步函数变迁史

const fs = require('fs')

#### 第一阶段，回调函数
```
function readfile(cb){
  fs.readFile('./package.json',(err,data)=>{
    if(err) return cb(err)
    
    cb(null,data)
  })
}

readFile((err,data)=>{
    if(!err) {
     data = JSON.parse(data)
     
     console.log(data.name)
    }
})
```
#### 第二阶段，promise
```
function readFileAsync (path) {
    return new Promise((resolve,reject) => {
        fs.readFile(path,(err,data) => {
            if(err) reject(err)
            else resolve(data)
        })
    })
}

readFileAsync('./package.json')
.then(data=>{
   data = JSON.parse(data)
   console.log(data.name)
})
.catch(err => {
    console.log(err)
})
```
#### co  +  generater function
```
const co = require('co')
const util = require('util')

co(function *() {
    let data=yield util.promisify(fs.readFile)('./package.json')
    data = JSON.parse(data)
    console.log(data.name)
)}
```
#### 第四阶段Async 
```
const readAsync = util.promisify(fs.readFile)

async function init() {
  let data =await readAsync('./package.json')
  data = JSON.parse(data)
  console.log(data.name)
}
```
>1. defer 属性
><script src="file.js" defer></script> 
>defer属性声明这个脚本中将不会有 document.write 或 dom 修改。
>浏览器将会并行下载 file.js 和其它有 defer 属性的script，而不会阻塞页面后续处理。

>每一个async属性的脚本都在它下载结束之后立刻执行，同时会在window的load事件之前执行。所以就有可能出现脚本执行顺序被打乱 的情况；每一个defer属性的脚本都是在页面解析完毕之后，按照原本的顺序执行，同时会在document的DOMContentLoaded之前执 行。
