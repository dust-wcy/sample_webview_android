## Hybrid技术开发之 Android WebView的可选替换方案。 

随着 Web 技术和移动设备的快速发展，Hybrid 技术目前已经成为一种主流常见的方案。一套好的Hybrid架构方案能让 App 既能拥有极致的体验和性能，同时也能拥有 Web技术灵活的开发模式、跨平台能力以及热更新机制，想想是不是都鸡冻不已。 

<!--more-->

Hybrid 技术开发，当前的趋势是HTML占据了越来越重要的位置，且H5不再是简单的浏览网页的行为，它承担着许多原本原生APP开发的功能。然后，当实际开发才发现Android平台H5的支持/渲染效率，是个非常麻烦的事情。

<!--more-->

当前WebView在Android各平台上表现的不一样，由于机型过多且集成的WebView内核版本不一致，不同手机的WebView兼容性和性能差异也较大。
如果APP对H5有较高的依赖性，则需要一个较好的、综合性能优异、各平台统一的WebView方案。

因此，就需要对各种WebView替换方案做优选比对。

> PS：iOS Hybrid开发方案的WebView没有太大问题。因为，UIWebView没有碎片化问题，且性能极佳，也不需要考虑太多兼容性的问题。



##### 目前已知 Android WebView的可选替换方案： 

1. Android系统默认的WebView
2. [腾讯 X5内核的X5WebView](https://x5.tencent.com/)
3. [Crosswalk 基于Chromium/Blink的WebView](https://github.com/crosswalk-project)
4. [Mozilla Gecko浏览器引擎的Geckoview](https://mozilla.github.io/geckoview/)


如上述几种方案的对照如下表：

##### 测试对比性能指标：(未设置浏览器缓存)

| WebView方案                | 实际效果 | html5test评测分数 | 方案说明 | 优缺点 |
|----------              |---------|--------        |-------  |-------|
| OriginalWebView(系统默认)  | 最差     | 485               | Android默认 | 优：没有额外的JAR及负担，原生API 缺: 兼容性，性能在不同手机上显示差别很大 |
| X5 WebView                 | 一般     | 494              | 腾讯产品，微信，QQ浏览器就是使用X5内核 | 优：提供了一个兼容性的解决方案,且微信，QQ浏览器都在用，可信度高  缺: 解决的能力一般，而且某些方面反而加大了开发工作量;而且不支持cordova | 
| Crosswalk                  | 最佳     | 498              |  国外为Android提供的一个融合chrome webkit的解决方案 | 优:没有兼容性，性能问题,且支持corodva 缺：18M的包，而且区分不同的arm,x86等CPU |
| GeckoView                  |          | 492              | | |

笔者有一个较老的华为荣耀3C手机，购置于3年前，分别使用系统自带的WebView,X5 WebView,Crosswalk三种模式访问html5test网站，得出的评分结果分别是:

> OriginalWebView
![系统默认](https://raw.githubusercontent.com/dust-yu/sample_webview_android/master/screenshot/Screenshot_20190506_193808_android.space.lingen.webviewdemo.jpg)

> X5 WebView
![X5 WebView](https://github.com/dust-yu/sample_webview_android/blob/master/screenshot/Screenshot_20190506_193824_android.space.lingen.webviewdemo.jpg?raw=true)

> Crosswalk WebView
![Crosswalk](https://github.com/dust-yu/sample_webview_android/blob/master/screenshot/Screenshot_20190506_193831_android.space.lingen.webviewdemo.jpg?raw=true)

> GeckoView
![GeckoView](https://github.com/dust-yu/sample_webview_android/blob/master/screenshot/Screenshot_20190506_193841_android.space.lingen.webviewdemo.jpg?raw=true)


如上所述，crosswalk的效果是显而易见；笔者所有公司的APP项目也是使用crosswalk做为Android WebView

1. 使用Chrome Webkit内核，兼容性不存在任何问题
2. 性能佳
3. 支持cordova
4. 支持前端人员可以在PC Chrome上联调

不足：

1. APP包增加了至少18M
2. 不同的CPU系统的包完全不同，笔者APP只支持arm，如果你还要支持X86 CPU，你还得加18M的大小，或者分2个APP


而X5做为腾讯的一款产品，主要目标也是解决兼容性问题，从分数上看，它在H5的支持度上并没有太多的改善；但兼容性方面应该有比较好的改善，同时有微信和QQ浏览器做支撑，也是一个比较好的选择

因此，笔者最终建议如下：

1. 你的APP对WebView需求不高，以纯原生为主，那就不用考虑WebView的兼容性及性能问题，使用系统自带的WebView就可以了
2. 如果你的APP有大量使用到WebView，加载一些H5网页，但在cordova等其它方面有要求，使用X5是一个比较好的选择，毕竟腾讯的实力摆在那，有问题你可以说，微信也是这样的（这个话非常有杀伤力）
3. 类似笔者公司这样的APP，对H5有非常高的要求，大量的业务系统是由H5完成，且需要cordova与原生进行大量的交互 ，那这种情况下,crosswalk是我们唯一的选择，虽然加大了18M，但带来的好处是显而易见的

