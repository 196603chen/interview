# 工程化题目汇总
- [webpack的作用]
    模块打包。可以将不同模块的文件打包整合在一起，并且保证它们之间的引用正确，执行有序。利用打包我们就可以在开发的时候根据我们自己的业务自由划分文件模块，保证项目结构的清晰和可读性。
    编译兼容。在前端的“上古时期”，手写一堆浏览器兼容代码一直是令前端工程师头皮发麻的事情，而在今天这个问题被大大的弱化了，通过webpack的Loader机制，不仅仅可以帮助我们对代码做polyfill， 还可以编译转换诸如.less, .vue, .jsx这类在浏览器无法识别的格式文件，让我们在开发的时候可以使用新特性和新语法做开发，提高开发效率。
    能力扩展。通过webpack的Plugin机制，我们在实现模块化打包和编译兼容的基础上，可以进一步实现诸如按需加载，代码压缩等一系列功能，帮助我们进一步提高自动化程度，工程效率以及打包输出的质量。
- [webpack的构建流程]
    从启动到结束会依次执行以下三大步骤：
    初始化流程：从配置文件和 Shell 语句中读取与合并参数，并初始化需要使用的插件和配置插件等执行环境所需要的参数
    编译构建流程：从 Entry 发出，针对每个 Module 串行调用对应的 Loader 去翻译文件内容，再找到该 Module 依赖的 Module，递归地进行编译处理
    输出流程：对编译后的 Module 组合成 Chunk，把 Chunk 转换成文件，输出到文件系统
    地址链接：https://mp.weixin.qq.com/s/PlqhRNZNIfBJHSVoVD3fHw
    流程如下：
        配置文件默认下为webpack.config.js，也或者通过命令的形式指定配置文件，主要作用是用于激活webpack的加载项和插件。webpack 将 webpack.config.js 中的各个配置项拷贝到 options 对象中，并加载用户配置的 plugins。完成上述步骤之后，则开始初始化Compiler编译对象，该对象掌控者webpack声明周期，不执行具体的任务，只是进行一些调度工作，Compiler 对象继承自 Tapable，初始化时定义了很多钩子函数，初始化完成后会调用Compiler的run来真正启动webpack编译构建流程，主要流程如下：
        compile 开始编译（执行了run方法后，首先会触发compile，主要是构建一个Compilation对象，该对象是编译阶段的主要执行者，主要会依次下述流程：执行模块创建、依赖收集、分块、打包等主要任务的对象） 
        make 从入口点分析模块及其依赖的模块，创建这些模块对象
        build-module 构建模块（这里主要调用配置的loaders，将我们的模块转成标准的JS模块，从配置的入口模块开始，分析其 AST，当遇到require等导入其它模块语句时，便将其加入到依赖的模块列表，同时对新找出的依赖模块递归分析，最终搞清所有模块的依赖关系）
        seal 封装构建结果（seal方法主要是要生成chunks，对chunks进行一系列的优化操作，并生成要输出的代码）
        emit 把各个chunk输出到结果文件（在确定好输出内容后，根据配置确定输出的路径和文件名）

    entry option(初始化option)----->run(开始编译)----->make(从entry开始递归的分析依赖，对每个依赖进行build) ----->before-resolve(对模块位置进行解析)---->build module(开始构建某个模块)--->normal module-loader(将loader加载完成的module进行编译，生成AST树)----->program(遍历AST,当遇到require等一些调用表达式时，收集依赖)--->seal(所有build完成，开始优化)-->emit(输出到dist目录)。
    
        1、读取webpack的配置参数；
        2、启动webpack，创建Compiler对象并开始解析项目；
        3、从入口文件（entry）开始解析，并且找到其导入的依赖模块，递归遍历分析，形成依赖关系树；
        4、对不同文件类型的依赖模块文件使用对应的Loader进行编译，最终转为Javascript文件；
        5、整个过程中webpack会通过发布订阅模式，向外抛出一些hooks，而webpack的插件即可通过监听这些关键的事件节点，执行插件任务进而达到干预输出结果的目的。
     文件的解析与构建是一个比较复杂的过程，在webpack源码中主要依赖于compiler和compilation两个核心对象实现。
     compiler对象是一个全局单例，他负责把控整个webpack打包的构建流程。compilation对象是每一次构建的上下文对象，它包含了当次构建所需要的所有信息，每次热更新和重新构建，compiler都会重新生成一个新的compilation对象，负责此次更新的构建过程。而每个模块间的依赖关系，则依赖于AST语法树。每个模块文件在通过Loader解析完成之后，会通过acorn库生成模块代码的AST语法树，通过语法树就可以分析这个模块是否还有依赖的模块，进而继续循环执行下一个模块的编译解析。最终Webpack打包出来的bundle文件是一个IIFE的执行函数。

- [说说Webpack中常见的Loader？解决了什么问题？]
    loader 用于对模块的源代码进行转换，在 import 或"加载"模块时预处理文件。在webpack内部中，任何文件都是模块，不仅仅只是js文件，默认情况下，在遇到import或者load加载模块的时候，webpack只支持对js文件打包。像css、sass、png等这些类型的文件的时候，webpack则无能为力，这时候就需要配置对应的loader进行文件内容的解析。当 webpack 碰到不识别的模块的时候，webpack 会在配置的中查找该文件解析规则
    关于配置loader的方式有三种：
        1、配置方式（推荐）：在 webpack.config.js文件中指定 loader
            关于loader的配置，我们是写在module.rules属性中，因为loader支持链式调用，链中的每个loader会处理之前已处理过的资源，最终变为js代码。顺序为相反的顺序执行，即上述执行方式为sass-loader、css-loader、style-loader.属性介绍如下：
                rules:[{
                    test: /\.css$/,
                    use:[ { loader: 'style-loader' }, {
                        loader: 'css-loader',
                        options: {
                        modules: true
                        }
                    },{ loader: 'sass-loader' }]
                }]
        2、内联方式：在每个 import 语句中显式指定 loader
            import Styles from 'style-loader!css-loader?modules!./styles.css';
        3、CLI 方式：在 shell 命令中指定它们
            webpack --module-bind jade-loader --module-bind 'css=style-loader!css-loader'
    常见的loader
        style-loader: 将css添加到DOM的内联样式标签style里
        css-loader :允许将css文件通过require的方式引入，并返回css代码
        less-loader: 处理less
        sass-loader: 处理sass
        postcss-loader: 用postcss来处理CSS
        autoprefixer-loader: 处理CSS3属性前缀，已被弃用，建议直接使用postcss
        file-loader: 分发文件到output目录并返回相对路径
        url-loader: 和file-loader类似，但是当文件小于设定的limit时可以返回一个Data Url
        html-minify-loader: 压缩HTML
        babel-loader :用babel来转换ES6文件到ES
        vue-loader
- [说下 webpack 的 loader 和 plugin 的区别，都使用过哪些 loader 和 plugin](#说下-webpack-的-loader-和-plugin-的区别都使用过哪些-loader-和-plugin)

    不同的作用
        Loader直译为"加载器"。Webpack将一切文件视为模块，但是webpack原生是只能解析js文件，如果想将其他文件也打包的话，就会用到loader。 所以Loader的作用是让webpack拥有了加载和解析 非JavaScript文件 的能力。
        Plugin直译为"插件"。Plugin可以扩展webpack的功能，让webpack具有更多的灵活性。 在 Webpack 运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果。
    不同的用法
        Loader在module.rules中配置，也就是说他作为模块的解析规则而存在。 类型为数组，每一项都是一个Object，里面描述了对于什么类型的文件（test），使用什么加载(loader)和使用的参数（options）
        Plugin在plugins中单独配置。 类型为数组，每一项是一个plugin的实例，参数都通过构造函数传入。
    常用的plugin:
        HtmlWebpackPlugin:html-webpack-plugin插件默认会创建一个HTML模板，并自动引入打包生成的几个主要的chunk包,也可以通过template属性配置自己的模板,使用minify属性可以配置压缩选项
            plugins: [
                new HtmlWebpackPlugin({
                        // 复制./src/js/index.html 文件
                        template: './src/js/index.html',
                        title: 'webpack build', // template设置时或者使用html loader时 不起作用
                        filename: 'index.html',
                        minify: {
                            collapseWhitespace: true, // 移除空格
                            removeComments: true // 删除html文件注释 打包时有效
                        }
                })
            ]

        DefinePlugin：DefinePlugin是webpack注入全局变量的插件，通常使用该插件来判别代码运行的环境变量。在使用该插件需要注意的是，如果在该插件配置了相关的参数，必须要源码中使用，webpack才会注入。
            new webpack.DefinePlugin({
                'process.env': '"dev"'
            })
　　        我们console.log(process)，可以看到里面的env为空，但是console.log(process.env)可以打印出dev

        ExtractTextPlugin:该插件的主要是为了抽离css样式,防止将样式打包在js中引起页面样式加载错乱的现象。
        CommonsChunkPlugin:提取公共的第三方模块，假设a模块引用jquery，b模块也引用jquery，如果不用该插件则会把jquery打包进a和b,使用插件后，则会单独抽离出jquery，然后在a和b这两个入口引入
            new CommonsChunkPlugin({
                name: "vendor", // 名称为vendor，第三方库集合
                minChunks: function (module, ) {
                    // node_modules中出来的都打到这个文件中
                    return module.context && module.context.includes("node_modules");
                }
            }),

            HashedModuleIdsPlugin：该插件会根据模块的相对路径生成一个四位数的hash作为模块id, 建议用于生产环境。保持第三方插件每次打包module.id不变化，固定下来模块的moduleId。然后hashId也不变化
            NamedModulesPlugin: 当开启 HMR 的时候使用该插件会显示模块的相对路径，建议用于开发环境。效果同 HashedModuleIdsPlugin

            vender中不止有node_module文件夹中的包，还包括runtime: 指在浏览器运行时，webpack 用来连接模块化的应用程序的所有代码。其中包含：在模块交互时，连接模块所需的加载和解析逻辑。包括浏览器中的已加载模块的连接，以及懒加载模块的执行逻辑。manifest: 当编译器(compiler)开始执行、解析和映射应用程序时，它会保留所有模块的详细要点。这个数据集合称为 “Manifest”，当完成打包并发送到浏览器时，会在运行时通过 Manifest 来解析和加载模块。无论你选择哪种模块语法，那些 import 或 require 语句现在都已经转换为 __webpack_require__ 方法，此方法指向模块标识符(module identifier)。通过使用 manifest 中的数据，runtime 将能够查询模块标识符，检索出背后对应的模块。当模块做出改变的时候manifest也会改变，同时也会导致vender改变，最后导致vender的缓存失效，这种失效并不是因为vender本身内容的改变导致的，所以我们需要分离runtime和manifest。
                new CommonsChunkPlugin({
                    name: 'manifest',
                    minChunks: Infinity
                }),
            children字段和async字段
            children和async作用于动态加载模块。如果没有设置children，那么在动态引入的多个脚本中公用的部分并不会被提取出来。如果设置了childrend: true，则公共部分会被提取到主脚本中。进一步设置async字段，那么提取出来的公共部分不会在主脚本中，而会生成一个单独文件异步引入。
        CopyWebpackPlugin：用于webpack打包时拷贝文件的插件包，常用于拷贝静态目录
        HotModuleReplacementPlugin：启用热替换模块(Hot Module Replacement)，也被称为 HMR。永远不要在生产环境(production)下启用 HMR
        UglifyJsPlugin ：丑化（可读性差）和压缩js（tree-shaking)
        ClearWebpackPlugin:清除目录下的文件

- [说一下 webpack 中 css-loader 和 style-loader 的区别，file-loader 和 url-loader 的区别](#说一下-webpack-中-css-loader-和-style-loader-的区别file-loader-和-url-loader-的区别)
    css-loader与style-loader
        style-loader:使用<style>将css-loader内部样式注入到我们的HTML页面
        css-loader: 加载.css文件

    url-loader依赖file-loader.url-loader可以将图片转为base64字符串，直接加载到js中。能更快的加载图片，一旦图片过大，就需要使用file-loader的加载本地图片，故url-loader可以设置图片超过多少字节时，使用file-loader加载图片。使用file-loader打包之后会把对应规则的文件，复制一份到打包之后的文件夹，生成新的文件名，并传给项目的输入文件使用；
        {
            test: /\.(png|svg|jpg|jpeg|gif)$/,
            loader: 'url-loader',
            options: {
            limit: 10000
            }
      }


- [说下 tree-shaking 的原理](#说下-tree-shaking-的原理)
    Tree shaking 是一种通过清除多余代码方式来优化项目打包体积的技术，专业术语叫 Dead code elimination
    在ES6以前，我们可以使用CommonJS引入模块：require()，这种引入是动态的，也意味着我们可以基于条件来导入需要的代码：
    let dynamicModule;
    // 动态导入
    if (condition) {
    myDynamicModule = require("foo");
    } else {
    myDynamicModule = require("bar");
    }
    但是CommonJS规范无法确定在实际运行前需要或者不需要某些模块，所以CommonJS不适合tree-shaking机制。因为tree shaking只能在静态modules下工作。ECMAScript 6 模块加载是静态的,因此整个依赖树可以被静态地推导出解析语法树。所以在 ES6 中使用 tree shaking 是非常容易的。
    tree shaking的原理是什么?
        1、ES6 Module引入进行静态分析，故而编译的时候正确判断到底加载了那些模块
        2、静态分析程序流，判断那些模块和变量未被使用或者引用，进而删除对应代码

-[common.js 和 es6 中模块引入的区别？]()
    CommonJS 是一种模块规范，最初被应用于 Nodejs，成为 Nodejs 的模块规范。运行在浏览器端的 JavaScript 由于也缺少类似的规范，在 ES6 出来之前，前端也实现了一套相同的模块规范 (例如: AMD)，用来对前端模块进行管理。自 ES6 起，引入了一套新的 ES6 Module 规范，在语言标准的层面上实现了模块功能，而且实现得相当简单，有望成为浏览器和服务器通用的模块解决方案。但目前浏览器对 ES6 Module 兼容还不太好，我们平时在 Webpack 中使用的 export 和 import，会经过 Babel 转换为 CommonJS 规范。在使用上的差别主要有：
    1、CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。
    2、CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。
    3、CommonJs 是单个值导出，ES6 Module可以导出多个
    4、CommonJs 是动态语法可以写在判断里，ES6 Module 静态语法只能写在顶层
    5、CommonJs 的 this 是当前模块，ES6 Module的 this 是 undefined

- [你知道sourceMap是什么吗？] 解决源代码和目标生成代码的映射关系
    Webpack配置里边的devtool参数,参数选择有多个
        source-map：source-map会为每一个打包后的模块生成独立的sourcemap文件
        inline-source-map:映射文件会被直接写进js当中，base64形式的字符串，在js的底部，它会使得js文件变得非常大
        cheap-inline-source-map:告诉我那一行出错就好，不用告诉我那一列出错，节约性能，提高打包速度 只映射业务代码，而不映射第三方包、库。属性在打包后同样会为每一个文件模块生成 .map文件，但是与source-map的区别在于cheap生成的 map文件会忽略原始代码中的列信息
        eval:eval 会将每一个module模块，执行eval，执行后不会生成sourcemap文件，仅仅是在每一个模块后，增加sourceURL来关联模块处理前后对应的关系。sourceURL的值是压缩前存放的代码的位置,并没有为每一个模块生成相对应的sourcemap，优点是：打包速度非常快，因为不需要生成sourcemap文件，缺点是：由于会映射到转换后的代码，而不是映射到原始代码，所以不能正确的显示行数。
        cheap-module-source-map：该属性的配置也是生成一个没有列的信息的sourceMaps文件，同时loader的sourcemap也被简化成为只包含对应行的。

        一般开发环境：cheap-module-eval-source-map
        一般正式环境：cheap-module-source-map

- [是否写过Loader？简单描述一下编写loader的思路？]

- [是否写过Plugin？简单描述一下编写plugin的思路？]

- [webpack的热更新原理]

- [说说Webpack Proxy工作原理？为什么能解决跨域?]()
         webpack proxy，即webpack提供的代理服务,基本行为就是接收客户端发送的请求后转发给其他服务器,其目的是为了便于开发者在开发模式下解决跨域问题（浏览器安全策略限制）
    想要实现代理首先需要一个中间服务器，webpack中提供服务器的工具为webpack-dev-server,webpack-dev-server是 webpack 官方推出的一款开发工具，将自动编译和自动刷新浏览器等一系列对开发友好的功能全部集成在了一起,目的是为了提高开发者日常的开发效率，「只适用在开发阶段」
        devServer: {
            contentBase: path.join(__dirname, 'dist'),
            compress: true,
            port: 9000,
            proxy: {
                '/api': {
                    target: 'https://api.github.com',
                    pathRewrite：{'^/api': '/'}默认情况下，我们的 /api 也会被写入到URL中，如果希望删除，可以使用pathRewrite
                    secure：默认情况下不接收转发到https的服务器上，如果希望支持，可以设置为false
                    changeOrigin：它表示是否更新代理后请求的 headers 中host地址
                }
            }
            // ...
        }
    proxy工作原理实质上是利用http-proxy-middleware 这个http代理中间件，实现请求转发给其他服务器
    在开发阶段， webpack-dev-server 会启动一个本地开发服务器，所以我们的应用在开发阶段是独立运行在 localhost的一个端口上，而后端服务又是运行在另外一个地址上
    所以在开发阶段中，由于浏览器同源策略的原因，当本地访问后端就会出现跨域请求的问题
    通过设置webpack proxy实现代理请求后，相当于浏览器与服务端中添加一个代理者
    当本地发送请求的时候，代理服务器响应该请求，并将请求转发到目标服务器，目标服务器响应数据后再将数据返回给代理服务器，最终再由代理服务器将数据响应给本地
    在代理服务器传递数据给本地浏览器的过程中，两者同源，并不存在跨域行为，这时候浏览器就能正常接收数据
    注意：「服务器与服务器之间请求数据并不会存在跨域行为，跨域行为是浏览器安全策略限制」
    连接：https://mp.weixin.qq.com/s/6nQ-m9HL3-FENv6vF4dOnQ

- [说说你是如何利用Webpack来优化前端性能的？]

    JS代码压缩
    CSS代码压缩
    Html文件代码压缩
    文件大小压缩
    图片压缩
    Tree Shaking
    代码分离
    内联 chunk
   连接： https://mp.weixin.qq.com/s/Gq0VTuCmLHlAan85y6fUDQ

- [说说提高webpack的构建速度的手段有哪些]

- [与webpack类似的工具还有哪些？区别是什么]
rollup:是一款 ES Modules 打包器，从作用上来看，Rollup 与 Webpack 非常类似。不过相比于 Webpack，Rollup要小巧的多.代码非常简洁，完成不像webpack那样存在大量引导代码和模块函数.
        rollup优点：代码效率更简洁、效率更高    默认支持 Tree-shaking 优化输出结果
        缺点：加载其他类型的资源文件或者支持导入 CommonJS 模块，又或是编译 ES 新特性，这些额外的需求 Rollup需要使用插件去完成。rollup并不适合开发应用使用，因为需要使用第三方模块，而目前第三方模块大多数使用CommonJs方式导出成员，并且rollup不支持HMR，使开发效率降低

Parcel：是一款完全零配置的前端打包器，它提供了 “傻瓜式” 的使用体验，只需了解简单的命令，就能构建前端应用程序。Parcel 跟 Webpack 一样都支持以任意类型文件作为打包入口，但建议使用HTML文件作为入口。
        支持HMR。Parcel有个十分好用的功能：支持自动安装依赖，像webpack开发阶段突然使用安装某个第三方依赖，必然会终止dev server然后安装再启动。而Parcel则免了这繁琐的工作流程
        Parcel能够零配置加载其他类型的资源文件，无须像webpack那样配置对应的loader。Parcel给开发者一种很大的自由度，只管去实现业务代码，其他事情用Parcel解决

Snowpack：开发阶段，每次保存单个文件时，Webpack和Parcel都需要重新构建和重新打包应用程序的整个bundle。而Snowpack为你的应用程序每个文件构建一次，就可以永久缓存，文件更改时，Snowpack会重新构建该单个文件

vite：是一种新型前端构建工具，能够显著提升前端开发体验。vite会直接启动开发服务器，不需要进行打包操作，也就意味着不需要分析模块的依赖、不需要编译，因此启动速度非常快。它是在页面请求时在编译，在把编译结果给浏览器。在热模块HMR方面，当修改一个模块的时候，仅需让浏览器重新请求该模块即可，无须像webpack那样需要把该模块的相关依赖模块全部编译一次，效率更高

webpack：webpack大而全，很多常用的功能做到开箱即用。有两大最核心的特点：「一切皆模块」和「按需加载」
    智能解析：对 CommonJS 、 AMD 、ES6 的语法做了兼容
    万物模块：对 js、css、图片等资源文件都支持打包
    开箱即用：HRM、Tree-shaking等功能
    代码分割：可以将代码切割成不同的 chunk，实现按需加载，降低了初始化时间
    插件系统，具有强大的 Plugin 接口，具有更好的灵活性和扩展性
    易于调试：支持 SourceUrls 和 SourceMaps
    快速运行：webpack 使用异步 IO 并具有多级缓存，这使得 webpack 很快且在增量编译上更加快
    生态环境好：社区更丰富，出现的问题更容易解决

    
- [讲一下 webpack 原理， loader 和 plugin，你知道哪些模块化标准，说下 cjs 和 esmodule 的区别](#讲一下-webpack-原理-loader-和-plugin你知道哪些模块化标准说下-cjs-和-esmodule-的区别)
- [Import 和 CommonJS 在 webpack 打包过程中有什么不同](#import-和-commonjs-在-webpack-打包过程中有什么不同)
- [脚手架具体都做了哪些事，webpack 具体做了什么配置，怎样优化的打包大小](#脚手架具体都做了哪些事webpack-具体做了什么配置怎样优化的打包大小)
- [介绍下 Webpack 的整个生命周期](#介绍下-webpack-的整个生命周期)
- [webpack 离线缓存静态资源如何做用 localStore](#webpack-离线缓存静态资源如何做用-localstore)
- [说一下 webpack 与 gulp 的区别（源码角度）](#说一下-webpack-与-gulp-的区别源码角度)
- [A、B 两个条件组件，如何做到 webpack 只打包条件为 true 的组件，false 的组件不打包](#ab-两个条件组件如何做到-webpack-只打包条件为-true-的组件false-的组件不打包)
- [webpack 怎么处理内联 css 的](#webpack-怎么处理内联-css-的)
- [webpack 如何做异步加载](#webpack-如何做异步加载)
- [Webpack 里面的插件时怎么实现的](#webpack-里面的插件时怎么实现的)
- [dev-server 是怎么跑起来的](#dev-server-是怎么跑起来的)
- [Webpack 抽取公共文件是怎么配置的](#webpack-抽取公共文件是怎么配置的)
- [import { Button } from 'antd'，打包的时候只打包 button，分模块加载，是怎么做到的](#import--button--from-antd打包的时候只打包-button分模块加载是怎么做到的)
- [使用 import 时，webpack 对 node_modules 里的依赖会做什么](#使用-import-时webpack-对-node_modules-里的依赖会做什么)
- [前端怎么做单元测试](#前端怎么做单元测试)
- [一般怎么组织 CSS（Webpack）](#一般怎么组织-csswebpack)
- [webpack 如何配 sass，需要配哪些 loader，配 css 需要哪些 loader](#webpack-如何配-sass需要配哪些-loader配-css-需要哪些-loader)
- [如何配置把 js、css、html 单独打包成一个文件](#如何配置把-jscsshtml-单独打包成一个文件)
- [webpack 和 gulp 的优缺点](#webpack-和-gulp-的优缺点)
- [如何实现分模块打包（多入口）](#如何实现分模块打包多入口)
- [Webpack 打包时 Hash 码是怎么生成的？随机值存在一样的情况，如何避免？](#webpack-打包时-hash-码是怎么生成的随机值存在一样的情况如何避免)
- [Webpack 做了什么？使用 webpack 构建时有无做一些自定义操作？](#webpack-做了什么使用-webpack-构建时有无做一些自定义操作)
- [为什么用 gulp 打包 node](#为什么用-gulp-打包-node)
- [Webpack 为什么慢，如何进行优化](#webpack-为什么慢如何进行优化)
- [git pull -rebase 和 git pull 的区别是什么？](#git-pull--rebase-和-git-pull-的区别是什么)
- [Webpack 打包出来的体积太大，如何优化体积？](#webpack-打包出来的体积太大如何优化体积)
- [Webpack 热更新的原理](#webpack-热更新的原理)
- [一个活动项目里包含多个活动，Webpack 如何实现单独打包某个活动？](#一个活动项目里包含多个活动webpack-如何实现单独打包某个活动)
- [请说明 JavaScript 进行压缩、合并、打包实现的原理是什么？为什么需要压缩、合并、打包？分别列出一种常用工具或插件](#请说明-javascript-进行压缩合并打包实现的原理是什么为什么需要压缩合并打包分别列出一种常用工具或插件)
- [请说出前端框架设计模式(MVVM 或 MVP 又或 MVC)的含义以及原理](#请说出前端框架设计模式mvvm-或-mvp-又或-mvc的含义以及原理)
- [开发环境热更新的优化方式](#开发环境热更新的优化方式)
- [AMD 和 CMD 有哪些区别？](#amd-和-cmd-有哪些区别)
- [你是怎么配置开发环境的？](#你是怎么配置开发环境的)
- [如何实现 webpack 持久化缓存](#如何实现-webpack-持久化缓存)
- [webpack 做过哪些优化，开发效率方面、打包策略方面等等](#webpack-做过哪些优化开发效率方面打包策略方面等等)

### 说下 webpack 的 loader 和 plugin 的区别，都使用过哪些 loader 和 plugin

公司：阿里、滴滴、挖财

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/229)

<br/>

### 介绍下 webpack，并说下 Webpack 的构建流程

公司：头条、挖财

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/227)

<br/>

### 说下 tree-shaking 的原理

公司：头条

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/226)

<br/>

### 讲一下 webpack 原理， loader 和 plugin，你知道哪些模块化标准，说下 cjs 和 esmodule 的区别

公司：头条

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/216)

<br/>

### Import 和 CommonJS 在 webpack 打包过程中有什么不同

公司：完美世界

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/360)

<br/>

### 说一下 webpack 中 css-loader 和 style-loader 的区别，file-loader 和 url-loader 的区别

公司：网易

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/349)

<br/>

### 脚手架具体都做了哪些事，webpack 具体做了什么配置，怎样优化的打包大小

公司：易车

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/340)

<br/>

### 介绍下 Webpack 的整个生命周期

公司：滴滴、挖财

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/339)

<br/>

### 如何用localStoragewebpack 离线缓存静态资源？

公司：滴滴

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/322)

<br/>

### 说一下 webpack 与 gulp 的区别（源码角度）

公司：自如

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/265)

<br/>

### A、B 两个条件组件，如何做到 webpack 只打包条件为 true 的组件，false 的组件不打包

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/578)

<br/>

### webpack 怎么处理内联 css 的

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/575)

<br/>

### webpack 如何做异步加载

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/574)

<br/>

### Webpack 里面的插件时怎么实现的

公司：阿里

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/543)

<br/>

### dev-server 是怎么跑起来的

公司：阿里

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/542)

<br/>

### Webpack 抽取公共文件是怎么配置的

公司：阿里

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/541)

<br/>

### import { Button } from 'antd'，打包的时候只打包 button，分模块加载，是怎么做到的

公司：滴滴

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/517)

<br/>

### 使用 import 时，webpack 对 node_modules 里的依赖会做什么

公司：滴滴

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/516)

<br/>

### 前端怎么做单元测试

公司：挖财

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/466)

<br/>

### 一般怎么组织 CSS（Webpack）

公司：沪江

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/454)

<br/>

### webpack 如何配 sass，需要配哪些 loader，配 css 需要哪些 loader

公司：饿了么

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/451)

<br/>

### 如何配置把 js、css、html 单独打包成一个文件

公司：饿了么

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/450)

<br/>

### webpack 和 gulp 的优缺点

公司：兑吧

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/424)

<br/>

### 如何实现分模块打包（多入口）

公司：兑吧

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/422)

<br/>

### Webpack 打包时 Hash 码是怎么生成的？随机值存在一样的情况，如何避免？

公司：微医

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/599)

<br/>

### Webpack 做了什么？使用 webpack 构建时有无做一些自定义操作？

公司：微医

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/598)

<br/>

### 为什么用 gulp 打包 node

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/703)

<br/>

### Webpack 为什么慢，如何进行优化

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/701)

<br/>

### git pull -rebase 和 git pull 的区别是什么？

公司：会小二

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/679)

<br/>

### Webpack 打包出来的体积太大，如何优化体积？

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/657)

<br/>

### Webpack 热更新的原理

公司：酷狗

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/616)

<br/>

### 一个活动项目里包含多个活动，Webpack 如何实现单独打包某个活动？

公司：酷狗

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/614)

<br/>

### 请说明 JavaScript 进行压缩、合并、打包实现的原理是什么？为什么需要压缩、合并、打包？分别列出一种常用工具或插件

公司：玄武科技

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/834)

<br/>

### 请说出前端框架设计模式(MVVM 或 MVP 又或 MVC)的含义以及原理

公司：玄武科技

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/833)

<br/>

### 开发环境热更新的优化方式

公司：高思教育

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/823)

<br/>

### AMD 和 CMD 有哪些区别？

公司：58

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/815)

<br/>

### 你是怎么配置开发环境的？

公司：58

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/813)

<br/>

### 如何实现 webpack 持久化缓存

公司：乘法云

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/807)

<br/>

### webpack 做过哪些优化，开发效率方面、打包策略方面等等

公司：滴滴、快手、掌门一对一、高思教育

分类：工程化

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/25)

<br/>

