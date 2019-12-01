---
layout:	post
title:	VSCode开发Vue的一些设置
author: StoneLi
description: VSCode开发Vue的一些设置
tags: [vue, vscode]
type: vue
---

> 本文主要讲解VSCode开发Vue的一些设置

1. VSCode使用```vetur```格式化Vue代码单引号变双引号解决方法：
 * 在设置中搜索 ```vetur```
 * 编辑```settings.json```添加以下配置:
   ```json
   "vetur.format.defaultFormatterOptions": {
     "prettier": {
       "semi": false,
       "singleQuote": true
     }
   }
   ```

2. eslint检查语法时会要求方法和括号之间留一个空格, 在项目中的```.eslintrc.js```文件中设置一下:
```js
module.exports = {
  rules: {
    "space-before-function-paren": 0
  }
}
```
