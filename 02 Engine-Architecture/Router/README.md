### Router 规则

EAD 交互由一些固定 API 格式进行路由，通过这些 API，执行不同的业务模块。
路由规则由四个部分组成：

- 应用路由
- 系统路由
- 视图路由
- 视图动作路由

除应用路由以外，其他路由都是基于标准的 RESTful 路由规则。

#### 应用路由

| 路由规则 | HTTP 方法 | Content-Type | 路由参数说明 | 路由说明 | 
| ------- | -------- | ------------ | ------- | ------- | 
| /{appName} | GET |  text/html | appName: 应用标识 | 应用入口，后端引擎收到请求，加载应用 HTML 文件 |

#### 系统路由

声明：

- 所有视图请求为 Content-Type 为 `application/json`；
- 公共前置路由 `/{appName}/api`, appName 为应用标识；

| 路由规则 | HTTP 方法 | 路由参数说明 | 路由说明 | 
| ------- | -------- | ------- | ------- | 
| /signin | POST |  | 登录应用 |
| /signout | POST | | 退出登录 |
| /res | GET | | 应用资源树请求 |
| /change-password | POST | | 修改密码 |
| /change-role | GET | | 角色切换 |
| /login-app | GET | | 单点登录 API |

#### 视图路由

声明：

- 所有视图请求为 HTTP 方法都为 `GET`；
- 所有视图请求为 Content-Type 为 `application/json`；
- 公共前置路由 `/{appName}/api`, appName 为应用标识；

| 路由规则 | 路由参数说明 | 路由说明 | 
| ------- | ------- | ------- | 
| /{resPath} | resPath: 资源路径 | 视图访问 |
| /{resPath}/{id} | resPath: 资源路径;<br/>id: 数据主键； | 详细数据视图访问 |
| /{resPath}/{id}/{attrName} | resPath: 资源路径;<br/>id: 数据主键;<br/>attrName: 子视图属性名称 | 子视图访问 |
| /{resPath}/{id}/{attrName}/{id} | resPath: 资源路径;<br/>id: 数据主键;<br/>attrName: 子视图属性名称;<br/>id: 子视图数据主键; | 子视图详细数据访问 |

> 注意：最后一条路由：{attrName}/{id} 可以进行嵌套，例如：{attrName}/{id}/{attrName}/{id}/{attrName}/{id}...

#### 视图动作路由

声明：
- 所有视图请求为 Content-Type 为 `application/json`；
- 公共前置路由 `/{appName}/api`, appName 为应用标识；

| 路由规则 | HTTP 方法 | 路由参数说明 | 路由说明 | 
| ------- | --------  | ------- | ------- | 
| /{resPath} | POST | resPath: 资源路径 | 数据新增操作 |
| /{resPath}/{actionKey} | POST | resPath: 资源路径;<br/> actionKey: 动作标识 | 其他特殊操作 |
| /{resPath}/{id} | PUT | resPath: 资源路径;<br/>id: 数据主键； | 数据修改操作 |
| /{resPath}/{actionKey}/{id} | PUT | resPath: 资源路径;<br/>id: 数据主键;<br/>actionKey: 动作标识 | 数据修改操作 |
| /{resPath} | DELETE | resPath: 资源路径;<br/> | 批量数据删除操作 |
| /{resPath}/{id} | DELETE | resPath: 资源路径;<br/>id: 数据主键; | 单条数据删除操作 |
| /{resPath}/{id}/{attrName} | POST | resPath: 资源路径;<br/>attrName: 子视图名称 | 子视图数据新增操作 |
| /{resPath}/{id}/{attrName}/{actionKey} | POST | resPath: 资源路径;<br/>attrName: 子视图名称<br/>actionKey: 动作标识 | 子视图其他特殊操作 |
| /{resPath}/{id}/{attrName}/{id} | PUT | resPath: 资源路径;<br/>id: 数据主键;<br/>attrName: 子视图名称;<br/>id: 子视图数据主键; | 子视图数据修改操作 |
| /{resPath}/{id}/{attrName}/{actionKey}/{id} | PUT | resPath: 资源路径;<br/>id: 数据主键;<br/> attrName: 子视图名称;<br/>id: 子视图数据主键;<br/>actionKey: 动作标识 | 子视图数据修改操作 |
| /{resPath}/{id}/{attrName} | DELETE | resPath: 资源路径;<br/>id: 数据主键;<br/>attrName: 子视图名称; | 子视图批量数据删除操作 |
| /{resPath}/{id}/{attrName}/{id} | DELETE | resPath: 资源路径;<br/>id: 数据主键;<br/>attrName: 子视图名称;<br/>id: 子视图数据主键; | 子视图单条数据删除操作 |


> 注意：<br/>
> 最后一条路由：{attrName}/{id} 可以进行嵌套，例如：{attrName}/{id}/{attrName}/{id}/{attrName}/{id}...<br/>







