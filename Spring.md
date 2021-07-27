
杨博超

SSM = spring + springMVC + mybatis

# 开发框架基本介绍
struts1, struts2, hibernate, spring, springMVC, mybatis

apache: struts1, struts2, mybatis  
jboss: hibernate  
interface21: spring, springMVC  

MVC框架：struts1, struts2, springMVC  
持久层框架：hibernate(全自动的持久层框架), mybatis(半自动的持久层框架)  
整合性框架/设计型框架：spring  

#### 原始
servlet + jsp + jdbc

#### struts1
struts1封装了servlet

#### struts2
struts2封装的是过滤器filter

# Spring 基本介绍

Spring是一个IOC和AOP的容器框架。

**非侵入式**：也可以称为轻量级，基于Spring开发的应用中的对象，可以不依赖于Spring的API。  
**依赖注入**：DI--Dependency Injection, 依赖注入，控制反转(IOC)最经典的实现。  
**面向切面编程**：AOP--Aspect Oriented Programming。  
**容器**：Spring是一个容器，因为它包含并且管理对象的生命周期。  
>由类实例化产生对象，由对象调用方法，最终实现功能。  
>Spring管理对象，管理对象的生命周期。  

**组件化**：Spring实现了简单的组件配置组合成一个复杂的应用。  
>Spring中的组件指的就是Spring管理的对象。  
>Spring能降低程序间的耦合。

**一站式**: 在IOC和AOP的基础上，可以整合各种企业应用的开源框架和第三方类库。


Spring Framework

|-- Web
|   |-- Web
|   |-- WebSocket
|   |-- Servlet
|   |-- Portlet
|-- Data Access/Intergration
|   |-- JDBC: Java DataBase Connectivity
|   |-- ORM: Object Relational Mapping
|   |-- OXM
|   |-- JMS
|   |-- Transactions
|-- AOP
|   |-- Aspects
|-- Instrumentation
|-- Messaging
|-- Core Container
|   |-- Beans
|   |-- Core
|   |-- Context
|   |-- SpEL: Spring Expression Language


# Spring IOC(DI)

IOC: Inversion of Control, 控制反转。  
DI: Dependency Injection, 依赖注入，控制反转(IOC)最经典的实现。


Spring applicationContext.xml
```xml
<?xml version="1.0" encoding="utf-8">
<!-- xmlns:xml namespaces 定义xml所可使用的标签 -->
<beans xmlns="" xmlns:xsi="" xsi:schemaLocation="">
  <!-- 
    <bean>: 定义一个Spring所管理的对象
	id: 定义该对象的唯一标识
	name: 定义该对象全限定名（包名+类名）
  -->
  <bean id="" class="" scope="">
    <!-- 
	  <property>: 为对象的某个属性赋值
	  name: 对象的属性名
	  value: 对象的属性值
	-->
	<property name="" value=""></property>
    <property name="" value=""></property>
	...
  </bean>
  ...
</beans>
```


<bean>: 



# Spring AOP
AOP: Aspect Oriented Programming, 面向切面编程。  
OOP: Object Oriented Programming, 面向对象编程。  

万物皆对象，但OOP存在某些缺陷，而AOP是为了补充这种缺陷而存在。



配置bean，id唯一标识，class标识路径



1.初始化Spring容器
2.通过getBean方法获取对象
Application.

