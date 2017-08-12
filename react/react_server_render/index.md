### 参考网址
[http://www.qingpingshan.com/jb/javascript/187275.html](http://www.qingpingshan.com/jb/javascript/187275.html)

### 1 传统SPA的渲染
一个React架构的SPA（单页应用）的渲染：客户端请求资源 --> 返回index.html模板 --> 请求JS文件并加载 --> React执行，挂载组件 --> 进入路由 --> 根据组件生命周期而做的初始化操作 --> 渲染组件
在这种模式下，一开始客户端只会收到一个没有内容的HTML文件，等待JS文件加载完毕后才会执行后续操作并渲染出页面。也就是说，无论是哪个路由下的页面，一开始都会先进入index.html这个入口，然后在客户端进行接下来的渲染，这对于大部分用户确实没有什么问题，但对于部分场景和搜索引擎就不是这样了。
* 部分场景
JS被禁用的场景，如微信，在某些情况下，在微信分享的页面可能会被判定为广告页面，在不做认证（备案）的情况下，JS脚本会被禁用，这会使得后续的渲染都无法执行。
* SEO（搜索引擎优化）
如果每次都将index.html作为入口，利用JS和Ajax来渲染，搜索引擎是基本无法获得真正的页面内容的。

### 2 服务端渲染的好处
* SEO：使页面在一开始就有一个HTML DOM结构，方便Google等搜索引擎的爬虫能爬到网页的内容。
* 首屏渲染速度更快（重点）：在服务器端就可以将数据组装成页面返回给客户端，无需等待JS文件下载执行的过程，减少了为获取首屏数据而异步请求导致的用户的等待时间。
* 同构：更易于维护，服务端和客户端可以共享某些代码。

### 3 同构方案
#### 3.1 什么是同构？
同一套代码，既可以运行在客户端（浏览器），又可以运行在服务器端（node）。

#### 3.2 为什么要做同构？
在同构模式下，客户端的代码也可以运行在服务器上，在服务器端就可以将数据组装成页面返回给客户端。
* 给页面的性能，尤其是首屏性能带来了巨大的提升可能。
* 为SEO提供了极大的便利。
* 在整个开发过程中，同构会极大地降低前后端的沟通成本，后端更加专注于业务模型，前端也可以专注于页面开发，中间的数据转换大可以交给node这一层来实现。

#### 3.3 React如何实现组件同构？
基于React的同构开发要归功于React提供的服务端渲染。
由于React本身的设计特点，它是以Virtual DOM的形式保存在内存中，这是服务端渲染的前提。React可以从Virtual DOM中生成一个字符串，而不是真正的DOM，这使得可以在客户端和服务端使用同一个React Component。
* 对于客户端，通过调用ReactDOM.render方法把Virtual DOM转换成真实DOM最后渲染到界面。
* 对于服务端，通过调用ReactDOMServer.renderToString方法接收一棵Virtual DOM树，把Virtual DOM转换成HTML字符串，代表了一段完整的HTML结构，最终以html的形式返回给客户端，从而达到服务端渲染的目的。
服务端不存在DOM，因此无法使用标准的ReactDOM.render方法，和ReactDOM.render方法不同，ReactDOMServer.renderToString方法去掉了用于表示渲染位置的参数。取而代之，该方法只只返回一个字符串，是一个快速的同步（阻塞式）函数，非常快。
需要注意的是，即使已经在服务端渲染好了的页面，还要在客户端重新使用ReactDom.render方法再render一次。因为所谓的服务端渲染，仅仅是渲染静态的页面内容而已，并不做任何的事件绑定，所有的事件绑定都是在客户端进行的。
有了服务端渲染，为何还要用客户端渲染？因为服务端渲染完得到的只是一堆字符串，客户端有JS，可以调用方法重绘浏览器界面。既然要再渲染一次，为何还要服务端渲染？为了SEO，使页面在一开始就有一个HTML DOM结构，方便Google等搜索引擎的爬虫能爬到网页的内容。
服务端已经渲染了一次React组件，还要在客户端中再渲染一次，会不会渲染两次React组件？不会，秘诀在于data-react-checksum属性：如果使用renderToString渲染组件，将React Component转化为HTML字符串，生成的HTML的第一个DOM会带有data-react-checksum属性，这个属性是通过adler32算法算出来的，如果两个组件有相同的props和DOM结构时，adler32算法算出的checksum值一样，当客户端渲染React组件时，首先计算出组件的checksum值，然后检索HTML DOM看是否存在值相同的data-react-checksum属性，如果存在，则组件只会渲染一次，如果不存在，则会抛出一个warning异常。也就是说，当服务器端和客户端渲染具有相同的props和相同DOM结构的组件时，该React组件只会渲染一次。

### 4 服务端组件的生命周期
服务端渲染只会走一遍生命周期，并且在第一次render后便会停止。一旦渲染为字符串，组件就会只调用位于render方法之前的组件生命周期方法，只有constructor、componentWillMount和render方法会被各调用一次，也就是说，componentDidMount和componentWillUnmount方法不会在服务端渲染过程中被调用，而componentWillMount在客户端渲染和服务端渲染下均有效。
当新建一个组件时，需要考虑到它可能既在服务端又在客户端进行渲染。这一点在创建事件监听器时尤为重要，因为并不存在一个生命周期方法会通知React Component是否已走完了整个生命周期。在componentWillMount内创建的所有事件监听器及定时器都可能潜在地导致服务端内存泄漏，最佳做法是只在componentDidMount内创建事件监听器及定时器，然后在componentWilUnmount内清除事件监听器及定时器。

### 5 状态管理
用Redux来管理React组件的状态，并配合强大的中间件Devtools、Thunk、Promise等来扩充应用。当进行服务端渲染时，创建store实例后，还必须把初始状态回传给客户端，客户端拿到初始状态后把它作为预加载状态来创建store实例。为什么客户端要拿到state来渲染？要保证客户端和服务端渲染的组件一致，否则客户端上生成的markup与服务端生成的markup不匹配，客户端将不得不再次加载数据，造成没必要的性能消耗。

### 6 路由方案
客户端路由使得客户端可以不依赖服务端，根据hash方式或调用history API，不同的URL渲染不同的视图，实现无缝的页面切换，用户体验极佳。但服务端渲染不同的地方在于，在渲染之前，必须根据URL正确找到相匹配的组件返回给客户端。
路由问题可以交由React-router解决。React Router为服务端渲染提供了两个API：
* match：在渲染之前根据URL匹配路由组件。
* RouterContext：处理匹配的结果，根据请求的路由地址将对应的组件获取内容，以同步的方式渲染路由组件。
React-router提供了一系列方法来在服务端捕获请求参数，并和renderToString结合来渲染出路由最终对应的组件，其基本原理是通过一个match方法来根据客户端请求的url解析出已经定义好的routes的props，解析结果是error（错误）、redirectLocation（重定向）和renderPorps（正常状况下解析到的props），只需要根据这些参数来执行对应的操作即可。除了前两种特殊情况和最后没有匹配到执行的404响应，一般情况下都会进入到render这个方法中根据renderProps来进行正常状况下的响应，直接把renderProps传给RouterContext即可，通过使用RouterContext，便得到了和客户端渲染时相似的Router组件，将其作为根组件并传入renderProps，再调用renderToString，一次具有路由的服务端渲染便完成。

### 7 静态资源处理方案
在客户端中，使用了大量的ES6/7语法，jsx语法，css资源，图片资源，最终通过webpack配合各种loader打包成一个文件最后运行在浏览器环境中。但在服务端，不支持import、jsx这种语法，并且无法识别对css、image资源后缀的模块引用，那么要处理这些静态资源，需要借助相关的工具、插件来使得Node.js解析器能够加载并执行这类代码。
开发环境和产品环境配置两套不同的解决方案：
#### 7.1 开发环境
* 引入babel-polyfill库来提供regenerator运行时和core-js来模拟全功能ES6环境。
* 引入babel-register，这是一个require钩子，会自动对require命令所加载的JS文件进行实时转码，该库只适用于开发环境。
* 引入css-modules-require-hook，同样是钩子，只针对样式文件，由于采用的是CSS Modules方案，并且使用SASS来书写代码，所以需要node-sass这个前置编译器来识别扩展名为.scss的文件，当然也可以采用LESS的方式，通过这个钩子，自动提取className哈希字符注入到服务端的React组件中。
* 引入asset-require-hook，来识别图片资源，对小于8K的图片转换成字符串，大于8k的图片转换成路径引用。

#### 7.2 产品环境
使用webpack分别对客户端和服务端代码进行打包。对于服务端代码，需要指定运行环境为node，并且提供polyfill，设置__filename和__dirname为true，由于是采用CSS Modules，服务端只需获取className，而无需加载样式代码，所以要使用css-loader/locals替代css-loader加载样式文件。

### 8 动态加载方案
对于大型Web应用程序来说，将所有代码打包成一个文件不是一种优雅的做法，特别是对于单页面应用，用户有时候并不想得到其余路由模块的内容，加载全部模块内容，不仅增加用户等待时间，而且会增加服务器负荷。webpack提供一个功能可以拆分模块，每一个模块称为chunk，这个功能叫Code Splitting。可以在代码库中定义分割点，调用require.ensure，实现按需加载，而对于服务端渲染，require.ensure是不存在的，因此需要判断运行环境，提供钩子函数。
