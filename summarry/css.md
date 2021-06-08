# Css题目汇总

- [如果要做优化，CSS提高性能的方法有哪些？]
    1、内联首屏关键CSS（关键的首屏代码内联）
    2、异步加载CSS
       解析规则：1、加载HTML资源 2、解析HTML 3、加载CSS资源，同时构建DOM树 4、解析CSS，同时渲染DOM树
        如果css过大则会阻塞在第三步。
        1、设置link标签media属性为noexis，浏览器会认为当前样式表不适用当前类型，会在不阻塞页面渲染的情况下再进行下载。加载完成后，将media的值设为screen或all，从而让浏览器开始解析CSS
        <link rel="stylesheet" href="mystyles.css" media="noexist" onload="this.media='all'">
        2、通过rel属性将link元素标记为alternate可选样式表，也能实现浏览器异步加载。同样别忘了加载完成之后，将rel设回stylesheet
        <link rel="alternate stylesheet" href="mystyles.css" onload="this.rel='stylesheet'">
    3、资源压缩
        利用webpack、gulp/grunt、rollup等模块化工具，将css代码进行压缩，使文件变小，大大降低了浏览器的加载时间
    4、合理使用选择器
        css匹配的规则是从右往左开始匹配，例如#markdown .content h3匹配规则如下：先找到h3标签元素。然后去除祖先不是.content的元素。最后去除祖先不是#markdown的元素
        如果嵌套的层级更多，页面中的元素更多，那么匹配所要花费的时间代价自然更高。所以我们在编写选择器的时候，可以遵循以下规则：
            a:不要嵌套使用过多复杂选择器，最好不要三层以上
            b:使用id选择器就没必要再进行嵌套
            c:通配符和属性选择器效率最低，避免使用
    5、减少使用昂贵的属性
        在页面发生重绘的时候，昂贵属性如box-shadow/border-radius/filter/透明度/:nth-child等，会降低浏览器的渲染性能
    6、不要使用@import
        @import会影响浏览器的并行下载，使得页面在加载时增加额外的延迟，增添了额外的往返耗时。而且多个@import可能会导致下载顺序紊乱
    7、减少重排操作，以及减少不必要的重绘
    8、cssSprite，合成所有icon图片，用宽高加上backgroud-position的背景图方式显现出我们要的icon图，减少了http请求
    9、把小的icon图片转成base64编码
    10、CSS3动画或者过渡尽量使用transform和opacity来实现动画，不要使用left和top属性
    css实现性能的方式可以从选择器嵌套、属性特性、减少http这三面考虑，同时还要注意css代码的加载顺序

- [link 和@inmport 区别]
    两者都是外部引用CSS的方式，但是存在一定的区别：
　　 区别1：link是XHTML标签，除了加载CSS外，还可以定义RSS等其他事务；@import属于CSS范畴，只能加载CSS。
　　 区别2：link引用CSS时，在页面载入时同时加载；@import需要页面网页完全载入以后加载。
　　 区别3：link是XHTML标签，无兼容问题；@import是在CSS2.1提出的，低版本的浏览器不支持。
　　 区别4：link支持使用Javascript控制DOM去改变样式；而@import不支持。


-[word-wrap  word-break  white-space ]
    word-wrap 允许长单词或 URL 地址换行到下一行。
        取值：break-word 它会截断单词 只会对超出容器的文字进行分析，会导致上一行有空白
              normal  只在允许的断字点换行（浏览器保持默认处理）。
    word-break:规定自动换行的处理方法。
        取值：normal使用浏览器默认的换行规则。
             break-all 允许在单词内换行。  对所有的进行分析 ，不会导致空白
             keep-all 只能在半角空格或连字符处换行。
    white-space属性指定元素内的空白怎样处理。
        取值：normal 默认。空白会被浏览器忽略。
             pre  空白会被浏览器保留。其行为方式类似 HTML 中的 <pre> 标签。
             nowrap 文本不会换行，文本会在在同一行上继续，直到遇到 <br> 标签为止。
             pre-wrap 保留空白符序列，但是正常地进行换行。
             pre-line 合并空白符序列，但是保留换行符。
             inherit 规定应该从父元素继承 white-space 属性的值。

-[content-visibility]
    content-visibility 是一个非常酷的新CSS功能，可以提高网站性能。它的工作原理基本上就像懒惰加载一样，只是不针对图片，而是针对任何HTML元素。您可以使用它来阻止网站的任何部分加载，直到其可见为止。

    使用也超级简单，只需将其应用到你所选择的元素上，就像这样：

    .content-below-fold {
        content-visibility: auto;
    }
    content-visibility 有三个值。默认情况下，它被设置为可见，在这种情况下，元素会像往常一样加载。另外，你也可以将其设置为hidden，在这种情况下，无论元素是否可见，都不会被渲染。另一方面，当设置为 auto 时，可见区域外的元素将被跳过，一旦出现在屏幕上，就会被渲染。

-[浏览器支持小于12px的字体]
    1、zoom(80%) 或者zoom(0.8)
    2、css3的属性scale(0.8)

-[滚动捕捉]
    CSS 滚动捕捉允许用户完成滚动之后将视口锁定到某个元素的位置。(如滚动结束之后把底部的图片完全显示，或者水平滚动时图片始终显示在中心位置)
    实现滚动捕捉主要依靠两个属性：容器元素的 scroll-snap-type 属性，以及子元素的 scroll-snap-align 属性。
    .container {
        scroll-snap-type: y mandatory;
    }
    .child {
        scroll-snap-align: start;
    }
    第一版中定义的属性与此不同，是通过 repeat 关键字实现的。这种定义捕捉的方式很局限，只在间距均匀的捕捉点（evenly-spaced snap points）场景下有效，如果个别子元素尺寸与其他的不一样，就会有问题。如下
    .container {
        /* OLD */
        scroll-snap-points-y: repeat(300px);
    }
    你可以同时使用这两种方法（如果布局允许），以便支持这两类浏览器：
    .container {
        scroll-snap-type: mandatory;
        scroll-snap-points-y: repeat(300px);
        scroll-snap-type: y mandatory;
    }
    scroll-snap-type可以设置2个属性
        1、mandatory 值表示，在用户停止滚动时，浏览器必须 滚动到一个捕捉点
        2、proximity 属性就没有严格——除非当前滚动的位置合适，否则 不会强制浏览器 滚动到捕捉点。一般当滚动停止在距离某个捕捉点几百像素内时，捕捉才会发生。
    scroll-snap-align通过这个属性，可以指定元素的哪一部分吸附到容器上。可能的取值有三个：start、center 和 end。
    scroll-snap-stop：“normal” vs. “always”
        默认情况下，滚动捕捉只会在用户停止滚动时发生，这表示如果滚动过猛，中间可能会跳过几个捕捉点，然后才会停止。可以通过给子元素设置 scroll-snap-stop: always 来改变这一行为。这会将强制滚动容器在用户继续滚动之前停留在在就近的一个元素上。

-[:is 和 :where]
    :is() 伪类来删除选择器列表中的重复项。
    :where() 伪类与 :is() 具有相同的语法和功能。它们之间的唯一区别是 :where() 不会增加整体选择器的特殊性（即某条CSS规则特殊性越高，它的样式越优先被采用）。
    :where() 伪类及其任何参数都不对选择器的特殊性有所帮助，它的特殊性始终为零。
    .main a:hover,
    .sidebar a:hover,
    .site-footer a:hover {
        /* markup goes here */
    }
    等同于：
    :is(.main, .sidebar, .site-footer) a:hover {
	    /* markup goes here */
    }
    等同于：
    :where(.main, .sidebar, .site-footer) a:hover {
        /* markup goes here */
    }
- [css 伪类与伪元素区别](#css-伪类与伪元素区别)
    定义：
        伪类的概念引入是为了能够表达在文档树语法之外无法通过简单的选择器表达的信息。伪类的语法是单个冒号带一个伪类名称。不区分大小写。有的伪类是互斥的，有的可以同时作用在同一个元素上，伪类也可以是动态的，来响应用户的交互。
        :link       默认带href属性的a标签的样式
        :visited    被访问过的链接的样式
        :hover      鼠标悬浮上去的样式
        :active      鼠标按下去的时候的样式
        上面四种定义的时候需要保证这样的顺序

        :focus  当前元素为focus状态
        :lang   lang(en) 对应html上的lang属性，符合的话执行样式
        :empty   选择没有子元素的元素执行样式
        :enable  选择表单元素具有非disable属性的元素，执行样式
        :disable  选择表单元素具有disable属性的元素。执行样式
        :checked 单选按钮或者多选按钮被选中的，执行样式
        :target  锚点跳转到的内容执行样式
        :root    匹配文档的根元素。html中默认就是html元素
        :default 默认状态的表单元素，比如默认选中的下拉框，单选按钮，多选按钮，执行样式
        :first-of-type 选中该元素是别人首个子元素，例如p:first-of-type  就是所以元素子元素中第一个p元素
        :last-of-type  意义和上面类似，代表最后一个
        :only-of-type  代表所有元素中只有一个该类型的元素p:only-of-type
        :only-child  例如p:only-child 代表子元素中只有一个元素，且必须是该类型p
        :first-child  例如p:first-child 代表是父元素中的第一个元素，且类型为p
        :last-child  意义同上，最后一个元素
        :nth-child(n) 例如p:nth-child(2) 表示选择子元素第二个且类型为p的元素，n从1开始算
        :nth-last-child(n) 意思和上面类似，只不过是从结尾开始往前数第n个，n是从1开始算
        :nth-of-type(n)  例如p:nth-of-type(2) 代表子元素中第二次出现的p元素  n从1开始算
        :nth-last-of-type(n)  意义和上面类似，只不过是从尾部往前数，n从1开始算
        :not()  例如 :not(p) 匹配非p元素
        伪元素是在html文档树语法的基础上创造的一种抽象概念，例如，文档树语法里并没有提供一种机制让我们访问一个元素的内容的首个字母或者首行，伪元素提供了访问这样难以访问的信息的能力，伪元素还提供了对dom节点内不存在的内容的引用和生成能力，比如::after ::before 提供了生成内容的能力。一个伪元素包含两个冒号 ::
        ::first-letter  匹配内容的首个字符
        ::first-line    匹配内容的首行内容
        ::before        匹配内容前面的部分
        ::after         匹配紧跟内容后面的部分
        ::selection     匹配用户通过鼠标或者其他设备选中的内容部分
    伪类和伪元素的异同
    1.写法的区别
    在css3中定义了双冒号代表伪元素，单冒号代表伪类。以此来区分伪元素和伪类。为了兼容老的浏览器，用单冒号类表达伪元素也是能够被识别的，比如写:after :before :first-line :fist-letter
    2.概念的区别
    伪类侧重丰富选择器的选择语法范围内元素的选择能力，伪元素侧重表达或者定义不在语法定义范围内的抽象元素。
    3.兼容性
    伪类和伪元素各自有一些存在的兼容问题。使用的时候留意兼容性。

- [说一下盒子模型，以及标准情况和 IE 下的区别](#说一下盒子模型以及标准情况和-ie-下的区别)
    盒子模型有两种，分别是 ie 盒子模型和标准 w3c 盒子模型。
    W3C 盒子模型的范围包括 margin、border、padding、content，并且 content 部分不包含其他部分。
    IE盒子模型的范围包括margin、border、padding、content,和w3c盒子模型不同的是，IE盒子模型的content部分包含了padding和border.
    我们设置的宽在标准模型种指的是content，而在IE模型中指的是content+padding+border
    .div1 {
            width: 100px;
            height: 80px;
            border: 10px solid #000;
            padding: 20px;
            background-color: red;
            margin: 50px;
        }
    w3c标准浏览器下
    占据的位置宽：100+20*2+10*2+50*2=260px 高：80+20*2+10*2+50*2=240px
    盒子的实际大小为：  宽：100+20*2+10*2=160px 高：80+20*2+10*2=140px
    .IE6以下版本：
    占据的位置宽：100+50*2=200px 高：80+50*2=180px
    盒子的实际大小为：宽：100px 高：80px
    IE5.5及更早的版本使用的是IE盒模型。IE6及其以上的版本在标准兼容模式下使用的是W3C的盒模型标准。我们说这是一个好消息因为这意味着此盒模型问题只会出现在IE5.5及其更早的版本中。只要为文档设置一个DOCTYPE，就会使得IE遵循标准兼容模式的方式工作。
    css3的box-sizing属性给了开发者选择盒模型解析方式的权利。W3C的盒模型方式被称为“content-box”，IE的被称为“border-box”，使用box-sizing: border-box;就是为了在设置有padding值和border值的时候不把宽度撑开。

    
- [Css 如何画出一个扇形，动手实现下](#css-如何画出一个扇形动手实现下)
- [iPhone 里面 Safari 上如果一个输入框 fixed 绝对定位在底部，当软键盘弹出的时候会有什么问题，如何解决](#iphone-里面-safari-上如果一个输入框-fixed-绝对定位在底部当软键盘弹出的时候会有什么问题如何解决)
- [BFC 是什么？触发 BFC 的条件是什么？有哪些应用场景？](#bfc-是什么触发-bfc-的条件是什么有哪些应用场景)
- [说一下什么是重绘重排，哪些操作会造成重绘重排](#说一下什么是重绘重排哪些操作会造成重绘重排)
- [什么情况会出现浏览器分层](#什么情况会出现浏览器分层)
- [通过 link 进来的 css 会阻塞页面渲染嘛，Js 会阻塞吗，如果会如何解决？](#通过-link-进来的-css-会阻塞页面渲染嘛js-会阻塞吗如果会如何解决)
- [使用 Css 实现一个水波纹效果](#使用-css-实现一个水波纹效果)
- [position 定位都有什么属性（不仅仅是绝对定位和相对定位/fix 定位）](#position-定位都有什么属性不仅仅是绝对定位和相对定位fix-定位)
- [说一下 Css 预处理器，Less 带来的好处？](#说一下-css-预处理器less-带来的好处)
- [Css 选择器都有什么，权重是怎么计算的](#css-选择器都有什么权重是怎么计算的)
- [布局都有什么方式，float 和 position 有什么区别](#布局都有什么方式float-和-position-有什么区别)
- [nth-child和nth-of-type 有什么区别](#nth-child和nth-type-of-有什么区别)
- [&lt;img&gt;是什么元素](#img是什么元素)
- [flex 布局，如何实现把八个元素分两行摆放](#flex-布局如何实现把八个元素分两行摆放)
- [Css 方式实现一个不知道宽高的 div 居中都有哪几种方法](#css-方式实现一个不知道宽高的-div-居中都有哪几种方法)
- [以下 css 最后是什么颜色](#以下-css-最后是什么颜色)
- [简述 Grid 布局](#简述-grid-布局)
- [动手实现一个左右固定100px，中间自适应的三列布局？(至少三种)](#动手实现一个左右固定100px中间自适应的三列布局至少三种)
- [屏幕占满和未占满的情况下，使 footer 固定在底部，尽量多种方法](#屏幕占满和未占满的情况下使-footer-固定在底部尽量多种方法)
- [Css 画一个三角形](#css-画一个三角形)
- [Css 超出省略怎么写，三行超出省略怎么写](#css-超出省略怎么写三行超出省略怎么写)
- [Css inherit、initial、unset 三者的区别](#css-inheritinitialunset-三者的区别)
- [介绍下 Flex 布局，属性都有哪些，都是干啥的](#介绍下-flex-布局属性都有哪些都是干啥的)
- [响应式布局用到的技术，移动端需要注意什么](#响应式布局用到的技术移动端需要注意什么)
- [移动端适配 1px 的问题](#移动端适配-1px-的问题)
- [居中为什么要使用 transform（为什么不使用 marginLeft/marginTop）](#居中为什么要使用-transform为什么不使用-marginleftmargintop)
- [介绍 css3 中 position:sticky](#介绍-css3-中-positionsticky)
- [清除浮动的方式](#清除浮动的方式)
- [transform 动画和直接使用 left、top 改变位置有什么优缺点](#transform-动画和直接使用-lefttop-改变位置有什么优缺点)
- [上下固定，中间滚动布局如何实现](#上下固定中间滚动布局如何实现)
- [如何实现高度自适应](#如何实现高度自适应)
- [em 和 px 的区别](#em-和-px-的区别)
- [以下选项为 css 盒模型属性有哪些？(多选题)](#以下选项为-css-盒模型属性有哪些多选题)
- [说下盒模型的区别？介绍一下标准的 CSS 盒模型？border-box 和 content-box 有什么区别？](#说下盒模型的区别介绍一下标准的-css-盒模型border-box-和-content-box-有什么区别)
- [Css 单位都有哪些？](#css-单位都有哪些)
- [Css 实现多列等高布局，要求元素实际占用的高度以多列中较高的为准](#css-实现多列等高布局要求元素实际占用的高度以多列中较高的为准)
- [一个标签的 class 样式的渲染顺序，id、class、标签、伪类的优先级](#一个标签的-class-样式的渲染顺序idclass标签伪类的优先级)
- [css 如何实现动画](#css-如何实现动画)
- [Css 如何实现一个半圆](#css-如何实现一个半圆)
- [请画出 css 盒模型，基于盒模型的原理，说明相对定位、绝对定位、浮动实现样式是如何实现的？](#请画出-css-盒模型基于盒模型的原理说明相对定位绝对定位浮动实现样式是如何实现的)
- [列举出 css 选择器有哪些分类，并至少写出三个 css 选择器之间的区别，适用场景](#列举出-css-选择器有哪些分类并至少写出三个-css-选择器之间的区别适用场景)
- [Css 实现 div 宽度自适应，宽高保持等比缩放](#css-实现-div-宽度自适应宽高保持等比缩放)
- [ul 内部除最后一个 li 以外设置右边框效果](#ul-内部除最后一个-li-以外设置右边框效果)
- [flex:1 的完整写法是？分别是什么意思？](#flex1-的完整写法是分别是什么意思)
- [行内元素和块级元素有什么区别](#行内元素和块级元素有什么区别)
- [link 和@inmport 区别](#link-和inmport-区别)
- [屏幕正中间有个元素A，元素A中有文字A，随着屏幕宽度的增加，始终需要满足下列条件](#屏幕正中间有个元素a元素a中有文字a随着屏幕宽度的增加始终需要满足下列条件)
- [怎样用 css 实现一个弹幕的效果，动手实现一下](#怎样用-css-实现一个弹幕的效果动手实现一下)

### css 伪类与伪元素区别

公司：滴滴、网易

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/18)

<br/>

### 说一下盒子模型，以及标准情况和 IE 下的区别

公司：兑吧

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/231)

<br/>

### Css 如何画出一个扇形，动手实现下

公司：头条

分类：Css、编程题

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/228)

<br/>

### iPhone 里面 Safari 上如果一个输入框 fixed 绝对定位在底部，当软键盘弹出的时候会有什么问题，如何解决

公司：快手

分类：JavaScript、Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/201)

<br/>

### BFC 是什么？触发 BFC 的条件是什么？有哪些应用场景？

公司：快手、伴鱼、网易

分类：CSS

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/194)

<br/>

### 说一下什么是重绘重排，哪些操作会造成重绘重排

公司：滴滴、伴鱼、菜鸟网络、58

分类：CSS

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/192)

<br/>

### 什么情况会出现浏览器分层

公司：伴鱼

分类：CSS

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/191)

<br/>

### 通过 link 进来的 css 会阻塞页面渲染嘛，Js 会阻塞吗，如果会如何解决？

公司：伴鱼

分类：CSS

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/190)

<br/>

### 使用 Css 实现一个水波纹效果

参考：[第二屏中的水波纹效果](https://mp.toutiao.com/auth/page/login/?redirect_url=JTJG#/?_k=1hjyly)

公司：头条

分类：Css、编程题

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/151)

<br/>

### position 定位都有什么属性（不仅仅是绝对定位和相对定位/fix 定位）

公司：头条

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/150)

<br/>

### 说一下 Css 预处理器，Less 带来的好处？

公司：头条

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/143)

<br/>

### Css 选择器都有什么，权重是怎么计算的

公司：完美世界

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/363)

<br/>

### 布局都有什么方式，float 和 position 有什么区别

公司：完美世界

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/362)

<br/>

### `nth-child`和`nth-of-type` 有什么区别

公司：网易

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/357)

<br/>

### `<img>`是什么元素

公司：网易

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/356)

<br/>

### flex 布局，如何实现把八个元素分两行摆放

公司：网易

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/355)

<br/>

### Css 方式实现一个不知道宽高的 div 居中都有哪几种方法

公司：阿里、滴滴、易车、新东方、虎扑、饿了么、爱范儿、心娱、58

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/335)

<br/>

### 以下 css 最后是什么颜色

```html
<style>
  div {
    color: red;
  }
  #title {
    color: yellow;
  }
  div.title {
    color: bule;
  }
</style>
<div class="title" id="title">abc</div>
```

公司：快手

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/296)

<br/>

### 简述 Grid 布局

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/278)

<br/>

### 动手实现一个左右固定100px，中间自适应的三列布局？(至少三种)

公司：自如、头条

分类：Css、编程题

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/275)

<br/>

### 屏幕占满和未占满的情况下，使 footer 固定在底部，尽量多种方法

分类：Css、编程题

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/256)

<br/>

### Css 画一个三角形

公司：会小二、高思教育、58

分类：Css、编程题

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/413)

<br/>

### Css 超出省略怎么写，三行超出省略怎么写

公司：虎扑

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/405)

<br/>

### Css inherit、initial、unset 三者的区别

公司：虎扑

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/404)

<br/>

### 介绍下 Flex 布局，属性都有哪些，都是干啥的

公司：阿里、虎扑、快手

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/403)

<br/>

### 响应式布局用到的技术，移动端需要注意什么

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/377)

<br/>

### 移动端适配 1px 的问题

公司：阿里

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/545)

<br/>

### 居中为什么要使用 transform（为什么不使用 marginLeft/marginTop）

公司：阿里

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/544)

<br/>

### 介绍 css3 中 position:sticky

公司：网易

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/529)

<br/>

### 清除浮动的方式

公司：头条

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/498)

<br/>

### transform 动画和直接使用 left、top 改变位置有什么优缺点

公司：有赞、腾讯应用宝

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/491)

<br/>

### 上下固定，中间滚动布局如何实现

公司：饿了么

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/449)

<br/>

### 如何实现高度自适应

公司：兑吧

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/430)

<br/>

### em 和 px 的区别

公司：兑吧

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/426)

<br/>

### 以下选项为 css 盒模型属性有哪些？(多选题)

```js
A.font
B.margin
C.padding
D.visible
E.border
```

公司：会小二

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/694)

<br/>

### 说下盒模型的区别？介绍一下标准的 CSS 盒模型？border-box 和 content-box 有什么区别？

公司：快手、会小二

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/678)

<br/>

### Css 单位都有哪些？

公司：快手

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/646)

<br/>

### Css 实现多列等高布局，要求元素实际占用的高度以多列中较高的为准

公司：快手

分类：Css、编程题

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/645)

<br/>

### 一个标签的 class 样式的渲染顺序，id、class、标签、伪类的优先级

公司：快手

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/644)

<br/>

### css 如何实现动画

公司：头条

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/628)

<br/>

### Css 如何实现一个半圆

公司：酷狗

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/615)

<br/>

### 请画出 css 盒模型，基于盒模型的原理，说明相对定位、绝对定位、浮动实现样式是如何实现的？

公司：玄武科技

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/840)

<br/>

### 列举出 css 选择器有哪些分类，并至少写出三个 css 选择器之间的区别，适用场景

公司：玄武科技

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/839)

<br/>

### Css 实现 div 宽度自适应，宽高保持等比缩放

公司：快手

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/805)

<br/>

### ul 内部除最后一个 li 以外设置右边框效果

公司：快手

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/804)

<br/>

### flex:1 的完整写法是？分别是什么意思？

公司：快手

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/803)

<br/>

### 行内元素和块级元素有什么区别

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/797)

<br/>

### link 和@inmport 区别

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/796)

<br/>

### 屏幕正中间有个元素A，元素A中有文字A，随着屏幕宽度的增加，始终需要满足下列条件

```js
/* 
  A元素垂直居中于屏幕中央
  A元素距离屏幕左右边距各10px
  A元素里面的文字A的font-size:20px；水平垂直居中
  A元素的高度始终是A元素宽度的50%；(如果搞不定可以实现为A元素的高度固定为200px)
  
  请用html及css实现
*/
```

公司：头条

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/867)

<br/>

### 怎样用 css 实现一个弹幕的效果，动手实现一下

公司：头条

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/145)

<br/>

### justify-content:space-between around 有什么区别

公司：快手

分类：Css

[答案&解析](https://github.com/lgwebdream/FE-Interview/issues/169)

<br/>