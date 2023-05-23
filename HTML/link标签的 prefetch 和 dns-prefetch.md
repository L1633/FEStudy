# prefetch、 preload、dns-prefetch、preconnet
  proload 资源在当前页面使用, 会优先加载
  prefetch 资源在未来页面使用, 空闲时加载

  ```
  <link rel='preload' href='style.css'>
  <link rel='prefetch' href='style.css'>

  ```

  dns-prefetch 即 DNS 预查询
  preconnect 即 DNS 预链接

  ```
  <link rel='dns-prefetch' href='https://....'>
  <link rel='preconnect' href='https://....'>

  ```

  prefetch 是资源预获取(和 preload 相关)
  dns-prefetch 是 DNS 预查询 （和 preconnect 相关）