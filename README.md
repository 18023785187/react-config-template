# React项目配置模板

```javascript
// 配合hym-react-cli使用
$ npm i -g hym-react-cli
$ npx hym-react-cli <project_name> --template <template>
// 详细说明看 https://github.com/18023785187/hym-react-cli.git
```

# 启动本地服务

```javascript
$ git clone https://github.com/18023785187/react-config-template.git
$ npm install
$ npm run dll
$ npm run start
```

# 说明

    这是基于create-react-app搭建的模板项目，该模板默认配置了Cesium、dll、stylus，下面会对扩展配置的使用进行简单说明。

## 扩展webpack

```javascript
/*
    node

    react-app-rewired 对webpack配置进行扩展。
                      用法: 在根目录新建 config-overrides.js文件, 然后配置npm命令，详情见package.json。
*/
$ npm i react-app-rewired customize-cra -D

/*
    config-overrides.js
*/
module.exports = function override(...args)
// args[i] = function (config, env) { return config }
// config: react输出的webpack配置对象
// env: 模式

/*
    如果不希望配置文件在根目录并且自定义文件名称，可以在package.json增加如下配置：

    package.json
*/
"config-overrides-path": path // path为你定义的路径，如： './目录/文件名'
```
## 配置dll

在使用的库过多或过大时我们需要配置dll对一些库单独打包以优化构建速度，所以在该模板中配置了dll。

```typescript
$ npm i webpack-cli add-asset-html-webpack-plugin -D
```

```typescript
/*
    dll的基本配置可以在以下文件中找到：
        ./scripts/dll.js    development配置
        ./scripts/dll_bundle.js     production配置
        ./scripts/overrides.js -> dllConfig     引入配置
*/
/*
    用法：
        1. 在dll.js和dll_bundle.js的vendor添加需要单独打包的库
        2. 执行一次npm run dll 和 npm run dll:build
*/
```

## 配置Cesium

./scripts/overrides.js -> cesiumConfig 已经包含了Cesium配置，下面只说明Cesium的静态资源配置：

```typescript
$ npm i cesium -S
$ npm i copy-webpack-plugin -D
```

```typescript
import { Ion } from 'cesium'

// 为当前项目配置Cesium静态资源的根路径
window.CESIUM_BASE_URL = process!.env!.NODE_ENV === 'development' ? devPath : prodPath
// 设置令牌
Ion.defaultAccessToken = xxx
```
