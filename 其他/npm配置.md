## `npm`配置淘宝源

1、安装`nrm`，源管理器
```js
npm install -g nrm
```

2、查看可用的源，带`*`是当前使用的源
```js
nrm ls
```

3、切换源
```js
nrm use taobao
```

4、 添加源
```js
nrm add xxx https://xxxx
```

5、删除源
```js
nrm del xxx
```

> Tips：`yarn`配置查看`yarn config`；`yarn`的源管理器`yrm`，安装方式`yarn global add yrm`

## `npm` 缓存清理

`npm`缓存位置
```bash
npm config get cache
```

`npm`缓存清理
* `--force`强制清理，可简写`-f`
```bash
npm cache clean --force
```