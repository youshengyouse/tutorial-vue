![vue01a](./imgs_tutorial/vue01a.jpg)

> QQ 共同学习群(**Vue**):  3180555323
>
> QQ 共同学习群(**laravel**): 2922800186
- lerna 官方网站：https://lerna.js.org/
- vue 官方网站：https://cn.vuejs.org/
- vue-cli 官方网站：https://cli.vuejs.org/zh/
- vuex 官方网站：https://vuex.vuejs.org/zh/
- vue-router 官方网站：https://router.vuejs.org/zh/
- Element ui 官方网站：https://element.eleme.cn/#/zh-CN/
- Mint ui 官方网站： http://mint-ui.github.io/#!/zh-cn
- dcloud提供的视频教程: https://learning.dcloud.io/#/


# 准备工作

> 以下使用的是 npm

### 1. 安装 git

下载地址： https://www.git-scm.com/download/win

### 2. 安装 nodejs

下载地址： http://nodejs.cn/download/

### 3. 全局安装 lerna，yarn，@vue/cli

```shell
$ npm install --global lerna yarn @vue/cli  # 耗时约4分钟(新装)，安装过程见01
```

有关vue项目的新建，我分三个部分来讲，分别针对不同水平的学生。其中使用vue create命令新建是必须要掌握，后面两种是可选的。

# 初级篇：使用vue/cli新建vue项目

适合初级玩家，只需要熟悉使用脚手架提供的vue create命令新建项目即可，非常方便，只需要几分种就ok

**目录准备**：从零开始

```shell
$ mkdir tutorial_vue && cd $_ && mkdir 01_vue 02_vuex 03_vue-router 04_element-ui 05_vuepress  node2
# 01_vue  vue教程,以下目录先建好放着，是我的讲课安排
# 02_vuex vuex教程
# 03_vue-router vue-router教程
# 04_element-ui element-ui教程
# 05_vuepress vuepress教程
# node2 本地安装包
```

**新建项目**

```shell
# 以下工作目录全是 F:\tutorial_vue\01_vue
$ vue create lesson01   # 耗时4分15秒，在vscode终端执行，使用vue_tutorial的预置也可手动
$ vue create lesson02   # 耗时1分15秒，安装过程见03

# 启动服务器,当前目录是F:\tutorial_vue\01_vue\lesson01|2
$ yarn serve
# 启动后修改app.vue演示一下，新手可以到此为止下面的可以不用看了。
```



# 中级篇：使用lerna来管理依赖模块

适合中级玩家，先简要说下lerna，它是一个用来管理有多个packages的项目的工具，https://lerna.js.org/#users  列出了好多项目是使用lerna来管理的，象著名的webpack，babel，react, 及我们后面会讲到的vue,element都是使用lerna来管理模块的。所以掌握它也是一个趋势。

**初始化 lerna项目**

```shell
$ lerna init # 当前目录为F:\tutorial_vue，耗时11秒，安装过程及生成内容见02
# 生成package.json，lerna.json，和packages目录
```
修改 lerna.json 如下

```json
{
  "packages": ["0001_vue/*","node2/*"],
  "version": "0.0.0"
}
#packages改为["0001_vue/*","node2/*"]，并删除生成的空目录packages

```

顶层 package.json 如下

```json
{
  "name": "root",
  "private": true,
  "devDependencies": {
    "lerna": "^3.20.2"
  }
}
# 不用修改
```
**使用lerna安装依赖**

```shell
$ lerna clean           # 耗时1分40秒，删除上面所有包中的node-modules
# $ lerna link --force-local   # 会在各个同级包中建立symbol link，打开node_modules中可以看到
$ lerna bootstrap --hoist  # 上面2个项目耗时3分20秒,安装过程见05，--hoist选项，所有公共的依赖都只会安装在根目录的node_modules中,不会在每个包目录下的node_modules中都保留各自的依赖包
```

**启动指定包**

```shell
$ lerna run serve --scope=lesson01 --stream # 启动耗时19秒，启动记录见安装过程04
#$ lerna run serve --concurrency 2 --stream  # 同时启动2个个服务都
# 如果出错，请3删，清缓存，删node_modules目录，删package-lock.json
```



# 高级篇：本地安装依赖包

为了更好的研究和自定制vue,vuex,vue-router,element-ui等库，建议本地安装它们，当然自己也的私有包也可使用这种方法安装，不用publish到npm 仓库中

**下载源码**，不再从仓库中下载了

```shell
# 本地node包的目录是F:\tutorial_vue\node2(名字位置随意)
$ git clone git@github.com:vuejs/vue.git         # 36M耗时51秒,版本2.6.11
$ git clone git@github.com:vuejs/vue-next.git    # 3.0.0-alpha.4
$ git clone git@github.com:vuejs/vuex.git        # 11M耗时10秒，版本3.1.2
$ git clone git@github.com:vuejs/vue-router.git  # 21M耗时19秒，版本3.1.5
$ git clone git@github.com:ElemeFE/element.git   # 82M耗时27秒(今天好快，达4M/s,上午10点快)
```

**源码打包**

```shell
#----------------------element-ui打包开始
# 参考官方 https://github.com/ElemeFE/element/blob/master/.github/CONTRIBUTING.zh-CN.md
$ npm install --verbose # 约3分钟,安装依赖,当前目录F:\tutorial_vue\node2\element
$ npm run dist   # 约1分30秒
$ npm run dev    # 将package.json中dev删除npm run bootstrap &&掉，3分多钟打开是element-ui官网内容
#----------------------element-ui打包结束
# vue,vuex,vue-router三个包源码中已有打包的的目录，可省略下面的打包步骤

#----------------------vue打包开始
$ npm install --verbose # 安装依赖,当前目录F:\tutorial_vue\node2\vue
$ npm run build    # 版本是2.6.11
#----------------------vue打包结束

#----------------------vuex打包开始
$ npm install --verbose # 安装依赖,当前目录F:\tutorial_vue\node2\vuex
$ npm run build   # 版本是3.1.2
$ npm run dev # 这里有官方提供的5个例子，会在后续的课程里讲
#----------------------vuex打包结束

#----------------------vue-router打包开始
$ npm install --verbose # 安装依赖,当前目录F:\tutorial_vue\node2\vue-router
$ npm run build   # 版本是3.1.5
$ npm run dev # 这里有官方提供的20个例子，会在后续的专门课程里讲
#----------------------vue-router打包结束
```

修改各个package中的 package.json 中相关依赖的版本

```shell
# 修改了3处
{
  "name": "lesson01",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint"
  },
  "dependencies": {
    "core-js": "^3.4.4",
    "vue": "^2.6.11",                               # 此处原来是2.6.10
    "vue-router": "^3.1.5",                         # 此处原来是3.1.2
    "vuex": "^3.1.2"
  },
  "devDependencies": {
    "@vue/cli-plugin-babel": "^4.1.0",
    "@vue/cli-plugin-eslint": "^4.1.0",
    "@vue/cli-plugin-router": "^4.1.0",
    "@vue/cli-plugin-vuex": "^4.1.0",
    "@vue/cli-service": "^4.1.0",
    "@vue/eslint-config-prettier": "^5.0.0",
    "babel-eslint": "^10.0.3",
    "eslint": "^5.16.0",
    "eslint-plugin-prettier": "^3.1.1",
    "eslint-plugin-vue": "^5.0.0",
    "node-sass": "^4.12.0",
    "prettier": "^1.19.1",
    "sass-loader": "^8.0.0",
    "vue-template-compiler": "^2.6.11"                # 此处原来是2.6.10
  }
}

```

重新安装各个package中的依赖

```shell
# 清除相应文件和目录
$ lerna clean           # 耗时1分40秒，删除上面2个包中的node-modules
$ rm -rf node_modules
$ rm package-lock.json
$ npm cache clean --force
$ lerna link convert # 这一步很关键，见下面详细描述

$ lerna bootstrap --hoist
$ lerna run serve --scope=lesson01 --stream

# 也可以启动各个本地包，方便源码的研究和教学
$ lerna run dev --scope=vuex --stream
```

总结：本节课的内容比较简单，从下节课开始进入vue  3.0的使用













---------------------------------以下部分不是教程的内容，可忽略不看----------------------------------------

# 安装过程

### 01：全局安装 lerna，yarn，@vue/cli

```shell
$ npm install --global lerna yarn @vue/cli
npm WARN deprecated core-js@2.6.11: core-js@<3 is no longer maintained and not recommended for usage due to the number of issues. Please, upgrade your dependencies to the actual version of core-js@3.
C:\Users\Administrator\AppData\Roaming\npm\yarn -> C:\Users\Administrator\AppData\Roaming\npm\node_modules\yarn\bin\yarn.js
C:\Users\Administrator\AppData\Roaming\npm\yarnpkg -> C:\Users\Administrator\AppData\Roaming\npm\node_modules\yarn\bin\yarn.js
C:\Users\Administrator\AppData\Roaming\npm\vue -> C:\Users\Administrator\AppData\Roaming\npm\node_modules\@vue\cli\bin\vue.js
C:\Users\Administrator\AppData\Roaming\npm\lerna -> C:\Users\Administrator\AppData\Roaming\npm\node_modules\lerna\cli.js
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.11 (node_modules\@vue\cli\node_modules\fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.11: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})

+ @vue/cli@4.1.2
+ lerna@3.20.2
+ yarn@1.21.1
removed 1 package and updated 3 packages in 55.988s

```

### 02：lerna 初始化

```shell
$ lerna init
lerna notice cli v3.20.2
lerna info Creating package.json
lerna info Creating lerna.json
lerna info Creating packages directory
lerna success Initialized Lerna files

# 查看生成的目录和文件
$ tree
.
|-- lerna.json
|-- package.json
`-- packages

1 directory, 2 files
```

### 03：vue create 生成新项目

```shell
$ vue create lesson01


Vue CLI v4.1.2
? Please pick a preset: vue-tutorial (node-sass, babel, router, vuex, eslint)


Vue CLI v4.1.2
✨  Creating project in F:\tutorial_vue\01_vue\lesson01.
⚙  Installing CLI plugins. This might take a while...

yarn install v1.21.1
info No lockfile found.
[1/4] Resolving packages...
[2/4] Fetching packages...
info fsevents@1.2.11: The platform "win32" is incompatible with this module.
info "fsevents@1.2.11" is an optional dependency and failed compatibility check. Excluding it from installation.


success Saved lockfile.
Done in 154.84s.
�🚀  Invoking generators...
�📦  Installing additional dependencies...

yarn install v1.21.1
[1/4] Resolving packages...
[2/4] Fetching packages...
info fsevents@1.2.11: The platform "win32" is incompatible with this module.
info "fsevents@1.2.11" is an optional dependency and failed compatibility check. Excluding it from installation.
[3/4] Linking dependencies...
[4/4] Building fresh packages...
success Saved lockfile.
Done in 49.46s.
⚓  Running completion hooks...

�📄  Generating README.md...

�🎉  Successfully created project lesson01.
�👉  Get started with the following commands:

 $ cd lesson01
 $ yarn serve
```

### 04：启动服务 lerna run serve --scope=lesson01 --stream

```shell
Administrator@MBB2019 MINGW64 /f/tutorials_vue (master)
$ lerna run serve --scope=lesson01 --stream
info cli using local version of lerna
lerna notice cli v3.20.2
lerna notice filter including "lesson01"
lerna info filter [ 'lesson01' ]
lerna info Executing command in 1 package: "npm run serve"
lesson01: > lesson01@0.1.0 serve F:\tutorials_vue\01_vue\lesson01
lesson01: > vue-cli-service serve
lesson01:  INFO  Starting development server...
lesson01: <s> [webpack.Progress] 0% compiling
lesson01: <s> [webpack.Progress] 10% building 0/0 modules 0 active
lesson01: <s> [webpack.Progress] 10% building 0/1 modules 1 active multi F:\tutorials_vue\node_modules\webpack-dev-server\client\index.js?http://192.168.0.102:8080/sockjs-node
...过程略
lesson01: <s> [webpack.Progress] 95% emitting HtmlWebpackPlugin
lesson01: <s> [webpack.Progress] 95% emitting CopyPlugin
lesson01: <s> [webpack.Progress] 98% after emitting
lesson01: <s> [webpack.Progress] 98% after emitting CopyPlugin
lesson01:  DONE  Compiled successfully in 7937ms9:57:27
lesson01: <s> [webpack.Progress] 100%
lesson01:   App running at:
lesson01:   - Local:   http://localhost:8080/
lesson01:   - Network: http://192.168.0.102:8080/
lesson01:   Note that the development build is not optimized.
lesson01:   To create a production build, run yarn build.
```

### 05：安装所有包的依赖 lerna bootstrap --hoist

```shell
$ lerna bootstrap --hoist
lerna notice cli v3.20.2
lerna info Bootstrapping 2 packages
lerna info Installing external dependencies
lerna info hoist Installing hoisted dependencies into root
lerna info hoist Pruning hoisted dependencies
lerna info hoist Finished pruning hoisted dependencies
lerna info hoist Finished bootstrapping root
lerna info Symlinking packages and binaries
lerna success Bootstrapped 2 packages
```

### 06：windows-build-tools 安装过程

```
Microsoft Windows [版本 10.0.18362.329]
(c) 2019 Microsoft Corporation。保留所有权利。

C:\Windows\system32>npm install --global --production windows-build-tools

> windows-build-tools@5.2.2 postinstall C:\Users\Administrator\AppData\Roaming\npm\node_modules\windows-build-tools
> node ./dist/index.js

Downloading python-2.7.15.amd64.msi
[============================================>] 100.0% of 20.25 MB (9 MB/s)
Downloaded python-2.7.15.amd64.msi. Saved to C:\Users\Administrator\.windows-build-tools\python-2.7.15.amd64.msi.
Downloading vs_BuildTools.exe
[>                                            ] 0.0% (0 B/s)
Downloaded vs_BuildTools.exe. Saved to C:\Users\Administrator\.windows-build-tools\vs_BuildTools.exe.

Starting installation...
Launched installers, now waiting for them to finish.
This will likely take some time - please be patient!

Status from the installers:
---------- Visual Studio Build Tools ----------
Successfully installed Visual Studio Build Tools.
------------------- Python --------------------

Now configuring the Visual Studio Build Tools and Python...

All done!

+ windows-build-tools@5.2.2
updated 1 package in 74.589s
```
### 07：`lerna link convert`后的变化

```json
# 根package.json由
{
  "name": "root",
  "private": true,
  "devDependencies": {
    "lerna": "^3.20.2"
  }
}
# 变成
{
  "name": "root",
  "private": true,
  "devDependencies": {
    "@babel/core": "^7.0.0",
    "@babel/plugin-proposal-class-properties": "^7.1.0",
    "@babel/plugin-syntax-dynamic-import": "^7.0.0",
    "@babel/plugin-syntax-jsx": "^7.0.0",
    "@babel/plugin-transform-flow-strip-types": "^7.0.0",
    "@babel/preset-env": "^7.0.0",
    "@babel/register": "^7.0.0",
    "@types/node": "^12.12.0",
    "@types/webpack": "^4.4.22",
    "@vue/cli-plugin-babel": "^4.1.0",
    "@vue/cli-plugin-eslint": "^4.1.0",
    "@vue/cli-plugin-router": "^4.1.0",
    "@vue/cli-plugin-vuex": "^4.1.0",
    "@vue/cli-service": "^4.1.0",
    "@vue/component-compiler-utils": "^2.6.0",
    "@vue/eslint-config-prettier": "^5.0.0",
    "acorn": "^5.2.1",
    "algoliasearch": "^3.24.5",
    "axios": "^0.19.0",
    "babel-cli": "^6.26.0",
    "babel-core": "^6.22.1",
    "babel-eslint": "^10.0.1",
    "babel-helper-vue-jsx-merge-props": "^2.0.3",
    "babel-loader": "^7.1.2",
    "babel-plugin-add-module-exports": "^0.2.1",
    "babel-plugin-istanbul": "^5.1.0",
    "babel-plugin-module-resolver": "^2.2.0",
    "babel-plugin-syntax-dynamic-import": "^6.18.0",
    "babel-plugin-syntax-jsx": "^6.18.0",
    "babel-plugin-transform-object-rest-spread": "^6.23.0",
    "babel-plugin-transform-vue-jsx": "^4.0.1",
    "babel-polyfill": "^6.22.0",
    "babel-preset-env": "^1.5.1",
    "babel-preset-flow-vue": "^1.0.0",
    "babel-preset-stage-2": "^6.24.1",
    "babel-regenerator-runtime": "^6.5.0",
    "browserstack-local": "^1.4.0",
    "buble": "^0.19.3",
    "chai": "^4.2.0",
    "chalk": "^2.3.0",
    "chokidar": "^1.7.0",
    "chromedriver": "^78.0.1",
    "codecov": "^3.0.0",
    "commitizen": "^2.9.6",
    "conventional-changelog": "^1.1.3",
    "conventional-changelog-cli": "^2.0.11",
    "copy-webpack-plugin": "^5.0.0",
    "coveralls": "^3.0.3",
    "cp-cli": "^1.0.2",
    "cross-env": "^5.2.0",
    "cross-spawn": "^6.0.5",
    "css-loader": "^2.1.0",
    "cz-conventional-changelog": "^2.0.0",
    "de-indent": "^1.0.2",
    "dotenv": "^8.0.0",
    "es6-promise": "^4.1.0",
    "escodegen": "^1.8.1",
    "eslint": "^5.12.0",
    "eslint-config-elemefe": "0.1.1",
    "eslint-loader": "^2.0.0",
    "eslint-plugin-flowtype": "^2.34.0",
    "eslint-plugin-html": "^4.0.1",
    "eslint-plugin-jasmine": "^2.8.4",
    "eslint-plugin-json": "^1.2.0",
    "eslint-plugin-prettier": "^3.1.1",
    "eslint-plugin-vue": "^5.0.0",
    "eslint-plugin-vue-libs": "^3.0.0",
    "express": "^4.14.1",
    "express-urlrewrite": "^1.2.0",
    "file-loader": "^3.0.1",
    "file-save": "^0.2.0",
    "flow-bin": "^0.61.0",
    "geckodriver": "^1.16.2",
    "gulp": "^4.0.0",
    "gulp-autoprefixer": "^6.0.0",
    "gulp-cssmin": "^0.2.0",
    "gulp-sass": "^4.0.2",
    "hash-sum": "^1.0.2",
    "he": "^1.1.1",
    "highlight.js": "^9.3.0",
    "html-webpack-plugin": "^3.2.0",
    "http-server": "^0.11.1",
    "jasmine": "2.8.0",
    "jasmine-core": "2.8.0",
    "json-loader": "^0.5.7",
    "json-templater": "^1.0.4",
    "karma": "^3.1.1",
    "karma-chrome-launcher": "^2.1.1",
    "karma-coverage": "^1.1.1",
    "karma-firefox-launcher": "^1.0.1",
    "karma-jasmine": "^1.1.0",
    "karma-mocha": "^1.3.0",
    "karma-mocha-reporter": "^2.2.3",
    "karma-phantomjs-launcher": "^1.0.4",
    "karma-safari-launcher": "^1.0.0",
    "karma-sauce-launcher": "^2.0.2",
    "karma-sinon-chai": "^2.0.2",
    "karma-sourcemap-loader": "^0.3.7",
    "karma-spec-reporter": "^0.0.32",
    "karma-webpack": "^4.0.0-rc.2",
    "lerna": "^3.20.2",
    "lint-staged": "^8.0.0",
    "lodash": "^4.17.4",
    "lodash.template": "^4.4.0",
    "lodash.uniq": "^4.5.0",
    "lru-cache": "^5.1.1",
    "markdown-it": "^8.4.1",
    "markdown-it-anchor": "^5.0.2",
    "markdown-it-chain": "^1.3.0",
    "markdown-it-container": "^2.0.0",
    "mini-css-extract-plugin": "^0.4.1",
    "mocha": "^6.0.2",
    "nightwatch": "^1.3.1",
    "nightwatch-helpers": "^1.2.0",
    "node-sass": "^4.11.0",
    "optimize-css-assets-webpack-plugin": "^5.0.1",
    "path-to-regexp": "^1.7.0",
    "phantomjs-prebuilt": "^2.1.14",
    "postcss": "^7.0.14",
    "prettier": "^1.19.1",
    "progress-bar-webpack-plugin": "^1.11.0",
    "puppeteer": "^1.11.0",
    "resolve": "^1.3.3",
    "rimraf": "^2.5.4",
    "rollup": "^1.1.0",
    "rollup-plugin-alias": "^1.3.1",
    "rollup-plugin-buble": "^0.19.6",
    "rollup-plugin-commonjs": "^9.2.0",
    "rollup-plugin-flow-no-whitespace": "^1.0.0",
    "rollup-plugin-node-resolve": "^4.0.0",
    "rollup-plugin-replace": "^2.1.0",
    "rollup-watch": "^4.0.0",
    "sass-loader": "^7.1.0",
    "select-version-cli": "^0.0.2",
    "selenium-server": "^2.53.1",
    "serialize-javascript": "^2.1.2",
    "shelljs": "^0.8.1",
    "sinon": "^7.2.7",
    "sinon-chai": "^3.3.0",
    "style-loader": "^0.23.1",
    "terser": "^3.17.0",
    "todomvc-app-css": "^2.1.0",
    "transliteration": "^1.1.11",
    "typescript": "^3.7.2",
    "uglifyjs-webpack-plugin": "^2.1.1",
    "uppercamelcase": "^1.1.0",
    "url-loader": "^1.0.1",
    "vue": "file:../vue",
    "vue-loader": "^15.2.1",
    "vue-router": "^3.0.1",
    "vue-template-compiler": "^2.5.22",
    "vue-template-es2015-compiler": "^1.6.0",
    "vuepress": "^0.14.1",
    "vuepress-theme-vue": "^1.1.0",
    "webpack": "^4.8.3",
    "webpack-cli": "^3.0.8",
    "webpack-dev-middleware": "^1.10.0",
    "webpack-dev-server": "^3.1.11",
    "webpack-hot-middleware": "^2.19.1",
    "webpack-node-externals": "^1.7.2",
    "weex-js-runtime": "^0.23.6",
    "weex-styler": "^0.3.0",
    "yorkie": "^2.0.0"
  },
  "dependencies": {
    "element-ui": "file:node2\\element",
    "lesson01": "file:0001_vue\\lesson01",
    "lesson02": "file:0001_vue\\lesson02",
    "vue": "file:node2\\vue",
    "vue-router": "file:node2\\vue-router",
    "vuex": "file:node2\\vuex"
  }
}
# F:\tutorials\0001_vue\lesson01\package.json 由
{
  "name": "lesson01",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint"
  },
  "dependencies": {
    "core-js": "^3.4.4",
    "vue": "^2.6.10",
    "vue-router": "^3.1.3",
    "vuex": "^3.1.2"
  },
  "devDependencies": {
    "@vue/cli-plugin-babel": "^4.1.0",
    "@vue/cli-plugin-eslint": "^4.1.0",
    "@vue/cli-plugin-router": "^4.1.0",
    "@vue/cli-plugin-vuex": "^4.1.0",
    "@vue/cli-service": "^4.1.0",
    "@vue/eslint-config-prettier": "^5.0.0",
    "babel-eslint": "^10.0.3",
    "eslint": "^5.16.0",
    "eslint-plugin-prettier": "^3.1.1",
    "eslint-plugin-vue": "^5.0.0",
    "node-sass": "^4.12.0",
    "prettier": "^1.19.1",
    "sass-loader": "^8.0.0",
    "vue-template-compiler": "^2.6.10"
  }
}
# 变成了
{
  "name": "lesson01",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint"
  },
  "dependencies": {
    "core-js": "^3.4.4",
    "vue": "file:../../node2/vue",
    "vue-router": "file:../../node2/vue-router",
    "vuex": "file:../../node2/vuex"
  }
}

# vuex/package.json中的devDependencies部分全部移到了根package.json下
# vue-router/package.json中的devDependencies只留下了{"vue": "file:../vue"}，其余转到了根package.json中
# vue/package.json中的devDependencies移到了根package.json中
# element/package.json中的dependencies，peerDependencies没有变化，但devDependencies变为了 {"vue-router": "file:../vue-router"}

```

# 填坑过程

### 坑 1：node-sass 安装失败

- 第 1 步，打开--verbose 才能真正找到问题出在哪里,本坑是因为找不到对应的 win32-x64-72_binding.node

  ```shell
  $ npm install --verbose
  ```

- 第 2 步，看 node-sass 官网,windows 缺少编译的运行环境，在 win 下安装 node-gyp 和 windows-build-tools, https://github.com/nodejs/node-gyp#on-windows

  ```shell
  $ npm install --global node-gyp
  $ npm install --global --production windows-build-tools # 以管理员身份安装，这一步会安装python
  ```

- 第 3 步：清缓存和删除 node_modules 目录

  ```shell
  $ rm -rf node_modules
  $ rm package-lock.json
  $ npm cache clean
  ```

### 坑 2：element 安装依赖时

在 element 包中执行 npm link 后发现报下面错误，再看看 npm install 也发现这个问题

```shell
npm ERR! Cannot read property 'match' of undefined
```

```shell
$ npm cache clear --force
$ rm -rf node_modules
$ rm package-lock.json
# 执行上面三句后再执行 npm install 或 npm link 就正常
$ npm install # 耗时2分26秒
```


# 编辑器准备

- Vscode: js,css,html 等前端语言编辑器(eslint+prettier)
- phpstorm: php 编辑器

### A: VSCode 配置

#### 格式化和校验

- 安装扩展 eslint: 在扩展中搜 eslint，安装即可，扩展 eslint 是基于 eslint 这个 node 包工作的，可项目或全局安装 eslint 包
- 安装扩展 Prettier - Code formatter：这个扩展要使用到 node 包 prettier，所以得全局或项目内安装它
- 安装扩展 EditorConfig for VS Code: 参考 `https://blog.csdn.net/Gabriel_wei/article/details/90286668`

```shell
$ npm install -g eslint            # 方便所有项目都使用
$ npm install -g prettier          # 扩展 Prettier - Code formatter 要用到它
$ npm install -g editorconfig
```

#### 其它

- 终端 shell 环境配置: `"terminal.integrated.shell.windows": "D:\\01Program Files\\Git\\bin\\bash.exe",`
- 安装自己喜欢的主题: 我装 AS Roma Dark，在扩展中搜 theme，出来一堆，网上能搜到好多主题推荐，在`https://marketplace.visualstudio.com/search?target=VSCode&category=Themes&sortBy=Installs`上搜到好多
- vetur 扩展：是开发 vue 必装扩展
- Chinese Lorem：输入 jw 按 tab 就可以生成假文字，是汉字的，比较好用，jw50 表示生成 50 个汉字
- Bracket Pair Colorizer: 这个实用，不同级别的括号使用不同的颜色区分，它有 2 个版本，我装的是第 2 版
- bracket-padder: 这个是为了提高效率用，会成对输入括号{}{}()
- open in browser: 可右键打开浏览器
- javascript(es6) code snippets，快速输入代码用
- 激活`emmet`，在配置中搜`emmet`，勾选 `trigger expansion on tab`

### B:PHPStorm 配置

- 终端 shell 环境配置: File -> Setting ->Tools -> Terminal，将 shell path 改为`D:\01Program Files\Git\bin\bash.exe`，并安装`native terminal`
- 安装自己喜欢的主题: File -> Setting -> Plugins 中 输入 theme 就可以搜到一堆主题，我一下子安装了 10 个主题
- ssh 配置：远程连接服务器，`Tools -> Deployment -> Configuration`
- 数据库配置:`View -> Tool Window -> Database`,第一次会提示下载相应的驱动



# 镜像改为淘宝

```shell
$ npm config set registry https://registry.npm.taobao.org
$ npm config list 查看配置
$ yarn config set registry https://registry.npm.taobao.org -g
$ yarn config list
```





# 参考部分(待整理)

lerna 用法参考: https://www.jianshu.com/p/f105e1427082

lerna 用法参考: https://www.jianshu.com/p/8b7e6025354b

lerna link 用法：https://stackoverflow.com/questions/49037987/allow-local-project-to-depend-on-local-lerna-packages?r=SearchResults

lerna 的 add 方法

```bash
# 给a, b 包中加入Lodash，会同时改变a,b模块中packages.json文件
lerna add lodash packages/a packages/b
# 给a 包中加入jquery, 使用--dev参数是使依赖加入到devDependencies中
lerna add jquery packages/a --dev
# 你也可以使用通配符, 下面这命令，会往所有re开头的模块包中加入依赖
lerna add jquery packages/re-*
# 指定特定的范围，要使用--scope参数，如下：给b包安装a模块
lerna add a --scope=b
```

yarn 包提升 https://github.com/Hy-Vee/lerna-yarn-workspaces-monorepo
npm link 的用法

```shell
npm link   // 或 yarn link，在希望同步开发的组件包下执行（假设为component-a）
```

```
npm link component-a// 或 yarn link component-a
```

yarn workspaces 的用法

在 package.json 中增加 workspaces 字段，写入同目录下的目录名，然后在 dependencies 中指定 workspaces 中指定的目录下的包名，最后执行

```
yarn// 或 yarn install 只适合" private": true的项目
```

安装本地包的方法

```json
# 修改F:\tutorial_vue\package.json如下，使用本地依赖包
{
  "name": "root",
  "private": true,
  "devDependencies": {
    "lerna": "^3.20.2"
  },
   "dependencies": {
    "vue": "file:node2\\vue",
    "vuex": "file:node2\\vuex",
    "vue-router": "file:node2\\vue-router",
    "element-ui": "file:node2\\element"
  }
}
```
