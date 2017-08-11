在做一个项目或者网站多语言的实现的时候，我们会遇到一个问题，如何准确地去检测并判断一个网页使用什么语言或者用户想要使用什么语言去阅读网页。

做出这些判断前，我们先了解一下，我们可以在什么地方去获取语言。

# 获取语言的六个主要位置

## **后端返回的数据**

想要呈现不同的语言给用户，与其在众多浏览器中，想方设法准确无误地找到某个语言，不如后端直接返回一个字段给前端，前端获取到相应语言，做相应地实现与渲染。

这种方法有优有劣。

1. 优点：如果是一个网站，里面有海量的内容需要翻译，同时需要支持非常多的地区，有些地区可能存在几种变体语言，那么单纯通过前端判断，难度大，而且容易出错。后端返回一个准确的语言给前端，前端只负责加载和渲染相应的文件，这样子无疑更好。
2. 缺点：如果一个网站，需要支持的国家或者地区只有数个，翻译的字段不多，这种情况下就不宜通过这种方法去判断语言了。可以把相应的翻译资源直接在前端加载好渲染，减少了请求后端这一步，无疑节省了时间,提高了性能。

------

## **在HTTP中的请求头中的Accept-Language**

```
Accept-Language:zh-CN,en;q=0.8,zh-TW;q=0.6,ja;q=0.4,zh;q=0.2
```

这里的语言顺序，是指用户想要获得的页面呈现的语言优先级，每个语言后面带着的是权重。当页面支持这个语言组中的某一些语言，那么优先显示排在前面的语言。如果都检测不到语言，最后会降级为开发文档页面提供的静态页面语言。

accept-language可以说是真正意义的用户倾向显示的语言，从一开始每个浏览器就已经为用户默认了一个语言优先级，这个优先级是在设置操作系统语言和地区的时候，自动为浏览器默认的优先级。

用户也可以在浏览器中，自定义这个语言组的优先级：

> Chrome

|           ![](/assets/1-1.png)           |
| :--------------------------------------: |
|               1、选择settings               |
|           ![](/assets/1-2.png)           |
| 2、所有language选项，里面的优先级就是accept-language中的优先级 |

> Firefox

| ![](/assets/1-3.png) |
| :------------------: |
|  1、打开工具栏-&gt;点击首选项   |
| ![](/assets/1-4.png) |
|    2、到内容栏，找到语言选项     |
| ![](/assets/1-5.png) |
|      3、设置语言优先级       |

> Opera

|    ![](/assets/1-6.png)    |
| :------------------------: |
| 1、在settings中找到浏览器一栏，找到语言选项 |
|    ![](/assets/1-7.png)    |
|        2、在语言里面设置优先级        |

> Safari

Safari的设置有些不同，并不能浏览器里面设置。

|   ![](/assets/1-8.png)   |
| :----------------------: |
|     1、到系统设置下，选择语言与地区     |
|   ![](/assets/1-9.png)   |
| 2、语言优先级（同时修改操作系统界面的显示语言） |

> IE

| ![](/assets/1-10.png) |
| :-------------------: |
|  1、在工具栏，找到internet选项  |
| ![](/assets/1-11.png) |
|    2、在常规中，外观下，找到语言    |
| ![](/assets/1-12.png) |
|       3、设置语言优先级       |

------

## **HTML中的lang元素**

在HTML文档中，lang元素也会声明语言，我们也可以从这个地方获取向用户提供页面的语言。

- 以下文段截取自W3C文档解释

> The`lang`attribute \(in no namespace\) specifies the primary language for the element's contents and for any of the element's attributes that contain text. Its value must be a valid BCP 47 language tag, or the empty string. Setting the attribute to the empty string indicates that the primary language is unknown.[\[BCP47\]](https://html.spec.whatwg.org/multipage/references.html#refsBCP47)
>
> The`lang`attribute in the[XML namespace](https://infra.spec.whatwg.org/#xml-namespace)is defined in XML.[\[XML\]](https://html.spec.whatwg.org/multipage/references.html#refsXML)
>
> If these attributes are omitted from an element, then the language of this element is the same as the language of its parent element, if any.
>
> The[`lang`](https://html.spec.whatwg.org/multipage/dom.html#attr-lang)attribute in no namespace may be used on any[HTML element](https://html.spec.whatwg.org/multipage/infrastructure.html#html-elements).
>
> The[`lang`attribute in theXML namespace](https://html.spec.whatwg.org/multipage/dom.html#attr-xml-lang)may be used on[HTML elements](https://html.spec.whatwg.org/multipage/infrastructure.html#html-elements)in[XML documents](https://dom.spec.whatwg.org/#xml-document), as well as elements in other namespaces if the relevant specifications allow it \(in particular, MathML and SVG allow[`lang`attributes in theXML namespace](https://html.spec.whatwg.org/multipage/dom.html#attr-xml-lang)to be specified on their elements\). If both the[`lang`](https://html.spec.whatwg.org/multipage/dom.html#attr-lang)attribute in no namespace and the[`lang`attribute in theXML namespace](https://html.spec.whatwg.org/multipage/dom.html#attr-xml-lang)are specified on the same element, they must have exactly the same value when compared in an[ASCII case-insensitive](https://infra.spec.whatwg.org/#ascii-case-insensitive)manner.
>
> Authors must not use the[`lang`attribute in theXML namespace](https://html.spec.whatwg.org/multipage/dom.html#attr-xml-lang)on[HTML elements](https://html.spec.whatwg.org/multipage/infrastructure.html#html-elements)in[HTML documents](https://dom.spec.whatwg.org/#html-document). To ease migration to and from XML, authors may specify an attribute in no namespace with no prefix and with the literal localname "`xml:lang`" on[HTML elements](https://html.spec.whatwg.org/multipage/infrastructure.html#html-elements)in[HTML documents](https://dom.spec.whatwg.org/#html-document), but such attributes must only be specified if a[`lang`](https://html.spec.whatwg.org/multipage/dom.html#attr-lang)attribute in no namespace is also specified, and both attributes must have the same value when compared in an[ASCII case-insensitive](https://infra.spec.whatwg.org/#ascii-case-insensitive)manner.

- 以下文段截取自[信息无障碍研究会](http://informationaccessibilityassociation.github.io/webAccessibility/language/1_html_lang.html)

> 1）它允许盲人翻译软件来代替变音字符的控制码，并插入必要的代码来防止二级盲文收缩的错误产生
>
> 2）支持多语言的语音合成将会定向适配网页指定语言的发音和语言，使用正确的发音读出文本
>
> 3）标记语言可以得益于未来的技术法阵，例如，不能在自主翻译语言的用户可以使用机器翻译不熟悉的语言。
>
> 4）标记语言可以帮助用户代理提供字典解释。

在lang中声明的语言，是开发者定义的这个页面的主要语言，是从页面文档角度希望呈现给用户的语言。如果用户希望翻译为别的语言，那么还可以结合一些无障碍化技术，或者一些在线翻译技术来进行转换。

当我们做多语言检测的时候，这个是个值得考虑入检测范围的一个获取语言的地方。但是从这个地方获取的语言，并不是从用户角度去出发，而更多是开发者角度，文档想让用户看到什么样的语言。

如果对比用户语言倾向性和开发者提供语言倾向性，应该用户倾向性优先级大于开发者倾向性的。也就是当用户倾向的语言都无法提供翻译，那么再降级为开发者想提供给用户翻译的语言。

------

## 浏览器中的Storage

**简介**

当我们获取到用户的语言信息后，我们往往会考虑将它们缓存下来，以便第二次访问的时候，可以跳过读取接口信息这一步，增加性能，加快速度。

Storage对象又分为localStorage和sessionStorage，它们的具体区别在这里不做详细介绍，可以访问[Storage](https://developer.mozilla.org/en-US/docs/Web/API/Storage)了解更多。

在这里，对它们做一些简单的介绍：

- localStorage：长期储存，在浏览器关闭后，信息依旧在本地，不会随着页面关闭而消失
- sessionStorage：会话储存，生命周期仅在会话连接保持的时候，当页面关闭、新开同样的页面和浏览器关闭后，存储的东西就会被清除。但是重新刷新页面或者恢复页面，信息不会受到影响。



**实现**

我们来看看如何通过storage来储存和读取语言信息：

```
localStorage.setItem('lng','zh');
localStorage.setItem('author','Toti')
```

然后在开发者工具里面-应用-储存-localStorage可以找到相应的信息：

| Key    | Value |
| ------ | ----- |
| lng    | zh    |
| author | Toti  |

获取localStorage里面的信息：

```
localStorage.getItem('lng')
//“zh”
```



**优化**

通常用户的语言选择是一系列的语言，我们通过读接口或者navigator对象获得的是一个数组，如果我们把每个语言都用一条key-value对去储存，那么会很浪费储存空间，这时候更好的一个做法是把语言组放在一条key-value里面，定义一种方法存入，再用同一种方法解析出来。

```
localStorage.setItem('lng','0=zh&1=en&2=ja')
```

| Key    | Value          |
| ------ | -------------- |
| lng    | 0=zh&1=en&2=ja |
| author | Toti           |

当我们取出数据的时候，只要以‘&’分隔每一个语言，再存入一个变量即可。



**用户分析**

在选择使用localStorage还是sessionStorage作为储存用户语言倾向数据的方式的时候，我们知道，用户的语言习惯不会每打开一次浏览器就改变一次，这是一个长期的习惯。当然用户出于各种原因会操作到切换语言的功能，但是我们依然可以认为这不是一个频繁的操作。

所以正常情况下，使用localStorage作为语言数据储存是更合理的选择；如果出于定制需求，选择sessionStorage也是可以的。



**兼容性与储存大小** 

我们来看一下它们的支持度和储存大小：

> 支持度

| Storage        | IE   | Chrome | Firefox | Opera | Safari |
| -------------- | ---- | ------ | ------- | ----- | ------ |
| localStorage   | 8    | 4      | 3.5     | 10.5  | 4      |
| sessionStorage | 8    | 5      | 2       | 10.5  | 4      |

> 移动端储存大小

| Storage        | Chrome    | Safari    | QQ        | UC    |
| :------------- | --------- | --------- | --------- | ----- |
| localStorage   | 2.49M     | 2.49M     | 2.49M     | 2.49M |
| sessionStorage | unlimited | unlimited | unlimited | 2.49M |

> - 在Android 6.0下存储大小为4.98M，但在7.0后改为2.49M
> - PC端的浏览器，储存大小大部分为4.98M

------

## Http中的Cookie

**简介**

Cookie是Http头中的一部分，当使用cookie去储存语言数据的时候，我们就要慎重地考虑cookie的大小。因为它是一种客户端-服务器式的储存，过程中需要连接，需要传输数据，这就意味着我们的数据大小，直接影响传输的速度，进而影响页面之后语言翻译。

更多关于cookie的资料，可以访问[cookie](https://developer.mozilla.org/en-US/docs/Web/API/Document/cookie)



**实现**

设置语言数据进cookie：

```
document.cookie = "lng=zh";
//“lng=zh”
```

获取数据：

```
document.cookie.replace(/(?:(?:^|.*;\s*)lng\s*\=\s*([^;]*).*$)|^.*$/, "$1");
//“zh”
```



**优化**

和Storage的储存优化一样，我们可以在一条key-value里面，存入多个语言数据，达到减小数据体积的作用。

```
document.cookie = "languages=lng0=zh&lng1=ja";
//“lng0=zh&lng1=ja“
```

获取的时候，根据相应的解析规则，取出其中的数据即可。



**分析**

使用cookie来储存语言数据，由于一般cookie的大小限制在4095B，所以cookie能携带的数据并不多。相比于Storage的5M大小，在能使用Storage的情况下，应该优先使用Storage。

但是具体情况下，需要根据语言数据存在的多个位置去获取信息，cookie自然是一个要进行检测的地方。

------

## NavigatorLanguage对象

**简介** 

Navigator对象的language接口是一个从浏览器本地获取用户语言倾向性的位置。但是navigator对象每个浏览器对它的现实都不一样，而且在国内的网络上所谓的说明文档也大都存在问题，使得不少开发者在使用的时候，多语言的功能并没有真实实现。



**说明文档乱象**

以下是[W3CSchool](http://www.w3school.com.cn/jsref/dom_obj_navigator.asp)中文版的navigator文档关于language部分：

| 属性                                       | 描述             |
| ---------------------------------------- | -------------- |
| [browserLanguage](http://www.w3school.com.cn/jsref/prop_nav_browserlanguage.asp) | 返回当前浏览器的语言。    |
| [systemLanguage](http://www.w3school.com.cn/jsref/prop_nav_systemlanguage.asp) | 返回 OS 使用的默认语言。 |
| [userLanguage](http://www.w3school.com.cn/jsref/prop_nav_userlanguage.asp) | 返回 OS 的自然语言设置。 |

以下是[W3CSchool](https://www.w3schools.com/jsref/obj_navigator.asp)英文版的：

| 属性                                       | 描述                                  |
| ---------------------------------------- | ----------------------------------- |
| [language](https://www.w3schools.com/jsref/prop_nav_language.asp) | Returns the language of the browser |

以下是[菜鸟教程](http://www.runoob.com/jsref/obj-navigator.html)的：

| 属性   | 描述   |
| ---- | ---- |
| 无    | 无    |

以下是[MDN](https://developer.mozilla.org/en-US/docs/Web/API/NavigatorLanguage/languages)的：

| 属性        | 描述                                       |
| --------- | ---------------------------------------- |
| language  | read-only property returns a string representing the preferred language of the user, usually the language of the browser UI. |
| languages | read-only property returns an array of [`DOMString`](https://developer.mozilla.org/en-US/docs/Web/API/DOMString)s representing the user's preferred languages. |



**language接口的多浏览器兼容性**

PC端：

| Api             | IE        | Chrome    | FireFox   | Opera     | Safari    |
| --------------- | --------- | --------- | --------- | --------- | --------- |
| language        | 11        | yes       | 1         | yes       | yes       |
| languages       | undefined | yes       | yes       | undefined | yes       |
| browserLanguage | yes       | undefined | undefined | undefined | undefined |
| systemLanguage  | yes       | undefined | undefined | undefined | undefined |
| userLanguage    | yes       | undefined | undefined | undefined | undefined |

移动端：

| Api       | Chrome    | Safari    | QQ        | UC        |
| --------- | --------- | --------- | --------- | --------- |
| language  | yes       | yes       | yes       | yes       |
| Languages | undefined | undefined | undefined | undefined |



**Language接口差异性**

由于网络上的文档解释千差万别，而许多开发者分享在论坛或者社区的文章也并没有详细谈到其中的差别，故在此一一解释其中的区别。

- language
  - 除了IE外，其他浏览器基本支持这个api，而IE则是到了11才实现。
  - Chrome下，language获取的是页面的界面语言，并不是获取的用户设置里面的优先级最高的语言！
    - 当界面的语言没有进行切换的时候，language和languages[0]相同
    - 当界面语言经过翻译后，language是界面语言，languages[0]依旧是用户语言优先级最高的语言
  - FF／Opera／Safari下，language始终代表的是用户优先级最高的语言，即langauge==languages[0]


- languages
  - IE／Opera／移动端，均不支持这个接口
  - Chrome／FireFox，代表用户设置的语言优先级数组，与http请求头中的accept-language的语言顺序一致
  - Safari中的languages数组长度为1，就只能获取到languages[0]


- systemLangauge
  - IE下才有的接口，指的是操作系统语言


- userLanguage
  - IE下才有的接口，指的是自然语言，就是在控制面板里面设置的地区语言


- browserLanguage
  - IE下才有的接口，指的并不是浏览器设置的语言！！！
  - 在IE5后，微软规定IE的浏览器语言与操作系统绑定，所以这个接口与systemLanguage表现一致，并不能用来判断浏览器中用户设置的语言。就是无论什么语言的IE，都是这个接口读出来的都是systemLanguage
  - [官方文档解释](https://msdn.microsoft.com/en-us/library/ms533542(v=vs.85).aspx)



**实现**

由于各浏览器的差异，所以实现以下的函数进行判断，尽可能忠于用户的倾向性：

```js
function detectLng(){
  	var lng=[],
  	 	Chrome=navigator.userAgent.indexOf('Chrome') > -1?true:false,
  		FF=navigator.userAgent.indexOf('Gecko') > -1?true:false;
  
    if((Chrome||FF)&&navigator.languages){
      //chrome和ff的客户端用languages获取
		lng=navigator.languaes;
    }else if(navigator.language){
      //safari，opera和一众移动端用langauge获取
        lng=navigator.language;
    }else if(navigator.userLanguage){
      //IE获取地区语言
        lng=navigator.userLanguage;
    }
}
```

