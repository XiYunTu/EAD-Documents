### 中间件

在 EAD 中，通过中间件的方式，可以对所有的请求进行前置处理。

由于 EAD 对视图请求及视图动作请求都是经过统一路由进行处理，所以在统一路由入口进行中间件的注入，则可以对所有的视图请求及视图动作请求执行前置方法；

#### 前置方法配置方法

第一步：实现前置类及方法编码，并加入Spring Bean
第二步：实现前置方法:
```
void {ClassName} {method}(HttpServletRequest request, HttpServletResponse response, ConfigService config, MessageBuilder message, EadApp eadApp, Locale locale) {
	
}
```
第三步：在开发者中心**驱动管理**中配置该驱动：
第四步：在开发者中心**变量管理**中把配置的驱动名称加入**before-filters**

> 注意：若有多个前置中间件，填入驱动名称，并使用逗号进行分割；

#### 前置方法举例：

- 中间件实现：Http Header Token自动登录
- 中间件类名：AccountAction
- 中间件方法名：headTokenAuth
- 中间件方法实现：

```
/**
   * HttpHead Token认证：
   *   1、认证通过后，若Session存在，则无需登录；
   *   2、认证通过后，若Session不存在，自动登录
   */
  public void headTokenAuth(HttpServletRequest request, HttpServletResponse response, 
      ConfigService configService, MessageBuilder messageBuilder, 
      EadApp eadApp, Locale locale) throws Exception {
    Session session = (Session) request.getSession().getAttribute("session");
    String account = RequestKit.getHead("userId", request);
    if (StringKit.isNotBlank(account)) {
      String time = RequestKit.getHead("time", request);
      String token = RequestKit.getHead("token", request);
      String scretKey = StringKit.toString(variableFactory.getValue("http-head-key"));
      //Http Head Token 认证
      if (AuthKit.authToken(account, StringKit.convertToLong(time), token, scretKey)) {
        //Session信息存在，无需登录
        if (session != null && account.equals(session.getAccount())) {
          return;
        } else { //Session 信息不存在，自动登录
          SysAccount sysAccount = sysAccountService.getByAccount(account);
          sysAccountService.autoLogin(sysAccount, request, response, eadApp, locale);
        }
      } else {
        throw new HillException("auth_failed");
      }
    }
  }
```
- 驱动名称：headTokenAuth
- 变量配置：before-filters 变量中加入：headTokenAuth

