搭配linux系统服务器总结

方案一：虚拟机方法：VMware12+centos6.9

遇到问题：linux服务器ping不通实体机IP，实体机IP能ping通虚拟机

原因：校园网禁止虚拟机



方案二：阿里云轻量级应用服务器+centos7.3+jdk1.8.0_171+tomcat8.5.32

步骤：

1. 阿里云官网租一台服务器，可选择云服务器ECS和轻量级应用服务器，我选择轻量级应用服务器

2. 系统镜像centos7.3

3. 官网下载Xshell6plus(包括Xshell6和Xftp 6)

4. 远程连接linux服务器：

   1. 在阿里云控制台上选择**远程连接**->选择方法3：**账号密码连接**![](G:\笔记\linux\搭建环境步骤\远程连接.PNG)
   2. 设置密码后，用Xshell6进行账号密码连接

5. 传输文件：

   1. linux系统安装JDK步骤：
      1. JDK官网下载JDK的linux版本*.tar.gz文件在本地主机
      2. 用Xftp传输本地JDK文件到linux系统文件用户文件中，用tar命令解压缩
      3. 配置jdk的环境变量
   2. linux系统安装tomcat步骤：
      1. tomcat官网下载tomcat的linux版本*.tar.gz文件在本地主机中
      2. 用Xftp传输本地的JDK文件到linux系统/etc文件中，用tar命令解压缩

6. tomcat使用步骤：

   1. 开启服务命令：cd 到tomcat的安装路径下的bin目录下，用命令./startup.sh启动

   2. 关闭服务命令：./shutdown.sh

   3. 若不能启动，判断8080端口是否打开

      1. 判断命令：netstat -atpn | grep 8080

      2. 如果没有打开，使用防火墙相关命令开启8080端口：**firewall-cmd --zone=public --add-port=8080/tcp --permanent**。`--zone=public：表示作用域为公共的；`

         `--add-port=8080/tcp：添加tcp协议的端口8080；`

         `--permanent：永久生效，如果没有此参数，则只能维持当前服务生命周期内，重新启动后失效；`

      3. 重启防火墙：**systemctl restart firewalld.service**

      4. 重新载入配置：**firewall-cmd --reload** 

      5. 重启tomcat

      6. 实体机用浏览器通过linux系统公网IP访问tomcat8080端口

   4. 若出现tomcat假死现象，可以通过在catalina.sh文件中JAVA-OPT那里添加一行**-Djava.security.egd=file:/dev/./urandom** 




