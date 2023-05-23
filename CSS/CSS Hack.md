# CSS Hack

CSS Hack原理是通过不同浏览器自身所带有的特别标识符以及 CSS 中优先级的机制来实现不同浏览器里CSS样式兼容性的问题。

https://www.bbsmax.com/A/D854oOBvJE/

https://blog.csdn.net/qq_31635733/article/details/81660897
## CSS Hack 常见形式
CSS Hack常见的有三种形式：
CSS属性Hack、CSS选择符Hack以及IE条件注释Hack， Hack主要针对IE浏览器。

* 属性级Hack：比如IE6能识别下划线“_”和星号“*”，IE7能识别星号“*”，但不能识别下划线”_ ”，而firefox两个都不能认识。

* 选择符级Hack：比如IE6能识别*html .class{}，IE7能识别*+html .class{}或者*:first-child+html .class{}。

* IE条件注释Hack：IE条件注释是微软IE5开始就提供的一种非标准逻辑语句。比如针对所有IE：&lt;!-[if IE]&gt;&lt;!-您的代码-&gt;&lt;![endif]&gt;，针对IE6及以下版本：&lt;!-[if it IE 7]&gt;&lt;!-您的代码-&gt;&lt;![endif]-&gt;，这类Hack不仅对CSS生效，对写在判断语句里面的所有代码都会生效。

PS：条件注释只有在IE浏览器下才能执行，这个代码在非IE浏览下被当做注释视而不见。可以通过IE条件注释载入不同的CSS、JS、HTML和服务器代码等

## 不推荐使用 CSS Hack
CSS hack是因为现有浏览器对标准的解析不同，为了兼容各浏览器，所采用的一种补救方法。CSS hack是一种类似作弊的手段，以欺骗浏览器的方式达到兼容的目的，是用浏览器的兼容性差异来解决浏览器的兼容性问题。因此，在设计之初，写CSS hack需要遵循以下三条原则：

* 有效： 能够通过 Web 标准的验证
* 只针对太古老的/不再开发的/已被抛弃的浏览器， 而不是目前的主流浏览器
* 代码要丑陋。让人记住这是一个不得已而为之的 Hack, 时刻记住要想办法去掉它。现在很多hacks已经抛弃了最初的原则，而滥用hack会导致浏览器更新之后产生更多的兼容性问题。因此，并不推荐使用CSS hack来解决兼容性问题。