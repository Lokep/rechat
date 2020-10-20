# 项目搭建过程

一、`create-react-app`构建`TypeScript`项目

```bash
  yarn create react-app react-admin-demos --template typescript
```
然后我们进入项目并启动

```bash
  cd react-admin-demos/
  yarn start
```

项目启动成功，浏览器会默认打开http://localhost:3000/，看到官方实例就算成功了。

使用`create-app-rewired`对`create-react-app` 进行自定义配置，同时还需要安装`customize-cra`：

```bash
  yarn add react-app-rewired customize-cra
```

修改package.json文件配置：

```json
"scripts": {
-   "start": "react-scripts start",
+   "start": "react-app-rewired start",
-   "build": "react-scripts build",
+   "build": "react-app-rewired build",
-   "test": "react-scripts test",
+   "test": "react-app-rewired test",
}
```
然后再安装`babel-plugin-import`，是一个按需加载组件和代码样式的babel插件。

```bash
yarn add babel-plugin-import
```

安装成功后，在根目录下新建config-overrides.js 用于修改默认配置。
```javascript
const { 
    override, 
    fixBabelImports,
} = require('customize-cra');

// 使用ant-design搭建React+ts项目，可在此重重定义antd全局样式
  const overConfig  = override(
    fixBabelImports('import', {
      libraryName: 'antd',
      libraryDirectory: 'es',
      style: 'css',
    })
  )

  module.exports = function (config, env) {
    return overConfig(config, env)
  }
```

现在antd的组件的js和css代码都会在项目中按需加载。


自定义主题

自定义主题需要用到 less 变量覆盖功能。我们可以引入 customize-cra 中提供的 less 相关的函数 addLessLoader 来帮助加载 less 样式，同时修改 config-overrides.js 文件如下。


```javascript
- const { override, fixBabelImports } = require('customize-cra');
+ const { override, fixBabelImports, addLessLoader } = require('customize-cra');

const overConfig  = override(
    fixBabelImports('import', {
        libraryName: 'antd',
        libraryDirectory: 'es',
-       style: 'css',
+      style: true,
    }),
+  addLessLoader({
+     javascriptEnabled: true,
+     modifyVars: { '@primary-color': '#009688' },
+  }),
);
```


接口代理
在React中我们使用http-proxy-middleware来解决接口代理的问题

```javascript
  yarn add http-proxy-middleware
```