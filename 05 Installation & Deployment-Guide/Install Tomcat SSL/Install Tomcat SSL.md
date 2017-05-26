### 1.Tomcat中conf/server.xml配置

#### 1.1 找到以下配置

![PNG](..\images\tomcat\ssl.png)

#### 1.2 取消上述配置注释，并修改

```
 <Connector port="8443" protocol="org.apache.coyote.http11.Http11Protocol"
            maxThreads="150" SSLEnabled="true" scheme="https" secure="true"
           	lientAuth="false" sslProtocol="TLS" 
           	keystoreFile="conf/test.jks"  
           	keystorePass="123456"/>

```

参数keystoreFile：证书相对tomcat安装目录路径。  
参数keystorePass：为生成证书时的密码。  

### 2.Tomcat中conf/web.xml配置

完成上述server.xml配置后，tomcat会支持http,https两种访问方式，如果需要禁止http方式访问则添加如下配置：

```
 <security-constraint>
        <web-resource-collection>
            <web-resource-name>securedapp</web-resource-name>
            <url-pattern>/*</url-pattern>
        </web-resource-collection>
        <user-data-constraint>
            <transport-guarantee>CONFIDENTIAL</transport-guarantee>
        </user-data-constraint>
    </security-constraint>
```
