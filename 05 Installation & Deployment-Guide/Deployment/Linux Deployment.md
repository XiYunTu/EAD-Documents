### 3.2 Linux下部署

#### 1. 准备程序包
第一步：设置配置数据库参数

![PNG](..\images\tomcat\7.png)

![PNG](..\images\tomcat\8.png)

第二步：使用相关程序打包

![PNG](..\images\tomcat\9.png)

#### 2. 部署

把打包好的 **gbros-web-1.0.war** 名称修改为：**gbros-web.war**，然后把该 war 包部署到服务器 Tomcat环境中。

拷贝位置为：`apache-tomcat-7.0.69 > webapps_pro > war` 下。

#### 3. Linux 下启动 Tomcat

在`apache-tomcat-7.0.69 > bin`下,执行命令：`sh catalina.sh start`

