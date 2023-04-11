---
highlight: srcery
theme: channing-cyan
---
### 路由

#### 什么是路由？

> 路由其实是网络工程中的一个术语
>
> -   在架构一个网络时，非常重要的两个设备就是路由器和交换机。
> -   当然，目前在我们生活中路由器也是越来越被大家所熟知，因为我们生活中都会用到路由器：
> -   事实上，路由器主要维护的是一个映射表；
> -   映射表会决定数据的流向；

路由的概念在软件工程中出现，最早是在后端路由中实现的，原因是web的发展主要经历了这样一些阶段：
后端路由阶段；
前后端分离阶段；
单页面富应用（SPA）；
#### SPA
**SPA**，即**单页面应用**(Single Page Application)。SPA 是一种特殊的 Web 应用，是加载单个 HTML 页面并在用户与应用程序交互时动态更新该页面的。它将所有的活动局限于一个 Web 页面中，仅在该 Web 页面初始化时加载相应的 HTML 、 JavaScript 、 CSS 。一旦页面加载完成， SPA 不会因为用户的操作而进行页面的重新加载或跳转，而是利用 JavaScript 动态的变换 HTML（采用的是 div 切换显示和隐藏），从而实现UI与用户的交互。在 SPA 应用中，应用加载之后就不会再有整页刷新。相反，展示逻辑预先加载，并有赖于内容Region（区域）中的视图切换来展示内容

现如今，为了配合单页面 `Web` 应用快速发展的节奏，各类**前端组件化技术栈**层出不穷。近几年来，通过不断的版本迭代， `vue` 和 `react` 两大技术栈脱颖而出，成为当下最受欢迎的两大技术栈。
+ 优点
    + **有良好的交互体验**
        + 能提升页面切换的体验 用户在访问应用页面时不会频繁的去切换浏览页面，从而避免页面的重新加载

    + **前后端分离开发**
        + 单页web应用可以和restful规约一起使用，通过restapi提供接口数据库，并使用ajax异步获取
    + **共用一套后端程序代码**
        + 不用修改后端程序代码可以同时用于web界面，手机，平板等多个客户端
    + **减轻服务器压力**
        + 服务器只用发数据就可以了，不用管展示的逻辑和页面的合成
+ 缺点
    + **SEO难度高**
        + 所有内容都在一个页面中动态替换显示
    + **前进，后退管理**
        + 由于单页Web应用在一个页面中显示所有的内容，所以不能使用浏览器的前进后退功能，所有的页面切换需要自己建立堆栈管理，当然此问题也有解决方案，比如利用URI中的散列+iframe实现；
    + ** 初次加载耗时多**
        + 为实现单页Web应用功能及显示效果，需要在加载页面的时候将JavaScript、CSS统一加载，部分页面可以在需要的时候加载。所以必须对JavaScript及CSS代码进行合并压缩处理；

#### URL的和hash

-   前端路由是如何做到URL和内容进行映射呢？监听URL的改变.

-   **URL的hash**

    -   URL的hash也就是锚点（#），本质上是改变`window.location`的`href`属性
    -   我们可以通过直接赋值`location.hash`来改变`href`,但是页面不发生刷新；
    -   ```
        <!DOCTYPE html>
        <html lang="en">
          <head>
            <meta charset="UTF-8" />
            <meta http-equiv="X-UA-Compatible" content="IE=edge" />
            <meta name="viewport" content="width=device-width, initial-scale=1.0" />
            <title>Document</title>
          </head>
          <body>
            <div id="app">
              <a href="#/home">home</a>
              <a href="#/about">about</a>
              <div class="router-view"></div>
            </div>
            <script>
              // 1.获取router-view
              const routerViewEl = document.querySelector(".router-view");
              // 2.监听hashchange
              window.addEventListener("hashchange", function () {
                switch (location.hash) {
                  case "#/home":
                    routerViewEl.innerHTML = "home";
                    break;
                  case "#/about":
                    routerViewEl.innerHTML = "about";
                    break;
                  default:
                    routerViewEl.innerHTML = "default";
                    break;
                }
              });
            </script>
          </body>
        </html>
        
        ```
    - ![GIF 2023-4-10 18-15-51.gif](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cd8f737ffb4145b1854ce8461628684e~tplv-k3u1fbpfcp-watermark.image?)
#### HTML5的History
    -   hash的优势就是兼容性更好，在老版IE中都可以运行，但是缺陷是有一个#，显得不像一个真实的路径。



> -   history接口是HTML5新增的，他有六种模式改变URL而不刷新页面
>
>     -   replaceState：替换原来的路径；
>
>     -   pushState：使用新的路径；
>
>         -   `pushState()` 不会造成 [`hashchange`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/hashchange_event) 事件调用，即使新的 URL 和之前的 URL 只是锚的数据不同。
>
>     -   ppopState：路径的回退；
>
>     -   go：向前或向后改变路径；
>
>     -   forward：向前改变路径；
>
>     -   back：向后改变路径；
>     - ![GIF 2023-4-10 18-37-55.gif](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6a2e3f21beb14489abb552e71240708d~tplv-k3u1fbpfcp-watermark.image?)
>

```js
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div id="app">
      <a href="#/home">home</a>
      <a href="#/about">about</a>
      <div class="router-view"></div>
    </div>
    <script>
      // 1.获取router-view
      const routerViewEl = document.querySelector(".router-view");
      // 2.监听所有a元素
      const aEls = document.getElementsByTagName("a");
      for (const aEl of aEls) {
        aEl.addEventListener("click", function (e) {
          e.preventDefault();
          const href = aEl.getAttribute("href");
          console.log("[Log] href-->", href);
          history.pushState({}, "", href);
        });
      }

      // 3，  监听popstate和go操作
      window.addEventListener("popstate", historyChange);
      window.addEventListener("go", historyChange);
      // 4.执行设置页面操作
      function historyChange() {
        console.log("[Log] location.hash-->", location.hash);
        switch (location.hash) {
          case "#/home":
            routerViewEl.innerHTML = "home";
            break;
          case "#/about":
            routerViewEl.innerHTML = "about";
            break;
          default:
            routerViewEl.innerHTML = "default";
            break;
        }
      }
    </script>
  </body>
</html>

```