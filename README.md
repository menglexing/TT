# 虎牙公共头

依赖 jQuery 1.7+

### 使用示例

专题活动(hd.huya.com, www.huya.com/act)

```html
    ...
    ...
    ...
    <script src="//a.msstatic.com/huya/common/jquery-1.12.4.min.js"></script>
    <script src="//a.msstatic.com/huya/common/header_lg.js"></script>
    ...
    <script src="//a.msstatic.com/huya/common/footer.js"></script>
</body>
```

个人中心(i.huya.com)

```html
    ...
    ...
    ...
    <script src="//a.msstatic.com/huya/common/jquery-1.12.4.min.js"></script>
    <script src="//a.msstatic.com/huya/common/header.js"></script>
    ...
    <script src="//a.msstatic.com/huya/common/footer.js"></script>
</body>
```

测试环境用

```html
<script src="//test.dwstatic.com/common/header_lg.js"></script>
<script src="//test.dwstatic.com/common/header.js"></script>
<script src="//test.dwstatic.com/common/footer.js"></script>
```

*注意：公共头的DOM是以异步的方式生成并插入到`<body>`中的，且`position: fixed;`，为了防止页面内容被遮挡，你需要在自己的项目中设置`body {padding-top: 50px;}`或者用一个高度为50px的空盒子为它预留位置*

### TT (Tiger Teeth ^_^)

公共头会创建一个全局对象 ——— TT

* TT.sudo(callback, todo)

  在未登录的情况下调用此方法，callback不会被执行，(todo不为`false`时)会触发登录操作；已登录的情况下，则会以当前用户信息`{uid, userName, userNick, userLogo}`作为实参调用callback: *（注意，callback的执行是异步的）*

```javascript
// 订阅
$('#J_follow').on('click', function(e){
    TT.sudo(function(userInfo){
        follow()
    })
})

// 欢迎
$('#welcome_text').text('未登录')

TT.sudo(function(data){
    $('#welcome_text').text('您好，' + data.userNick)
}, false)
```

* TT.getUserInfo(callback)

  获取用户信息

```javascript
TT.getUserInfo(function(data){
    // 当用户未登录时，data.uid => 0, data.userName => '', data.userNick => '', data.userLogo => ''
})
```

* TT.login

  登录相关

  * TT.login.login() 

    登录

  * TT.login.logout() 

    退出

  * TT.login.register() 

    注册

  * TT.login.check(callback) 

    检测是否已登录，以检测结果(布尔值)作为实参调用callback *(不是通过读cookie实现，而是发请求到后台查询的，所以callback是异步调用的)*

* TT.event

  全局事件总线，文档见 https://github.com/huya-fed/Event

* TT.store

  具有跨域能力的localStorage，文档见 https://github.com/huya-fed/crossStorage

* TT.domain

  各项目域名
  
  * TT.domain.main 主站
  
  * TT.domain.i 个人中心
  
  * TT.domain.hd 活动

* TT.app

  各项目的URL
  
  * TT.app.main 等价于 => `location.protocol + '//' + TT.domain.main + '/'`
  
  * TT.app.i 等价于 => `location.protocol + '//' + TT.domain.i + '/'`
  
  * TT.app.hd 等价于 => `location.protocol + '//' + TT.domain.hd + '/'`


* TT.trimUrl(url)

  将http的url中的协议部分去掉，内部实现：`return $.trim(url).replace(/^http\:/, '')`
  
* TT.log(a, b, c...)

  封装了`console.log`，仅在开发环境和测试环境下可用；如需在线上环境使用，须设置`localStorage.setItem('TT_DEBUG', 1)`

* TT.env

  环境标识（0, 生产环境; 1, 测试环境; 2, 开发环境;）
  
* TT.isProd

  是否是生产环境，等价于 `TT.env === 0`
