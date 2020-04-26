---
layout:	post
title:	VSCode开发Vue的一些设置
author: StoneLi
description: VSCode开发Vue的一些设置
tags: [vue, vscode]
type: vue
---

> 本文主要讲解VSCode开发Vue的一些设置

1. VSCode使用```vetur```格式化Vue代码单引号变双引号, 末尾分号解决方法：
 * 在设置中搜索 ```vetur```
 * 编辑```settings.json```添加以下配置:
   ```json
    {
        "[vue]": {
            "editor.defaultFormatter": "octref.vetur" // 使用 vetur 格式化规则
        },
        "vetur.format.defaultFormatterOptions": {
            "prettier": {
                "semi": false, // 去掉分号
                "singleQuote": true, // true 为使用单引号
            },
        },
        "vetur.format.defaultFormatter.js": "vscode-typescript", // js 使用 typescript
        "vetur.format.defaultFormatter.html": "js-beautify-html", // html 使用 beautify
        "javascript.format.insertSpaceBeforeFunctionParenthesis": true // 函数名字和括号前加空格
    }
   ```

2. eslint检查语法时会要求方法和括号之间留一个空格以及上面1中的问题, 在项目中的```.eslintrc.js```文件中设置一下:
```js
module.exports = {
  rules: {
    'quotes': [1, 'single'], //引号类型 `` "" ''
    'semi': [2, 'never'], // 语句强制分号结尾
    'space-before-function-paren': [0, 'always'] //函数定义时括号前面要不要有空格
  }
}
```
