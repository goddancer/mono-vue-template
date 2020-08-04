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

