# 阿里云服务器搭建
## 一、JDK8安装
### 1.将jdk8压缩包上传到阿里云上并解压
### 2.环境配置
2.1 `vim /etc/profile`\
2.2添加配置信息\
`export JAVA_HOME=/usr/local/java/jdk8 `\
`export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar`\
`export PATH=$PATH:$JAVA_HOME/bin`\
2.3 使配置文件生效 `source /etc/profile`\
2.4 测试jdk是否安装成功 `java -version`
## 二、Mysql安装
暂时使用本地的Mysql
## 三、Tomcat安装
1.将tomcat压缩包上传到阿里云并解压\
2.环境配置\
2.1修改apache-tomcat/bin/startup.sh\
`export JAVA_HOME=/usr/local/java/jdk8`\
`export TOMCAT_HOME=/usr/local/apache-tomcat-9.0.16`\
`export CATALINA_HOME=/usr/local/apache-tomcat-9.0.16`\
`export CLASS_PATH=$JAVA_HOME/bin/lib:$JAVA_HOME/jre/lib:$JAVA_HOME/lib/tool.jar`\
`export PATH=$PATH:/usr/local/java/jdk8/bin:/usr/local/apache-tomcat-9.0.16/bin`\
2.2 修改apache-tomcat/bin/shutdown.sh\
`export JAVA_HOME=/usr/local/java/jdk8`\
`export TOMCAT_HOME=/usr/local/apache-tomcat-9.0.16`\
`export CATALINA_HOME=/usr/local/apache-tomcat-9.0.16`\
`export CLASS_PATH=$JAVA_HOME/bin/lib:$JAVA_HOME/jre/lib:$JAVA_HOME/lib/tool.jar`\
`export PATH=$PATH:/usr/local/java/jdk8/bin:/usr/local/apache-tomcat-9.0.16/bin`\
2.3 安装iptables 修改/etc/sysconfig/iptables\
`-A INPUT -p tcp -m state --state NEW -m tcp --dport 80 -j ACCEPT`\
2.4 设置iptables开机启动\
`systemctl enable iptables.service`\
2.5 去阿里云管理控制台添加8080端口的安全组\
2.6 查看开放端口 `netstat -tlunp`\
2.7解决8005端口报错tomcat未成功启动和无法关闭的问题\
`#修改$JAVA_HOME/jre/lib/security/java.security 文件中 securerandom.source 配置项`\
`将原本的：securerandom.source=file:/dev/random `\
`修改为： securerandom.source=file:/dev/urandom`
## 四、Zookeeper安装
1.将zookeeper压缩包上传到阿里云上并解压\
2.环境配置\
2.1 在zookeeper中新建data文件夹,作为zookeeper数据存储文件夹\
`mkdir data`\
2.2 进入conf文件夹 复制zoo_sample.cfg并起名为zoo.cfg\
`cp zoo_sample.cfg zoo.cfg`\
2.3 修改zoo.cfg中dataDir属性值为新建data文件夹的路径\
`vim zoo.cfg`\
2.4 进入zookeeper/bin目录,使用zkServer.sh start 启动zookeeper\
2.5 开放2181端口\
`-A INPUT -p tcp -m state --state NEW -m tcp --dport 8080 -j ACCEPT`
## 五、Dubbo安装
1.将dubbo-admin.war上传到阿里云服务器的tomcat中webapps下\
2.项目部署\
2.1先启动zookeeper,然后启动tomcat,启动成功后关闭tomcat,删除dubbo-admin.war,防止下次启动tomcat时覆盖这个项目.修改webapps/dubbo-admin/WEB-INF下的dubbo.properties\
`dubbo.registry.address=zookeeper://服务器ip:2181`\
`#登录时的用户名和密码,默认都为root`\
`dubbo.admin.root.password=root`\
`dubbo.admin.guest.password=guest`\
2.2 部署完成后,先启动zookeeper,再启动tomcat,在浏览器中访问`http://服务器ip:8080/dubbo-admin/`输入用户名密码后可以看到dubbo的图形用户管理界面\
## 六、Nginx安装
## 七、VSFTPD安装
## 八、Mosquitto安装
## 九、Redis安装
