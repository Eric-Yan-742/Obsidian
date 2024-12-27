# 客户端

1. 在系统环境中设置好JAVA_HOME和PATH
2. Log On → 选择Local System Account，勾选Allow Service to interact with desktop
3. 在系统**服务**中关闭Apache Tomcat服务会快一点

# idea

- 设置好JDK
- Create Project: Jakarta EE, Web App, rest set to default
- 添加服务器: Run → Edit Confiturations → Tomcat (local) → Configure
- Artifact (默认应该有): Project Structure → Artifact (+) → Web app (exploded) → From module (select current project)

# 服务器输出乱码解决

> [!info] 使用IDEA修改Web项目访问路径，以及解决Apache Tomcat控制台中文乱码问题_idea tomcat访问路径配置_帅龍之龍的博客-CSDN博客  
> 前言：大学时，基本是用Eclipse创建Maven项目，自行整合SSH或SSM框架，然后发布的项目都是带有访问路径的，如：localhost:8080/xxx。使用Eclipse修改访问路径比较好改，就修改一处地方就可以了，而IDEA的话需要修改好几处地方。在此实战并记录一下在IDEA工具如何正确修改Web项目访问路径，此次项目就以SSM框架的项目做演示。_idea tomcat访问路径配置  
> [https://blog.csdn.net/Cai181191/article/details/121537901](https://blog.csdn.net/Cai181191/article/details/121537901)