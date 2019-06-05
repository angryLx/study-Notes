# something about Browser

## 关于缓存
#### 缓存的主要目的，在一定时间内减少数据请求，加快页面的加载速度。目前浏览器缓存机制共有9种，HTML文件缓存、Application、localStorage、sessionStorage、indexDB、Web SQL、Cookis、Cache Stroage、Application Cache。
HTML文件缓存在一定时间内

LocalStorage主要指本地缓存，在关闭窗口后依然会存在。第一次进入页面加载完毕后，第二次进入页面若localStorage中还存在对应数据，将不再进行二次请求，加快页面的渲染速度。

SessionStorage在关闭窗口后就会清除，同一浏览器窗口同一端口下的项目，sessionStroage会受到污染。

Cookis主要用户身份验证，时间范围内，cookis未过期的情况下，可直接验证cookis对应的用户信息。
