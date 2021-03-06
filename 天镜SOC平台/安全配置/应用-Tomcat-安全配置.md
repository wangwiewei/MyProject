# Tomcat 安全配置

### 1. 网络访问控制

- 如果您的业务不需要使用 Tomcat 管理后台管理业务代码，建议您使用[安全组防火墙](https://www.alibabacloud.com/help/zh/doc-detail/25475.htm)功能对管理后台 URL 地址进行拦截，或直接将 Tomcat 部署目录中 webapps 文件夹中的 manager、host-manager 文件夹全部删除，并注释 Tomcat 目录中 conf 文件夹中的 tomcat-users.xml 文件中的所有代码。
- 如果您的业务系统确实需要使用 Tomcat 管理后台进行业务代码的发布和管理，建议为 Tomcat 管理后台配置强口令，并修改默认 admin 用户，且密码长度不低于10位，必须包含大写字母、特殊符号、数字组合。

### 2. 开启 Tomcat 的访问日志

修改 conf/server.xml 文件，将下列代码取消注释：

```xml
<Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"   
prefix="localhost_access_log." suffix=".txt" pattern="common" resolveHosts="false"/>
```

启用访问日志功能，重启 Tomcat 服务后，在 tomcat_home/logs 文件夹中就可以看到访问日志。

### 3. Tomcat 默认帐号安全

修改 Tomcat 安装目录 conf 下的 tomcat-user.xml 文件，重新设置复杂口令并保存文件。重启 Tomcat 服务后，新口令即生效。

### 4. 修改默认访问端口

修改 conf/server.xml 文件把默认的 8080 访问端口改成其它端口。

### 5. 重定向错误页面

修改访问 Tomcat 错误页面的返回信息，在 webapps\manger 目录中创建相应的401.html、404.htm、500.htm 文件，然后在 conf/web.xml 文件的最后一行之前添加下列代码：

```xml
  <error-page>      
                             <error-code>401</error-code>              
                             <location>/401.htm</location>          
                     </error-page>          
                     <error-page>    
                             <error-code>404</error-code>        
                             <location>/404.htm</location>          
                     </error-page>  
                     <error-page>    
                             <error-code>500</error-code>  
                             <location>/500.htm</location>      
                      </error-page>
```

### 6. 禁止列出目录

在web.xml文件中，防止直接访问目录时由于找不到默认页面，而列出目录下的文件的情况。

```xml
<param-name>listings</param-name>
        <param-value>false</param-value>
```

### 7. 删除文档和示例程序

删除 webapps 目录下的 docs、examples、manager、ROOT、host-manager 文件夹。