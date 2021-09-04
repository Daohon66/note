**什么是bean？**

就是交给IOC容器管理的对象

**bean的作用域？**

- singleton：Spring默认的单例bean
- prototype：每次请求都会创建一个新的bean实例
- request：每次HTTP请求都会产生一个新的bean，只在当前HTTP request内有效
- session: 每次HTTP请求都会产生一个新的bean，只在当前HTTP session内有效

​                //注解的方式配置bean的作用域 @Bean @Scope(value = ConfigurableBeanFactory.SCOPE_PROTOTYPE) public Person personPrototype() {    return new Person(); }              

**单例bean的线程安全？**

1.在bean中尽量避免定义可变的成员变量

2.在类中定义ThreadLocal成员变量，将可变的成员变量保存在ThreadLocal中

**@Component和@Bean的区别？**

1.@Component作用于类，@Bean注解作用于方法

2.@Component通常是通过类路径扫描来自动侦测以及自动装配到Spring容器中。@Bean注解方法产生bean，@Bean告诉了Spring是某个类的示例，当我需要使用的时候交给我。

3.@Bean 注解比 Component 注解的自定义性更强，而且很多地方我们只能通过 @Bean 注解来注册 bean。比如当我们引用第三方库中的类需要装配到 Spring容器时，则只能通过 @Bean来实现。

**将一个类声明为bean的注解有哪些？**

一般使用 @Autowired 注解自动装配 bean，要想把类标识成可用于 @Autowired 注解自动装配的 bean 的类,采用以下注解

- @Componment：通用注解，可标注任意类为Spring组件。如果一个Bean不知道属于哪个层，可以使用@Componment注解。
- @Repository：对应Dao层，数据库相关
- @Service：服务层，业务逻辑，需要调用Dao层
- @Controller：对应于Spring MVC控制层，接受用户请求并调用Service层返回数据给前端

**Bean的生命周期？**

Bean容器去找配置文件中的Spring Bean的定义，利用反射API创建一个Bean的实例，如果涉及一些对象的属性值就利用set方法去设置，之后就开始检查Aware的相关接口并设置相关的依赖，比如：如果Bean实现了BeanNameAware接口就调用setBeanName方法传入Bean的名字，实现了其他Aware接口的就去调对应的方法。接着如果有和加载这个 Bean 的 Spring 容器相关的BeanPostProcessor对象，就执行postProcessBeforeInitialization()方法，然后如果实现了InitializingBean接口就执行afterPropertiesSet()方法，接下来是去检查是否配置了自定义的init-method方法，有的话就执行指定的方法。会再一次看一下也没有和加载这个Bean的Spring容器相关的BeanPostProcessor对象，就执行postProcessAfterInitialization()方法，最后要销毁Bean的时候如果Bean实现了DisposableBean接口，就执行destroy()方法，如果Bean的配置文件中的定义包含destory-method属性就执行指定的方法。