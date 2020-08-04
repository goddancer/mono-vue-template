# mono-vue-template

## 食用方式

* root

```json
{
  "private": true,
  "workspaces": {
    "packages": [
      "packages/common",
      "packages/apps/*"
    ],
    "nohoist": [
      "**/mobx"
    ]
  },
  "scripts": {
    "init": "yarn",
    "start:app1": "yarn workspace @local/app1 start",
    "build:app1": "yarn workspace @local/app1 build",
  },
}
```

* packages/apps/app1

```json
{
  "name": "@local/app1",
  "version": "1.0.0",
  "private": true,
  "dependencies": {
    "@local/common": "*",
    "mobx": "^5.15.4",
    "mobx-react-lite": "^2.0.7",
    "react": "^16.13.1",
    "react-dom": "^16.13.1"
  },
}
```
* app1项目中直接从common下引用即可:

```javascript
import { hello } from '@local/common/src/utils/hello'
import { helloService } from '@local/common/services/hello'
```

* packages/common

```json
{
  "name": "@local/common",
  "version": "1.0.0"
}
```

## yarn workspace 安装依赖三种方式

* 给某个package安装|删除[add|remove]依赖：
  - 向dependencies添加依赖：`yarn workspace <project-name> [add|remove] <module-name>`
  - 向devDependencies添加依赖：`yarn workspace <project-name> [add|remove] <module-name> --dev`
* 给所有的package安装依赖：
  - <b>只要执行`yarn workspace`命令的时候，不添加具体的project-name，则默认对所有的project执行该操作</b>
  - `yarn workspace [add|remove] <module-name> [--dev]`
* 给root安装依赖：
  - `yarn [add|remove] -W -D <project-name>`

## 参考

* [yarn workspace使用说明](https://classic.yarnpkg.com/zh-Hans/docs/workspaces/)
* [关于nohoist](https://classic.yarnpkg.com/blog/2018/02/15/nohoist/)
* [workspace有哪些不足和限制](https://hateonion.me/posts/b2b0/)

## 最佳实践

* 存在common-module或npm源各种包之间互相依赖的情况，其中A依赖C，B依赖C，假如C修复一个bug，则需要去A和B将C的依赖升级一下，比较繁琐。
* 此时可以通过workspace的方式，将公用的依赖抽离到common中，通过软连接的方式进行引用，修改某依赖以后，全量build发布一下即可。[lerna也适合做这个事](https://zhuanlan.zhihu.com/p/71385053)
