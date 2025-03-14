## 运行时工具postCss与Browserslist

​		最近一个项目报错，在本地运行时正常，云桌面install包正常，但运行vite有以下报错

```
[plugin:vite:css] [postcss] Unknown browser query basedir=$(dirname " $ （ hO " $ | sed 's'. Maybe you are using old Browserslist or made typo in query. 
```

<b>前端开发环境工具链</b>

​		在开发环境，vue会通过vite，实时打包成es-module。其中js文件不需要配置，而vue，css，less，ts等等文件，都需要通过<b>vite的插件</b>或者<b>其他工具</b>来实现插入到已经打包成es-module的文件中。最后通过浏览器进行渲染。PostCSS 就是一种用 JavaScript 工具和插件转换 CSS 代码的工具。

<b>分析报错内容</b>

​		从报错来看错误是由 PostCSS 的 BrowsersIist 配置引起的 。

​		BrowsersIist是用于指定目标浏览器范围的工具，用于自动添加CSS前缀和进行相关的浏览器兼容性处理。根据错误消息，可能是BrowsersIist配置中出现了错误。需要检查项目中的postcss.configjs或者package.json文件，找到Browserslist相关的配置项，并确保其正确性。

 		常见的错误可能包括：

1. 字符串中的拼写错误．请查你的Browserslist查询字符串是否有拼写错误，比如ie误写为ei

 	2. 查询表达式中的语法错误：检查你的查询表达式中是否有语法错误，比如缺少逗号或者括号不匹配等。
 	3. 是否使用了旧版本的BrowsersIist，尝试升级到最新版本，以确保兼容性和正确性

### 尝试解决

解决思路：

1. 先将项目及包全部重新安装，并检查所有使用到BrowsersIist 的包是否有不兼容问题  =》安装成功，不存在不兼容

 	2. 检查使用的工具链vite、postcss、browserslist的配置文件及命令 =》未发现问题
 	3. 查看.bin及browserslist内部命令，直接执行 =》执行失败，猜测与浏览器版本相关
 	4. 增加browserslist配置文件，指定浏览器运行版本=》解决问题，browserslist在不指定chrome版本时，默认使用的版本和云桌面版本不一致。

### 总结

<b>Browserslist</b>

在工程中使用 Browserslist 有两种常见方式：

① 在 `package.json` 相应字段中增加；② 独立的 `browserslistrc` 文件

在 `package.json` 中声明

```javascript
"browserslist": [
  "> 1%",
  "last 2 versions",
	"not ie <= 8"
]
```

通过 `browserslistrc` 配置

```javascript
# Browsers that we support
> 1%
last 2 versions
not ie <= 8
```

<b>postCss</b>

​		PostCSS 是一种 JavaScript 工具，可将 CSS 代码转换为抽象语法树 (AST)，然后提供 API（应用程序编程接口）用于使用 JavaScript 插件对其进行分析和修改。

​		PostCSS 提供了一个庞大的插件生态系统来执行不同的功能，如 linting、缩小、插入供应商前缀和许多其他事情。它既不是后处理器也不是预处理器，它只是一个将特殊的postCss插件语法转换为 Vanilla CSS 的转译器。可以将其视为 CSS 的babel工具。



