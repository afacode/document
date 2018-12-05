### node 第三方包
1. [momentjs](http://momentjs.cn/) 日期处理类库 
2. [xml2js](https://www.npmjs.com/package/xml2js) xml 与 json 互转
```js 
const xml2js = require('xml2js')
module.exports = {
  xml2Json: xml => {
    return new Promise((resolve, reject) => {
      const parseString = xml2js.parseString

      parseString(xml, (err, result) => {
        if (err) reject(err)
        else resolve(result)
      })
    })
  },
  json2Xml = obj => {
    const builder = new xml2js.Builder();
    return builder.buildObject(obj);
  }
} 
```
3. [Clipboard.js](https://github.com/zenorocha/clipboard.js) javscript内容复制
4. [http-errors](./http-errors.md) node 错误处理