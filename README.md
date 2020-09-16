## 规范的Git提交说明

- 提供更多的历史信息，方便快速浏览
- 可以过滤某些`commit`，便于筛选代码`review`
- 可以追踪`commit`生成更新日志
- 可以关联**issues**

### `Git`提交说明结构

`Git`**提交说明**可分为三个部分：`Header`、`Body`和`Footer`。

```javascript
<Header> <Body> <Footer>
```

### `Header`

`Header`部分包括三个字段`type`（必需）、`scope`（可选）和`subject`（必需）。

```javascript
<type>(<scope>): <subject>ja
```

> Vue源码的**提交说明**省略了`scope`。

#### `type`

`type`用于说明 `commit` 的提交性质。

| 值       | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| feat     | 新增一个功能                                                 |
| fix      | 修复一个bug                                                  |
| docs     | 文档变更                                                     |
| style    | 代码格式（不影响功能，例如空格、分号等格式修正）             |
| refactor | 代码重构                                                     |
| perf     | 改善性能                                                     |
| test     | 测试                                                         |
| build    | 变更项目构建或外部依赖（例如scopes：`webpack`、`gulp`、`npm`等）   |
| ci       | 更改持续集成软件的配置文件和`package`中的`scripts`命令，例如scopes：Travis、Circle等 |
| chore    | 变更构建流程活辅助工具                                       |
| revert   | 代码回退                                                     |

#### `scope`

`scope`说明`commit`影响的范围。`scope`依据项目而定，例如在业务项目中可以依据菜单或者功能模块划分，如果是组件库开发，则可以依据组件划分。

> 提示：`scope`可以省略。

#### `subject`

`subject`是`commit`的简短描述。

### `Body`

`commit`的详细描述，说明代码提交的详细说明。

### `Footer`

如果代码的提交是**不兼容变更**或**关闭缺陷**，则`Footer`必需，否则可以省略。

#### 不兼容变更

当前代码与上一个版本不兼容，则`Footer`以**BREAKING CHANGE**开头，后面是对变动的描述、以及变动的理由和迁移方法。

#### 关闭缺陷

如果当前提交是针对特定的issue，那么可以在`Footer`部分填写需要关闭的单个 issue 或一系列issues。

## Commitizen

[commitizen/cz-cli](https://github.com/commitizen/cz-cli)是一个可以实现规范的**提交说明**的工具：

**When you commit with Commitizen, you'll be prompted to fill out any required commit fields at commit time. No more waiting until later for a git commit hook to run and reject your commit (though that can still be helpful). No more digging through CONTRIBUTING.md to find what the preferred format is. Get instant feedback on your commit message formatting and be prompted for required fields.**

提供选择的提交信息类别，快速生成**提交说明**。安装cz工具:

```bash
npm install -g commitizen
```

## Commitizen适配器

### cz-conventional-changelog

如果需要在项目中使用**commitizen**生成符合AngularJS规范的**提交说明**，初始化**cz-conventional-changelog**适配器：

```bash
commitizen init cz-conventional-changelog --save --save-exact
```

初始化命令主要进行了3件事情

1. 在项目中安装**cz-conventional-changelog** 适配器依赖
2. 将适配器依赖保存到`package.json`的`devDependencies`字段信息
3. 在`package.json`中新增`config.commitizen`字段信息，主要用于配置cz工具的适配器路径：

```json
"devDependencies": {
 "cz-conventional-changelog": "^2.1.0"
},
"config": {
  "commitizen": {
    "path": "./node_modules/cz-conventional-changelog"
  }
}
```

## Commitizen日志

如果使用了[cz](https://github.com/commitizen/cz-cli)工具集，配套[conventional-changelog](https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/conventional-changelog)可以快速生成开发日志：

```bash
npm install conventional-changelog conventional-changelog-cli -D
```

在`pacage.json`中加入生成日志命令：

```json
"version": "conventional-changelog -p angular -i CHANGELOG.md -s -r 0 && git add CHANGELOG.md"
```

You could follow the following workflow

- Make changes
- Commit those changes
- Pull all the tags
- Run the npm version [patch|minor|major] command
- Push

执行`npm run version`后可查看生产的日志[CHANGELOG.md](https://github.com/ziyi2/cz-example/blob/master/CHANGELOG.md)。

> 注意要使用正确的`Header`的`type`，否则生成的日志会不准确，这里只是一个示例，生成的日志不是很严格。

## 配置

```bash
npm install @commitlint/cli @commitlint/config-conventional conventional-changelog conventional-changelog-cli cz-conventional-changelog husky -D
```

创建**commitlint.config.js**

```js
module.exports = {
  extends: ['@commitlint/config-conventional']
}
```

修改**package.json**

```json
"scripts": {
    "version": "conventional-changelog -p angular -i CHANGELOG.md -s -r 0 && git add CHANGELOG.md"
},
"config": {
    "commitizen": {
        "path": "./node_modules/cz-conventional-changelog"
    }
},
"husky": {
    "hooks": {
        "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
    }
}
```

## 提交说明

- git cz
- Select the type of change that you're committing
  - feat:     A new feature
  - fix:      A bug fix
  - docs:     Documentation only changes
  - style:    Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
  - refactor: A code change that neither fixes a bug nor adds a feature
  - perf:     A code change that improves performance
  - test:     Adding missing tests or correcting existing tests
- What is the scope of this change (e.g. component or file name): (press enter to skip)   填写所修改的组件、文件名。**可跳过**
- Write a short, imperative tense description of the change 进行一个简短的描述  max 94
- Provide a longer description of the change 进行一个详细的描述**可跳过**
- Are there any breaking changes? 是否发生重大改变 **可跳过**
  - A BREAKING CHANGE commit requires a body. Please enter a longer description of the commit itself：重大改变的描述

- git pull
- git push

## 参考

[Cz工具集使用介绍 - 规范Git提交说明](https://juejin.im/post/6844903831893966856)

