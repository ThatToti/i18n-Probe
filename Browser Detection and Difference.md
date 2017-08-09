在做一个项目或者网站多语言的实现的时候，我们会遇到一个问题，如何准确地去检测并判断一个网页使用什么语言或者用户想要使用什么语言去阅读网页。

做出这些判断前，我们先了解一下，我们可以在什么地方去获取语言。

# 获取语言的六个主要位置

**后端返回的数据**

想要呈现不同的语言给用户，与其在众多浏览器中，想方设法准确无误地找到某个语言，不如后端直接返回一个字段给前端，前端获取到相应语言，做相应地实现与渲染。

这种方法有优有劣。

1. 优点：如果是一个网站，里面有海量的内容需要翻译，同时需要支持非常多的地区，有些地区可能存在几种变体语言，那么单纯通过前端判断，难度大，而且容易出错。后端返回一个准确的语言给前端，前端只负责加载和渲染相应的文件，这样子无疑更好。
2. 缺点：如果一个网站，需要支持的国家或者地区只有数个，翻译的字段不多，这种情况下就不宜通过这种方法去判断语言了。可以把相应的翻译资源直接在前端加载好渲染，减少了请求后端这一步，无疑节省了时间,提高了性能。

---

**在HTTP中的请求头中的Accept-Language**

```
Accept-Language:zh-CN,en;q=0.8,zh-TW;q=0.6,ja;q=0.4,zh;q=0.2
```

这里的语言顺序，是指用户想要获得的页面呈现的语言优先级，每个语言后面带着的是权重。当页面支持这个语言组中的某一些语言，那么优先显示排在前面的语言。如果都检测不到语言，最后会降级为开发文档页面提供的静态页面语言。

accept-language可以说是真正意义的用户倾向显示的语言，从一开始每个浏览器就已经为用户默认了一个语言优先级，这个优先级是在设置操作系统语言和地区的时候，自动为浏览器默认的优先级。

用户也可以在浏览器中，自定义这个语言组的优先级：

> Chrome

| ![](/assets/1-1.png) |
| :---: |
| 1、选择settings |
| ![](/assets/1-2.png) |
| 2、所有language选项，里面的优先级就是accept-language中的优先级 |

> Firefox

| ![](/assets/1-3.png) |
| :---: |
| 1、打开工具栏-&gt;点击首选项 |
| ![](/assets/1-4.png) |
| 2、到内容栏，找到语言选项 |
| ![](/assets/1-5.png) |
| 3、设置语言优先级 |

> Opera

| ![](/assets/1-6.png) |
| :---: |
| 1、在settings中找到浏览器一栏，找到语言选项 |
| ![](/assets/1-7.png) |
| 2、在语言里面设置优先级 |

> Safari

Safari的设置有些不同，并不能浏览器里面设置。

| ![](/assets/1-8.png) |
| :---: |
| 1、到系统设置下，选择语言与地区 |
| ![](/assets/1-9.png) |
| 2、语言优先级（同时修改操作系统界面的显示语言） |

> IE

| ![](/assets/1-10.png) |
| :---: |
| 1、在工具栏，找到internet选项 |
| ![](/assets/1-11.png) |
| 2、在常规中，外观下，找到语言 |
| ![](/assets/1-12.png) |
| 3、设置语言优先级 |

---

**HTML中的lang元素**

在HTML文档中，lang元素也会声明语言，我们也可以从这个地方获取向用户提供页面的语言。

* 以下文段截取自W3C文档解释

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

* 以下文段截取自[信息无障碍研究会](http://informationaccessibilityassociation.github.io/webAccessibility/language/1_html_lang.html)

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

---































