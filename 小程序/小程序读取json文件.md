小程序无法直接读取本地`json`文件，想要解决这个问题应该有很多方法

我使用的方法是在在本地维护一个`app.json.ts`文件，通过脚本去编译`app.json.ts`输出小程序需要的`app.json`

脚本文件`app.json.generate.ts`

```ts
// @ts-ignore
const jsonData = require(`./app.json.ts`);
// @ts-ignore
const fs = require('fs');
fs.writeFileSync(`./app.json`, JSON.stringify(jsonData.app))
```

通过`npx ts-node app.json.generate.ts`生成`app.json`