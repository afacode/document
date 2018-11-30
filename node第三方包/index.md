#### node 第三方包
1. [日期处理类库](http://momentjs.cn/) ``` npm install moment --save ```
2. [xml 与 json 互转](https://www.npmjs.com/package/xml2js) 
``` 
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
3. 