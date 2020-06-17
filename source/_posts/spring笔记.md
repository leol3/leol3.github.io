---
title: spring笔记
date: 2020-06-17 16:52:01
tags:
---

<h4>依赖注入（DI）</h4>  
spring最认同的技术是控制反转的依赖注入模式。控制反转（Ioc）是一个通用的概念，它可以用许多不同的方式表达，依赖注入仅仅是控制反转的一个具体的例子。  
依赖注入可以以向构造函数传递参数的方式发生，或者通过使用setter方法post-construction。  

<h4>面向方面的程序设计</h4>  
spring框架的一个关键组件是面向方面的程序设计（AOP）框架。一个程序中跨越多个点的功能被称为横切关注点，这些横切关注点在概念上独立于应用程序的业务逻辑。  

---  
spring提供两种不同类型的容器：BeanFactory 容器和ApplicationContext 容器。  
有三个重要的方法把配置元数据提供给spring容器：  
* 基于XML的配置文件
* 基于注解的配置
* 基于java的配置  

spring框架支持以下五种bean的作用域：singleton、prototype、request、session和global session。  

spring支持两种类型的事务管理：  
* 编程式事务管理：这意味着你在编程的帮助下管理事务，有极大的灵活性，但却很难维护。
* 声明式事务管理：这意味着你从业务代码中分离事务管理，仅仅使用注释或XML配置来管理事务。  

声明式事务实例：  

```
<aop:config>
      <aop:pointcut id="createOperation" expression="execution(* com.tutorialspoint.StudentJDBCTemplate.create(..))"/>
      <aop:advisor advice-ref="txAdvice" pointcut-ref="createOperation"/>
</aop:config>
```