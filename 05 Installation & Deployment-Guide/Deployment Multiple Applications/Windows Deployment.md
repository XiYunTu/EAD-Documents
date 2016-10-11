#### 1. 部署

分别将打包好的不同应用的 **gbros-web.war** 包部署到服务器 Tomcat环境中。

拷贝位置为：`Tomcat > webapps_pro > war` 下。
          `Tomcat > webapps_ead > war` 下。
          `Tomcat > webapps_demo > war` 下。
          `Tomcat > webapps_study > war` 下。
          `Tomcat > webapps_xyt > war` 下。

#### 2. Tomcat相关配置

第一步：在Tomcat/conf/server.xml中添加相关应用配置；

![PNG](..\images\tomcat\11.png)

第二步： 在Tomcat/bin/catalina.bat中添加tomcat内存配置；

![PNG](..\images\tomcat\12.png)

#### 3.多应用下Tomcat共享JAR配置（扩展配置）
第一步：在Tomcat下建立/shared/lib目录。  
第二步：将各个应用下中/ROOT/WEB-INF/lib相同JAR文件删除，统一放到shared/lib目录中。  
第三步：在Tomcat/catalina.prpperties中添加配置：
![PNG](..\images\tomcat\13.png)

#### 4. Windows 下启动 Tomcat

第一种方式：Windows 服务方式启动

在 Windows 服务中启动 Tomcat 服务；

![PNG](..\images\tomcat\2.png)

第二种方式：命令行方式启动

在 Windows 服务中，启动 Tomcat 或者在 `Tomcat > bin` 下，启动 startup.bat。

当看到以下界面时，说明启动成功：

![PNG](..\images\tomcat\4.png)



