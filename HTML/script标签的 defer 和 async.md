# script 标签的 defer 和 async
  html 的解析顺序:

  遇到无以上两个属性的 script 标签: HTML 暂停解析, 下载 JS, 执行 JS , 再继续解析 HTML;
  遇到带有 defer 属性的 script 标签: HTML 继续解析, 并行下载 JS, HTML 解析完再执行 JS;
  遇到带有 async 属性的 script 标签: HTML 继续解析, 并行下载 JS, 执行 JS, 再解析 HTML;
   
  注意: 下载 可以并行， 但是 解析 不能并行 ， 是 `互斥的` 线程!