# DOCTYPE
  `DOCTYPE` 标签是一种标准通用标记语言的文档类型声明，它的目的是要告诉标准通用标记语言解析器，它应该使用什么样的文档类型定义 `（DTD）` 来解析文档。
  
  `DTD` 是 : 文档类型定义 `（DTD, Document Type Definition）` 是一种特殊文档，它规定、约束符合标准通用标示语言（SGML）或 SGML 子集可扩展标示语言（XML）规则的定义和陈述, 以便浏览器正确呈现内容;

  `HTML 4.01` 中, <!DOCTYPE> 声明引用 DTD, 因为 `HTML 4.01` 基于 `SGML`;

  `HTML5` 不基于 `SGML`，所以不需要引用 `DTD`, 但需要 doctype 来规范浏览器行为;

  简单来说 `DTD` 是一系列的语法规则，用来定义XML或(X)TML的文件类型。浏览器会使用它来判断文档类型，决定使用何种协议来解析，以及切换浏览器模式。


## 示例

All HTML documents must start with a <!DOCTYPE> declaration.

The declaration is not an HTML tag. It is an "information" to the browser about what document type to expect.

In older documents (HTML 4 or XHTML, because HTML 4.01 was based on SGML), the declaration is more complicated because the declaration must refer to a DTD (Document Type Definition). 

`XHTML 1.1`:
```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">
```

`HTML 4.01`:
```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
```

`HTML5`

In HTML 5, the declaration is simple:

```
<!DOCTYPE html>
```
Browsers, however, do not use DTDs at all. They read the Doctype to determine the rendering mode, but the rules for parsing the document are entirely baked into the browser.

This is why HTML 5 has a Doctype (to determine the rendering mode) but not DTD.

## 总结

  `DOCTYPE` 是用来声明文档类型和 DTD 规范的，一个主要的用途便是文件的合法性验证。

  如果文件代码不合法，那么浏览器解析时便会出一些差错,;
  
  `<!DOCTYPE>` 用于告知浏览器该以何种模式来渲染文档;

