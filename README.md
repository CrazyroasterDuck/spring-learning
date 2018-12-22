
# spring-learning
这是关于spring的一些概念记录
DI（依赖注入）
  ：使用反射技术将接口和实现分离。DI容器就是管理JavaBean的生命周期，以及管理多个javabean之间的注入关系。
AOP（面向切面编程）
  ：利用动态代理技术，在不改变原有代码的基础上，对原有的模块进行功能上的加强并进行扩展，是扩展的功能和被扩展的模块充分解耦，利于软件项目奖的模块化设计。
  
在Spring中创建JAVABEAN
  ：1.使用xml声明法创建对象 （不再使用new Object()方法）
  使用xml声明法创建对象并使用getBean()方法获取对象 ;若使用getBean（xx.class）出现NoUniqueBeanDefinitionException异常可以使用getBean(String)来具体指定对象的Id
  2.使用<context:component-scan base-package="XX"></context:component-scan>创建对象，作用是在指定的xx包中扫描符合创建对象的类。若某些类需要被spring实例化则class类的上方必须使用@Component，@Repository来进行声明该类是个组件。获取对象还是使用getBean()方法。
  3.使用全注解创建对象，不再使用xml。与<beans>标记相同作用的注解是@Configuration，与<bean>标记相同功能的注解是@Bean,@Bean注解声明的创建对象的方法名称可以是任意的，但是必须要有返回值不然会出现没有声明返回值的异常。若使用getBean（xx.class）出现NoUniqueBeanDefinitionException异常可以使用getBean(String)来具体指定对象的Id
  4.使用@ComponentScan(basePackages=xx)创建对象并获取.作用同2相同，也可以进行类扫描并实例化
  5.使用@ComponentScan的basePackageClasses属性进行扫描，如：@ComponentScan(basePackageClasses={Userinfo.class})则只扫描Userinfo类所在的包。@ComponentScan注解默认扫描的是使用@Configuration注解的配置类所在的包路径下的所有组件，包含所有子包中的组件。
  
  
DI的使用
  1.使用xml声明法注入对象 
  <bean id="userinfo1" class="entity.Userinfo"></bean>
  <bean id="test1" class="test.Test1">
    <property name="userinfo" ref="userinfo1"></property>
  <bean>
  2.使用注解声明法注入对象
  使用<context:component-scan base-package="XX"></context:component-scan>配置代码来发现JavaBean并实例化成类的对象，可以再使用@Autowired注解对变量进行值的注入，也可以是使用注解向构造方法参数注入。在set方法中使用@wired注解。
  
   
  
