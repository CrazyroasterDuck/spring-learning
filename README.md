
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
  
1.使用@Bean向工厂方法的参数传参。当找不到符合条件的JavaBean对象的时候，使用@Autaowired(required=false)可以避免异常的发生。

2.当使用@Bean注入多个相同类型的对象时，会出现异常。可以使用@Resource（name=XX）来指定注入的对象。使用@Bean创建的JavaBean的id值就是方法名称，可以使用@Bean（name=XXXX）来重命名。

3.使用<constructor-arg>对构造方法进行注入，也可以使用C命名空间对构造方法进行注入。
Spring上下文环境的相关知识：上下文环境可以理解成Spring运行的环境。可以创建多个Spring上下文环境，不同的上下文环境中的Javabean对象不可以共享。但是使用@Import（XX.class）可以共享导入的上下文环境中的JavaBean对象。

4.在DI容器中创建Singleton单例和Prototype多例的JavaBean对象需要注意，Singleton是非线程的安全的，当多个线程访问同一个实例变量，此变量值有可能被覆盖。当有多个线程对同一个实例变量进行写操作的时候，要避免非线程安全需要使用prototype多例

5.Spring中提供PropertyPlaceholderConfigurer类来将外部属性文件中的值注入。

6.AOP面向切面编程的实现原理是代理模式。可以在不改变原始代码的基础上进行功能上的增强，使原始对象中的代码与增强代码进行充分的解耦。在静态代理中，代理对象和被代理对象必须实现同一个接口，完整保留被代理对象的接口样式，并且一直保持接口不变的原则。这样不利于功能的增强。动态代理类的对象由Proxy.java类的newProxyInstace()方法进行创建,增强的算法需要由InocationHandler接口来进行实现。

7.AOP的相关概念
	横切关注点：通用性的功能：方法执行时间的记录、访问权限的验证、常规日志的记录、数据库的事物处理。
	切面Aspect：就是对横切关注点的模块化，即：将横切关注点的功能代码提取出来放入一个单独的类中进行统一处理。
	切点Pointcut：就是精确地定义在什么位置放置切面。
	通知Advice:定义了应用切面的时机。Before\After\Around\After-returning\After-throwing
	织入Weaving:就是把切面用用到指定的对象中，在Spring的AOP中织入的原理是由JVM创建出代理对象，在代理对象中调用原始对象中的方法，再结合增强算法实现的。
切面表达式：execution(* service.UserinfoService.*(..))
解释：*：任意的返回值  service：包名  UserinfoService：包中的类  *：任意名称的方法  ..：任意类型的参数

8.在注解中使用bean(XX)表达式来指定切面应用于指定的id值为XX的对象。
例：@Around(value="execution(* service..*(..)) and bean(XX)")

使用注解@Pointcut定义全局切点，来将多处相同的execution表达式进行全局化，以减少冗余的配置。
例:
@Pointcut(value=“execution（*service.UserinfoService.*(..)）”)
	public void publicPointcut(){}
@Before(value="publicPointcut()")
	public void beginRun(){System.out.println("XX")}
...

9.可以使用注解向切面传入参数，即若Service业务方法有参数，切面类也能接受这个参数，并可以进行预处理。
例:
@Pointcut(value=“execution（* service.UserinfoService.save1(int)&&args(userId)）”)
	public void publicPointcut(int userId){}

  
