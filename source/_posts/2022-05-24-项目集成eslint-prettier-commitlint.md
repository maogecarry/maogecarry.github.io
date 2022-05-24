---
title: '项目集成eslint,prettier,commitlint'
date: 2022-05-24 17:24:37
tags:
		- 'JavaScript'
---

# 项目集成eslint，prettier，commitlint

## 一. 代码规范

<!--more-->
### 1.1. 集成editorconfig配置

EditorConfig 有助于为不同 IDE 编辑器上处理同一项目的多个开发人员维护一致的编码风格。

```yaml
# http://editorconfig.org

root = true

[*] # 表示所有文件适用
charset = utf-8 # 设置文件字符集为 utf-8
indent_style = space # 缩进风格（tab | space）
indent_size = 2 # 缩进大小
end_of_line = lf # 控制换行类型(lf | cr | crlf)
trim_trailing_whitespace = true # 去除行首的任意空白字符
insert_final_newline = true # 始终在文件末尾插入一个新行

[*.md] # 表示仅 md 文件适用以下规则
max_line_length = off
trim_trailing_whitespace = false
```

VSCode需要安装一个插件：EditorConfig for VS Code

### 1.2. 使用prettier工具

1.安装prettier

```shell
npm install prettier -D
```

2.配置.prettierrc文件：

* useTabs：使用tab缩进还是空格缩进，选择false；
* tabWidth：tab是空格的情况下，是几个空格，选择2个；
* printWidth：当行字符的长度，推荐80，也有人喜欢100或者120；
* singleQuote：使用单引号还是双引号，选择true，使用单引号；
* trailingComma：在多行输入的尾逗号是否添加，设置为 `none`；
* semi：语句末尾是否要加分号，默认值true，选择false表示不加；

```json
{
  "useTabs": false,
  "tabWidth": 2,
  "printWidth": 80,
  "singleQuote": true,
  "trailingComma": "none",
  "semi": false
}
```

3.创建.prettierignore忽略文件

```
/dist/*
.local
.output.js
/node_modules/**

**/*.svg
**/*.sh

/public/*
```

4.VSCode需要安装prettier的插件

5.测试prettier是否生效

* 测试一：在代码中保存代码；
* 测试二：配置一次性修改的命令；

在package.json中配置一个scripts：

```json
    "prettier": "prettier --write ."
```

### 1.3. 使用ESLint检测

1.VSCode需要安装ESLint插件

2.解决eslint和prettier冲突的问题：

安装插件：

```shell
npm i eslint eslint-plugin-prettier eslint-config-prettier -D
```

设置eslint内部配置

```shell
npm init @eslint/config
```

选择完这些配置之后，eslint会生成一个.eslintrc.js的文件

增加.eslintignore文件

```
node_modules
package.json
package-lock.json
```

添加prettier插件：

```json
  extends: [
    "some-other-config-you-use",
    "prettier"
  ],
```

### 1.4. git Husky和eslint

我们希望保证代码仓库中的代码都是符合eslint规范的，我们需要在执行 `git commit ` 命令的时候对其进行校验，如果不符合eslint规范，那么自动通过规范进行修复；

那么如何做到这一点呢？可以通过Husky工具：

* husky是一个git hook工具，可以帮助我们触发git提交的各个阶段：pre-commit、commit-msg、pre-push

如何使用husky呢？

这里我们可以使用自动配置命令：

```shell
npx husky-init && npm install
```

这里会做三件事：

1.安装husky相关的依赖

2.在项目目录下创建 `.husky` 文件夹：

```shell
npx huksy install
```

3.在package.json中添加两个脚本：

```json
"prepare": "husky install",
"lint": "eslint --ext .js,.ts,.vue.jsx,.tsx src",
```

4.在husky文件夹中加入两个文件

```bash
pre-commit
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npm run lint
```

```bash
commit-msg
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npx --no-install commitlint --edit "$1"
```

### 1.5. git commit规范

#### 1.5.1. 代码提交风格

git commit按照统一的风格来提交，这样可以快速定位每次提交的内容，方便之后对版本进行控制。

使用工具：`Commitizen`

1.安装 @commitlint/config-conventional 和 @commitlint/cli

```shell
npm i @commitlint/config-conventional @commitlint/cli -D
```

2.在根目录创建commitlint.config.js文件，配置commitlint

```js
module.exports = {
  extends: ['@commitlint/config-conventional']
}
```

3.使用husky生成commit-msg文件，验证提交信息：

这个就是上面新增的husky文件的命令行方式，但是在执行时会出现问题，目前还没解决。

```shell
npx husky add .husky/commit-msg "npx --no-install commitlint --edit $1"
```


