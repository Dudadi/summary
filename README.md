# vue+nginx实现灰度能力

前端在进行版本发布时，为了保证业务的可靠性，往往会先进性阶段性的灰度。先通过白名单，再陆续增加引流 10%、30%、80%、100%。本文介绍一种通过nginx的简单配置实现通过cookie、query、header实现前后端灰度。

### 1. 前端通用模板
一般灰度能力不仅仅包含前端的灰度，还包含后端的灰度。首先介绍一种通用的前端模板，包含axios封装、api mock能力。 api是基于axios，在`axios.intercepteors.request`中增加用于日志定位的`x-request-id`以及用于后台灰度的`version`字段，这样可以保证前后端同步进入灰度环境；在`axios.interceptors.response`中可以增加统一错误码的处理。mock能力的原理是通过vue.config.js中的`devServer.before`中判断是否mock，如果是mock则api走mock-server.js。mock-server读取mock文件夹的mock数据，并放在`app._router.stack`中。vue的项目都是通过webpack打包，webpack再通过express起一个nodejs服务。在`app._router.stack`中放入mock数据，类似于下面的操作，相当于提前增加一个`/hello`目录的路由。后续如果请求该路由，不需要请求外部接口，直接返回`hello world`数据。
```js
// 首页路由
router.get('/hello', (req, res) => {
  res.send('hello world');
});
```
### 2. nginx配置
再介绍下如果通过nginx实现cookie、query的灰度转发。nginx是一个异步框架的web服务器，可以用作反向代理、负载平衡器和HTTP缓存，本文仅使用它的反向代理能力。
首先通过upstream配置增加灰度服务集群apache001和生产服务集群apache002.
```
upstream apache001 {
  server 127.0.0.1:8081;
}

upstream apache002 {
  server 127.0.0.1:8082;
}
```
在具体location转发时，通过`$http_cookie`获取请求中的cookie数据，如果匹配正则表达式`.*version=1.0.0.*`则表示cookie中存在一个version值为1.0.0；通过`$arg_version = 1.0.0`表示url中是否存在一个值为`1.0.0`的version；通过`$http_version = 1.0.0`表示header中是否存在一个值为`1.0.0`的version。如果任意一个满足，则走灰度集群apache001，否则走生产服务集群apache002；

```
set $group apache002;
if ( $http_cookie ~* ".*version=1.0.0.*" ) {
    set $group apache001;
}
if ( $arg_version = 1.0.0 ) {
    set $group apache001;
}
if ( $http_version = 1.0.0 ) {
    set $group apache001;
}
proxy_connect_timeout 500s;
proxy_read_timeout 500s;
proxy_send_timeout 500s;
proxy_set_header Host $http_host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $remote_addr;
proxy_pass http://$group;
}
```




### 3. Tips
+ nginx作为一个服务它不会监听ctrl+c的信号杀死进程。windows强制停止nginx进程： taskkill /f /IM nginx.exe
