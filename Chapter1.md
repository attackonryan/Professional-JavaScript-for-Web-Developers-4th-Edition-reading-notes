# 第一章: 什么是JavaScript
## 1.1历史回顾
1995年，网景公司一位名叫 __Brendan Eich__ 的工程师，开始为即将发布的Netscape Navigator2开发一个叫Mocha(后改名为LiveScript)的脚本语言。

在Netscape Navigator2正式发布前，网景把LiveScript改名为 __JavaScript__，以便搭上媒体当时热烈炒作Java的顺风车。  

由于JavaScript1.0很成功，网景又在Netscape Navigator3中发布了1.1版本。1997年，JavaScript1.1作为提案被提交给欧洲计算机制造商协会(Ecma)。
第39技术委员会(TC39)承担了“标准化一门通用、跨平台、厂商中立的脚本语言的语法和语义”的任务。他们花了数月时间打造出 __ECMA-262__，也就是 __ECMAScript__ 这个新的脚本语言标准。  
## 1.2JavaScript实现
完整的JavaScript实现包含以下几个部分

1. 核心(ECMAScript)
2. 文档对象模型(DOM)
3. 浏览器对象模型(BOM)

### 1.2.1ECMAScript
ECMAScript不局限于Web浏览器，Web浏览器只是ECMAScript实现可能存在的一种 __宿主环境__ (host environment)。宿主环境提供ECMAScript的基准实现和与环境自身交互必需的扩展(如DOM)。Node.js也是一种宿主环境。  

在基本层面上，ECMA-262描述了ECMAScript语言的如下部分：

- 语法
- 类型
- 语句
- 关键字
- 保留字
- 操作符
- 全局对象  

ECMAScript只是对实现这个规范描述的所有方面的一门语言的称呼。JavaScript实现了ECMAScript，Adobe ActionScript也实现了。 

ECMA-262第1版本质上与网景的JavaScript1.1相同，只不过删除了所有浏览器特定的代码，外加少量细微的修改。 

ECMA-262第2版只做了一些编校工作，没有增减或改变任何特性。  

ECMA-262第3版第一次真正对这个标准进行了更新，更新了字符串处理、错误定义和数值输出。还增加了正则表达式、新的控制语句、try/catch异常处理的支持，以及为了更好地让标准国际化所做的少量修改。对很多人来说，这标志着ECMAScript作为一门真正的编程语言的时代终于来了。  

ECMA-262第4版是对这门语言的彻底修订，但由于改动过大，此版在发布之前被放弃。  

ECMA-262第5版于2009年12月3日正式发布。第5版致力于厘清第3版存在的歧义，也增加了新功能，包括原生的解析和序列化JSON数据的JSON对象、方便继承和高级属性定义的方法，以及新的增强ECMAScript引擎解释和执行代码能力的严格模式。  

ECMA-262第6版，俗称ES6、ES2015或ES Harmony，于2015年6月发布。这一版包含了大概这个规范有史以来最重要的一批增强特性。ES6正式支持了类、模块、迭代器、生成器、箭头函数、期约、反射、代理和众多新的数据类型。  

ECMA-262第7版，也称为ES7或ES2016，于2016年6月发布。这次修订只包含少量语法层面的增强，如Array.prototype.includes和指数操作符。  

ECMA-262第8版，也称为ES8或ES2017，完成于2017月6月。这一版主要增加了异步函数(async/awit)、SharedArrayBuffer及Atomics API，以及Object.values()/Object.entries()/Object.getOwnPropertyDescriptors()和字符串填充方法，另外明确支持对象字面量最后的逗号。  

ECMA-262第9版，也称为ES8或ES2018，发布于2018年6月。这次修订包括异步迭代、剩余和扩展属性、一组新的正则表达式特性、Promise.prototype.finally()，以及模板字面量修订。

ECMA-262第10版，也称为ES8或ES2019，发布于2019年6月。这次修订增加了Array.prototype.flat()/flatMap()、String.prototype.trimStart()/trimEnd()、Object.fromEntries()方法，以及Symbol.prototype.description属性，明确定义了Function.prototype.toString()的返回值并固定了Array.prototype.sort()的顺序。另外，这次修订解决了与JSON字符串兼容的问题，并定义了catch子句的可选绑定。  

### DOM
__文档对象模型__(DOM, Document Object Model)是一个应用编程接口(API)，用于在HTML中使用扩展的XML。DOM将整个页面抽象为一组分层节点。HTML或XML页面的每个组成部分都是一种节点，包含不同的数据。  

DOM通过创建表示文档的树，让开发者可以随心所欲地控制网页的内容和结构。使用DOM API，可以轻松地删除、添加、替换、修改节点。  

1998年10月，DOM Level1成为W3C的推荐标准。这个规范由两个模块组成：DOM Core和DOM HTML。前者提供了一种映射XML文档，从而方便访问和操作文档任意部分的方式；后者扩展了前者，并添加了特定于HTML的对象和方法。  

DOM Level1的目标是映射文档结构，而DOM Level2的目标则宽泛得多。这个对最初DOM的扩展增加了对鼠标和用户界面事件、范围、遍历的支持、而且通过对象接口支持了CSS。另外，DOM Level1中的DOM Core也被扩展以包含对XML命名空间的支持。
    
DOM Level2的新增了以下模块，以支持新的接口。

- DOM视图：描述追踪文档不同视图(如应用CSS样式前后的文档)的接口。
- DOM事件：描述事件及事件处理的接口。
- DOM样式：描述处理元素CSS样式的接口。
- DOM遍历和范围：描述遍历和操作DOM树的接口。  
  

DOM Level3进一步扩展了DOM，增加了以统一的方式加载和保存文档的方法，还有验证文档的方法(DOM Validation)。在Level3中，DOM Core经过扩展支持了所以XML1.0的特性。  

除了DOM Core和DOM HTML接口，有些其他语言也发布了自己的DOM标准。下面列出的语言是基于XML的，每一种都增加了该语言独有的DOM方法和接口：

- 可伸缩矢量图(SVG, Scalable Vector Graphics)
- 数学标记语言(MathML, Mathematical Markup Language)
- 同步多媒体集成语言(SMIL, Synchronized Multimedia Integration Language)

### BOM 
__浏览器对象模型__(BOM)API用于支持访问和操作浏览器的窗口。总的来说，BOM主要针对浏览器窗口和子窗口(frame)，不过人们通常会把任何特定于浏览器的扩展都归在BOM的范畴内。比如，下面就是这样一些扩展：

- 弹出新浏览器窗口的能力。
- 移动、缩放和关闭浏览器窗口的能力。
- navigator对象，提供关于浏览器的详尽信息。
- location对象，提供浏览器加载页面的详尽信息。
- screen对象，提供关于用户屏幕分辨率的详尽信息。
- performance对象，提供浏览器内存占用、导航行为和时间统计的详尽信息。
- 对cookie的支持。
- 其他自定义对象，如XMLHttpRequest和IE的ActiveXObject。



