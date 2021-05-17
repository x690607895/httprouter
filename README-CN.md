# HttpRouter [![Build Status](https://travis-ci.org/julienschmidt/httprouter.svg?branch=master)](https://travis-ci.org/julienschmidt/httprouter) [![Coverage Status](https://coveralls.io/repos/github/julienschmidt/httprouter/badge.svg?branch=master)](https://coveralls.io/github/julienschmidt/httprouter?branch=master) [![GoDoc](https://godoc.org/github.com/julienschmidt/httprouter?status.svg)](http://godoc.org/github.com/julienschmidt/httprouter)

HttpRouter是一个使用go语言开发的轻量级、高性能的http请求路由（也可以叫我*multiplexer*或者更短点也可以叫*mux*）

与go的`net/http`包不同，这个路由可以进行请求匹配和带参数的路由。他的性能更加好

这个路由优化了高性能和更小的内存占用，它支持很长的路由和巨大的数字。一个复杂的动态的查找树结构被用来进行高效的匹配

## Features

**进行显示的匹配：** 对比其他的路由模块类似：[`http.ServeMux`](https://golang.org/pkg/net/http/#ServeMux)，一个请求可以同时被多个表达式满足。因此他们有一些尴尬的匹配规则，类似*longest match*（长匹配）或者*first registered, first matched*（优先注册优先匹配）。通过设计这个路由，一个路由可以只匹配一个或者不匹配路由。因此他们也没有意外的匹配，并且对seo和用户体验很友好

**不用在关心最后的/：** 选择你喜欢的url风格，如果丢失了一个/或者多了一个/，客户端路由会自动改变。当然如果有一个新的路由也有对应的处理逻辑他也会被使用。如果你不喜欢这样你也可以关闭大它[turn off this behavior](https://godoc.org/github.com/julienschmidt/httprouter#Router.RedirectTrailingSlash)。

**自动修正path：**  出了检查丢失和多余的斜杠外没有任何其他性能损耗，router还可以修复错误的cases和移除一些多余的东西（例如：`../` 或者 `//`）。你是其中的一个使用者吗[CAPTAIN CAPS LOCK](http://www.urbandictionary.com/define.php?term=Captain+Caps+Lock)？HttpRouter可以帮助你做一些没有大小写之分的查找并且可以帮你重定向正确的url

**在你的路由里增加参数：** 停止对你的url路径进行分析，只给path部分一个名字，路由就会动态的把这些参数的值告诉你。通过设计路由，参数可以非常容易的给到你。

**没有额外的消耗** 匹配和分发模块几乎没有额外消耗。唯一的堆分配是在对path的参数进行k-v遍历的时候和构造请求和响应对象的时候（后者主要依赖 `Handler`/`HandlerFunc` API）。在三个参数的api中如果请求的路径没有参数则不进行必要的堆分配。