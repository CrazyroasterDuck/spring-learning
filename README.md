
# spring-learning
这是关于spring的一些概念记录
DI（依赖注入）
  ：使用反射技术将接口和实现分离。DI容器就是管理JavaBean的生命周期，以及管理多个javabean之间的注入关系。
AOP（面向切面编程）
  ：利用动态代理技术，在不改变原有代码的基础上，对原有的模块进行功能上的加强并进行扩展，是扩展的功能和被扩展的模块充分解耦，利于软件项目奖的模块化设计。
在Spring中创建JAVABEAN
  ：1.使用xml声明法创建对象 
  使用xml声明法创建对象并使用getBean()方法获取对象 ;若使用getBean（xx.class）出现NoUniqueBeanDefinitionException异常可以使用getBean(String)来具体指定对象的Id
  2.使用<context:component-scan base-package="XX">创建对象，作用是在指定的xx包中扫描符合创建对象的类。
  
  
