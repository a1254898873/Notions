# Spring

## 控制反转（IOC）

控制反转是一种思想，有两种实现：依赖注入（被动）和依赖查找（主动）

![2859906351-5c0f7ff5ba595](https://raw.githubusercontent.com/a1254898873/images/master/202203241050496.png)



### IOC容器

传统方法：

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050498.jpg)

有了IOC容器后：

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050499.jpg)



![在这里插入图片描述](https://raw.githubusercontent.com/a1254898873/images/master/202203241050500)

![583919050-5c10776aae27d](https://raw.githubusercontent.com/a1254898873/images/master/202203241050501.png)





### BeanDefinition

在Spring中，BeanDefinition是对Bean的定义与描述

![1432313052-5c107ad88188e_fix732](https://raw.githubusercontent.com/a1254898873/images/master/202203241050502.png)

![20200613014222379](https://raw.githubusercontent.com/a1254898873/images/master/202203241050503.png)



```java
public interface BeanDefinition extends AttributeAccessor, BeanMetadataElement {

    // 单例、原型标识符
    String SCOPE_SINGLETON = ConfigurableBeanFactory.SCOPE_SINGLETON;
    String SCOPE_PROTOTYPE = ConfigurableBeanFactory.SCOPE_PROTOTYPE;

    // 标识 Bean 的类别，分别对应 用户定义的 Bean、来源于配置文件的 Bean、Spring 内部的 Bean
    int ROLE_APPLICATION = 0;
    int ROLE_SUPPORT = 1;
    int ROLE_INFRASTRUCTURE = 2;

    // 设置、返回 Bean 的父类名称
    void setParentName(@Nullable String parentName);
    String getParentName();

    // 设置、返回 Bean 的 className
    void setBeanClassName(@Nullable String beanClassName);
    String getBeanClassName();

    // 设置、返回 Bean 的作用域
    void setScope(@Nullable String scope);
    String getScope();

    // 设置、返回 Bean 是否懒加载
    void setLazyInit(boolean lazyInit);
    boolean isLazyInit();
    
    // 设置、返回当前 Bean 所依赖的其它 Bean 名称。
    void setDependsOn(@Nullable String... dependsOn);
    String[] getDependsOn();
    
    // 设置、返回 Bean 是否可以自动注入。只对 @Autowired 注解有效
    void setAutowireCandidate(boolean autowireCandidate);
    boolean isAutowireCandidate();
    
    // 设置、返回当前 Bean 是否为主要候选 Bean 。
    // 当同一个接口有多个实现类时，通过该属性来配置某个 Bean 为主候选 Bean。
    void setPrimary(boolean primary);
    boolean isPrimary();

    // 设置、返回创建该 Bean 的工厂类。
    void setFactoryBeanName(@Nullable String factoryBeanName);
    String getFactoryBeanName();
    
    // 设置、返回创建该 Bean 的工厂方法
    void setFactoryMethodName(@Nullable String factoryMethodName);
    String getFactoryMethodName();
    
    // 返回该 Bean 构造方法参数值、所有属性
    ConstructorArgumentValues getConstructorArgumentValues();
    MutablePropertyValues getPropertyValues();

    // 返回该 Bean 是否是单例、是否是非单例、是否是抽象的
    boolean isSingleton();
    boolean isPrototype();
    boolean isAbstract();

    // 返回 Bean 的类别。类别对应上面的三个属性值。
    int getRole();

    ...
}
```



### BeanFactory

容器的实现类并不是唯一的，Spring 框架提供了多个容器的实现，这些容器分为两套体系：一套是早期的 BeanFactory 体系；还有一套是现在常用的 ApplicationContext，也可称为应用上下文。

BeanFactory 是 Sping 框架的基础接口，一般是面向 Spring 本身；而 ApplicationContext 是以 BeanFactory 为基础进行综合能力扩展，用于满足大型业务应用的创建， ApplicationContext 一般面向使用 Sping 框架的开发者。几乎所有的应用场合我们都是直接使用 ApplicationContet 而非底层的 BeanFactory。

![453024611-5c107e4e8e375](https://raw.githubusercontent.com/a1254898873/images/master/202203241050504.png)

```java
public interface BeanFactory {

    // 对 FactoryBean 的转义定义，因为如果使用 bean 的名字检索 FactoryBean 得到的对象是工厂生成的对象，
    // 如果需要得到工厂本身，需要转义（FactoryBean 在后续会详细介绍）
    String FACTORY_BEAN_PREFIX = "&";
    
    // 根据 bean 的名字，获取在容器中 bean 实例
    Object getBean(String name) throws BeansException;
    
    //根据 bean 的名字和 Class 类型来得到 bean 实例，增加了类型安全验证机制。
    <T> T getBean(String name, @Nullable Class<T> requiredType) throws BeansException;
    Object getBean(String name, Object... args) throws BeansException;
    <T> T getBean(Class<T> requiredType) throws BeansException;
    <T> T getBean(Class<T> requiredType, Object... args) throws BeansException;
    
    // 提供对 bean 的检索，看看是否在容器有这个名字的 bean
    boolean containsBean(String name);
    
    // 根据 bean 名字，判断这个 bean 是不是单例
    boolean isSingleton(String name) throws NoSuchBeanDefinitionException;
    
    // 根据 bean 名字，判断这个 bean 是不是原型
    boolean isPrototype(String name) throws NoSuchBeanDefinitionException;
    
    // 根据 bean 名字，判断是否与指定的类型匹配
    boolean isTypeMatch(String name, ResolvableType typeToMatch) throws NoSuchBeanDefinitionException;
    boolean isTypeMatch(String name, @Nullable Class<?> typeToMatch) throws NoSuchBeanDefinitionException;
    
    // 得到 bean 实例的 Class 类型
    Class<?> getType(String name) throws NoSuchBeanDefinitionException;
    
    // 得到bean 的别名，如果根据别名检索，那么其原名也会被检索出来
    String[] getAliases(String name);
}
```









```java
public interface ApplicationContext extends EnvironmentCapable, ListableBeanFactory, HierarchicalBeanFactory,
        MessageSource, ApplicationEventPublisher, ResourcePatternResolver {

    // 返回此应用程序上下文的唯一ID
    @Nullable
    String getId();

    // 返回此上下文所属的应用程序名称
    String getApplicationName();

    // 返回应用上下文具像化的类名
    String getDisplayName();

    // 返回第一次加载此上下文时的时间戳
    long getStartupDate();

    // 获取父级应用上下文
    @Nullable
    ApplicationContext getParent();

    // 将 AutowireCapableBeanFactory 接口暴露给外部使用
    AutowireCapableBeanFactory getAutowireCapableBeanFactory() throws IllegalStateException;
}
```



### refresh

```java
public void refresh() throws BeansException, IllegalStateException {
        synchronized (this.startupShutdownMonitor) {
            //刷新前的预处理;
            prepareRefresh();

            //获取BeanFactory；默认实现是DefaultListableBeanFactory，在创建容器的时候创建的
            ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();

            //BeanFactory的预准备工作（BeanFactory进行一些设置，比如context的类加载器，BeanPostProcessor和XXXAware自动装配等）
            prepareBeanFactory(beanFactory);

            try {
                //BeanFactory准备工作完成后进行的后置处理工作
                postProcessBeanFactory(beanFactory);

                //执行BeanFactoryPostProcessor的方法；
                invokeBeanFactoryPostProcessors(beanFactory);

                //注册BeanPostProcessor（Bean的后置处理器），在创建bean的前后等执行
                registerBeanPostProcessors(beanFactory);

                //初始化MessageSource组件（做国际化功能；消息绑定，消息解析）；
                initMessageSource();

                //初始化事件派发器
                initApplicationEventMulticaster();

                //子类重写这个方法，在容器刷新的时候可以自定义逻辑；如创建Tomcat，Jetty等WEB服务器
                onRefresh();

                //注册应用的监听器。就是注册实现了ApplicationListener接口的监听器bean，这些监听器是注册到ApplicationEventMulticaster中的
                registerListeners();

                //初始化所有剩下的非懒加载的单例bean
                finishBeanFactoryInitialization(beanFactory);

                //完成context的刷新。主要是调用LifecycleProcessor的onRefresh()方法，并且发布事件（ContextRefreshedEvent）
                finishRefresh();
            }

            ......
    }
```



### registerBeanDefinition

BeanFactory 实现类是 DefaultListableBeanFactory，是一个具有注册功能的完整 Bean 工厂，注册 Bean 的方法是registerBeanDefinition，DefaultListableBeanFactory 通过实现 BeanDefinitionRegistry 接口，重写该方法,它定义了关于 BeanDefinition 的注册、移除、查询等一系列的操作。

```java
public interface BeanDefinitionRegistry extends AliasRegistry {

    // 注册 BeanDefinition
    void registerBeanDefinition(String beanName, BeanDefinition beanDefinition) throws BeanDefinitionStoreException;

    // 移除 BeanDefinition
    void removeBeanDefinition(String beanName) throws NoSuchBeanDefinitionException;

    // 获取 BeanDefinition
    BeanDefinition getBeanDefinition(String beanName) throws NoSuchBeanDefinitionException;

    // 根据 beanName 判断容器是否存在对应的 BeanDefinition 
    boolean containsBeanDefinition(String beanName);

    // 获取所有的 BeanDefinition
    String[] getBeanDefinitionNames();

    // 获取 BeanDefinition 数量
    int getBeanDefinitionCount();

    // 判断 beanName 是否被占用
    boolean isBeanNameInUse(String beanName);
}
```



### bean的生命周期

![v2-0924b5bf3ed072086d513ca5f15d1d89_r](https://raw.githubusercontent.com/a1254898873/images/master/202203241050505.jpg)

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050506.jpg)



### bean的创建过程

![1cf50d3b5c11487aa696bc60a01037f7_tplv-k3u1fbpfcp-zoom-crop-mark_1304_1304_1304_734 (1)](https://raw.githubusercontent.com/a1254898873/images/master/202203241050507.jpg)

一级缓存：singletonObjects  --->  存放完整bean的单例池

二级缓存：earlySingletonObjects  --->  存放非完整bean的单例池，存放早期暴露的bean

三级缓存：singletonFactories  --->  存放可以创建bean的factory，这里的factory主要是去创建bean的代理对象




### aware接口

Spring中提供一些Aware相关接口，像是BeanNameAware、ApplicationContextAware、ResourceLoaderAware、ServletContextAware等等，这些接口中都有且只有一个去掉接口名中的Aware后缀的设置方法，例如xxxAware接口只有一个setXxx()的方法，目的就是给实现该接口的类的xxx属性设置值。aware的含义是感应的，那么在spring中这些实现xxxAware接口的类是如何实现感应并设置xxx属性的值的呢，答案就是在spring容器中在工厂类创建实例后使用instanceof判断实例是否属于xxxAware接口的实例，如果结果是true的话，那么spring容器类就会调用实例的setXxx()方法给实例的xxx属性设置值。简单来说就是实现这些 Aware接口的Bean在被初始之后，可以从Spring容器中取得一些相对应的资源，例如实现BeanFactoryAware接口的Bean在初始后，Spring容器将会注入BeanFactory的实例，而实现ApplicationContextAware接口的Bean，在Bean被初始后，将会被注入 ApplicationContext的实例等等。
在大多数情况下，我们应该避免使用任何 Aware 接口，除非我们需要它们。实现这些接口会将代码耦合到Spring框架。





### BeanPostProcessor

BeanPostProcessor 接口是 Spring 提供的众多接口之一，他的作用主要是如果我们需要在Spring 容器完成 Bean 的实例化、配置和其他的初始化前后添加一些自己的逻辑处理，我们就可以定义一个或者多个 BeanPostProcessor 接口的实现，然后注册到容器中。

```java
public interface BeanPostProcessor {
    @Nullable
    default Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        return bean;
    }

    @Nullable
    default Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        return bean;
    }
}
```

由方法名字也可以看出，前者在实例化及依赖注入完成后、在任何初始化代码（比如配置文件中的init-method）调用之前调用；后者在初始化代码调用之后调用。此处需要注意的是：接口中的两个方法都要将传入的 bean 返回，而不能返回 null，如果返回的是 null 那么我们通过 getBean() 方法将得不到目标。



自定义类来实现 BeanPostProcessor 接口

```java
public class MyBeanPostProcessor implements BeanPostProcessor {

    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("bean 对象初始化之前······");
        return bean;
        // return bean对象监控代理对象
    }

    public Object postProcessAfterInitialization(final Object beanInstance, String beanName) throws BeansException {
        // 为当前 bean 对象注册监控代理对象，负责增强 bean 对象方法的能力
        Class beanClass = beanInstance.getClass();
        if (beanClass == ISomeService.class) {
            Object proxy = Proxy.newProxyInstance(beanInstance.getClass().getClassLoader(),
                    beanInstance.getClass().getInterfaces(),
                    new InvocationHandler() {
                        /**
                         * @param proxy 代理监控对象
                         * @param method doSome()方法
                         * @param args doSome()方法执行时接收的实参
                         * @return
                         * @throws Throwable
                         */
                        public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                            System.out.println("ISomeService 中的 doSome() 被拦截了···");
                            String result = (String) method.invoke(beanInstance, args);
                            return result.toUpperCase();
                        }
                    });
            return proxy;
        }
        return beanInstance;
    }
}
```

我们自定义的类实现了 BeanPostProcessor 接口，主要对 postProcessAfterInitialization() 方法进行了实现，用于增强 bean 对象的能力，这里我们使用了一下小例子，就是将当前 bean 对象返回的结果全部改为大写，因此这里我们使用到了一个代理对象，对 ISomeService 中的 doSome() 方法进行拦截。在 invoke() 方法中，对结果进行转换。



### springboot的自动装配

![在这里插入图片描述](https://raw.githubusercontent.com/a1254898873/images/master/202203241050508)

@SpringBootApplication包含：

@Configuration(@SpringBootConfiguration实质就是一个@Configuration）

这个注解实际上就是代表了一个配置类，相当于一个beans.xml文件

@EnableAutoConfiguration

在这个注解中，最重要的是它导入了一个类EnableAutoConfigurationImportSelector

它是一个ImportSelector接口的实现类，而ImportSelector接口中的selectImports方法所返回的类将

被Spring容器管理起来。

@ComponentScan

其实就是自动扫描并加载符合条件的组件或bean定义，最终将这些bean定义加载到容器中





### Spring中的事务

![image2.png](https://raw.githubusercontent.com/a1254898873/images/master/202203241050509.png)

Spring事务管理是基于接口代理（JDK）或动态字节码（CGLIB）技术，然后通过AOP实施事务增强的。当我们执行添加了事务特性的目标方式时，系统会通过目标对象的代理对象调用DataSourceTransactionManager对象，在事务开始的时，执行doBegin方法，事务结束时执行doCommit或doRollback方法。









## 切面编程（AOP）

![图片](https://raw.githubusercontent.com/a1254898873/images/master/202203241050510.png)



![图片](https://raw.githubusercontent.com/a1254898873/images/master/202203241050511.png)



### 动态代理

![image6.png](https://raw.githubusercontent.com/a1254898873/images/master/202203241050512.jpg)

动态代理可以在运行期间创建某个接口

```java
public interface Hello {
    void morning(String name);
}
```

```java
public class Main {
    public static void main(String[] args) {
        InvocationHandler handler = new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                System.out.println(method);
                if (method.getName().equals("morning")) {
                    System.out.println("Good morning, " + args[0]);
                }
                return null;
            }
        };
        Hello hello = (Hello) Proxy.newProxyInstance(
            Hello.class.getClassLoader(), // 传入ClassLoader
            new Class[] { Hello.class }, // 传入要实现的接口
            handler); // 传入处理调用方法的InvocationHandler
        hello.morning("Bob");
    }
}

interface Hello {
    void morning(String name);
}

```

在运行期动态创建一个interface实例的方法如下：

- 定义一个InvocationHandler实例，它负责实现接口的方法调用；
- 通过Proxy.newProxyInstance()创建interface实例
- 将返回的Object强制转型为接口。



### 原理

1、假如目标对象(被代理对象)实现接口，则底层可以采用JDK动态代理机制为目标对象创建代理对象（目标类和代理类会实现共同接口）。

![preview](https://raw.githubusercontent.com/a1254898873/images/master/202203241050513.png)

2、假如目标对象(被代理对象)没有实现接口，则底层可以采用CGLIB代理机制为目标对象创建代理对象（默认创建的代理类会继承目标对象类型）。

![image3.png](https://raw.githubusercontent.com/a1254898873/images/master/202203241050514.png)



### 切入点表达式

切入点表达式的语法格式规范是：

```java
execution([权限修饰符] [返回值类型] [简单类名/全类名] [方法名] ([参数列表]))
```

```java
表达式：

execution(* com.atguigu.spring.ArithmeticCalculator.*(…))

含义：

ArithmeticCalculator接口中声明的所有方法。第一个“”代表任意修饰符及任意返回值。第二个“”代表任意方法。“…”匹配任意数量、任意类型的参数。若目标类、接口与该切面类在同一个包中可以省略包名。
```

```java
表达式：

execution(public * ArithmeticCalculator.*(…))

含义：

ArithmeticCalculator接口的所有公有方法
```

```java
表达式：

execution(public double ArithmeticCalculator.*(…))

含义：

ArithmeticCalculator接口中返回double类型数值的方法
```

```java
表达式：

execution(public double ArithmeticCalculator.*(double, …))

含义：

第一个参数为double类型的方法。“…” 匹配任意数量、任意类型的参数。
```

```java
表达式：

execution(public double ArithmeticCalculator.*(double, double))

含义：

参数类型为double，double类型的方法
```

```java
@Aspect	//切面注解
@Component	//其他业务层
public class LogUtli {
//	方法执行开始，表示目标方法是com.spring.inpl包下的任意类的任意以两个int为参数，返回int类型参数的方法
	@Before("execution(public int com.spring.inpl.*.*(int, int))")
	public static void LogStart(JoinPoint joinPoint) {
		System.out.println("通知记录开始...");
	}
//	方法正常执行完之后
	/**
	 * 在程序正常执行完之后如果有返回值，我们可以对这个返回值进行接收
	 * returning用来接收方法的返回值
	 * */
	@AfterReturning(pointcut="public int com.spring.inpl.*.*(int, int)",returning="result")
	public static void LogReturn(JoinPoint joinPoint,Object result) {
		System.out.println("【" + joinPoint.getSignature().getName() + "】程序方法执行完毕了...结果是：" + result);
	}
}
```

重用切入点表达式

```java
@Aspect	//切面注解
@Component	//其他业务层
public class LogUtli {
	/**
	 * 定义切入点表达式的可重用方法
	 * */
	@Pointcut("execution(public int com.spring.inpl.MyMathCalculator.*(int, int))")
	public void MyCanChongYong() {}
	
//	方法执行开始
	@Before("MyCanChongYong()")
	public static void LogStart(JoinPoint joinPoint) {
		Object[] args = joinPoint.getArgs();	//获取到参数信息
		Signature signature = joinPoint.getSignature(); //获取到方法签名
		String name = signature.getName();	//获取到方法名
		System.out.println("【" + name + "】记录开始...执行参数：" + Arrays.asList(args));
	}
//	方法正常执行完之后
	/**
	 * 在程序正常执行完之后如果有返回值，我们可以对这个返回值进行接收
	 * returning用来接收方法的返回值
	 * */
	@AfterReturning(value="MyCanChongYong()",returning="result")
	public static void LogReturn(JoinPoint joinPoint,Object result) {
		System.out.println("【" + joinPoint.getSignature().getName() + "】程序方法执行完毕了...结果是：" + result);
	}
	
//	异常抛出时
	/**
	 * 在执行方法想要抛出异常的时候，可以使用throwing在注解中进行接收，
	 * 其中value指明执行的全方法名
	 * throwing指明返回的错误信息
	 * */
	@AfterThrowing(value="MyCanChongYong()",throwing="e")
	public static void LogThowing(JoinPoint joinPoint,Object e) {
		System.out.println("【" + joinPoint.getSignature().getName() +"】发现异常信息...,异常信息是：" + e);
	}
	
//	结束得出结果
	@After(value = "execution(public int com.spring.inpl.MyMathCalculator.add(int, int))")
	public static void LogEnd(JoinPoint joinPoint) {
		System.out.println("【" + joinPoint.getSignature().getName() +"】执行结束");
	}
	
	/**
	 * 环绕通知方法
	 * @throws Throwable 
	 * */
	@Around("MyCanChongYong()")
	public Object MyAround(ProceedingJoinPoint pjp) throws Throwable {
		
//		获取到目标方法内部的参数
		Object[] args = pjp.getArgs();
		
		System.out.println("【方法执行前】");
//		获取到目标方法的签名
		Signature signature = pjp.getSignature();
		String name = signature.getName();
		Object proceed = null;
		try {
//			进行方法的执行
			proceed = pjp.proceed();
			System.out.println("方法返回时");
		} catch (Exception e) {
			System.out.println("方法异常时" + e);
		}finally{
			System.out.println("后置方法");
		}
		
		//将方法执行的返回值返回
		return proceed;
	}
}

```



### 实例

一、自定义一个注解PermissionsAnnotation

1、使用@Target、@Retention、@Documented 自定义一个注解

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface PermissionAnnotation{
}
```

2、创建第一个AOP切面类

```java
@Aspect
@Component
@Order(1)
public class PermissionFirstAdvice {

    // 定义一个切面，括号内写入第1步中自定义注解的路径
    @Pointcut("@annotation(com.mu.demo.annotation.PermissionAnnotation)")
    private void permissionCheck() {
    }

    @Around("permissionCheck()")
    public Object permissionCheckFirst(ProceedingJoinPoint joinPoint) throws Throwable {
        System.out.println("===================第一个切面===================：" + System.currentTimeMillis());

        //获取请求参数，详见接口类
        Object[] objects = joinPoint.getArgs();
        Long id = ((JSONObject) objects[0]).getLong("id");
        String name = ((JSONObject) objects[0]).getString("name");
        System.out.println("id1->>>>>>>>>>>>>>>>>>>>>>" + id);
        System.out.println("name1->>>>>>>>>>>>>>>>>>>>>>" + name);

        // id小于0则抛出非法id的异常
        if (id < 0) {
            return JSON.parseObject("{\"message\":\"illegal id\",\"code\":403}");
        }
        return joinPoint.proceed();
    }
}
```

3、使用

```java
@RestController
@RequestMapping(value = "/permission")
public class TestController {
    @RequestMapping(value = "/check", method = RequestMethod.POST)
    // 添加这个注解
    @PermissionsAnnotation()
    public JSONObject getGroupList(@RequestBody JSONObject request) {
        return JSON.parseObject("{\"message\":\"SUCCESS\",\"code\":200}");
    }
}
```

二、jointpoint的使用

```java

@Aspect
@Component
@Order(0) //数字越小，所在切面类越先执行
public class PermissionSecondAdvice {

   @Pointcut("@annotation(com.example.demo.PermissionsAnnotation)")
   private void permissionCheck() {
   }

   @Around("permissionCheck()")
   public Object permissionCheckSecond(ProceedingJoinPoint joinPoint) throws Throwable {
       System.out.println("===================第二个切面===================：" + System.currentTimeMillis());

       //获取请求参数，详见接口类
       Object[] objects = joinPoint.getArgs();
       Long id = ((JSONObject) objects[0]).getLong("id");
       String name = ((JSONObject) objects[0]).getString("name");
       System.out.println("id->>>>>>>>>>>>>>>>>>>>>>" + id);
       System.out.println("name->>>>>>>>>>>>>>>>>>>>>>" + name);

       // name不是管理员则抛出异常
       if (!name.equals("admin")) {
           return JSON.parseObject("{\"message\":\"not admin\",\"code\":403}");
       }
       return joinPoint.proceed();
   }
}
```

三、在自定义的注解中设置参数

```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.METHOD})
@Documented
public @interface LogAnnotation {
    //模块名称
    String moudleName() default "";
    //操作内容
    String option() default "";
}
```

```java
@LogAnnotation(moudleName = "用户管理模块", option = "添加用户")
    @Override
    public void addUser() {
        //执行用户添加操作
        System.out.println("添加了一个用户");
    }
```

```java
@Aspect
@Component
public class LogInterceptor {

    /**
     * 定义用户管理模块的切点
     */
    @Pointcut("execution(* com.jjh.testoperation..*.*(..))")
    public void userManagerPointcut(){
    }

    @After("userManagerPointcut()")
    public void afterLog(JoinPoint joinPoint) throws NoSuchMethodException {
        //用的最多通知的签名
        Signature signature = joinPoint.getSignature();
        MethodSignature msg=(MethodSignature) signature;
        Object target = joinPoint.getTarget();
        //获取注解标注的方法
        Method method = target.getClass().getMethod(msg.getName(), msg.getParameterTypes());
        //通过方法获取到自定义的注解
        LogAnnotation annotation = method.getAnnotation(LogAnnotation.class);

        //记录日志
        DateFormat format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        System.out.println(format.format(new Date())
                + "  " + annotation.moudleName() + "  " + annotation.option());

    }
}
```





## SpringBoot配置文件的说明

#### properties说明

1.语法: k-v结构 key=value
2.数据类型: 默认是String数据类型 不要添加多余的""号
3.字符数据类型: properties的默认的加载的编码格式为ISO-8859-1 所以添加中文是需要字符转意.
4.缺点: 所有的key都必须手动的编辑 没有办法复用 所以引入了yml配置

#### YML配置文件说明

1.语法 K-V结构 写法上 key:value 实质上 key=value
key:value中间使用 (:+空格) 分隔
key与key之间有父子级关系的. 所以写的时候注意缩进项.
YML配置文件默认的格式都是UTF-8编码 所以可以直接编辑中文



#### 加载顺序

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050516.png)



#### 多环境

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050517.png)

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050518.png)







## 拦截器、过滤器、监听器

### 过滤器

#### 定义

如同它的名字一样，过滤器是处于客户端和服务器资源文件之间的一道过滤网，帮助我们过滤掉一些不符合要求的请求，通常用作 Session 校验，判断用户权限，如果不符合设定条件，则会被拦截到特殊的地址或者基于特殊的响应。



#### 使用

首先需要实现 Filter接口然后重写它的三个方法

- init 方法：在容器中创建当前过滤器的时候自动调用
- destory 方法：在容器中销毁当前过滤器的时候自动调用
- doFilter 方法：过滤的具体操作

```java
@Log4j2
public class MyFilter implements Filter {

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        log.info("初始化过滤器");
    }
  
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse response, FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest request = (HttpServletRequest)servletRequest;
        HttpServletResponseWrapper wrapper = new HttpServletResponseWrapper((HttpServletResponse) response);
        String requestUri = request.getRequestURI();
        log.info("请求地址是："+requestUri);
        if (requestUri.contains("/addSession")
            || requestUri.contains("/removeSession")
            || requestUri.contains("/online")
            || requestUri.contains("/favicon.ico")) {
            filterChain.doFilter(servletRequest, response);
        } else {
            wrapper.sendRedirect("/online");
        }
    }
  
    @Override
    public void destroy() {
        //在服务关闭时销毁
        log.info("销毁过滤器");
    }
}
```



### 拦截器

我们需要实现 HandlerInterceptor 类，并且重写三个方法：

- preHandle：在 Controoler 处理请求之前被调用，返回值是 boolean类型，如果是true就进行下一步操作；若返回false，则证明不符合拦截条件，在失败的时候不会包含任何响应，此时需要调用对应的response返回对应响应。
- postHandler：在 Controoler 处理请求执行完成后、生成视图前执行，可以通过ModelAndView对视图进行处理，当然ModelAndView也可以设置为 null。
- afterCompletion：在 DispatcherServlet 完全处理请求后被调用，通常用于记录消耗时间，也可以对一些资源进行处理。

```java
@Log4j2
@Component
public class MyInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        log.info("【MyInterceptor】调用了:{}", request.getRequestURI());
        request.setAttribute("requestTime", System.currentTimeMillis());
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response,
                           Object handler, ModelAndView modelAndView) throws Exception {
        if (!request.getRequestURI().contains("/online")) {
            HttpSession session = request.getSession();
            String sessionName = (String) session.getAttribute("name");
            if ("haixiang".equals(sessionName)) {
                log.info("【MyInterceptor】当前浏览器存在 session:{}",sessionName);
            }
        }
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response,
                                Object handler, Exception ex) throws Exception {
        long duration = (System.currentTimeMillis() - (Long)request.getAttribute("requestTime"));
        log.info("【MyInterceptor】[{}]调用耗时:{}ms",request.getRequestURI(), duration);
    }
}
```

### 监听器

我们通过 HttpSessionListener来统计当前在线人数、ip等信息，为了避免并发问题我们使用原子int来计数。

ServletContext,是一个全局的储存信息的空间，它的生命周期与Servlet容器也就是服务器保持一致，服务器关闭才销毁。

request，一个用户可有多个；

session，一个用户一个；而servletContext，所有用户共用一个。所以，为了节省空间，提高效率，ServletContext中，要放必须的、重要的、所有用户需要共享的线程又是安全的一些信息。

因此我们这里用ServletContext来存储在线人数sessionCount最为合适。

```java
@Log4j2
public class MyHttpSessionListener implements HttpSessionListener {

    public static AtomicInteger userCount = new AtomicInteger(0);

    @Override
    public synchronized void sessionCreated(HttpSessionEvent se) {
        userCount.getAndIncrement();
        se.getSession().getServletContext().setAttribute("sessionCount", userCount.get());
        log.info("【在线人数】人数增加为:{}",userCount.get());
      
        //此处可以在ServletContext域对象中为访问量计数，然后传入过滤器的销毁方法
        //在销毁方法中调用数据库入库，因为过滤器生命周期与容器一致
    }

    @Override
    public synchronized void sessionDestroyed(HttpSessionEvent se) {
        userCount.getAndDecrement();
        se.getSession().getServletContext().setAttribute("sessionCount", userCount.get());
        log.info("【在线人数】人数减少为:{}",userCount.get());
    }
}
```



### 注册

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Autowired
    MyInterceptor myInterceptor;

	//注册拦截器
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(myInterceptor);
    }

    /**
     * 注册过滤器
     * @return
     */
    @Bean
    public FilterRegistrationBean filterRegistrationBean(){
        FilterRegistrationBean filterRegistration = new FilterRegistrationBean();
        filterRegistration.setFilter(new MyFilter());
        filterRegistration.addUrlPatterns("/*");
        return filterRegistration;
    }

    /**
     * 注册监听器
     * @return
     */
    @Bean
    public ServletListenerRegistrationBean registrationBean(){
        ServletListenerRegistrationBean registrationBean = new ServletListenerRegistrationBean();
        registrationBean.setListener(new MyHttpRequestListener());
        registrationBean.setListener(new MyHttpSessionListener());
        return registrationBean;
    }
}


```



### 拦截器与过滤器的区别

**1.参考标准**

- 过滤器是 JavaEE 的标准，依赖于 Servlet 容器，生命周期也与容器一致，利用这一特性可以在销毁时释放资源或者数据入库。
- 拦截器是SpringMVC中的内容，依赖于web框架，通常用于验证用户权限或者记录日志，但是这些功能也可以利用 AOP 来代替。

**2.实现方式**

- 过滤器是基于回调函数实现，无法注入 ioc 容器中的 bean。
- 拦截器是基于反射来实现，因此拦截器中可以注入 ioc 容器中的 bean，例如注入 Redis 的业务层来验证用户是否已经登录。



## 多数据源

### 原理

springboot支持多数据源配置，spring-jdbc下的AbstractRoutingDataSource.java类，内部提供了一个Map（targetDataSources）,装载用户定义的多数据源。在程序运行时，只需指定使用这个集合中的那个数据源，spring就会自动切换到哪个数据源下。我们将使用自定义注解实现切换。


![在这里插入图片描述](https://raw.githubusercontent.com/a1254898873/images/master/202203241050519.png)





### MyBatis-Plus实现



```java
spring:
  datasource:
    dynamic:
      primary: master #设置默认的数据源或者数据源组,默认值即为master
      strict: false #严格匹配数据源,默认false. true未匹配到指定数据源时抛异常,false使用默认数据源
      datasource:
        master:
          url: jdbc:mysql://xx.xx.xx.xx:3306/dynamic
          username: root
          password: 123456
          driver-class-name: com.mysql.jdbc.Driver # 3.2.0开始支持SPI可省略此配置
        slave_1:
          url: jdbc:mysql://xx.xx.xx.xx:3307/dynamic
          username: root
          password: 123456
          driver-class-name: com.mysql.jdbc.Driver
        slave_2:
          url: ENC(xxxxx) # 内置加密,使用请查看详细文档
          username: ENC(xxxxx)
          password: ENC(xxxxx)
          driver-class-name: com.mysql.jdbc.Driver
       #......省略
       #以上会配置一个默认库master，一个组slave下有两个子库slave_1,slave_2

```

```java
@Service
@DS("slave")
public class UserServiceImpl implements UserService {

  @Autowired
  private JdbcTemplate jdbcTemplate;

  public List selectAll() {
    return  jdbcTemplate.queryForList("select * from user");
  }
  
  @Override
  @DS("slave_1")
  public List selectByCondition() {
    return  jdbcTemplate.queryForList("select * from user where age >10");
  }
}

```


<div style="page-break-after:always"></div>
## POJO

![image.png](https://raw.githubusercontent.com/a1254898873/images/master/202203241050520.awebp)



PO（Persistant Object - 持久化对象）

该概念随着ORM产生，可以看成是与数据库中的表相映射的Java对象。通常就是对应数据库中某个表中的一条记录。PO仅仅用于表示数据，没有任何数据操作。通常遵守Java Bean的规范，拥有 getter/setter方法。

PO的特点：

- PO的属性是和sql语句查询返回的字段一一对应的
- PO对象需要实现序列化接口
- 一个POJO持久化后就是PO



BO（Business Object - 业务对象）

BO通常位于业务层，要区别于直接对外提供服务的服务层：BO提供了基本业务单元的基本业务操作，在设计上属于被服务层业务流程调用的对象，一个业务流程可能需要调用多个BO来完成。





DO（Domain Object - 领域对象）

领域对象就是从现实世界中抽象出来的有形或无形的业务实体。通常位于业务层中。与数据库表结构一一对应。





VO（Value Object/View Object - 值对象/视图对象）

用于返回数据。 





DTO（Data Transfer Object - 数据传输对象）

用于接收数据。





DO 与 PO 的区别

1、某些场景下，PO 也没有对应的 DO，例如老师 Teacher 和学生 Student 存在多对多的关系，在关系数据库中，这种关系需要表现为一个中间表，也就对应有一个 TeacherAndStudentPO 的 PO，但这个 PO 在业务领域没有任何现实的意义，它完全不能与任何 DO 对应上。这里要特别声明，并不是所有多对多关系都没有业务含义，这跟具体业务场景有关，例如：两个 PO 之间的关系会影响具体业务，并且这种关系存在多种类型，那么这种多对多关系也应该表现为一个 DO，又如：“角色”与 “资源” 之间存在多对多关系，而这种关系很明显会表现为一个 DO——“权限”。

2、某些情况下，为了某种持久化策略或者性能的考虑，一个 PO 可能对应多个 DO，反之亦然。例如客户 Customer 有其联系信息 Contacts，这里是两个一对一关系的 DO，但可能出于性能的考虑（极端情况，权作举例），为了减少数据库的连接查询操作，把 Customer 和 Contacts 两个 DO 数据合并到一张数据表中。反过来，如果一本图书 Book，有一个属性是封面 cover，但该属性是一副图片的二进制数据，而某些查询操作不希望把 cover 一并加载，从而减轻磁盘 IO 开销，同时假设 ORM 框架不支持属性级别的延迟加载，那么就需要考虑把 cover 独立到一张数据表中去，这样就形成一个 DO 对应对个 PO 的情况。

3、PO 的某些属性值对于 DO 没有任何意义，这些属性值可能是为了解决某些持久化策略而存在的数据，例如为了实现 “乐观锁”，PO 存在一个 version 的属性，这个 version 对于 DO 来说是没有任何业务意义的，它不应该在 DO 中存在。同理，DO 中也可能存在不需要持久化的属性。





<img src="https://raw.githubusercontent.com/a1254898873/images/master/202207101147117.png" alt="在这里插入图片描述" style="zoom:67%;" />



<img src="https://raw.githubusercontent.com/a1254898873/images/master/202207101147991.png" alt="在这里插入图片描述" style="zoom:67%;" />



## mapstruct

一、配置

```java
<dependency>
    <groupId>org.mapstruct</groupId>
    <artifactId>mapstruct</artifactId>
    <version>${org.mapstruct.version}</version>
</dependency>

```

这还没完，还需要在pom中的build部分，增加一个插件。搞这么复杂，是因为它的原理和lombok是一样的，同样通过APT在编译器实现的。

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050521.awebp)

这意味着，它的代码，在编译期就完成了。不需要反射，所以效率就和直接写get、set，是一样的。

```java
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.8.1</version>
    <configuration>
        <source>1.8</source>
        <target>1.8</target>
        <annotationProcessorPaths>
            <path>
                <groupId>org.mapstruct</groupId>
                <artifactId>mapstruct-processor</artifactId>
                <version>${org.mapstruct.version}</version>
            </path>
            <path>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>1.18.16</version>
            </path>
            <path>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok-mapstruct-binding</artifactId>
                <version>0.2.0</version>
            </path>
        </annotationProcessorPaths>
    </configuration>
</plugin>

```

```java
@Mapper(nullValueCheckStrategy = NullValueCheckStrategy.ALWAYS)
public interface Transform {
    Transform T = Mappers.getMapper(Transform.class);
    Member fromMemberEntity(MemberEntity entity);
    MemberEntity fromMember(Member member);
}

```

二、使用



```java
// 实体类
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class User {
    private Integer id;
    private String name;
    private String createTime;
    private LocalDateTime updateTime;
}

// 被映射类VO1:和实体类一模一样
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class UserVO1 {
    private Integer id;
    private String name;
    private String createTime;
    private LocalDateTime updateTime;
}

// 被映射类VO1:比实体类少一个字段
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class UserVO2 {
    private Integer id;
    private String name;
    private String createTime;

}

```



```java
@Mapper(componentModel = "spring")
public interface UserCovertBasic {
    //UserCovertBasic INSTANCE = Mappers.getMapper(UserCovertBasic.class);

    /**
     * 字段数量类型数量相同，利用工具BeanUtils也可以实现类似效果
     * @param source
     * @return
     */
    UserVO1 toConvertVO1(User source);
    User fromConvertEntity1(UserVO1 userVO1);

    /**
     * 字段数量类型相同,数量少：仅能让多的转换成少的，故没有fromConvertEntity2
     * @param source
     * @return
     */
    UserVO2 toConvertVO2(User source);
}

```

字段名称不同可以使用

```java
@Mapping(source = "xx", target = "xx")
```



```java
@RestController
public class TestController {

    @GetMapping("convert")
    public Object convertEntity() {
        User user = User.builder()
                .id(1)
                .name("张三")
                .createTime("2020-04-01 11:05:07")
                .updateTime(LocalDateTime.now())
                .build();
        List<Object> objectList = new ArrayList<>();

        objectList.add(user);

        // 使用mapstruct
        UserVO1 userVO1 = UserCovertBasic.INSTANCE.toConvertVO1(user);
        objectList.add("userVO1:" + UserCovertBasic.INSTANCE.toConvertVO1(user));
        objectList.add("userVO1转换回实体类user:" + UserCovertBasic.INSTANCE.fromConvertEntity1(userVO1));
        // 输出转换结果
        objectList.add("userVO2:" + " | " + UserCovertBasic.INSTANCE.toConvertVO2(user));
        // 使用BeanUtils
        UserVO2 userVO22 = new UserVO2();
        BeanUtils.copyProperties(user, userVO22);
        objectList.add("userVO22:" + " | " + userVO22);

        return objectList;
    }
}

```



## 异步任务

### 使用

在Spring Boot中，我们只需要通过使用@Async注解就能简单的将原来的同步函数变为异步函数

```java
@Slf4j
@Component
public class AsyncTasks {

    public static Random random = new Random();

    @Async
    public void doTaskOne() throws Exception {
        log.info("开始做任务一");
        long start = System.currentTimeMillis();
        Thread.sleep(random.nextInt(10000));
        long end = System.currentTimeMillis();
        log.info("完成任务一，耗时：" + (end - start) + "毫秒");
    }

    @Async
    public void doTaskTwo() throws Exception {
        log.info("开始做任务二");
        long start = System.currentTimeMillis();
        Thread.sleep(random.nextInt(10000));
        long end = System.currentTimeMillis();
        log.info("完成任务二，耗时：" + (end - start) + "毫秒");
    }

    @Async
    public void doTaskThree() throws Exception {
        log.info("开始做任务三");
        long start = System.currentTimeMillis();
        Thread.sleep(random.nextInt(10000));
        long end = System.currentTimeMillis();
        log.info("完成任务三，耗时：" + (end - start) + "毫秒");
    }

}
```

为了让@Async注解能够生效，还需要在Spring Boot的主程序中配置@EnableAsync，如下所示：

```java
@EnableAsync
@SpringBootApplication
public class Chapter75Application {

    public static void main(String[] args) {
        SpringApplication.run(Chapter75Application.class, args);
    }

}
```











### 线程池配置

```java
@Configuration
@EnableAsync
public class AsyncConfiguration {

    @Bean("doSomethingExecutor")
    public Executor doSomethingExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        // 核心线程数：线程池创建时候初始化的线程数
        executor.setCorePoolSize(10);
        // 最大线程数：线程池最大的线程数，只有在缓冲队列满了之后才会申请超过核心线程数的线程
        executor.setMaxPoolSize(20);
        // 缓冲队列：用来缓冲执行任务的队列
        executor.setQueueCapacity(500);
        // 允许线程的空闲时间60秒：当超过了核心线程之外的线程在空闲时间到达之后会被销毁
        executor.setKeepAliveSeconds(60);
        // 线程池名的前缀：设置好了之后可以方便我们定位处理任务所在的线程池
        executor.setThreadNamePrefix("do-something-");
        // 缓冲队列满了之后的拒绝策略：由调用线程处理（一般是主线程）
        executor.setRejectedExecutionHandler(new ThreadPoolExecutor.DiscardPolicy());
        executor.initialize();
        return executor;
    }
    
}

```

```java
@RestController
public class AsyncController {

    @Autowired
    private AsyncService asyncService;

    @GetMapping("/open/something")
    public String something() {
        int count = 10;
        for (int i = 0; i < count; i++) {
            asyncService.doSomething("index = " + i);
        }
        return "success";
    }
}


@Slf4j
@Service
public class AsyncService {

    // 指定使用beanname为doSomethingExecutor的线程池
    @Async("doSomethingExecutor")
    public String doSomething(String message) {
        log.info("do something, message={}", message);
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            log.error("do something error: ", e);
        }
        return message;
    }
}

```

### 获取异步方法返回值

当异步方法有返回值时，如何获取异步方法执行的返回结果呢？这时需要异步调用的方法带有返回值CompletableFuture。

CompletableFuture是对Feature的增强，Feature只能处理简单的异步任务，而CompletableFuture可以将多个异步任务进行复杂的组合。如下：


```java
@RestController
public class AsyncController {

    @Autowired
    private AsyncService asyncService;

    @SneakyThrows
    @ApiOperation("异步 有返回值")
    @GetMapping("/open/somethings")
    public String somethings() {
        CompletableFuture<String> createOrder = asyncService.doSomething1("create order");
        CompletableFuture<String> reduceAccount = asyncService.doSomething2("reduce account");
        CompletableFuture<String> saveLog = asyncService.doSomething3("save log");
        
        // 等待所有任务都执行完
        CompletableFuture.allOf(createOrder, reduceAccount, saveLog).join();
        // 获取每个任务的返回结果
        String result = createOrder.get() + reduceAccount.get() + saveLog.get();
        return result;
    }
}


@Slf4j
@Service
public class AsyncService {

    @Async("doSomethingExecutor")
    public CompletableFuture<String> doSomething1(String message) throws InterruptedException {
        log.info("do something1: {}", message);
        Thread.sleep(1000);
        return CompletableFuture.completedFuture("do something1: " + message);
    }

    @Async("doSomethingExecutor")
    public CompletableFuture<String> doSomething2(String message) throws InterruptedException {
        log.info("do something2: {}", message);
        Thread.sleep(1000);
        return CompletableFuture.completedFuture("; do something2: " + message);
    }

    @Async("doSomethingExecutor")
    public CompletableFuture<String> doSomething3(String message) throws InterruptedException {
        log.info("do something3: {}", message);
        Thread.sleep(1000);
        return CompletableFuture.completedFuture("; do something3: " + message);
    }
}

```



## 单元测试

```java

@RunWith(SpringRunner.class)
@SpringBootTest
public class LearnServiceTest {

    @Autowired
    private LearnService learnService;
    
    @Test
    public void getLearn(){
        LearnResource learnResource=learnService.selectByKey(1001L);
        Assert.assertThat(learnResource.getAuthor(),is("嘟嘟MD独立博客"));
    }
}

```

顶部只要@RunWith(SpringRunner.class)和SpringBootTest即可，想要执行的时候，鼠标放在对应的方法，右键选择run该方法即可。单元测试一般用于service层的测试。





## 枚举类

#### 使用

枚举类中不能存在set方法

```java

 
public enum Color {
     
     RED("红色", 1), GREEN("绿色", 2), BLANK("白色", 3), YELLO("黄色", 4);
     
     
    private String name ;
    private int index ;
     
    private Color( String name , int index ){
        this.name = name ;
        this.index = index ;
    }
     
    public String getName() {
        return name;
    }

    public int getIndex() {
        return index;
    }

     
 
}

```

```java
public class B {
 
    public static void main(String[] args) {
 
        //输出某一枚举的值
        System.out.println( Color.RED.getName() );
        System.out.println( Color.RED.getIndex() );
 
        //遍历所有的枚举
        for( Color color : Color.values()){
            System.out.println( color + "  name: " + color.getName() + "  index: " + color.getIndex() );
        }
    }
 
}
```



#### 码值转换

```java
public enum PayEnum implements BaseEnum{
    WAITING_PAY("1","0","待支付","unionpay","银联"),
    SUCCESS_PAY("2","1","支付成功","unionpay","银联"),
    FAIL_PAY("3","2","支付失败","unionpay","银联"),

    ALIPAY_WAITING_PAY("1","3","待支付","alipay","支付宝");

    private String key;
    private String value;
    private String desc;

    //本类扩展字段 用于对接不同系统的支付状态
    private String channel;
    private String channelDesc;

    private PayEnum(String key,String value,String desc,String channel,String channelDesc){
        this.key = key;
        this.value = value;
        this.desc = desc;
        this.channel = channel;
        this.channelDesc = channelDesc;
    }
    
    @Override
    public String getKey() {
        return this.key;
    }

    @Override
    public String getValue() {
        return this.value;
    }

    @Override
    public String getDesc() {
        return this.desc;
    }

    public String getChannel() {
        return channel;
    }

    public String getChannelDesc() {
        return channelDesc;
    }
    /**
     * 根据 key 和 channel 获得枚举实例
     * @param key
     * @param channel
     * @return
     */
    public static PayEnum match(String key, String channel) {
        PayEnum[] enums = PayEnum.values();
        for (PayEnum payEnum : enums) {
            if (key.equals(payEnum.getKey()) && channel.equals(payEnum.getChannel()))
                return payEnum;
        }
        return null;
    }
}

```



```java
public static void main(String[] args) {
    String key = "1";
    String channel = "unionpay";
    PayEnum match = match(key, channel);
    System.out.println(match.getKey() + "==" + match.getValue() 
    + "===" + match.getDesc() + "===" + 
    "==" + match.getChannel());
}
结果：1==0===待支付=====unionpay

```





## 工具类

1、定义final类，私有化构造函数，类内部全采用静态方法



2、定义抽象类，类内部全部采用静态方法



3、不建议采用接口+静态方法 或者 非私有化构造函数 + 静态方法









## 代码规范

1、定义配置信息

有时候我们为了统一管理会把一些变量放到 yml 配置文件中

![图片](https://raw.githubusercontent.com/a1254898873/images/master/202203241050522.png)

用 @ConfigurationProperties 代替 @Value

**使用方法**

定义对应字段的实体

```java
@Data
// 指定前缀
@ConfigurationProperties(prefix = "developer")
@Component
public class DeveloperProperty {
    private String name;
    private String website;
    private String qq;
    private String phoneNumber;
}
```

使用时注入这个bean

```java
@RestController
@RequiredArgsConstructor
public class PropertyController {
 
    final DeveloperProperty developerProperty;
 
    @GetMapping("/property")
    public Object index() {
       return developerProperty.getName();
    }
}
```



2、用@RequiredArgsConstructor代替@Autowired

我们都知道注入一个 bean 有三种方式哦（set 注入, 构造器注入, 注解注入），Spring 推荐我们使用构造器的方式注入 Bean

RequiredArgsConstructor：lombok提供





3、抛异常而不是返回

在写业务代码的时候，经常会根据不同的结果返回不同的信息，尽量减少返回，会显得代码比较乱

反例

![图片](https://raw.githubusercontent.com/a1254898873/images/master/202203241050523.png)

正例

![图片](https://raw.githubusercontent.com/a1254898873/images/master/202203241050524.png)





4、减少不必要的db

尽可能的减少对数据库的查询

举例子

删除一个服务（已下架或未上架的才能删除），之前有看别人写的代码，会先根据id查询该记录，然后做一些判断

反例

![图片](https://raw.githubusercontent.com/a1254898873/images/master/202203241050525.png)

正例

![图片](https://raw.githubusercontent.com/a1254898873/images/master/202203241050526.png)











# SpringMVC

## 工作流程

![preview](https://raw.githubusercontent.com/a1254898873/images/master/202203241050527.png)

![preview](https://raw.githubusercontent.com/a1254898873/images/master/202203241050528.jpg)

用户向服务器发送请求，请求会到DispatcherServlet，DispatcherServlet 对请求URL进行解析，得到请求资源标识符（URI），然后根据该URI，调用HandlerMapping获得该Handler配置的所有相关的对象（包括一个Handler处理器对象、多个HandlerInterceptor拦截器对象），最后以HandlerExecutionChain对象的形式返回。

DispatcherServlet 根据获得的Handler，选择一个合适的HandlerAdapter。提取Request中的模型数据，填充Handler入参，开始执行Handler（Controller)。 在填充Handler的入参过程中，根据你的配置，Spring将帮你做一些额外的工作：

- HttpMessageConveter： 将请求消息（如Json、xml等数据）转换成一个对象，将对象转换为指定的响应信息
- 数据转换：对请求消息进行数据转换。如String转换成Integer、Double等
- 数据格式化：对请求消息进行数据格式化。 如将字符串转换成格式化数字或格式化日期等
- 数据验证： 验证数据的有效性（长度、格式等），验证结果存储到BindingResult或Error中

Handler执行完成后，向DispatcherServlet 返回一个ModelAndView对象；根据返回的ModelAndView，选择一个适合的ViewResolver返回给DispatcherServlet；ViewResolver 结合Model和View，来渲染视图，最后将渲染结果返回给客户端。





## DispatcherServlet

作用

1、DispatcherServlet是Spring MVC中最核心的对象；
2、DispatcherServlet用于拦截Http请求；
3、并根据请求的URL，找到与之对应的Controller控制器中的方法，来完成对该Http请求的处理。

![在这里插入图片描述](https://raw.githubusercontent.com/a1254898873/images/master/202203241050529)



DispatcherSevlet中最重要的是doService方法：

```java
    //获取请求，设置一些request的参数，然后分发给doDispatch
	@Override
	protected void doService(HttpServletRequest request, HttpServletResponse response) throws Exception {
		if (logger.isDebugEnabled()) {
			String resumed = WebAsyncUtils.getAsyncManager(request).hasConcurrentResult() ? " resumed" : "";
			logger.debug("DispatcherServlet with name '" + getServletName() + "'" + resumed +
					" processing " + request.getMethod() + " request for [" + getRequestUri(request) + "]");
		}
 
		// Keep a snapshot of the request attributes in case of an include,
		// to be able to restore the original attributes after the include.
		Map<String, Object> attributesSnapshot = null;
		if (WebUtils.isIncludeRequest(request)) {
			attributesSnapshot = new HashMap<String, Object>();
			Enumeration<?> attrNames = request.getAttributeNames();
			while (attrNames.hasMoreElements()) {
				String attrName = (String) attrNames.nextElement();
				if (this.cleanupAfterInclude || attrName.startsWith("org.springframework.web.servlet")) {
					attributesSnapshot.put(attrName, request.getAttribute(attrName));
				}
			}
		}
 
		// Make framework objects available to handlers and view objects.
		/* 设置web应用上下文**/
		request.setAttribute(WEB_APPLICATION_CONTEXT_ATTRIBUTE, getWebApplicationContext());
		/* 国际化本地**/
		request.setAttribute(LOCALE_RESOLVER_ATTRIBUTE, this.localeResolver);
		/* 样式**/
		request.setAttribute(THEME_RESOLVER_ATTRIBUTE, this.themeResolver);
		//设置样式资源
		request.setAttribute(THEME_SOURCE_ATTRIBUTE, getThemeSource());
		//请求刷新时保存属性
		FlashMap inputFlashMap = this.flashMapManager.retrieveAndUpdate(request, response);
		if (inputFlashMap != null) {
			request.setAttribute(INPUT_FLASH_MAP_ATTRIBUTE, Collections.unmodifiableMap(inputFlashMap));
		}
		//Flash attributes 在对请求的重定向生效之前被临时存储（通常是在session)中，并且在重定向之后被立即移除
		request.setAttribute(OUTPUT_FLASH_MAP_ATTRIBUTE, new FlashMap());
		//FlashMap 被用来管理 flash attributes 而 FlashMapManager 则被用来存储，获取和管理 FlashMap 实体.
		request.setAttribute(FLASH_MAP_MANAGER_ATTRIBUTE, this.flashMapManager);
 
		try {
			doDispatch(request, response);
		}
		finally {
			if (!WebAsyncUtils.getAsyncManager(request).isConcurrentHandlingStarted()) {
				// Restore the original attribute snapshot, in case of an include.
				if (attributesSnapshot != null) {
					restoreAttributesAfterInclude(request, attributesSnapshot);
				}
			}
		}
	}

```

 doService方法对Request设置了一些全局属性，最终接下来的操作是在doDispatcher：

```java
     /**
	 *将Handler进行分发，handler会被handlerMapping有序的获得
	 *通过查询servlet安装的HandlerAdapters来获得HandlerAdapters来查找第一个支持handler的类
	 *所有的HTTP的方法都会被这个方法掌控。取决于HandlerAdapters 或者handlers 他们自己去决定哪些方法是可用
	 *@param request current HTTP request
	 *@param response current HTTP response
	 */
	protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
		/* 当前HTTP请求**/
		HttpServletRequest processedRequest = request;
		HandlerExecutionChain mappedHandler = null;
		boolean multipartRequestParsed = false;
 
		WebAsyncManager asyncManager = WebAsyncUtils.getAsyncManager(request);
 
		try {
			ModelAndView mv = null;
			Exception dispatchException = null;
 
			try {
				//判断是否有文件上传
				processedRequest = checkMultipart(request);
				multipartRequestParsed = (processedRequest != request);
 
				// getHandler中遍历HandlerMapping，获取对应的HandlerExecutionChain
                // HandlerExecutionChain，包含HandlerIntercrptor和HandlerMethod
				mappedHandler = getHandler(processedRequest);
				if (mappedHandler == null || mappedHandler.getHandler() == null) {
					noHandlerFound(processedRequest, response);
					return;
				}
 
				
				//获得HandlerAdapter
				HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());
 
				//获得HTTP请求方法
				String method = request.getMethod();
				boolean isGet = "GET".equals(method);
				if (isGet || "HEAD".equals(method)) {
					long lastModified = ha.getLastModified(request, mappedHandler.getHandler());
					if (logger.isDebugEnabled()) {
						logger.debug("Last-Modified value for [" + getRequestUri(request) + "] is: " + lastModified);
					}
					if (new ServletWebRequest(request, response).checkNotModified(lastModified) && isGet) {
						return;
					}
				}
				//如果有拦截器的话，会执行拦截器的preHandler方法
				if (!mappedHandler.applyPreHandle(processedRequest, response)) {
					return;
				}
 
				//返回ModelAndView
				mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
 
				if (asyncManager.isConcurrentHandlingStarted()) {
					return;
				}
				//当view为空时，，根据request设置默认view
				applyDefaultViewName(processedRequest, mv);
				//执行拦截器的postHandle
				mappedHandler.applyPostHandle(processedRequest, response, mv);
			}
			catch (Exception ex) {
				dispatchException = ex;
			}
			processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);
		}
		catch (Exception ex) {
			triggerAfterCompletion(processedRequest, response, mappedHandler, ex);
		}
		catch (Error err) {
			triggerAfterCompletionWithError(processedRequest, response, mappedHandler, err);
		}
		finally {
			//判断是否是异步请求
			if (asyncManager.isConcurrentHandlingStarted()) {
				// Instead of postHandle and afterCompletion
				if (mappedHandler != null) {
					mappedHandler.applyAfterConcurrentHandlingStarted(processedRequest, response);
				}
			}
			else {
				// Clean up any resources used by a multipart request.
				//删除上传资源
				if (multipartRequestParsed) {
					cleanupMultipart(processedRequest);
				}
			}
		}
	}

```

## HandlerMapping

HandlerMapping 叫做处理器映射器，它的作用就是根据当前 request 找到对应的 Handler 和 Interceptor，然后封装成一个 HandlerExecutionChain 对象返回。

```java
public interface HandlerMapping {
    String BEST_MATCHING_HANDLER_ATTRIBUTE = HandlerMapping.class.getName() + ".bestMatchingHandler";
    @Deprecated
    String LOOKUP_PATH = HandlerMapping.class.getName() + ".lookupPath";
    String PATH_WITHIN_HANDLER_MAPPING_ATTRIBUTE = HandlerMapping.class.getName() + ".pathWithinHandlerMapping";
    String BEST_MATCHING_PATTERN_ATTRIBUTE = HandlerMapping.class.getName() + ".bestMatchingPattern";
    String INTROSPECT_TYPE_LEVEL_MAPPING = HandlerMapping.class.getName() + ".introspectTypeLevelMapping";
    String URI_TEMPLATE_VARIABLES_ATTRIBUTE = HandlerMapping.class.getName() + ".uriTemplateVariables";
    String MATRIX_VARIABLES_ATTRIBUTE = HandlerMapping.class.getName() + ".matrixVariables";
    String PRODUCIBLE_MEDIA_TYPES_ATTRIBUTE = HandlerMapping.class.getName() + ".producibleMediaTypes";
    default boolean usesPathPatterns() {
        return false;
    }
    @Nullable
    HandlerExecutionChain getHandler(HttpServletRequest request) throws Exception;
}
```



## HandlerAdapter

​	因为Spring MVC中的Handler是4种形式，但是Servlet需要的处理方法的结构却是固定的，都是以request和response为方法入参，那么如何让固定的Servlet处理方法调用灵活的Handler来进行处理呢？这就是HandlerAdapter要做的事情–> 适配。

​	 它的作用是：根据 Handler 来找到支持它的 HandlerAdapter，通过 HandlerAdapter 执行这个 Handler 得到 ModelAndView 对象。





## ModelAndView

ModelAndView指模型和视图的集合，既包含模型又包含视图；ModelAndView一般可以作为Controller的返回值，所以它的实例是开发者自己手动创建的。

```java
public class ModelAndView {
	@Nullable
	private Object view; // 可以是View，也可以是String
	@Nullable
	private ModelMap model;

	// 显然，你也可以自己就放置好一个http状态码进去
	@Nullable
	private HttpStatus status;	
	// 标记这个实例是否被调用过clear()方法~~~
	private boolean cleared = false;

	// 总共这几个属性：它提供的构造函数非常的多  这里我就不一一列出
	public void setViewName(@Nullable String viewName) {
		this.view = viewName;
	}
	public void setView(@Nullable View view) {
		this.view = view;
	}
	@Nullable
	public String getViewName() {
		return (this.view instanceof String ? (String) this.view : null);
	}
	@Nullable
	public View getView() {
		return (this.view instanceof View ? (View) this.view : null);
	}
	public boolean hasView() {
		return (this.view != null);
	}
	public boolean isReference() {
		return (this.view instanceof String);
	}

	// protected方法~~~
	@Nullable
	protected Map<String, Object> getModelInternal() {
		return this.model;
	}
	public ModelMap getModelMap() {
		if (this.model == null) {
			this.model = new ModelMap();
		}
		return this.model;
	}

	// 操作ModelMap的一些方法如下：
	// addObject/addAllObjects

	public void clear() {
		this.view = null;
		this.model = null;
		this.cleared = true;
	}
	// 前提是：this.view == null 
	public boolean isEmpty() {
		return (this.view == null && CollectionUtils.isEmpty(this.model));
	}
	
	// 竟然用的was，歪果仁果然严谨  哈哈
	public boolean wasCleared() {
		return (this.cleared && isEmpty());
	}
}

```

## ModelAndViewContainer

ModelAndViewContainer中其实就是一个ModelMap，一系列的操作都是基于ModelMap的。

```java
public class ModelAndViewContainer {
 
	private boolean ignoreDefaultModelOnRedirect = false;
	
	//视图，可以是实际视图也可以是String类型的逻辑视图
	private Object view;
 
	//默认使用的Model
	private final ModelMap defaultModel = new BindingAwareModelMap();
	
	//redirect类型的Model
	private ModelMap redirectModel;
 
	private boolean redirectModelScenario = false;
	
	//用于设置SessionAttribute使用完的标志
	private final SessionStatus sessionStatus = new SimpleSessionStatus();
 
	//请求是否已经处理完成的标志
	private boolean requestHandled = false;
 
 
	
	public void setIgnoreDefaultModelOnRedirect(boolean ignoreDefaultModelOnRedirect) {
		this.ignoreDefaultModelOnRedirect = ignoreDefaultModelOnRedirect;
	}
 
	//设置视图名称
	public void setViewName(String viewName) {
		this.view = viewName;
	}
 
	
	public String getViewName() {
		return (this.view instanceof String ? (String) this.view : null);
	}
 
	
	public void setView(Object view) {
		this.view = view;
	}
 
	public Object getView() {
		return this.view;
	}
 
	public boolean isViewReference() {
		return (this.view instanceof String);
	}
 
 
	public ModelMap getModel() {
		if (useDefaultModel()) {
			return this.defaultModel;
		}
		else {
			return (this.redirectModel != null) ? this.redirectModel : new ModelMap();
		}
	}
 
	//和model相关的处理方法
     /*
       * 假设redirectModelScenario = R ，ignoreDefaultModelOnRedirect = I ，（redirectModel == null）= M
       * 那么（R, I, M）共有8中组合情况，useDefaultModel返回false（也就是使用redirectModel）只有三种情况：
       * （1,1,0）、（1,1,1）、(1,0,0)
       * a：如果同时设置了redirectModelScenario和ignoreDefaultModelOnRedirect为true，那么无论redirectModel
       *    是否为null，都会使用redirectModel；
       * b：如果设置了redirectModelScenario为true，而ignoreDefaultModelOnRedirect为false，同时redirectModel
       *    为null，那么也会使用redirectModel；
	*/
	private boolean useDefaultModel() {
		return (!this.redirectModelScenario || (this.redirectModel == null && !this.ignoreDefaultModelOnRedirect));
	}
 
	public ModelMap getDefaultModel() {
		return this.defaultModel;
	}
 
	public void setRedirectModel(ModelMap redirectModel) {
		this.redirectModel = redirectModel;
	}
 
	public void setRedirectModelScenario(boolean redirectModelScenario) {
		this.redirectModelScenario = redirectModelScenario;
	}
 
	public SessionStatus getSessionStatus() {
		return this.sessionStatus;
	}
 
	
	public void setRequestHandled(boolean requestHandled) {
		this.requestHandled = requestHandled;
	}
 
	
	public boolean isRequestHandled() {
		return this.requestHandled;
	}
 
	public ModelAndViewContainer addAttribute(String name, Object value) {
		getModel().addAttribute(name, value);
		return this;
	}
 
 
	public ModelAndViewContainer addAttribute(Object value) {
		getModel().addAttribute(value);
		return this;
	}
 
	public ModelAndViewContainer addAllAttributes(Map<String, ?> attributes) {
		getModel().addAllAttributes(attributes);
		return this;
	}
 
	public ModelAndViewContainer mergeAttributes(Map<String, ?> attributes) {
		getModel().mergeAttributes(attributes);
		return this;
	}
 
	public ModelAndViewContainer removeAttributes(Map<String, ?> attributes) {
		if (attributes != null) {
			for (String key : attributes.keySet()) {
				getModel().remove(key);
			}
		}
		return this;
	}
 
	
	public boolean containsAttribute(String name) {
		return getModel().containsAttribute(name);
	}
 
}
```

ModelMap是一个HashMap，主要用于数据的存取。

```java
@SuppressWarnings("serial")
public class ModelMap extends LinkedHashMap<String, Object> {
 
	
	public ModelMap() {
	}
 
	public ModelMap(String attributeName, Object attributeValue) {
		addAttribute(attributeName, attributeValue);
	}
 
	public ModelMap(Object attributeValue) {
		addAttribute(attributeValue);
	}
 
 
	public ModelMap addAttribute(String attributeName, Object attributeValue) {
		Assert.notNull(attributeName, "Model attribute name must not be null");
		put(attributeName, attributeValue);
		return this;
	}
 
	public ModelMap addAttribute(Object attributeValue) {
		Assert.notNull(attributeValue, "Model object must not be null");
		if (attributeValue instanceof Collection && ((Collection<?>) attributeValue).isEmpty()) {
			return this;
		}
		return addAttribute(Conventions.getVariableName(attributeValue), attributeValue);
	}
 
	public ModelMap addAllAttributes(Collection<?> attributeValues) {
		if (attributeValues != null) {
			for (Object attributeValue : attributeValues) {
				addAttribute(attributeValue);
			}
		}
		return this;
	}
 
 
	public ModelMap addAllAttributes(Map<String, ?> attributes) {
		if (attributes != null) {
			putAll(attributes);
		}
		return this;
	}
 
 
	public ModelMap mergeAttributes(Map<String, ?> attributes) {
		if (attributes != null) {
			for (Map.Entry<String, ?> entry : attributes.entrySet()) {
				String key = entry.getKey();
				if (!containsKey(key)) {
					put(key, entry.getValue());
				}
			}
		}
		return this;
	}
 
	
	public boolean containsAttribute(String attributeName) {
		return containsKey(attributeName);
	}
 
}
```

## MultipartResolver

MultipartResolver 用于处理文件上传，当收到请求时 DispatcherServlet#checkMultipart() 方法会调用 MultipartResolver#isMultipart() 方法判断请求中是否包含文件。如果请求数据中包含文件，则调用 MultipartResolver#resolveMultipart() 方法对请求的数据进行解析。
然后将文件数据解析成 MultipartFile 并封装在 MultipartHttpServletRequest(继承了 HttpServletRequest) 对象中，最后传递给 Controller

```java
public interface MultipartResolver {
    //判断request是否为文件上传请求
    boolean isMultipart(HttpServletRequest request);
    
    //将 HttpServletRequest 转换为 MultipartHttpServletRequest
    MultipartHttpServletRequest resolveMultipart(HttpServletRequest request) throws MultipartException;
    
    //清空： 入参是MultipartHttpServletRequest
    void cleanupMultipart(MultipartHttpServletRequest request);
}

```

## ViewResolver

ViewResolver的主要作用是把一个逻辑上的视图名称解析为一个真正的视图

```java
public interface ViewResolver {
    @Nullable
    View resolveViewName(String viewName, Locale locale) throws Exception;
}
```





## WebMvcConfigurer

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050530.png)

#### 1、configurePathMatch：匹配路由请求规则

```java
@Override
public void configurePathMatch(PathMatchConfigurer configurer) {
    super.configurePathMatch(configurer);
    /*
     * 1.ServletMappings 设置的是 "/" 2.setUseSuffixPatternMatch默认设置为true,
     * 那么,"/user" 就会匹配 "/user.*",也就是说,"/user.html" 的请求会被 "/user" 的 Controller所拦截.
     * 3.如果该值为false,则不匹配
     */
    configurer.setUseSuffixPatternMatch(false);

    /*
     * setUseTrailingSlashMatch的默认值为true
     * 也就是说, "/user" 和 "/user/" 都会匹配到 "/user"的Controller
     */
    configurer.setUseTrailingSlashMatch(true);
}
```

#### 2、addFormatters：注册自定义的Formatter和Convert

```java
@Bean
public EnumConverterFactory enumConverterFactory() {
    return new EnumConverterFactory();
}

/**
 * 注册自定义的Formatter和Convert,例如, 对于日期类型,枚举类型的转化.
 * 不过对于日期类型,使用更多的是使用
 *
 * @DateTimeFormat(pattern = "yyyy-MM-dd")
 * private Date createTime;
 */
@Override
public void addFormatters(FormatterRegistry registry) {
    super.addFormatters(registry);
    registry.addConverterFactory(enumConverterFactory());
}

/**
 * SpringMVC支持绑定枚举值参数。
 * 匹配规则 :
 * 字符串则尝试根据Enum#name()转换。
 * 如果找不到匹配的则返回null
 */
public class EnumConverterFactory implements ConverterFactory<String, Enum> {
    
    @Override
    public <T extends Enum> Converter<String, T> getConverter(Class<T> targetType) {
        return new String2EnumConverter(targetType);
    }

    class String2EnumConverter<T extends Enum<T>> implements Converter<String, T> {

        private Class<T> enumType;

        private String2EnumConverter(Class<T> enumType) {
            this.enumType = enumType;
        }

        @Override
        public T convert(String source) {
            if (source != null && !source.isEmpty()) {
                try {
                    return Enum.valueOf(enumType, source);
                } catch (Exception e) {
                }
            }
            return null;
        }
    }
}
```

#### 3、addArgumentResolvers：添加自定义方法参数处理器

需求：想要在Controller的参数列表中注入当前回话的自定义PlatformSession对象。

```java
@Getter
@Setter
public class PlatformSession<T> {
    private T id;
    private String username;
    private int expireTime;
}
```

解决方法：创建自定义参数处理器，通过addArgumentResolvers添加到SpringMVC中。

自定义参数处理器

```java
public class PlatformSessionArgumentResolvers implements HandlerMethodArgumentResolver {
    @Override
    public boolean supportsParameter(MethodParameter methodParameter) {
        return PlatformSession.class.equals(methodParameter.getClass());
    }

    @Override
    public Object resolveArgument(MethodParameter methodParameter, ModelAndViewContainer modelAndViewContainer, NativeWebRequest nativeWebRequest, WebDataBinderFactory webDataBinderFactory) throws Exception {
        HttpServletRequest request = nativeWebRequest.getNativeRequest(HttpServletRequest.class);
        return PlatformSessionUtil.getSession(request);
    }
}
```

配置类

```java
@Bean
PlatformSessionArgumentResolvers platformSessionArgumentResolvers() {
    return new PlatformSessionArgumentResolvers();
}

@Override
public void addArgumentResolvers(List<HandlerMethodArgumentResolver> argumentResolvers) {
    super.addArgumentResolvers(argumentResolvers);
    argumentResolvers.add(platformSessionArgumentResolvers());
}
```

#### 4、addViewControllers：添加自定义视图控制器

```java
@Override
public void addViewControllers(ViewControllerRegistry registry) {
    super.addViewControllers(registry);
    // 对 "/hello" 的 请求 redirect 到 "/home"
    registry.addRedirectViewController("/hello", "/home");
    // 对 "/admin/**" 的请求 返回 404 的 http 状态
    registry.addStatusController("/admin/**", HttpStatus.NOT_FOUND);
    // 将 "/home" 的 请求响应为返回 "home" 的视图 
    registry.addViewController("/home").setViewName("home");
}
```

#### 5、addResourceHandlers：添加静态资源处理器

```java
@Override
public void addResourceHandlers(ResourceHandlerRegistry registry) {
    super.addResourceHandlers(registry);
    // 通过 "/home.html" 请求, 来访问 /resource/static/home.html 静态资源
    registry.addResourceHandler("/home.html").addResourceLocations("classpath:/static/home.html");
}
```

6、addCorsMappings：实现跨域

```java
@Configuration
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {

        registry.addMapping("/api/**")
            .allowedOrigins("https://domain2.com")
            .allowedMethods("PUT", "DELETE")
            .allowedHeaders("header1", "header2", "header3")
            .exposedHeaders("header1", "header2")
            .allowCredentials(true).maxAge(3600);

        // Add more mappings...
    }
}

```



#### 6、addInterceptors：添加拦截器

```java
@Override
public void addInterceptors(InterceptorRegistry registry) {

}

```

```java
@Component
public class RequestInterceptor implements HandlerInterceptor {


}

```







# Spring Security

## 权限系统设计

### 1、表设计

权限表：

```sql
DROP TABLE IF EXISTS `sys_permission`;
CREATE TABLE `sys_permission` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `parentId` int(11) NOT NULL,
  `name` varchar(50) NOT NULL,
  `css` varchar(30) DEFAULT NULL,
  `href` varchar(1000) DEFAULT NULL,
  `type` tinyint(1) NOT NULL,
  `permission` varchar(50) DEFAULT NULL,
  `sort` int(11) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=53 DEFAULT CHARSET=utf8mb4;
```

```sql
INSERT INTO `sys_permission` VALUES ('1', '0', '用户管理', 'fa-users', '', '1', '', '1');
INSERT INTO `sys_permission` VALUES ('2', '1', '用户查询', 'fa-user', '/api/getPage?pageName=user/user-list', '1', '', '2');
INSERT INTO `sys_permission` VALUES ('3', '2', '查询', '', '', '2', 'sys:user:query', '100');
INSERT INTO `sys_permission` VALUES ('4', '2', '新增', '', '', '2', 'sys:user:add', '100');
INSERT INTO `sys_permission` VALUES ('5', '2', '删除', null, null, '2', 'sys:user:del', '100');
INSERT INTO `sys_permission` VALUES ('6', '1', '修改密码', 'fa-pencil-square-o', '/api/getPage?pageName=user/user-change-password', '1', 'sys:user:password', '4');
INSERT INTO `sys_permission` VALUES ('7', '0', '系统设置', 'fa-gears', '', '1', '', '5');
INSERT INTO `sys_permission` VALUES ('8', '7', '菜单', 'fa-cog', '/api/getPage?pageName=permission/permission-list', '1', '', '6');
INSERT INTO `sys_permission` VALUES ('9', '8', '查询', '', '', '2', 'sys:menu:query', '100');
INSERT INTO `sys_permission` VALUES ('10', '8', '新增', '', '', '2', 'sys:menu:add', '100');
INSERT INTO `sys_permission` VALUES ('11', '8', '删除', '', '', '2', 'sys:menu:del', '100');
INSERT INTO `sys_permission` VALUES ('12', '7', '角色', 'fa-user-secret', '/api/getPage?pageName=role/role-list', '1', '', '7');
INSERT INTO `sys_permission` VALUES ('13', '12', '查询', '', '', '2', 'sys:role:query', '100');
INSERT INTO `sys_permission` VALUES ('14', '12', '新增', '', '', '2', 'sys:role:add', '100');
INSERT INTO `sys_permission` VALUES ('15', '12', '删除', '', '', '2', 'sys:role:del', '100');
INSERT INTO `sys_permission` VALUES ('16', '0', '文件管理', 'fa-folder-open', '/api/getPage?pageName=file/file-list', '1', '', '8');
INSERT INTO `sys_permission` VALUES ('17', '16', '查询', '', '', '2', 'sys:file:query', '100');
INSERT INTO `sys_permission` VALUES ('18', '16', '删除', '', '', '2', 'sys:file:del', '100');
INSERT INTO `sys_permission` VALUES ('19', '0', '数据源监控', 'fa-eye', 'druid/index.html', '1', '', '9');
INSERT INTO `sys_permission` VALUES ('20', '0', '接口swagger', 'fa-file-pdf-o', 'swagger-ui.html', '1', '', '10');
INSERT INTO `sys_permission` VALUES ('21', '0', '代码生成', 'fa-wrench', '/api/getPage?pageName=generate/edit', '1', 'generate:edit', '11');
INSERT INTO `sys_permission` VALUES ('22', '0', '日志查询', 'fa-reorder', '/api/getPage?pageName=log/log-list', '1', 'sys:log:query', '13');
INSERT INTO `sys_permission` VALUES ('23', '8', '修改', null, null, '2', 'sys:menu:edit', '100');
INSERT INTO `sys_permission` VALUES ('24', '12', '修改', null, null, '2', 'sys:role:edit', '100');
INSERT INTO `sys_permission` VALUES ('25', '2', '修改', null, null, '2', 'sys:user:edit', '100');

```

角色表：

```sql
DROP TABLE IF EXISTS `sys_role`;
CREATE TABLE `sys_role` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(50) NOT NULL,
  `description` varchar(100) DEFAULT NULL,
  `createTime` datetime NOT NULL,
  `updateTime` datetime NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `name` (`name`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=16 DEFAULT CHARSET=utf8mb4;
```

```sql
INSERT INTO `sys_role` VALUES ('1', 'ADMIN', '管理员', '2017-05-01 13:25:39', '2019-06-04 02:25:13');
INSERT INTO `sys_role` VALUES ('2', 'USER', '普通用户', '2017-08-01 21:47:31', '2019-05-30 09:08:24');
INSERT INTO `sys_role` VALUES ('3', 'TEACHER', '', '2019-03-27 02:10:23', '2019-05-23 07:48:01');
INSERT INTO `sys_role` VALUES ('4', 'test', 'test', '2019-04-29 02:16:48', '2019-05-22 09:51:26');
```



角色权限中间表：

```sql
DROP TABLE IF EXISTS `sys_role_permission`;
CREATE TABLE `sys_role_permission` (
  `roleId` int(11) NOT NULL,
  `permissionId` int(11) NOT NULL,
  PRIMARY KEY (`roleId`,`permissionId`),
  KEY `fk_sysrolepermission_permissionId` (`permissionId`),
  CONSTRAINT `fk_permission_roleId` FOREIGN KEY (`roleId`) REFERENCES `sys_role` (`id`) ON DELETE CASCADE,
  CONSTRAINT `fk_sysrolepermission_permissionId` FOREIGN KEY (`permissionId`) REFERENCES `sys_permission` (`id`) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```



用户表：

```sql
DROP TABLE IF EXISTS `sys_user`;
CREATE TABLE `sys_user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `username` varchar(50) NOT NULL,
  `password` varchar(60) NOT NULL,
  `nickname` varchar(255) DEFAULT NULL,
  `headImgUrl` varchar(255) DEFAULT NULL,
  `phone` varchar(11) DEFAULT NULL,
  `telephone` varchar(30) DEFAULT NULL,
  `email` varchar(50) DEFAULT NULL,
  `birthday` date DEFAULT NULL,
  `sex` tinyint(1) DEFAULT NULL,
  `status` tinyint(1) NOT NULL DEFAULT '1',
  `createTime` datetime NOT NULL,
  `updateTime` datetime NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `username` (`username`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=45 DEFAULT CHARSET=utf8mb4;
```



用户角色中间表：

```sql
DROP TABLE IF EXISTS `sys_role_user`;
CREATE TABLE `sys_role_user` (
  `userId` int(11) NOT NULL,
  `roleId` int(11) NOT NULL,
  PRIMARY KEY (`userId`,`roleId`),
  KEY `fk_roleId` (`roleId`),
  CONSTRAINT `fk_roleId` FOREIGN KEY (`roleId`) REFERENCES `sys_role` (`id`),
  CONSTRAINT `fk_userId` FOREIGN KEY (`userId`) REFERENCES `sys_user` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```





### 2、权限设计

查询所有权限信息

```java
    @Override
    public Results<JSONArray> listAllPermission() {
        logger.info(getClass().getName() + ".listAllPermission()");
        //1.1、先查询出所有信息
        List<SysPermission> datas = permissionDao.findAll();
        //1.2、创建个空json数组存放整理完毕的数据
        JSONArray array = new JSONArray();
        logger.info(getClass().getName() + ".setPermissionsTree(?,?,?)");
        //1.3、调用整理的静态方法，结构化数据
        TreeUtils.setPermissionsTree(0, datas, array);
        return Results.success(array);
    }
```

查询出所有信息后转化为树形结构返回给前端



新增权限信息

```java
    @Override
    public Results<SysPermission> save(SysPermission sysPermission) {
        return (permissionDao.save(sysPermission) > 0) ? Results.success() : Results.failure();
    }
```

如果最后返回结构大于0即为保存成功





删除权限信息

```java
    @Override
    public Results delete(Integer id) {
        permissionDao.deleteById(id);
        SysPermission sysPermission = permissionDao.getSysPermissionByParentId(id);
        if (sysPermission != null){
            delete(sysPermission.getId());
        }
        return Results.success();
    }
```

因为权限是树形结构，需要递归删除





根据用户权限生成菜单

```java
    @Override
    public Results<JSONArray> getMenu(Long userId) {
        //1.1、先查询出所有信息
        List<SysPermission> datas = permissionDao.ListByUserId(userId);
        datas = datas.stream().filter(p -> p.getType().equals(1)).collect(Collectors.toList());
        //1.2、创建个空json数组存放整理完毕的数据
        JSONArray array = new JSONArray();
        //1.3、调用整理的静态方法，结构化数据
        TreeUtils.setPermissionsTree(0, datas, array);
        return Results.success(array);
    }
```

type类型为1表示菜单





### 3、角色设计

保存角色信息

```java
    public Results<SysRole> save(RoleDto roleDto) {
        //2.1、先保存角色信息"
        roleDao.saveRole(roleDto);
        List<Long> permissionIds = roleDto.getPermissionIds();
        //2.2、移除0的权限,permission id是从1开始
        permissionIds.remove(0L);
        //2.3、保存角色对应的所有权限
        if (!CollectionUtils.isEmpty(permissionIds)) {
            rolePermissionDao.save(roleDto.getId(), permissionIds);
        }
        return Results.success();

    }
```



更新角色信息

```java
    public Integer update(RoleDto roleDto) {
        List<Long> permissionIds = roleDto.getPermissionIds();
        permissionIds.remove(0L);
        //5.1、更新角色权限之前要删除该角色之前的所有权限
        rolePermissionDao.deleteRolePermission(roleDto.getId());
        //5.2、判断该角色是否有赋予权限值，有就添加
        if(!CollectionUtils.isEmpty(permissionIds)){
            rolePermissionDao.save(roleDto.getId(), permissionIds);
        }
        return roleDao.update(roleDto);
    }
```

要先删除角色之前的所有权限





删除角色

```java
    public Results<SysRole> delete(Integer id) {
        List<SysRoleUser> datas = roleUserDao.listAllSysRoleUserByRoleId(id);
        if(datas.size() <= 0){
            roleDao.delete(id);
            return Results.success();
        }
        return Results.failure(ResponseCode.USER_ROLE_NO_CLEAR.getCode(),ResponseCode.USER_ROLE_NO_CLEAR.getMessage());
    }

```

如果用户角色中间表的信息没有提前删除，则会导致删除角色失败





### 4、用户设计

保存用户

```java
    public Results<SysUser> save(SysUser sysUser, Integer roleId) {
        if(roleId != null) {
            //保存到用户表
            userDao.save(sysUser);
            SysRoleUser sysRoleUser = new SysRoleUser();
            sysRoleUser.setRoleId(roleId);
            sysRoleUser.setUserId(sysUser.getId().intValue());
            //保存到用户角色表
            roleUserDao.save(sysRoleUser);

            return Results.success();
        }
        return Results.failure();
    }
```



更新用户

```java
    public Results<SysUser> updateUser(SysUser sysUser, Integer roleId) {
        if (roleId != null){
            //1、先保存用户信息
            userDao.updateUser(sysUser);
            //2、再保存用户对应的角色信息
            SysRoleUser sysRoleUser = new SysRoleUser();
            sysRoleUser.setUserId(sysUser.getId().intValue());
            sysRoleUser.setRoleId(roleId);
            //3、根据用户id查询角色id，用户有值则执行更新操作，没值则执行保存操作
            if (roleUserDao.getRoleUserByUserId(sysUser.getId().intValue()) != null){
                roleUserDao.updateSysRoleUser(sysRoleUser);
            }else{
                roleUserDao.save(sysRoleUser);
            }
            return Results.success();
        }else {
            return Results.failure();
        }
    }
```



删除用户

```java
    @Override
    public Integer deleteUser(Long id) {
        //1、删除角色
        roleUserDao.deleteRoleUserByUserId(id);
        //2、删除用户
        return userDao.deleteUser(id);
    }
```

















## 工作流程

![preview](https://raw.githubusercontent.com/a1254898873/images/master/202203241050531.jpg)

![Spring Security FilterChain](https://raw.githubusercontent.com/a1254898873/images/master/202203241050532.png)

一个认证过程，其实就是过滤器链上的一个绿色矩形Filter所要执行的过程。

基本的认证过程有三步骤：

1. Filter拦截请求，生成一个未认证的Authentication，交由AuthenticationManager进行认证；
2. AuthenticationManager的默认实现ProviderManager会通过AuthenticationProvider对Authentication进行认证，其本身不做认证处理；
3. 如果认证通过，则创建一个认证通过的Authentication返回；否则抛出异常，以表示认证不通过。



## 配置

```java
@Configuration	
@EnableWebSecurity	
public class SecurityConfig extends WebSecurityConfigurerAdapter {	
    @Override	
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {	
        auth.userDetailsService(userDetailService).passwordEncoder(new BCryptPasswordEncoder());	
    }	
    @Override	
    public void configure(WebSecurity web) throws Exception {	
        web.ignoring().antMatchers("/resources/**/*.html", "/resources/**/*.js");	
    }	
    @Override	
    protected void configure(HttpSecurity http) throws Exception {	
       http	
       .formLogin()	
       .loginPage("/login_page")	
       .passwordParameter("username")	
       .passwordParameter("password")	
       .loginProcessingUrl("/sign_in")	
       .permitAll()	
       .and().authorizeRequests().antMatchers("/test").hasRole("test")	
       .anyRequest().authenticated().accessDecisionManager(accessDecisionManager())	
       .and().logout().logoutSuccessHandler(new MyLogoutSuccessHandler())	
       .and().csrf().disable();	
       http.addFilterAt(getAuthenticationFilter(),UsernamePasswordAuthenticationFilter.class);	
       http.exceptionHandling().accessDeniedHandler(new MyAccessDeniedHandler());	
       http.addFilterAfter(new MyFittler(), LogoutFilter.class);	
    }	
}
```

1、configure(AuthenticationManagerBuilder auth)

AuthenticationManager 的建造器，配置 AuthenticationManagerBuilder 会让Security 自动构建一个 AuthenticationManager（该类的功能参考流程图）；如果想要使用该功能你需要配置一个 UserDetailService 和 PasswordEncoder。UserDetailsService 用于在认证器中根据用户传过来的用户名查找一个用户， PasswordEncoder 用于密码的加密与比对，我们存储用户密码的时候用PasswordEncoder.encode() 加密存储，在认证器里会调用 PasswordEncoder.matches() 方法进行密码比对。如果重写了该方法，Security 会启用 DaoAuthenticationProvider 这个认证器，该认证就是先调用 UserDetailsService.loadUserByUsername 然后使用 PasswordEncoder.matches() 进行密码比对，如果认证成功成功则返回一个 Authentication 对象。



2、configure(WebSecurity web)

这个配置方法用于配置静态资源的处理方式，可使用 Ant 匹配规则。



3、configure(HttpSecurity http)

```java
http	
.formLogin()	
.loginPage("/login_page")	
.passwordParameter("username")	
.passwordParameter("password")	
.loginProcessingUrl("/sign_in")	
.permitAll()
```

这是配置登录相关的操作从方法名可知，配置了登录页请求路径，密码属性名，用户名属性名，和登录请求路径，permitAll()代表任意用户可访问



```java
http	
.authorizeRequests()	
.antMatchers("/test").hasRole("test")	
.anyRequest().authenticated()	
.accessDecisionManager(accessDecisionManager());
```

以上配置是权限相关的配置，配置了一个 /test url 该有什么权限才能访问， anyRequest() 表示所有请求，authenticated() 表示已登录用户才能访问， accessDecisionManager() 表示绑定在 url 上的鉴权管理器



```java
http	
.logout()	
.logoutUrl("/logout")	
.logoutSuccessHandler(new MyLogoutSuccessHandler())
```

登出相关配置，这里配置了登出 url 和登出成功处理器:

```java
http	
.exceptionHandling()	
.accessDeniedHandler(new MyAccessDeniedHandler());
```

上面代码是配置鉴权失败的处理器。

```java
http.addFilterAfter(new MyFittler(), LogoutFilter.class);	
http.addFilterAt(getAuthenticationFilter(),UsernamePasswordAuthenticationFilter.class);
```

上面代码展示如何在过滤器链中插入自己的过滤器，addFilterBefore 加在对应的过滤器之前，addFilterAfter 加在对应的过滤器之后，addFilterAt 加在过滤器同一位置，事实上框架原有的 Filter 在启动 HttpSecurity 配置的过程中，都由框架完成了其一定程度上固定的配置，是不允许更改替换的。根据测试结果来看，调用 addFilterAt 方法插入的 Filter ，会在这个位置上的原有 Filter 之前执行。

注：关于 HttpSecurity 使用的是链式编程，其中 http.xxxx.and.yyyyy 这种写法和 http.xxxx;http.yyyy 写法意义一样。



## SecurityContextHolder

SpringSecurity会将用户的信息存储在SecurityContextHolder中，以下为SecurityContextHolder的模型

![在这里插入图片描述](https://raw.githubusercontent.com/a1254898873/images/master/202203241050533.png)

SecurityContextHolder中包含了SecurityContext对象，SecurityContext中包含了Authentication对象。

Authentication对象的功能有两个：

1. 作为后续验证的入参
2. 获取当前验证通过的用户信息

Authentication包含三个属性：

1. principal：用户身份，如果是用户/密码认证，这个属性就是UserDetails实例
2. credentials：通常就是密码，在大多数情况下，在用户验证通过后就会被清除，以防密码泄露。
3. authorities：用户权限













## Filter



AbstractAuthenticationProcessingFilter

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050534.png)

AbstractAuthenticationProcessingFilter是一个接口，在整个身份验证的流程中主要处理的工作就是所有与Web资源相关的事情，并且将其封装成Authentication对象，最后调用AuthenticationManager的验证方法。

Spring Security为我们提供了3个已经实现的Filter。UsernamePasswordAuthenticationFilter，BasicAuthenticationFilter和 RememberMeAuthenticationFilter。如果不做任何个性化的配置，UsernamePasswordAuthenticationFilter和BasicAuthenticationFilter会在默认的过滤器链中。这两种认证方式也就是默认的认证方式。



UsernamePasswordAuthenticationFilter

UsernamePasswordAuthenticationFilter作为实际的前台，会将客户端提交的username和password封装成一个UsernamePasswordAuthenticationToken交给认证管理部门(AuthenticationManager)进行认证。如此，她的任务就完成了。



BasicAuthenticationFilter

该前台(Filter)只会处理含有Authorization的Header，且小写化后的值以basic开头的请求，否则该前台(Filter)不负责处理。该Filter会从header中获取Base64编码之后的username和password，创建UsernamePasswordAuthenticationToken提供给认证管理部门(AuthenticationMananager)进行认证。



## Authentication

Authentication是一个接口，用来表示用户认证信息，在用户登录认证之前相关信息会封装为一个Authentication具体实现类的对象，在登录认证成功之后又会生成一个信息更全面，包含用户权限等信息的Authentication对象，然后把它保存在 SecurityContextHolder所持有的SecurityContext中，供后续的程序进行调用，如访问权限的鉴定等。



```java
public interface Authentication extends Principal, Serializable {
    
    // 该principal具有的权限。AuthorityUtils工具类提供了一些方便的方法。
    Collection<? extends GrantedAuthority> getAuthorities();
    // 证明Principal的身份的证书，比如密码。
    Object getCredentials();
    // authentication request的附加信息，比如ip,sessionId。
    Object getDetails();
    // 当事人。在username+password模式中为username，在有userDetails之后可以为userDetails。
    Object getPrincipal();
    // 是否已经通过认证。
    boolean isAuthenticated();
    // 设置通过认证。
    void setAuthenticated(boolean isAuthenticated) throws IllegalArgumentException;
}
```

```java
// 设置
SecurityContextHolder.getContext().setAuthentication(anAuthentication);
// 获取
Authentication existingAuth = SecurityContextHolder.getContext()
                .getAuthentication();
```





## AuthenticationManager

![在这里插入图片描述](https://raw.githubusercontent.com/a1254898873/images/master/202203241050535.png)

AuthenticationManager是用来实现身份认证的API接口，入参是Authentication，最常用的子类是ProviderManager

```java
public interface AuthenticationManager {

  Authentication authenticate(Authentication authentication)
    throws AuthenticationException;

}

```

其中authenticate()方法运行后可能会有三种情况：

1. 验证成功，返回一个带有用户信息的Authentication。
2. 验证失败，抛出一个AuthenticationException异常。
3. 无法判断，返回null。



## ProviderManager

ProviderManager是上面的AuthenticationManager最常见的实现，它不自己处理验证，而是将验证委托给其所配置的AuthenticationProvider列表，然后会依次调用每一个 AuthenticationProvider进行认证，这个过程中只要有一个AuthenticationProvider验证成功，就不会再继续做更多验证，会直接以该认证结果作为ProviderManager的认证结果。

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050536.jpg)

认证过程：

1. 用户使用用户名和密码进行登录。
2. Spring Security将获取到的用户名和密码封装成一个Authentication接口的实现类，比如常用的UsernamePasswordAuthenticationToken。
3. 将上述产生的Authentication对象传递给AuthenticationManager的实现类ProviderManager进行认证。
4. ProviderManager依次调用各个AuthenticationProvider进行认证，认证成功后返回一个封装了用户权限等信息的Authentication对象。
5. 将AuthenticationManager返回的Authentication对象赋予给当前的SecurityContext。



## UserDetailsService

Spring Security中进行身份验证的是AuthenticationManager接口，ProviderManager是它的一个默认实现，但它并不用来处理身份认证，而是委托给配置好的AuthenticationProvider，每个AuthenticationProvider会轮流检查身份认证。检查后或者返回Authentication对象或者抛出异常。

验证身份就是加载响应的UserDetails，看看是否和用户输入的账号、密码、权限等信息匹配。此步骤由实现AuthenticationProvider的DaoAuthenticationProvider（它利用UserDetailsService验证用户名、密码和授权）处理。包含 GrantedAuthority 的 UserDetails对象在构建 Authentication对象时填入数据。

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050537.png)

该接口只提供了一个方法：

```java
UserDetails loadUserByUsername(String username) throws UsernameNotFoundException;
```

可以取出用户的用户名和密码封装成UserDetails返回

## UserDetails

从上面UserDetailsService 可以知道最终交给Spring Security的是UserDetails 。该接口是提供用户信息的核心接口。该接口实现仅仅存储用户的信息。后续会将该接口提供的用户信息封装到认证对象Authentication中去。UserDetails 默认提供了：

- 用户的权限集， 默认需要添加ROLE_ 前缀
- 用户的加密后的密码， 不加密会使用{noop}前缀
- 应用内唯一的用户名
- 账户是否过期
- 账户是否锁定
- 凭证是否过期
- 用户是否可用



## 身份授权

![image-20220123165915885](https://raw.githubusercontent.com/a1254898873/images/master/202203241050538.png)

![SpringSecurity 原理解析【3】：认证与授权_SpringSecurity_02](https://raw.githubusercontent.com/a1254898873/images/master/202203241050539.png)



Spring Security中，系统进行认证以后，获得了当前的Authorities，紧接着，Spring Security会进行鉴权，判断他是否有权限。

这个判断主要通过投票管理器（AccessDecisionManager）+投票器（Voter）实现。

授权模型：为了便于开发，设计的安全访问规则来源有好几种，不同的安全访问规则需要不同的处理机制来解析。能对某种安全访问规则做出当前请求能否通过该规则并访问到资源的决断的对象在Spring Security中抽象为AccessDecisionVoter：选民，做出决断的动作称为：vote：投票。AccessDecisionVoter只能对支持的安全访问规则做出赞成、反对、弃权之一的决断，每个决断1个权重。接口规范如下：



```java
public interface AccessDecisionVoter {
	
	// 赞成票
	int ACCESS_GRANTED = 1;
	// 弃权票
	int ACCESS_ABSTAIN = 0;
	// 反对票
	int ACCESS_DENIED = -1;

	/**
	 *	是否支持对某配置数据进行投票 
	 */
	boolean supports(ConfigAttribute attribute);

	/**
	 * 是否支持对某类型元数据进行投票 
	 */
	boolean supports(Class clazz);

	/**
	 *  对认证信息进行投票
	 */
	int vote(Authentication authentication, S object,Collectionattributes);
}


```



Spring Security中对安全访问规则的最终决断是基于投票结果然后根据决策方针才做出的结论。能做出最终决断的接口为：AccessDecisionManager。

![AccessDecisionManager接口](https://raw.githubusercontent.com/a1254898873/images/master/202203241050540.awebp)







## jwt

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050541.png)



```java

 
 /**
  * jwt 认证拦截器 用于拦截 请求 提取jwt 认证
  *
  * @author dax
  * @since 2019/11/7 23:02
  */
 @Slf4j
 public class JwtAuthenticationFilter extends OncePerRequestFilter {
     private static final String AUTHENTICATION_PREFIX = "Bearer ";
     /**
      * 认证如果失败由该端点进行响应
      */
     private AuthenticationEntryPoint authenticationEntryPoint = new SimpleAuthenticationEntryPoint();
     private JwtTokenGenerator jwtTokenGenerator;
     private JwtTokenStorage jwtTokenStorage;
 
 
     public JwtAuthenticationFilter(JwtTokenGenerator jwtTokenGenerator, JwtTokenStorage jwtTokenStorage) {
         this.jwtTokenGenerator = jwtTokenGenerator;
         this.jwtTokenStorage = jwtTokenStorage;
     }
 
 
     @Override
     protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain chain) throws IOException, ServletException {
         // 如果已经通过认证
         if (SecurityContextHolder.getContext().getAuthentication() != null) {
             chain.doFilter(request, response);
             return;
         }
         // 获取 header 解析出 jwt 并进行认证 无token 直接进入下一个过滤器  因为  SecurityContext 的缘故 如果无权限并不会放行
         String header = request.getHeader(HttpHeaders.AUTHORIZATION);
         if (StringUtils.hasText(header) && header.startsWith(AUTHENTICATION_PREFIX)) {
             String jwtToken = header.replace(AUTHENTICATION_PREFIX, "");
 
 
             if (StringUtils.hasText(jwtToken)) {
                 try {
                     authenticationTokenHandle(jwtToken, request);
                 } catch (AuthenticationException e) {
                     authenticationEntryPoint.commence(request, response, e);
                 }
             } else {
                 // 带安全头 没有带token
                 authenticationEntryPoint.commence(request, response, new AuthenticationCredentialsNotFoundException("token is not found"));
             }
 
         }
         chain.doFilter(request, response);
     }
 
     /**
      * 具体的认证方法  匿名访问不要携带token
      * 有些逻辑自己补充 这里只做基本功能的实现
      *
      * @param jwtToken jwt token
      * @param request  request
      */
     private void authenticationTokenHandle(String jwtToken, HttpServletRequest request) throws AuthenticationException {
 
         // 根据我的实现 有效token才会被解析出来
         JSONObject jsonObject = jwtTokenGenerator.decodeAndVerify(jwtToken);
 
         if (Objects.nonNull(jsonObject)) {
             String username = jsonObject.getStr("aud");
 
             // 从缓存获取 token
             JwtTokenPair jwtTokenPair = jwtTokenStorage.get(username);
             if (Objects.isNull(jwtTokenPair)) {
                 if (log.isDebugEnabled()) {
                     log.debug("token : {}  is  not in cache", jwtToken);
                 }
                 // 缓存中不存在就算 失败了
                 throw new CredentialsExpiredException("token is not in cache");
             }
             String accessToken = jwtTokenPair.getAccessToken();
 
             if (jwtToken.equals(accessToken)) {
                   // 解析 权限集合  这里
                 JSONArray jsonArray = jsonObject.getJSONArray("roles");
 
                 String roles = jsonArray.toString();
 
                 List<GrantedAuthority> authorities = AuthorityUtils.commaSeparatedStringToAuthorityList(roles);
                 User user = new User(username, "[PROTECTED]", authorities);
                 // 构建用户认证token
                 UsernamePasswordAuthenticationToken usernamePasswordAuthenticationToken = new UsernamePasswordAuthenticationToken(user, null, authorities);
                 usernamePasswordAuthenticationToken.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));
                 // 放入安全上下文中
                 SecurityContextHolder.getContext().setAuthentication(usernamePasswordAuthenticationToken);
             } else {
                 // token 不匹配
                 if (log.isDebugEnabled()){
                     log.debug("token : {}  is  not in matched", jwtToken);
                 }
 
                 throw new BadCredentialsException("token is not matched");
             }
         } else {
             if (log.isDebugEnabled()) {
                 log.debug("token : {}  is  invalid", jwtToken);
             }
             throw new BadCredentialsException("token is invalid");
         }
     }
 }

```

```java
        @Override
         protected void configure(HttpSecurity http) throws Exception {
             http.csrf().disable()
                     .cors()
                     .and()
                     // session 生成策略用无状态策略
                        			   		.sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS) 
                     .and()
                     .exceptionHandling().accessDeniedHandler(new SimpleAccessDeniedHandler()).authenticationEntryPoint(new SimpleAuthenticationEntryPoint())
                     .and()
                     .authorizeRequests().anyRequest().authenticated()
                     .and()
                     .addFilterBefore(preLoginFilter, UsernamePasswordAuthenticationFilter.class)
                     // jwt 必须配置于 UsernamePasswordAuthenticationFilter 之前
                     .addFilterBefore(jwtAuthenticationFilter, UsernamePasswordAuthenticationFilter.class)
                     // 登录  成功后返回jwt token  失败后返回 错误信息
                     .formLogin().loginProcessingUrl(LOGIN_PROCESSING_URL).successHandler(authenticationSuccessHandler).failureHandler(authenticationFailureHandler)
                     .and().logout().addLogoutHandler(new CustomLogoutHandler()).logoutSuccessHandler(new CustomLogoutSuccessHandler());
 
         }
```



## session管理

```java
            .and()
                .sessionManagement() // 添加 Session管理器
                .invalidSessionUrl("/session/invalid") // Session失效后跳转到这个链接
                .maximumSessions(1)
                .expiredSessionStrategy(sessionExpiredStrategy)
                .and()
```

maximumSessions配置了最大Session并发数量为1个，如果mrbird这个账户登录后，在另一个客户端也使用mrbird账户登录，那么第一个使用mrbird登录的账户将会失效，类似于一个先入先出队列。expiredSessionStrategy配置了Session在并发下失效后的处理策略，这里为我们自定义的策略MySessionExpiredStrategy。

```java
@Component
public class MySessionExpiredStrategy implements SessionInformationExpiredStrategy {

    @Override
    public void onExpiredSessionDetected(SessionInformationExpiredEvent event) throws IOException, ServletException {
        HttpServletResponse response = event.getResponse();
        response.setStatus(HttpStatus.UNAUTHORIZED.value());
        response.setContentType("application/json;charset=utf-8");
        response.getWriter().write("您的账号已经在别的地方登录，当前登录已失效。如果密码遭到泄露，请立即修改密码！");
    }
}

```

获取在线登录用户数量的方法

```java
	/**
     * 获取系统在线用户数量
     * @return
     */
	public Integer getOnlineUserNum(SessionRegistry sessionRegistry){
        List<String> list = sessionRegistry.getAllPrincipals().stream()
                .filter(u -> !sessionRegistry.getAllSessions(u, false).isEmpty())
                .map(Object::toString)
                .collect(Collectors.toList());
        return list.size();
    }

```

移除登录用户的session

```java
	private void removeSesion(HttpServletRequest request) {
		HttpSession session = request.getSession();
		// 手动让系统中的session失效 。
		session.invalidate();
	}

```





# SpringCache

## 使用

使用SpringCache分为很简单的三步：加依赖，开启缓存，加缓存注解。

1、加依赖

```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>

```

2、开启缓存

在启动类加上@EnableCaching注解即可开启使用缓存

```java
@SpringBootApplication
@EnableCaching
public class CachingApplication {

	public static void main(String[] args) {
		SpringApplication.run(CachingApplication.class, args);
	}

}
```

3、加缓存注解

在要缓存的方法上面添加@Cacheable注解，即可缓存这个方法的返回值。

```java
@Override
@Cacheable("books")
public Book getByIsbn(String isbn) {
    simulateSlowService();
    return new Book(isbn, "Some book");
}

// Don't do this at home
private void simulateSlowService() {
    try {
        long time = 3000L;
        Thread.sleep(time);
    } catch (InterruptedException e) {
        throw new IllegalStateException(e);
    }
}
```



## 常用注解

@Cacheable

@Cacheble注解表示这个方法有了缓存的功能，方法的返回值会被缓存下来，下一次调用该方法前，会去检查是否缓存中已经有值，如果有就直接返回，不调用方法。如果没有，就调用方法，然后把结果缓存起来。这个注解一般用在查询和增加方法上。



@CachePut

加了@CachePut注解的方法，会把方法的返回值put到缓存里面缓存起来，供其它地方使用。它通常用在更新方法上。



@CacheEvict

使用了CacheEvict注解的方法，会清空指定缓存。一般用在更新或者删除的方法上。





## 使用Redis作为缓存



```java
@Configuration
@SuppressWarnings("all")
@EnableCaching
public class RedisConfig {

    @Bean
    public CacheManager cacheManager(RedisConnectionFactory factory) {
        RedisSerializer<String> redisSerializer = new StringRedisSerializer();
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        //解决查询缓存转换异常的问题
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);

        // 配置序列化（解决乱码的问题）
        RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
                .entryTtl(Duration.ofDays(1))
                .serializeKeysWith(RedisSerializationContext.SerializationPair.fromSerializer(redisSerializer))
                .serializeValuesWith(RedisSerializationContext.SerializationPair.fromSerializer(jackson2JsonRedisSerializer))
                .disableCachingNullValues();

        RedisCacheManager cacheManager = RedisCacheManager.builder(factory)
                .cacheDefaults(config)
                .build();
        return cacheManager;
    }
}

```





# Spring Session

1、引入依赖

```java
<dependency>
    <groupId>org.springframework.session</groupId>
    <artifactId>spring-session-data-redis</artifactId>
</dependency>
```

2、配置

```java
## Session 存储方式
spring.session.store-type=redis

## Session 过期时间，默认单位为 s
server.servlet.session.timeout=600
## Session 存储到 Redis 键的前缀
spring.session.redis.namespace=test:spring:session

## Redis 相关配置
spring.redis.host=127.0.0.1
spring.redis.password=****
spring.redis.port=6379

```





# Mybatis

## 使用

### 1、全注解

```java
public interface UserMapper2 {
    @Select("select * from user")
    List<User> getAllUsers();

    @Results({
            @Result(property = "id", column = "id"),
            @Result(property = "username", column = "u"),
            @Result(property = "address", column = "a")
    })
    @Select("select username as u,address as a,id as id from user where id=#{id}")
    User getUserById(Long id);

    @Select("select * from user where username like concat('%',#{name},'%')")
    List<User> getUsersByName(String name);

    @Insert({"insert into user(username,address) values(#{username},#{address})"})
    @SelectKey(statement = "select last_insert_id()", keyProperty = "id", before = false, resultType = Integer.class)
    Integer addUser(User user);

    @Update("update user set username=#{username},address=#{address} where id=#{id}")
    Integer updateUserById(User user);

    @Delete("delete from user where id=#{id}")
    Integer deleteUserById(Integer id);
}
```

UserMapper2创建好之后，还要配置mapper扫描，有两种方式，一种是直接在UserMapper2上面添加@Mapper注解，这种方式有一个弊端就是所有的Mapper都要手动添加，要是落下一个就会报错，还有一个一劳永逸的办法就是直接在启动类上添加Mapper扫描，如下：

```java
@SpringBootApplication
@MapperScan(basePackages = "org.sang.mybatis.mapper")
public class MybatisApplication {
    public static void main(String[] args) {
        SpringApplication.run(MybatisApplication.class, args);
    }
}
```

### 2、xml

```java
public interface UserMapper {
    List<User> getAllUser();

    Integer addUser(User user);

    Integer updateUserById(User user);

    Integer deleteUserById(Integer id);
}
```



```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.sang.mybatis.mapper.UserMapper">
    <select id="getAllUser" resultType="org.sang.mybatis.model.User">
        select * from t_user;
    </select>
    <insert id="addUser" parameterType="org.sang.mybatis.model.User">
        insert into user (username,address) values (#{username},#{address});
    </insert>
    <update id="updateUserById" parameterType="org.sang.mybatis.model.User">
        update user set username=#{username},address=#{address} where id=#{id}
    </update>
    <delete id="deleteUserById">
        delete from user where id=#{id}
    </delete>
</mapper>
```

那么这个UserMapper.xml到底放在哪里呢？有两个位置可以放，第一个是直接放在UserMapper所在的包下面：

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050542.png)

放在这里的UserMapper.xml会被自动扫描到，但是有另外一个Maven带来的问题，就是java目录下的xml资源在项目打包时会被忽略掉，所以，如果UserMapper.xml放在包下，需要在pom.xml文件中再添加如下配置，避免打包时java目录下的XML文件被自动忽略掉：

```java
<build>
    <resources>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.xml</include>
            </includes>
        </resource>
        <resource>
            <directory>src/main/resources</directory>
        </resource>
    </resources>
</build>
```

当然，UserMapper.xml也可以直接放在resources目录下，这样就不用担心打包时被忽略了，但是放在resources目录下，又不能自动被扫描到，需要添加额外配置。例如我在resources目录下创建mapper目录用来放mapper文件，如下：

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050543.png)

此时在application.properties中告诉mybatis去哪里扫描mapper：

```java
mybatis.mapper-locations=classpath:mapper/*.xml
```





## resultType



1、基本类型

```apl
    <!-- 
        指定 resultType 返回值类型时 String 类型的，
        string 在这里是一个别名，代表的是 java.lang.String 

        对于引用数据类型，都是将大写字母转小写，比如 HashMap 对应的别名是 'hashmap'
        基本数据类型考虑到重复的问题，会在其前面加上 '_'，比如 byte 对应的别名是 '_byte'
    -->
    <select id="getEmpNameById" resultType="string">
        select username from t_employee where id = #{id}
    </select>
```



2、返回 JavaBean 类型

如果列名和JavaBean不一致，但列名符合单词下划线分割，Java是驼峰命名法，则mapUnderscoreToCamelCase可设置为true

```apl
    <!-- 
        通过 resultType 指定查询的结果是 Employee 类型的数据  
        只需要指定 resultType 的类型，MyBatis 会自动将查询的结果映射成 JavaBean 中的属性
    -->
    <select id="getEmpById" resultType="employee">
        select * from t_employee where id = #{id}
    </select>
```

3、返回List类型

resultType=List中元素的类型

```java
    // 假如是全表查询数据，将查询的数据封装成 Employee 类型的集合
    List<Employee> getAllEmps();
```

```apl
    <!--
        注意这里的 resultType 返回值类型是集合内存储数据的类型，不是 'list'
    -->
    <select id="getAllEmps" resultType="employee">
        select * from t_employee
    </select>
```

4、返回Map类型

单条记录：resultType =map 

多条记录：resultType =Map中value的类型



单条

```java
    //  根据 id 查询信息，并把结果信息封装成 Map 
    Map<String, Object> getEmpAsMapById(Integer id);
```

```apl
    <!-- 
        注意这里的 resultType 返回值类型是 'map'
     -->
    <select id="getEmpAsMapById" resultType="map">
        select * from t_employee where id = #{id}
    </select>
```



多条

```java
    // 查询所有员工的信息，把数据库中的 'id' 字段作为 key,对应的 value 封装成 Employee 对象
    // @MapKey 中的值表示用数据库中的哪个字段名作 key
    @MapKey("id")
    Map<Integer, Employee> getAllEmpsAsMap();
```

```apl
    <!--
        注意 resultType 返回值类型，不再是 'map'，而是 Map 的 value 对应的 JavaBean 类型
    -->
    <select id="getAllEmpsAsMap" resultType="employee">
        select * from t_employee
    </select>
```





## resultMap

1、字段映射

```apl
<resultMap id="getUserByIdMap" type="User">
	<result property="id" column="uid"></result>
</resultMap>

```

这里面column对应的是数据库的列名或别名；property对应的是结果集的字段或属性。

select语句中加入resultMap="getUserByIdMap"。



2、一对一关联查询（association）

```apl
<select id="getUserById" resultType="User">
        SELECT
            u.id,
            u.username,
            u.password,
            u.address,
            u.email,
            r.id as 'role_id',
            r.name as 'role_name'
        FROM
            USER u
                LEFT JOIN user_roles ur ON u.id = ur.user_id
                LEFT JOIN role r ON r.id = ur.role_id
        where u.id=#{id}
    </select>


```



```xml
<resultMap id="userMap" type="User">
	<id property="id" column="id"></id>
	<result property="username" column="username"></result>
	<result property="password" column="password"></result>
	<result property="address" column="address"></result>
	<result property="email" column="email"></result>
	
	<association property="role" javaType="Role">
		<id property="id" column="role_id"></id>
		<result property="name" column="role_name"></result>
	</association>
</resultMap>
```



```json
{
    "id": "1001",
    "username": "后羿",
    "password": "123456",
    "address": "北京市海淀区",
    "email": "510273027@qq.com",
    "role": {
        "id": "3",
        "name": "射手"
    }
}
```



3、一对多关联查询（collection）

```xml
<resultMap id="userMap" type="User">
	<id property="id" column="id"></id>
	<result property="username" column="username"></result>
	<result property="password" column="password"></result>
	<result property="address" column="address"></result>
	<result property="email" column="email"></result>
	
	<collection property="roles" ofType="Role">
		<id property="id" column="role_id"></id>
		<result property="name" column="role_name"></result>
	</collection>
</resultMap>
```



```json
{
    "id": "1003",
    "username": "貂蝉",
    "password": "123456",
    "address": "北京市东城区",
    "email": "510273027@qq.com",
    "roles": [
        {
            "id": "1",
            "name": "中单"
        },
        {
            "id": "2",
            "name": "打野"
        }
    ]
}
```









## 传递多参数

1、不需要写parameterType参数

```xml
<select id="getXXXBeanList" resultType="XXBean">

　　select t.* from tableName where id = #{0} and name = #{1}  

</select>  
```

由于是多参数那么就不能使用parameterType， 改用#｛index｝是第几个就用第几个的索引，索引从0开始



2、注解

```java
public List<XXXBean> getXXXBeanList(@Param("id")String id, @Param("code")String code);  

```

```xml
<select id="getXXXBeanList" resultType="XXBean">

　　select t.* from tableName where id = #{id} and name = #{code}  

</select>  
```



3、map封装

```xml
<select id="getXXXBeanList" parameterType="hashmap" resultType="XXBean">

　　select 字段... from XXX where id=#{xxId} code = #{xxCode}  

</select>  

```

其中hashmap是mybatis自己配置好的直接使用就行。map中key的名字是那个就在#{}使用那个。





## 占位符

#{} 能防止sql 注入，${} 不能防止sql 注入





## 工作流程



![MyBatis执行流程](https://raw.githubusercontent.com/a1254898873/images/master/202203241050544.png)

![在这里插入图片描述](https://raw.githubusercontent.com/a1254898873/images/master/202203241050545.png)

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050546.awebp)



## SqlSessionFactory

SqlSessionFactory是MyBatis的关键对象,它是个单个数据库映射关系经过编译后的内存镜像.SqlSessionFactory对象的实例可以通过SqlSessionFactoryBuilder对象类获得,而SqlSessionFactoryBuilder则可以从XML配置文件或一个预先定制的Configuration的实例构建出SqlSessionFactory的实例.每一个MyBatis的应用程序都以一个SqlSessionFactory对象的实例为核心.同时SqlSessionFactory也是线程安全的,SqlSessionFactory一旦被创建,应该在应用执行期间都存在.在应用运行期间不要重复创建多次,建议使用单例模式.SqlSessionFactory是创建SqlSession的工厂.

```java
//SqlSessionFactory接口源码如下所示:

package org.apache.ibatis.session;

import java.sql.Connection;

public interface SqlSessionFactory {

  SqlSession openSession();//这个方法最经常用,用来创建SqlSession对象.

  SqlSession openSession(boolean autoCommit);
  SqlSession openSession(Connection connection);
  SqlSession openSession(TransactionIsolationLevel level);

  SqlSession openSession(ExecutorType execType);
  SqlSession openSession(ExecutorType execType, boolean autoCommit);
  SqlSession openSession(ExecutorType execType, TransactionIsolationLevel level);
  SqlSession openSession(ExecutorType execType, Connection connection);

  Configuration getConfiguration();

}
```



## SqlSession

SqlSession是MyBatis的关键对象,是执行持久化操作的独享,类似于JDBC中的Connection.它是应用程序与持久层之间执行交互操作的一个单线程对象,也是MyBatis执行持久化操作的关键对象.SqlSession对象完全包含以数据库为背景的所有执行SQL操作的方法,它的底层封装了JDBC连接,可以用SqlSession实例来直接执行被映射的SQL语句.每个线程都应该有它自己的SqlSession实例.SqlSession的实例不能被共享,同时SqlSession也是线程不安全的,绝对不能讲SqlSeesion实例的引用放在一个类的静态字段甚至是实例字段中.也绝不能将SqlSession实例的引用放在任何类型的管理范围中,比如Servlet当中的HttpSession对象中.使用完SqlSeesion之后关闭Session很重要,应该确保使用finally块来关闭它.

```java
//SqlSession接口源码如下所示:

package org.apache.ibatis.session;

import java.io.Closeable;
import java.sql.Connection;
import java.util.List;
import java.util.Map;

import org.apache.ibatis.executor.BatchResult;

public interface SqlSession extends Closeable {

  <T> T selectOne(String statement);

  <T> T selectOne(String statement, Object parameter);

  <E> List<E> selectList(String statement);

  <E> List<E> selectList(String statement, Object parameter);

  <E> List<E> selectList(String statement, Object parameter, RowBounds rowBounds);

  <K, V> Map<K, V> selectMap(String statement, String mapKey);

  <K, V> Map<K, V> selectMap(String statement, Object parameter, String mapKey);

  <K, V> Map<K, V> selectMap(String statement, Object parameter, String mapKey, RowBounds rowBounds);

  void select(String statement, Object parameter, ResultHandler handler);

  void select(String statement, ResultHandler handler);

  void select(String statement, Object parameter, RowBounds rowBounds, ResultHandler handler);

  int insert(String statement);

  int insert(String statement, Object parameter);

  int update(String statement);

  int update(String statement, Object parameter);

  int delete(String statement);

  int delete(String statement, Object parameter);

  void commit();

  void commit(boolean force);

  void rollback();

  void rollback(boolean force);

  List<BatchResult> flushStatements();

  void close();

  void clearCache();

  Configuration getConfiguration();

  <T> T getMapper(Class<T> type);

  Connection getConnection();
}

```



## Executor

SimpleExecutor：每执行一次update或select，就开启一个Statement对象，用完立刻关闭Statement对象。

ReuseExecutor：执行update或select，以sql作为key查找Statement对象，存在就使用，不存在就创建，用完后，不关闭Statement对象，而是放置于Map内，供下一次使用。简言之，就是重复使用Statement对象。

BatchExecutor：执行update（没有select，JDBC批处理不支持select），将所有sql都添加到批处理中（addBatch()），等待统一执行（executeBatch()），它缓存了多个Statement对象，每个Statement对象都是addBatch()完毕后，等待逐一执行executeBatch()批处理。与JDBC批处理相同。







## StatementHandler

![image-20200609160840547](https://raw.githubusercontent.com/a1254898873/images/master/202203241050547.png)

这里要注意这三者之间比例是1：1：n。也就是说多个SQL操作对应一个会话，和唯一的执行器以及N个StatementHandler。这里的N取决于通过会话调用了多少次Sql（命中缓存除外）。

```java
// 基于JDBC 声明Statement
Statement prepare(Connection connection, Integer transactionTimeout)
    throws SQLException;
// 为Statement 设置方法
void parameterize(Statement statement)
    throws SQLException;
// 添加批处理（并非执行）
void batch(Statement statement)
    throws SQLException;
// 执行update操作
int update(Statement statement)
    throws SQLException;
// 执行query操作
<E> List<E> query(Statement statement, ResultHandler resultHandler)
    throws SQLException;
```





## ParameterHandler

参数处理即将Java Bean转换成数据类型。总共要经历过三个步骤，参数转换、参数映射、参数赋值。



![image-20200609165232832](https://raw.githubusercontent.com/a1254898873/images/master/202203241050548.png)



参数转换：将JAVA 方法中的普通参数，封装转换成Map，以便map中的key和sql的参数引用相对应。



参数映射：映射是指Map中的key如何与SQL中绑定的参数相对应



参数赋值：通过TypeHandler 为PrepareStatement设置值，通常情况下一般的数据类型MyBatis都有与之相对应的TypeHandler



## ResultsetHandle

![image-20200609173114311](https://raw.githubusercontent.com/a1254898873/images/master/202203241050549.png)

1. 读取结果集
2. 遍历结果集当中的行
3. 创建对象
4. 填充属性





## mybatis中的动态代理

在使用Mybatis的时候，我们可以只定义一个XxxMaper接口，然后直接利用这个接口定义的抽象方法来进行增删改查操作，Mybatis内部实际上利用了动态代理技术帮我们生成了这个接口的代理类。

每次当我们调用sqlSession的getMapper方法时，都会创建一个新的动态代理类实例，如：

```java
sqlSession.getMapper(UserMapper.class);
```





## 一级缓存

Mybatis对缓存提供支持，但是在没有配置的默认情况下，它只开启一级缓存，一级缓存只是相对于同一个SqlSession而言。所以在参数和SQL完全一样的情况下，我们使用同一个SqlSession对象调用一个Mapper方法，往往只执行一次SQL，因为使用SelSession第一次查询后，MyBatis会将其放在缓存中，以后再查询的时候，如果没有声明需要刷新，并且缓存没有超时的情况下，SqlSession都会取出当前缓存的数据，而不会再次发送SQL到数据库。

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050550.png)



命中条件

以下数据必须全部相同才可以命中缓存：

1. 相同的statement id
2. 相同的Session
3. 相同的Sql与参数
4. 返回行范围相同





## 二级缓存

MyBatis的二级缓存是Application级别的缓存，它可以提高对数据库查询的效率，以提高应用的性能。

SqlSessionFactory层面上的二级缓存默认是不开启的，二级缓存的开席需要进行配置，实现二级缓存的时候，MyBatis要求返回的POJO必须是可序列化的。 也就是要求实现Serializable接口，配置方法很简单，只需要在映射XML文件配置就可以开启缓存了。

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050551.png)



缓存命中条件

MyBatis一二级缓存的CacheKey是一至的，必须满足以条件才可以命中缓：

1. 相同的statement id
2. 相同的Sql与参数
3. 返回行范围相同
4. 没有使用ResultHandler来自定义返回数据
5. 没有配置UseCache=false 来关闭缓存
6. 没有配置FlushCache=true 来清空缓存
7. 在调用存储过程中不能使用出参，即Parameter中mode=out|inout



## 分页

1、limit分页

```java
    <select id="getUserInfo1" parameterType="map" resultType="dayu">
        select * from user
        <if test="startPos!=null and pageSize!=null">
            limit ${startPos},${pageSize}
        </if>
    </select>

```

```java
List<User> getUserInfo1(Map<String,Object> map);
```



2、RowBounds分页

RowBounds帮我们省略了limit的内容，我们只需要在业务层关注分页即可！无须再传入指定数据！


但是，这个属于逻辑分页，即实际上sql查询的是所有的数据，在业务层进行了分页而已，比较占用内存，而且数据更新不及时，可能会有一定的滞后性！不推荐使用！

```java
@Test
    public void selectUserRowBounds() {
        SqlSession session = MybatisUtils.getSession();
        UserMapper mapper = session.getMapper(UserMapper.class);
//        List<User> users = session.selectList("com.dy.mapper.UserMapper.getUserInfoRowBounds",null,new RowBounds(0, 5));
        List<User> users = mapper.getUserInfoRowBounds(new RowBounds(0,5));
        for (User map: users){
            System.out.println(map);
        }
        session.close();
    }

```



3、PageHelper

```java
    <dependency>
       <groupId>com.github.pagehelper</groupId>
       <artifactId>pagehelper</artifactId>
       <version>5.1.7</version>
    </dependency>
```

```java
    <plugins>
        <plugin interceptor="com.github.pagehelper.PageInterceptor" />
    </plugins>

```

```java
@Test
    public void selectUserPageHelper() {
        SqlSession session = MybatisUtils.getSession();
        UserMapper mapper = session.getMapper(UserMapper.class);
        //第二种，Mapper接口方式的调用，推荐这种使用方式。
        PageHelper.startPage(1, 3);
        List<User> list = mapper.getUserInfo();
        //用PageInfo将包装起来
        PageInfo page = new PageInfo(list);
        for (User map: list){
            System.out.println(map);
        }
        System.out.println("page:---"+page);
        session.close();
    }
```





# MybatisPlus

## 配置

1、引入最新的依赖

```java
<dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>Latest Version</version>
</dependency>
```

2、配置datasource



3、在Spring启动类中添加MapperScan注解

```java
@SpringBootApplication
@MapperScan("com.example.mybatisplus.mapper")
public class MybatisPlusApplication {
    public static void main(String[] args) {
        SpringApplication.run(MybatisPlusApplication.class, args);
    }
}
```



4、创建Mapper类时，继承BaseMapper类，这是MybatisPlus提供的一个基类，封装了常用的查询操作

```java
public interface UserMapper extends BaseMapper<UserDO> {
}
```



## IService

MP除了通用的Mapper还是通用的Servcie层，这也减少了相对应的代码工作量。

```java
public interface EmployeeService extends IService<Employee> {
}
```

```java
@Service
public class EmployeeImpl extends ServiceImpl<EmployeeMapper, Employee> implements EmployeeService {
}
```





## 逻辑删除

1、配置文件

```java
mybatis-plus:
  global-config:
    db-config:
      logic-delete-field: flag # 全局逻辑删除的实体字段名(since 3.3.0,配置后可以忽略不配置步骤2)
      logic-delete-value: 1 # 逻辑已删除值(默认为 1)
      logic-not-delete-value: 0 # 逻辑未删除值(默认为 0)
```

2、实体类字段上加上@TableLogic注解

```java
@TableLogic
private Integer deleted;
```





## 分页

1、配置

```java

@Configuration
public class MyBatisPlusConfig {
 
    /**
     * 分页插件
     * @return
     */
    @Bean
    public PaginationInterceptor paginationInterceptor() {
        return new PaginationInterceptor();
    }
}
```



2、简单查询

```java
    /**
     * 分页查询
     */
    @Test
    public void selectByPage() {
        QueryWrapper<User> wrapper = new QueryWrapper();
        wrapper.like("name", "雨").lt("age", 40);
 
        Page<User> page = new Page<>(1,2);
 
        //IPage<User> userIPage = userMapper.selectPage(page, wrapper);
 
        IPage<Map<String, Object>> mapIPage = userMapper.selectMapsPage(page, wrapper);
 
 
        System.out.println("总页数"+mapIPage.getPages());
        System.out.println("总记录数"+mapIPage.getTotal());
        List<Map<String, Object>> records = mapIPage.getRecords();
        records.forEach(System.out::println);
    }
```



3、自定义sql分页查询

有时候查询的数据难免会出现多表连接查询，或者是一些复杂的sql语句，但是这些语句也是需要支持分页查询的

IPage是返回封装好的结果，Page是传过去的参数

```java
IPage<UserVo> selectPageVo(IPage<?> page, Integer state);
// or (class MyPage extends Ipage<UserVo>{ private Integer state; })
MyPage selectPageVo(MyPage page);
// or
List<UserVo> selectPageVo(IPage<UserVo> page, Integer state);
```

```xml
<select id="selectPageVo" resultType="xxx.xxx.xxx.UserVo">
    SELECT id,name FROM user WHERE state=#{state}
</select>
```

如果返回类型是 IPage 则入参的 IPage 不能为null,因为 返回的IPage == 入参的IPage
如果返回类型是 List 则入参的 IPage 可以为 null(为 null 则不分页),但需要你手动 入参的IPage.setRecords(返回的 List);
如果 xml 需要从 page 里取值,需要 page.属性 获取



## Wrapper

eq：等于，ne：不等于

```java
@Test
public void testSelectOne() {
    QueryWrapper<User> queryWrapper = new QueryWrapper<>();
    queryWrapper.eq("name", "Tom");
 
    User user = userMapper.selectOne(queryWrapper);
    System.out.println(user);
}
```

gt：大于，ge：大于等于，lt：小于，le：小于等于

```java
 
@Test
public void testDelete() {
 
    QueryWrapper<User> queryWrapper = new QueryWrapper<>();
    queryWrapper
        .isNull("name")
        .ge("age", 12)
        .isNotNull("email");
    int result = userMapper.delete(queryWrapper);
    System.out.println("delete return count = " + result);
}
```



## 乐观锁

1、配置

```java
@Configuration
public class MyConfig {

    //乐观锁插件
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
        return interceptor;
    }

}
```

2、在实体类的字段上加上@Version注解

```java
@Version
private Integer version
```



## 自动填充

实体类加注解

```java
/**
     * 创建时间
     */
    @TableField(fill = FieldFill.INSERT)
    private Date createTime;

    /**
     * 修改时间
     */
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private Date updateTime;
    /**
     * 逻辑删除
     */
    @TableField(fill = FieldFill.INSERT)
    @TableLogic
    private Integer deleteFlag;

```



实现字段填充控制器，编写自定义填充规则

```java
@Component
public class MyMetaObjectHandler implements MetaObjectHandler {

    private static final Logger LOGGER = LoggerFactory.getLogger(MyMetaObjectHandler.class);
    
    // 新增的时候自动填充
    @Override
    public void insertFill(MetaObject metaObject) {
        LOGGER.info("start insert fill ....");
        this.setFieldValByName("createTime",new Date(),metaObject);
        this.setFieldValByName("updateTime", new Date(), metaObject);
        this.setFieldValByName("deleteFlag", 0, metaObject);
    }

	// 更新的税后自动填充
    @Override
    public void updateFill(MetaObject metaObject) {
        LOGGER.info("start update fill ....");
        this.setFieldValByName("updateTime", new Date(),metaObject);
    }
}
```







# Activiti

## 数据库表

- 　    act_hi_*：'hi’表示 history，此前缀的表包含历史数据，如历史(结束)流程实例，变量，任务等等。

- 　　act_ge_*：'ge’表示 general，此前缀的表为通用数据，用于不同场景中。

- 　　act_evt_*：'evt’表示 event，此前缀的表为事件日志。

- 　　act_procdef_*：'procdef’表示 processdefine，此前缀的表为记录流程定义信息。

- 　　act_re_*：'re’表示 repository，此前缀的表包含了流程定义和流程静态资源(图片，规则等等)。

- 　　act_ru_*：'ru’表示 runtime，此前缀的表是记录运行时的数据，包含流程实例，任务，变量，异步任务等运行中的数据。Activiti只在流程实例执行过程中保存这些数据，在流程结束时就会删除这些记录。



| 表名                  | 表注释                                                       |
| --------------------- | ------------------------------------------------------------ |
| act_ge_bytearray      | 二进制数据表，存储通用的流程定义和流程资源。                 |
| act_ge_property       | 系统相关属性，属性数据表存储整个流程引擎级别的数据，初始化表结构时，会默认插入三条记录。 |
| act_re_deployment     | 部署信息表                                                   |
| act_re_model          | 流程设计模型部署表                                           |
| act_re_procdef        | 流程定义数据表                                               |
| act_ru_deadletter_job | 作业死亡信息表，作业失败超过重试次数                         |
| act_ru_event_subscr   | 运行时事件表                                                 |
| act_ru_execution      | 运行时流程执行实例表                                         |
| act_ru_identitylink   | 运行时用户信息表                                             |
| act_ru_integration    | 运行时积分表                                                 |
| act_ru_job            | 运行时作业信息表                                             |
| act_ru_suspended_job  | 运行时作业暂停表                                             |
| act_ru_task           | 运行时任务信息表                                             |
| act_ru_timer_job      | 运行时定时器作业表                                           |
| act_ru_variable       | 运行时变量信息表                                             |
| act_hi_actinst        | 历史节点表                                                   |
| act_hi_attachment     | 历史附件表                                                   |
| act_hi_comment        | 历史意见表                                                   |
| act_hi_detail         | 历史详情表，提供历史变量的查询                               |
| act_hi_identitylink   | 历史流程用户信息表                                           |
| act_hi_procinst       | 历史流程实例表                                               |
| act_hi_taskinst       | 历史任务实例表                                               |
| act_hi_varinst        | 历史变量表                                                   |
| act_evt_log           | 流程引擎的通用事件日志记录表                                 |
| act_procdef_info      | 流程定义的动态变更信息                                       |





## 核心类

### ProcessEngine

流程引擎的抽象，可以通过此类获取需要的所有服务



### TaskService

流程运行过程中，每个任务节点的相关操作接口，如complete,delete,delegate等。



### RepositoryService

流程定义和部署相关的存储服务。



### RuntimeService

流程运行时相关的服务，如根据流程好启动流程实例startProcessInstanceByKey。



### HistoryService

历史记录相关服务接口





## BPM节点



1、StartEvent

开始事件

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050552.png)



2、EndEvent

结束事件

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050553.png)



3、Task

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050554.png)

| 属性名称        | 属性说明                                                 |
| --------------- | -------------------------------------------------------- |
| Assignee        | 指定用户任务的处理人                                     |
| Cadidate Users  | 指定用户任务的候选人，多个用逗号隔开                     |
| Cadidate Groups | 指定多个候选组，多个用逗号隔开                           |
| Due Date        | 设置任务的到期日，通常用变量代替而不是设定一个具体的日期 |
| Priority        | 设定任务的优先级，取值区间[0,100]                        |





4、ParallelGateway

并行网关，顺序流没有条件解析，且分支执行不分先后。例如下面流程，需要科任老师批准、班主任批准，请假流程才能结束。

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050556.png)



5、ExclusiveGateway

排他网关，条件解析为true的顺序流会被选中，流程往前走。例如下面流程，请假天数小于等于1天，科任老师老师批准，请假流程结束；请假天数大于1天，就需要班主任批准。

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050557.png)





6、InclusiveGateway

包含网关，并行与排他的结合体，所有顺序流的条件都会被解析，结果为true的顺序会以并行方式继续执行。例如下面流程，请假天数小于等于1天，科任老师老师批准，请假流程结束；请假天数大于1天，班主任批准，请假流程结束；请假天数大于三天，就需要班主任批准、校长批准，请假流程才能结束。

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050558.png)















## 配置数据库

在resources文件下创建activiti.cfg.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="processEngineConfiguration" class="org.activiti.engine.impl.cfg.StandaloneProcessEngineConfiguration">
        <property name="databaseType" value="mysql"></property>
        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/activiti"></property>
        <property name="jdbcDriver" value="com.mysql.jdbc.Driver"></property>
        <property name="jdbcUsername" value="root"></property>
        <property name="jdbcPassword" value="123456"></property>
    </bean>
</beans>

```





## 流程控制

### 流程部署



```java
/**
 * 流程定义的部署
 * activiti表有哪些？
 * act_re_deployment  流程定义部署表，记录流程部署信息
 * act_re_procdef     流程定义表，记录流程定义信息
 * act_ge_bytearray   资源表（bpmn文件及png文件）
 */
@Test
public void createDeploy() { 
    RepositoryService repositoryService = processEngine.getRepositoryService();

    Deployment deployment = repositoryService.createDeployment()
            .addClasspathResource("diagram/holiday.bpmn")//添加bpmn资源
            .addClasspathResource("diagram/holiday.png")
            .name("请假申请单流程")
            .deploy(); 
    log.info("流程部署id:" + deployment.getName());
    log.info("流程部署名称:" + deployment.getId());
    }
```





### 流程定义查询



```java
@Test
public void queryProcessDefinition() { 
    String processDefinitionKey = "holiday"; 
    RepositoryService repositoryService = processEngine.getRepositoryService();
    //查询流程定义
    ProcessDefinitionQuery processDefinitionQuery = repositoryService.createProcessDefinitionQuery();
 
    List<ProcessDefinition> list = processDefinitionQuery.processDefinitionKey(processDefinitionKey)
            .orderByProcessDefinitionVersion().desc().list();

    for (ProcessDefinition processDefinition : list) {
        log.info("------------------------");
        log.info("流  程  部  署id：" + processDefinition.getDeploymentId());
        log.info("流程定义id：" + processDefinition.getId());
        log.info("流程定义名称：" + processDefinition.getName());
        log.info("流程定义key：" + processDefinition.getKey());
        log.info("流程定义版本：" + processDefinition.getVersion());
    }
}
```





### 流程定义删除



```java
/**
 * 删除已经部署成功的流程定义
 * 背后影响的表:
 * act_ge_bytearray
 * act_re_deployment
 * act_re_procdef
 */
@Test
public void deleteDeployment() {
    String deploymentId = "a2833cf7-10bb-11ea-9ac9-00155d65d6c0";
    RepositoryService repositoryService = processEngine.getRepositoryService();
    //删除流程定义，如果该流程定义已有流程实例启动则删除时出错
//        repositoryService.deleteDeployment(deploymentId);

    //设置true 级联删除流程定义，即使该流程有流程实例启动也可以删除，设置为false非级别删除方式，如果流程
    repositoryService.deleteDeployment(deploymentId, true);
}
```



### 启动流程



```java
@Test
public void startProcessInstance() {
    RuntimeService runtimeService = processEngine.getRuntimeService();
    //启动流程实例,同时还要指定业务标识businessKey  它本身就是请假单的id
    //第一个参数：是指流程定义key
    //第二个参数：业务标识businessKey
    ProcessInstance processInstance = runtimeService.startProcessInstanceByKey("holiday", "1001");

    String businessKey = processInstance.getBusinessKey();
    System.out.println("businessKey:" + businessKey);
}
```

Businesskey：业务标识，通常为业务表的主键，业务标识和流程实例一一对应。业务标识来源于业务系统。存储业务标识就是根据业务标识来关联查询业务系统的数据。
比如：请假流程启动一个流程实例，就可以将请假单的id作为业务标识存储到activiti中，将来查询activiti的流程实例信息就可以获取请假单的id从而关联查询业务系统数据库得到请假单信息。





### 查询流程实例



```java
@Test
public void queryProcessInstance() { 
    String processDefinitionKey = "holiday"; 
    RuntimeService runtimeService = processEngine.getRuntimeService();

    List<ProcessInstance> list = runtimeService.createProcessInstanceQuery()
            .processDefinitionKey(processDefinitionKey).list();

    for (ProcessInstance processInstance : list) {
        System.out.println("-----------------------------");
        System.out.println("流程实例id:" + processInstance.getProcessInstanceId());
        System.out.println("所属流程定义id:" + processInstance.getProcessDefinitionId());
        System.out.println("是否执行完成:" + processInstance.isEnded());
        System.out.println("是否暂停:" + processInstance.isSuspended());
    }
}
```





### 任务查询

流程启动后，任务的负责人就可以查询自己当前需要处理的任务，查询出来的任务都是该用户的待办任务。

```java
    //2.任务查询
    //流程启动后，任务的负责人就可以查询自己当前需要处理的任务，查询出来的任务都是该用户的待办任务。
    @Test
    public void testFindPersonalTaskList() {
        //任务负责人
        String assignee = "liuky";

        //根据流程key 和 任务负责人 查询任务
        List<Task> list = taskService.createTaskQuery()
                .processDefinitionKey("myProcess_1")
                .taskAssignee(assignee)
                .list();

        for (Task task : list) {

            System.out.println("流程实例id：" + task.getProcessInstanceId());
            System.out.println("任务id：" + task.getId());
            System.out.println("任务负责人：" + task.getAssignee());
            System.out.println("任务名称：" + task.getName());

        }

        //流程实例id：a9b162aa-ffda-11eb-bad1-02004c4f4f50
        //任务id：a9b5815e-ffda-11eb-bad1-02004c4f4f50
        //任务负责人：liuky
        //任务名称：提交申请

    }

```



### 完成任务



```java
    @Test
    public void completTask(){

        //根据流程key和任务的负责人查询任务并选择其中的一个任务处理,这里用的
        //是singleResult返回一条，真实环境中是通过步骤5中查询出所有的任务，然后在页面上选择一个任务进行处理.
        Task task = taskService.createTaskQuery()
                .processDefinitionKey("myProcess_1") //流程Key
                .taskAssignee("liuky")  //要查询的负责人
                .singleResult();
                
        //完成任务,参数：任务id
        taskService.complete(task.getId());

    }

```







## 流程变量

### 设置流程变量

流程变量可以在启动流程定义时把我们的对象作为流程变量设置进去，也可以使用setVariable设置

```java
    /**设置流程变量*/
	@Test
	public void setVariables(){
		/**与任务（正在执行）*/
		TaskService taskService = processEngine.getTaskService();
		//任务ID
		String taskId = "2104";
		/**一：设置流程变量，使用基本数据类型*/
//		taskService.setVariableLocal(taskId, "请假天数", 5);//与任务ID绑定
//		taskService.setVariable(taskId, "请假日期", new Date());
//		taskService.setVariable(taskId, "请假原因", "回家探亲，一起吃个饭");
		/**二：设置流程变量，使用javabean类型*/
		/**
		 * 当一个javabean（实现序列号）放置到流程变量中，要求javabean的属性不能再发生变化
		 *    * 如果发生变化，再获取的时候，抛出异常
		 *  
		 * 解决方案：在Person对象中添加：
		 * 		private static final long serialVersionUID = 6757393795687480331L;
		 *      同时实现Serializable 
		 * */
		Person p = new Person();
		p.setId(20);
		p.setName("翠花");
		taskService.setVariable(taskId, "人员信息(添加固定版本)", p);
		
		System.out.println("设置流程变量成功！");
	}
```

说明：

1)           流程变量的作用域就是流程实例，所以只要设置就行了，不用管在哪个阶段设置

2)           基本类型设置流程变量，在taskService中使用任务ID，定义流程变量的名称，设置流程变量的值。

3)           Javabean类型设置流程变量，需要这个javabean实现了Serializable接口

4)           设置流程变量的时候，向act_ru_variable这个表添加数据




### 获取流程变量



```java
        /**获取流程变量*/
	@Test
	public void getVariables(){
		/**与任务（正在执行）*/
		TaskService taskService = processEngine.getTaskService();
		//任务ID
		String taskId = "2104";
		/**一：获取流程变量，使用基本数据类型*/
//		Integer days = (Integer) taskService.getVariable(taskId, "请假天数");
//		Date date = (Date) taskService.getVariable(taskId, "请假日期");
//		String resean = (String) taskService.getVariable(taskId, "请假原因");
//		System.out.println("请假天数："+days);
//		System.out.println("请假日期："+date);
//		System.out.println("请假原因："+resean);
		/**二：获取流程变量，使用javabean类型*/
		Person p = (Person)taskService.getVariable(taskId, "人员信息(添加固定版本)");
		System.out.println(p.getId()+"        "+p.getName());
	}
```

说明：

1. 流程变量的获取针对流程实例（即1个流程），每个流程实例获取的流程变量时不同的

2. 使用基本类型获取流程变量，在taskService中使用任务ID，流程变量的名称，获取流程变量的值。

3. Javabean类型设置获取流程变量，除了需要这个javabean实现了Serializable接口外，还要求流程变量对象的属性不能发生变化，否则抛出异常。解决方案，固定序列化ID
   



### 流程控制中的流程变量



![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050559.png)

```java
 1     /**
 2      * 流程部署
 3      * `act_ge_bytearray` 流程定义的资源信息，包含bpmn和png流程文件信息
 4      * `act_re_deployment` 流程部署信息，包含流程名称,ID,Key等
 5      * `act_re_procdef` 流程定义信息
 6      */
 7     @Test
 8     public void deployment(){
 9         //获取ProcessEngine对象   默认配置文件名称：activiti.cfg.xml  并且configuration的Bean实例ID为processEngineConfiguration
10         ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
11         //获取RepositoryService对象进行流程部署
12         RepositoryService repositoryService = processEngine.getRepositoryService();
13         //进行部署,将对应的流程定义文件生成到数据库当中，作为记录进行保存
14         Deployment deployment = repositoryService.createDeployment()
15                 .addClasspathResource("flowchart/process.bpmn")     //加载流程文件
16                 .addClasspathResource("flowchart/process.png")
17                 .name("请假流程")       //设置流程名称
18                 .key("processKey")
19                 .deploy();              //部署
20 
21         //输出部署信息
22         System.out.println("流程名称："+deployment.getName());
23         System.out.println("流程ID："+deployment.getId());
24         System.out.println("流程Key："+deployment.getKey());
25     }

部署流程
```



```java
 1  /**
 2      * 启动一个流程实例
 3      */
 4     @Test
 5     public void getInstance(){
 6         //获取ProcessEngine对象   默认配置文件名称：activiti.cfg.xml  并且configuration的Bean实例ID为processEngineConfiguration
 7         ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
 8         //获取RuntimeService
 9         RuntimeService runtimeService = processEngine.getRuntimeService();
10         //定义一个Map集合，存放assignee的值
11         Map<String,Object> assMap=new HashMap<>();
12         assMap.put("assignee01","zhangsan");
13         assMap.put("assignee02","lisi");
14         assMap.put("assignee03","wangwu");
15         assMap.put("assignee04","zhaoliu");
16         //启动一个流程实例
17         ProcessInstance myProcess = runtimeService.startProcessInstanceByKey("myProcess", assMap);
18         System.out.println("流程实例Name:"+myProcess.getName());
19     }
```















## springboot整合activiti7



```java


/**
 * <p>Activiti控制器</p>
 * @author FRH
 * @time 2018年12月10日上午9:30:18
 * @version 1.0
 */
@Controller
@RequestMapping("/demo")
public class DemoController {

private static final Logger logger = LoggerFactory.getLogger(DemoController.class);
	
	/** 流程定义和部署相关的存储服务 */
	@Autowired
	private RepositoryService repositoryService;
	
	/** 流程运行时相关的服务 */
	@Autowired
	private RuntimeService runtimeService;
	
	/** 节点任务相关操作接口 */
	@Autowired
	private TaskService taskService;
	
	/** 流程图生成器 */
	@Autowired
	private ProcessDiagramGenerator processDiagramGenerator;

	/** 历史记录相关服务接口 */
	@Autowired
	private HistoryService historyService;

	
	
	/**
	 * <p>跳转到测试主页面</p>
	 * @return String 测试主页面
	 * @author FRH
	 * @time 2018年12月10日上午11:12:28
	 * @version 1.0
	 */
	@RequestMapping(value="/toIndex.html")
	public String toTestPage() {
		return "/index";
	}
	
	
	
	/**
	 * <p>跳转到上级审核页面</p>
	 * @return String 上级审核页面
	 * @author FRH
	 * @time 2018年12月5日下午2:31:42
	 * @version 1.0
	 */
	@RequestMapping(value="/toLeave")
	public String employeeLeave() {
		return "/employeeLeave";
	}
	
	
	
	/**
	 * <p>启动请假流程（流程key即xml中定义的ID为leaveProcess）</p>
	 * @return String 启动的流程ID
	 * @author FRH
	 * @time 2018年12月10日上午11:12:50
	 * @version 1.0
	 */
	@RequestMapping(value="/start")
	@ResponseBody
	public String start() {
		/*
		 *  xml中定义的ID
		 */
		String instanceKey = "leaveProcess";
		logger.info("开启请假流程...");
		
		
		/*
		 *  设置流程参数，开启流程
		 */
		Map<String,Object> map = new HashMap<String,Object>();
        map.put("jobNumber","A1001");
        map.put("busData","bus data");
		ProcessInstance instance = runtimeService.startProcessInstanceByKey(instanceKey, map);//使用流程定义的key启动流程实例，key对应helloworld.bpmn文件中id的属性值，使用key值启动，默认是按照最新版本的流程定义启动
		
		logger.info("启动流程实例成功:{}", instance);
		logger.info("流程实例ID:{}", instance.getId());
		logger.info("流程定义ID:{}", instance.getProcessDefinitionId());
		
		
		/*
		 * 验证是否启动成功
		 */
	    //通过查询正在运行的流程实例来判断
	    ProcessInstanceQuery processInstanceQuery = runtimeService.createProcessInstanceQuery();
	    //根据流程实例ID来查询
	    List<ProcessInstance> runningList = processInstanceQuery.processInstanceId(instance.getProcessInstanceId()).list();
	    logger.info("根据流程ID查询条数:{}", runningList.size());
		
	    
	    /*
	     *  返回流程ID
	     */
		return instance.getId();
	}
	
	
	
	/**
	 * <p>查看当前流程图</p>
	 * @param instanceId 流程实例
	 * @param response void 响应
	 * @author FRH
	 * @time 2018年12月10日上午11:14:12
	 * @version 1.0
	 */
	@ResponseBody
	@RequestMapping(value="/showImg")
	public void showImg(String instanceId, HttpServletResponse response) {
		/*
		 * 参数校验
		 */
		logger.info("查看完整流程图！流程实例ID:{}", instanceId);
		if(StringUtils.isBlank(instanceId)) return;
		
		
		/*
		 *  获取流程实例
		 */
		HistoricProcessInstance processInstance = historyService.createHistoricProcessInstanceQuery().processInstanceId(instanceId).singleResult();
		if(processInstance == null) {
			logger.error("流程实例ID:{}没查询到流程实例！", instanceId);
			return;
		}
		
		// 根据流程对象获取流程对象模型
		BpmnModel bpmnModel = repositoryService.getBpmnModel(processInstance.getProcessDefinitionId());
		
		
		/*
		 *  查看已执行的节点集合
		 *  获取流程历史中已执行节点，并按照节点在流程中执行先后顺序排序
		 */
		// 构造历史流程查询
		HistoricActivityInstanceQuery historyInstanceQuery = historyService.createHistoricActivityInstanceQuery().processInstanceId(instanceId);
		// 查询历史节点
		List<HistoricActivityInstance> historicActivityInstanceList = historyInstanceQuery.orderByHistoricActivityInstanceStartTime().asc().list();
		if(historicActivityInstanceList == null || historicActivityInstanceList.size() == 0) {
			logger.info("流程实例ID:{}没有历史节点信息！", instanceId);
			outputImg(response, bpmnModel, null, null);
			return;
		}
		// 已执行的节点ID集合(将historicActivityInstanceList中元素的activityId字段取出封装到executedActivityIdList)
		List<String> executedActivityIdList = historicActivityInstanceList.stream().map(item -> item.getActivityId()).collect(Collectors.toList());
		
		/*
		 *  获取流程走过的线
		 */
		// 获取流程定义
		ProcessDefinitionEntity processDefinition = (ProcessDefinitionEntity) ((RepositoryServiceImpl) repositoryService).getDeployedProcessDefinition(processInstance.getProcessDefinitionId());
		List<String> flowIds = ActivitiUtils.getHighLightedFlows(bpmnModel, processDefinition, historicActivityInstanceList);
		
		
		/*
		 * 输出图像，并设置高亮
		 */
		outputImg(response, bpmnModel, flowIds, executedActivityIdList);
	}
	


	/**
	 * <p>员工提交申请</p>
	 * @param request 请求
	 * @return String 申请受理结果
	 * @author FRH
	 * @time 2018年12月10日上午11:15:09
	 * @version 1.0
	 */
	@RequestMapping(value="/employeeApply")
	@ResponseBody
	public String employeeApply(HttpServletRequest request){
		/*
		 * 获取请求参数
		 */
		String taskId = request.getParameter("taskId"); // 任务ID
		String jobNumber = request.getParameter("jobNumber"); // 工号
		String leaveDays = request.getParameter("leaveDays"); // 请假天数
		String leaveReason = request.getParameter("leaveReason"); // 请假原因
		
		
		/*
		 *  查询任务
		 */
		Task task = taskService.createTaskQuery().taskId(taskId).singleResult();
		if(task == null) {
			logger.info("任务ID:{}查询到任务为空！", taskId);
			return "fail";
		}

		
		/*
		 * 参数传递并提交申请
		 */
        Map<String, Object> map = new HashMap<String, Object>();
        map.put("days", leaveDays);
        map.put("date", new Date());
        map.put("reason", leaveReason);
        map.put("jobNumber", jobNumber);
        taskService.complete(task.getId(), map);
        logger.info("执行【员工申请】环节，流程推动到【上级审核】环节");
        
        /*
         * 返回成功
         */
		return "success";
	}
	
	
	/**
	 * <p>跳转到上级审核页面</p>
	 * @return String 页面
	 * @author FRH
	 * @time 2018年12月5日下午2:31:42
	 * @version 1.0
	 */
	@RequestMapping(value="/viewTask")
	public String toHigherAudit(String taskId, HttpServletRequest request) {
		/*
		 * 获取参数
		 */
		logger.info("跳转到任务详情页面，任务ID:{}", taskId);
		if(StringUtils.isBlank(taskId)) return "/higherAudit";
		
		
		/*
		 *  查看任务详细信息
		 */
		Task task = taskService.createTaskQuery().taskId(taskId).singleResult();
		if(task == null) {
			logger.info("任务ID:{}不存在！", taskId);
			return "/higherAudit";
		}
		
		
		/*
		 * 完成任务
		 */
		Map<String, Object> paramMap = taskService.getVariables(taskId);
		request.setAttribute("task", task);
		request.setAttribute("paramMap", paramMap);
		return "higherAudit";
	}
	
	
	
	/**
	 * <p>跳转到部门经理审核页面</p>
	 * @param taskId 任务ID
	 * @param request 请求
	 * @return String 响应页面
	 * @author FRH
	 * @time 2018年12月6日上午9:54:34
	 * @version 1.0
	 */
	@RequestMapping(value="/viewTaskManager")
	public String viewTaskManager(String taskId, HttpServletRequest request) {
		/*
		 * 获取参数
		 */
		logger.info("跳转到任务详情页面，任务ID:{}", taskId);
		if(StringUtils.isBlank(taskId)) return "/manageAudit";
		
		
		/*
		 *  查看任务详细信息
		 */
		Task task = taskService.createTaskQuery().taskId(taskId).singleResult();
		if(task == null) {
			logger.info("任务ID:{}不存在！", taskId);
			return "/manageAudit";
		}
		
		
		/*
		 * 完成任务
		 */
		Map<String, Object> paramMap = taskService.getVariables(taskId);
		request.setAttribute("task", task);
		request.setAttribute("paramMap", paramMap);
		return "manageAudit";
	}
	
	

	/**
	 * <p>上级审核</p>
	 * @param request 请求
	 * @return String 受理结果
	 * @author FRH
	 * @time 2018年12月10日上午11:19:44
	 * @version 1.0
	 */
	@ResponseBody
	@RequestMapping(value="/higherLevelAudit")
	public String higherLevelAudit(HttpServletRequest request) {
		/*
		 * 获取请求参数
		 */
		String taskId = request.getParameter("taskId");
		String higherLevelOpinion = request.getParameter("sug");
		String auditStr = request.getParameter("audit");
		logger.info("上级审核任务ID:{}", taskId);
		if(StringUtils.isBlank(taskId)) return "fail";

		
		/*
		 * 查找任务
		 */
		Task task = taskService.createTaskQuery().taskId(taskId).singleResult();
		if(task == null) {
			logger.info("审核任务ID:{}查询到任务为空！", taskId);
			return "fail";
		}
		
		
		/*
		 * 设置局部变量参数，完成任务
		 */
		Map<String, Object> map = new HashMap<String, Object>();
		map.put("audit", "1".equals(auditStr) ? false : true);
		map.put("higherLevelOpinion", higherLevelOpinion);
		taskService.complete(taskId, map);
		return "success";
	}
	
	
	
	/**
	 * <p>部门经理审核</p>
	 * @param request 请求
	 * @return String 受理结果
	 * @author FRH
	 * @time 2018年12月10日上午11:20:44
	 * @version 1.0
	 */
	@ResponseBody
	@RequestMapping(value="/divisionManagerAudit")
	public String divisionManagerAudit(HttpServletRequest request) {
		/*
		 * 获取请求参数
		 */
		String taskId = request.getParameter("taskId");
		String opinion = request.getParameter("sug");
		String auditStr = request.getParameter("audit");
		logger.info("上级审核任务ID:{}", taskId);
		if(StringUtils.isBlank(taskId)) return "fail";

		
		/*
		 * 查找任务
		 */
		Task task = taskService.createTaskQuery().taskId(taskId).singleResult();
		if(task == null) {
			logger.info("审核任务ID:{}查询到任务为空！", taskId);
			return "fail";
		}
		
		
		/*
		 * 设置局部变量参数，完成任务
		 */
		Map<String, Object> map = new HashMap<String, Object>();
		map.put("audit", "1".equals(auditStr) ? false : true);
		map.put("managerOpinion", opinion);
		taskService.complete(taskId, map);
		return "success";
	}
	
	
	/**
	 * <p>查看任务</p>
	 * @param request 请求
	 * @return String  任务展示页面
	 * @author FRH
	 * @time 2018年12月10日上午11:21:33
	 * @version 1.0
	 */
	@RequestMapping(value="/toShowTask")
	public String toShowTask(HttpServletRequest request) {
		/*
		 * 获取请求参数
		 */
		List<Task> taskList = taskService.createTaskQuery().list();
		if(taskList == null || taskList.size() == 0) {
			logger.info("查询任务列表为空！");
			return "/task";
		}
		
		
		/*
		 * 查询所有任务，并封装
		 */
		List<Map<String, String>> resultList = new ArrayList<Map<String, String>>();
		for(Task task : taskList) {
			Map<String, String> map = new HashMap<String, String>();
			map.put("taskId", task.getId());
			map.put("name", task.getName());
			map.put("createTime", task.getCreateTime().toString());
			map.put("assignee", task.getAssignee());
			map.put("instanceId", task.getProcessInstanceId());
			map.put("executionId", task.getExecutionId());
			map.put("definitionId", task.getProcessDefinitionId());
			resultList.add(map);
		}
		
		
		/*
		 * 返回结果
		 */
		logger.info("返回集合:{}", resultList.toString());
		request.setAttribute("resultList", resultList);
		return "/task";
	}
	


	/**
	 * <p>输出图像</p>
	 * @param response 响应实体
	 * @param bpmnModel 图像对象
	 * @param flowIds 已执行的线集合
	 * @param executedActivityIdList void 已执行的节点ID集合
	 * @author FRH
	 * @time 2018年12月10日上午11:23:01
	 * @version 1.0
	 */
	private void outputImg(HttpServletResponse response, BpmnModel bpmnModel, List<String> flowIds, List<String> executedActivityIdList) {
		InputStream imageStream = null;
		try {
			imageStream = processDiagramGenerator.generateDiagram(bpmnModel, executedActivityIdList, flowIds, "宋体", "微软雅黑", "黑体", true, "png");
			// 输出资源内容到相应对象
			byte[] b = new byte[1024];
			int len;
			while ((len = imageStream.read(b, 0, 1024)) != -1) {
				response.getOutputStream().write(b, 0, len);
			}
			response.getOutputStream().flush();
		}catch(Exception e) {
			logger.error("流程图输出异常！", e);
		} finally { // 流关闭
			StreamUtils.closeInputStream(imageStream);
		}
	}
	


	/**
	 * <p>判断流程是否完成</p>
	 * @param processInstanceId 流程实例ID
	 * @return boolean 已完成-true，未完成-false
	 * @author FRH
	 * @time 2018年12月10日上午11:23:26
	 * @version 1.0
	 */
	public boolean isFinished(String processInstanceId) {
        return historyService.createHistoricProcessInstanceQuery().finished().processInstanceId(processInstanceId).count() > 0;
    }
	
}


```





















# TomCat

## 架构

![这里写图片描述](https://raw.githubusercontent.com/a1254898873/images/master/202203241050560.awebp)



Tomcat中最顶层的容器是Server，代表着整个服务器，从上图中可以看出，一个Server可以包含至少一个Service，用于具体提供服务。
    Service主要包含两个部分：Connector和Container。从上图中可以看出 Tomcat 的心脏就是这两个组件，他们的作用如下：

1. Connector用于处理连接相关的事情，并提供Socket与Request和Response相关的转化; 
2. Container用于封装和管理Servlet，以及具体处理Request请求；



一个Tomcat中只有一个Server，一个Server可以包含多个Service，一个Service只有一个Container，但是可以有多个Connectors，这是因为一个服务可以有多个连接，如同时提供Http和Https链接，也可以提供向相同协议不同端口的连接,示意图如下（Engine、Host、Context下边会说到）：

![这里写图片描述](https://raw.githubusercontent.com/a1254898873/images/master/202203241050561.awebp)



多个 Connector 和一个 Container 就形成了一个 Service，有了 Service 就可以对外提供服务了，但是 Service 还要一个生存的环境，必须要有人能够给她生命、掌握其生死大权，那就非 Server 莫属了！所以整个 Tomcat 的生命周期由 Server 控制。





## Connector

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050562.png)

![这里写图片描述](https://raw.githubusercontent.com/a1254898873/images/master/202203241050563.awebp)



Connector就是使用ProtocolHandler来处理请求的，不同的ProtocolHandler代表不同的连接类型。
    其中ProtocolHandler由包含了三个部件：Endpoint、Processor、Adapter。
    （1）Endpoint用来处理底层Socket的网络连接，Processor用于将Endpoint接收到的Socket封装成Request，Adapter用于将Request交给Container进行具体的处理。
    （2）Endpoint由于是处理底层的Socket网络连接，因此Endpoint是用来实现TCP/IP协议的，而Processor用来实现HTTP协议的，Adapter将请求适配到Servlet容器进行具体的处理。
    （3）Endpoint的抽象实现AbstractEndpoint里面定义的Acceptor和AsyncTimeout两个内部类和一个Handler接口。Acceptor用于监听请求，AsyncTimeout用于检查异步Request的超时，Handler用于处理接收到的Socket，在内部调用Processor进行处理。





## TomCat与servlet

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050564.awebp)

Servlet：Servlet 是一个基于 Java 技术开发的 web 组件，它运行在服务器端，由 Servlet容器管理。主要用于处理用户的请求并生成动态的信息。

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050565.awebp)

Tomcat：是一个Servlet容器。Servlet容器也叫做Servlet引擎，是Web服务器或应用程序服务器的一部分，用于在发送的请求和响应之上提供网络服务，解码基于 MIME的请求，格式化基于MIME的响应。Servlet没有main方法，不能独立运行，它必须被部署到Servlet容器中，由容器来实例化和调用 Servlet的方法（如doGet()和doPost()），Servlet容器在Servlet的生命周期内包容和管理Servlet。

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050566.jpg)

处理过程：

1）客户端（通常都是浏览器）访问Web服务器，发送HTTP请求。
2）Web服务器接收到请求后，传递给Servlet容器。
3）Servlet容器加载Servlet，产生Servlet实例后，向其传递表示请求和响应的对象。
4）Servlet实例使用请求对象得到客户端的请求信息，然后进行相应的处理。
5）Servlet实例将处理结果通过响应对象发送回客户端，容器负责确保响应正确送出，同时将控制返回给Web服务器。









## 线程池

Tomcat作为一个web容器，是以一个进程的形式运行的；当一个请求到达后，Tomcat就会创建一个线程来处理，请求处理完成后再把线程销毁掉。

这意味着在一个程序运行过程中，需要多次创建并销毁线程，创建并销毁线程的过程势必会消耗内存。为了节省内存资源的消耗，就提出了线程池的概念。

![Java面试必考问题：Tomcat为啥不用Java原生线程池](https://raw.githubusercontent.com/a1254898873/images/master/202203241050567.png)

1. 每次提交任务时，如果线程数还没达到coreSize就创建新线程并绑定该任务。所以第coreSize次提交任务后线程总数必达到coreSize，不会重用之前的空闲线程。
2. 线程数达到coreSize后，新增的任务就放到工作队列里，而线程池里的线程则努力的使用take()从工作队列里拉活来干。
3. 如果队列是个有界队列，又如果线程池里的线程不能及时将任务取走，工作队列可能会满掉，插入任务就会失败，此时线程池就会紧急的再创建新的临时线程来补救。
4. 临时线程使用poll(keepAliveTime，timeUnit)来从工作队列拉活，如果时候到了仍然两手空空没拉到活，表明它太闲了，就会被解雇掉。
5. 如果core线程数＋临时线程数 >maxSize，则不能再创建新的临时线程了，转头执行RejectExecutionHanlder。默认的AbortPolicy抛RejectedExecutionException异常，其他选择包括静默放弃当前任务(Discard)，放弃工作队列里最老的任务(DisacardOldest)，或由主线程来直接执行(CallerRuns).



![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050568.png)

 tomcat线程池有如下参数：

   maxThreads：最大线程数，tomcat能创建来处理请求的最大线程数

   maxSpareTHreads,：最大空闲线程数，在最大空闲时间内活跃过，但现在处于空闲，若空闲时间大于最大空闲时   间，则回收，小于则继续存活，等待被调度。

   minSpareTHreads：最小空闲线程数，无论如何都会存活的最小线程数

   acceptCount,：最大等待队列数 ，请求并发大于tomcat线程池的处理能力，则被放入等待队列等待被处理。

   maxIdleTime,：最大空闲时间，超过这个空闲时间，且线程数大于最小空闲数的，都会被回收   







# Ngnix

## 配置结构



```
main        # 全局配置，对全局生效
├── events  # 配置影响 Nginx 服务器或与用户的网络连接
├── http    # 配置代理，缓存，日志定义等绝大多数功能和第三方模块的配置
│   ├── upstream # 配置后端服务器具体地址，负载均衡配置不可或缺的部分
│   ├── server   # 配置虚拟主机的相关参数，一个 http 块中可以有多个 server 块
│   ├── server
│   │   ├── location  # server 块可以包含多个 location 块，location 指令用于匹配 uri
│   │   ├── location
│   │   └── ...
│   └── ...
└── ...

```

1. 配置文件由指令与指令块构成；

2. 每条指令以 ; 分号结尾，指令与参数间以空格符号分隔；
3. 指令块以 {} 大括号将多条指令组织在一起；
4. include 语句允许组合多个配置文件以提升可维护性；
5. 使用 # 符号添加注释，提高可读性；
6. 使用 $ 符号使用变量；
7. 部分指令的参数支持正则表达式；



## location语法

注意：~ 代表自己输入的英文字母

| 匹配符 | 匹配规则                     | 优先级 |
| ------ | ---------------------------- | ------ |
| =      | 精确匹配                     | 1      |
| ^~     | 以某个字符串开头             | 2      |
| ~      | 区分大小写的正则匹配         | 3      |
| ~*     | 不区分大小写的正则匹配       | 4      |
| !~     | 区分大小写不匹配的正则       | 5      |
| !~*    | 不区分大小写不匹配的正则     | 6      |
| /      | 通用匹配，任何请求都会匹配到 | 7      |

```
    #优先级1,精确匹配，根路径
    location =/ {
        return 400;
    }

    #优先级2,以某个字符串开头,以av开头的，优先匹配这里，区分大小写
    location ^~ /av {
       root /data/av/;
    }

    #优先级3，区分大小写的正则匹配，匹配/media*****路径
    location ~ /media {
          alias /data/static/;
    }

    #优先级4 ，不区分大小写的正则匹配，所有的****.jpg|gif|png 都走这里
    location ~* .*\.(jpg|gif|png|js|css)$ {
       root  /data/av/;
    }

    #优先7，通用匹配
    location / {
        return 403;
    }

```













## 典型配置

```
user  nginx;                        # 运行用户，默认即是nginx，可以不进行设置
worker_processes  1;                # Nginx 进程数，一般设置为和 CPU 核数一样
error_log  /var/log/nginx/error.log warn;   # Nginx 的错误日志存放目录
pid        /var/run/nginx.pid;      # Nginx 服务启动时的 pid 存放位置

events {
    use epoll;     # 使用epoll的I/O模型(如果你不知道Nginx该使用哪种轮询方法，会自动选择一个最适合你操作系统的)
    worker_connections 1024;   # 每个进程允许最大并发数
}

http {   # 配置使用最频繁的部分，代理、缓存、日志定义等绝大多数功能和第三方模块的配置都在这里设置
    # 设置日志模式
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;   # Nginx访问日志存放位置

    sendfile            on;   # 开启高效传输模式
    tcp_nopush          on;   # 减少网络报文段的数量
    tcp_nodelay         on;
    keepalive_timeout   65;   # 保持连接的时间，也叫超时时间，单位秒
    types_hash_max_size 2048;

    include             /etc/nginx/mime.types;      # 文件扩展名与类型映射表
    default_type        application/octet-stream;   # 默认文件类型

    include /etc/nginx/conf.d/*.conf;   # 加载子配置项
    
    server {
    	listen       80;       # 配置监听的端口
    	server_name  localhost;    # 配置的域名
    	
    	location / {
    		root   /usr/share/nginx/html;  # 网站根目录
    		index  index.html index.htm;   # 默认首页文件
    		deny 172.168.22.11;   # 禁止访问的ip地址，可以为all
    		allow 172.168.33.44； # 允许访问的ip地址，可以为all
    	}
    	
    	error_page 500 502 503 504 /50x.html;  # 默认50x对应的访问页面
    	error_page 400 404 error.html;   # 同上
    }
}

```

server中的listen为虚拟地址的端口

server_name 自己定

location 一般用  ~ /xx/

```
server {
        listen       80;
        server_name  localhost;

		#前端页面
        location / {
			#Linux上的前端工程打包目录
			root   /usr/local/dt/dist; 
			#防止重定向页面刷新 路由失效
			try_files $uri $uri/ /index.html;
			index index.html index.htm;
        }
		
		#后台接口地址
	    location /api/{
			 proxy_set_header Host $host;
			 proxy_set_header X-Real-IP $remote_addr;
			 proxy_set_header REMOTE-HOST $remote_addr;
			 proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			 #填写你接口地址和端口，利用 nginx 反向代理进行服务转发
			 proxy_pass  http://localhost:9090/api/;
		}
    }

```







## 反向代理



```
server {
  listen 9001;
  server_name *.sherlocked93.club;

  location ~ /edu/ {
    proxy_pass http://127.0.0.1:8080;
  }
  
  location ~ /vod/ {
    proxy_pass http://127.0.0.1:8081;
  }
}
```



## 解决跨域问题

在前端服务地址为 fe.sherlocked93.club 的页面请求 be.sherlocked93.club 的后端服务导致的跨域，可以这样配置：

```
server {
  listen 9001;
  server_name fe.sherlocked93.club;

  location / {
    proxy_pass be.sherlocked93.club;
  }
}

```



这样就将对前一个域名 fe.sherlocked93.club 的请求全都代理到了 be.sherlocked93.club，前端的请求都被我们用服务器代理到了后端地址下，绕过了跨域。
这里对静态文件的请求和后端服务的请求都以 fe.sherlocked93.club 开始，不易区分，所以为了实现对后端服务请求的统一转发，通常我们会约定对后端服务的请求加上 /apis/ 前缀或者其他的 path 来和对静态资源的请求加以区分，此时我们可以这样配置：



```
# 请求跨域，约定代理后端服务请求path以/apis/开头
location ^~/apis/ {
    # 这里重写了请求，将正则匹配中的第一个分组的path拼接到真正的请求后面，并用break停止后续匹配
    rewrite ^/apis/(.*)$ /$1 break;
    proxy_pass be.sherlocked93.club;
  
    # 两个域名之间cookie的传递与回写
    proxy_cookie_domain be.sherlocked93.club fe.sherlocked93.club;
}

```

这样，静态资源我们使用 fe.sherlocked93.club/xx.html，动态资源我们使用 fe.sherlocked93.club/apis/getAwo，浏览器页面看起来仍然访问的前端服务器，绕过了浏览器的同源策略，毕竟我们看起来并没有跨域。



## 负载均衡

    upstream xx {   
    
    }

```
    upstream myserver {   
      server 192.167.4.32:5000;
      server 192.168.4.32:8080;
    }
    

    server {
        listen       80;   #监听端口
        server_name  192.168.4.32;   #监听地址
   
        location  / {       
           root html;  #html目录
           index index.html index.htm;  #设置默认页
           proxy_pass  http://myserver;  #请求转向 myserver 定义的服务器列表      
        } 
    }

```

（1）轮询

默认设置，按请求的时间顺序依次逐一分配，如果服务器down掉，能自动剔除。



（2）权重

weight 越高，被分配的客户端越多，默认为 1。比如：

```
      upstream myserver {   
        server 192.167.4.32:5000 weight=10;
        server 192.168.4.32:8080 weight=5;
      }

```



（3）ip

 按请求 ip 的 hash 值分配，每个访客固定访问一个后端服务器。比如：

```
      upstream myserver { 
        ip_hash;  
        server 192.167.4.32:5000;
        server 192.168.4.32:8080;
      }

```



（4）fair

按后端服务器的响应时间来分配，响应时间短的优先分配到请求。比如：

```
      upstream myserver { 
        fair;  
        server 192.168.4.32:5000;
        server 192.168.4.32:8080;
      }

```







# minIO

## 安装

创建目录：一个用来存放配置，一个用来存储上传文件的目录。

```
mkdir -p /data/minio/config
mkdir -p /data/minio/data
```



```
docker run -p 9010:9010 -p 9020:9020 \
 --name minio \
 -d --restart=always \
 -e "MINIO_ACCESS_KEY=minioadmin" \
 -e "MINIO_SECRET_KEY=minioadmin" \
 -v /data/minio/data:/data \
 -v /data/minio/config:/root/.minio \
 minio/minio server \
 /data --console-address ":9020" -address ":9010"
```





## 整合springboot

要使用 MinIO 的 SDK，得导入其依赖包。为了方便操作 I/O流，我们同时导入一个工具包：

```
<!-- MinIO SDK-->
<dependency>
    <groupId>io.minio</groupId>
    <artifactId>minio</artifactId>
    <version>8.0.3</version>
</dependency>
<!-- I/O工具包，方便I/O操作-->
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.7</version>
</dependency>
```

关于 MinIO 的一切操作都得通过 MinioClient 对象来进行，创建该对象的方式如下：

```
MinioClient minioClient = MinioClient.builder()
                .endpoint("服务地址")
                .credentials("账号", "密码")
                .build();
```



yml配置

```
minio:
  url: http://rudecrab.com:9000 # 服务地址
  access: minioadmin # 账号
  secret: minioadmin # 密码
  bucket: file # Bucket

```



service类

```java
@Service
public class MinioService {
    Logger log = LoggerFactory.getLogger(MinioService.class);

    private final String bucket;
    private final MinioClient minioClient;

    public MinioService(@Value("${minio.url}") String url,
                        @Value("${minio.access}") String access,
                        @Value("${minio.secret}") String secret,
                        @Value("${minio.bucket}") String bucket) throws Exception {
        this.bucket = bucket;
        minioClient = MinioClient.builder()
                .endpoint(url)
                .credentials(access, secret)
                .build();
        // 初始化Bucket
        initBucket();
    }

    private void initBucket() throws Exception {
        // 应用启动时检测Bucket是否存在
        boolean found = minioClient.bucketExists(BucketExistsArgs.builder().bucket(bucket).build());
        // 如果Bucket不存在，则创建Bucket
        if (!found) {
            minioClient.makeBucket(MakeBucketArgs.builder().bucket(bucket).build());
            log.info("成功创建 Bucket [{}]", bucket);
        }
    }

    /**
     * 上传文件
     * @param is 输入流
     * @param object 对象（文件）名
     * @param contentType 文件类型
     */
    public void putObject(InputStream is, String object, String contentType) throws Exception {
        long start = System.currentTimeMillis();
        minioClient.putObject(PutObjectArgs.builder()
                .bucket(bucket)
                .object(object)
                .contentType(contentType)
                .stream(is, -1, 1024 * 1024 * 10) // 不得小于 5 Mib
                .build());
        log.info("成功上传文件至云端 [{}]，耗时 [{} ms]", object, System.currentTimeMillis() - start);
    }

    /**
     * 获取文件流
     * @param object 对象（文件）名
     * @return 文件流
     */
    public GetObjectResponse getObject(String object) throws Exception {
        long start = System.currentTimeMillis();
        GetObjectResponse response = minioClient.getObject(GetObjectArgs.builder()
                .bucket(bucket)
                .object(object)
                .build());
        log.info("成功获取 Object [{}]，耗时 [{} ms]", object, System.currentTimeMillis() - start);
        return response;
    }

    /**
     * 删除对象（文件）
     * @param object 对象（文件名）
     */
    public void removeObject(String object) throws Exception {
        minioClient.removeObject(RemoveObjectArgs.builder()
                .bucket(bucket)
                .object(object)
                .build());
        log.info("成功删除 Object [{}]", object);
    }
}
```





控制器：

```java
@RestController
@RequestMapping("/file")
public class FileController {
    @Autowired
    private MinioService minioService;

    // 上传，上传成功会返回文件名
    @PostMapping
    public String upload(MultipartFile file) throws Exception {
        // 获取文件后缀名
        String extension = FilenameUtils.getExtension(file.getOriginalFilename());
        // 为了避免文件名重复，使用UUID重命名文件，将横杠去掉
        String fileName = UUID.randomUUID().toString().replace("-", "") + "." + extension;
        // 上传
        minioService.putObject(file.getInputStream(), fileName, file.getContentType());
        // 返回文件名
        return fileName;
    }
 
    // 根据文件名下载文件
    @GetMapping("{fileName}")
    public void download(HttpServletRequest request, HttpServletResponse response, @PathVariable("fileName") String fileName) throws Exception  {
        // 设置响应类型
        response.setCharacterEncoding(request.getCharacterEncoding());
        response.setContentType("application/octet-stream");
        response.setHeader("Content-Disposition", "attachment; filename=" + fileName);
        // 获取文件流
        GetObjectResponse objectResponse = minioService.getObject(fileName);
        // 将文件流输出到响应流
        IOUtils.copy(objectResponse, response.getOutputStream());
        // 结束
        response.flushBuffer();
        objectResponse.close();
    }
 
    // 根据文件名删除文件
    @DeleteMapping("{fileName}")
    public String remove(@PathVariable("fileName") String fileName) throws Exception  {
        minioService.removeObject(fileName);
        return "success";
    }
}
```





```java
public enum ViewContentType {
    DEFAULT("default","application/octet-stream"),
    JPG("jpg", "image/jpeg"),
    TIFF("tiff", "image/tiff"),
    GIF("gif", "image/gif"),
    JFIF("jfif", "image/jpeg"),
    PNG("png", "image/png"),
    TIF("tif", "image/tiff"),
    ICO("ico", "image/x-icon"),
    JPEG("jpeg", "image/jpeg"),
    WBMP("wbmp", "image/vnd.wap.wbmp"),
    FAX("fax", "image/fax"),
    NET("net", "image/pnetvue"),
    JPE("jpe", "image/jpeg"),
    RP("rp", "image/vnd.rn-realpix");

    private String prefix;

    private String type;

    public static String getContentType(String prefix){
        if(StrUtil.isEmpty(prefix)){
            return DEFAULT.getType();
        }
        prefix = prefix.substring(prefix.lastIndexOf(".") + 1);
        for (ViewContentType value : ViewContentType.values()) {
            if(prefix.equalsIgnoreCase(value.getPrefix())){
                return value.getType();
            }
        }
        return DEFAULT.getType();
    }

    ViewContentType(String prefix, String type) {
        this.prefix = prefix;
        this.type = type;
    }

    public String getPrefix() {
        return prefix;
    }

    public String getType() {
        return type;
    }
}
```







# Docker

## ubuntu安装docker

```
sudo apt-get install docker.io
```



## 基本命令

![21](https://raw.githubusercontent.com/a1254898873/images/master/202203241050569.png)

```
docker <object> <command> <options>
```

1.3版本之后使用该格式命令

使用以下语法：

- object 表示将要操作的 Docker 对象的类型。这可以是 container、image、network 或者 volume 对象。
- command 表示守护程序要执行的任务，即 run 命令。
- options 可以是任何可以覆盖命令默认行为的有效参数，例如端口映射的 --publish 选项



（1）镜像

拉取镜像

```
docker image pull xx
```

查看本地镜像

```
docker image ls

docker images
```

删除本地镜像

```
docker image rm 镜像的标识
```

镜像的导入导出

```
# 将本地的镜像导出
docker save -o 导出的路径 镜像id
# 加载本地的镜像文件
docker load -i 镜像文件
```

镜像的运行

```
docker container run -d -p  宿主机端口:容器端口 --name 容器名称 镜像的标识|镜像名称[tag]
# -d: 代表后台运行容器
# -p: 宿主机端口:容器端口: 为了映射当前Linux的端口和容器的端口
# -v: 宿主机目录:容器目录
# -e 设置环境变量
# --name 容器名称: 指定容器的名称
```



（2）容器

查看正在运行的容器

```
docker container ls [OPTIONS]
如果使用 -a 标记，还可以看到处于停止（Exited）状态的容器。
docker container inspect
可以看到内网地址
```

进入容器内部

```
docker container  exec -it 容器id bash
```

停止容器

```
docker container  stop 容器id
```

重启容器

```
docker container start 容器id
```

删除容器（删除容器前要停止容器）

```
docker container rm 容器id
```



（3）仓库

登录

```
docker login
```

退出

```
docker logout
```

推送镜像

```
docker push username/ubuntu:18.04
```





（4）网络

当你安装Docker时，它会自动创建三个网络。你可以使用以下docker network ls命令列出这些网络：

```
[root@server1 ~]# docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
0147b8d16c64        bridge              bridge              local
2da931af3f0b        host                host                local
63d31338bcd9        none                null                local

```

默认是bridge

将容器连接到网络

```
docker network connect <network identifier> <container identifier>
```



容器之间相互通信

1、新建网络

```
docker network create -d bridge my-net
```

2、创建容器并指定网络

```
docker run -it --rm --name busybox1 --network my-net busybox sh
```

```
docker run -it --rm --name busybox2 --network my-net busybox sh
```







## Dockerfile

（1）构建步骤：

1. 编写一个 dockerfile  文件
2. docker build 构建成为一个镜像
3. docker run 运行镜像
4. docker push 发布镜像（咱们可以发布到 DockerHub，也可以发布到阿里云上面）



（2）注意事项

1. 每个 DockerFile 的保留字（指令），都必须是大写的
2. DockerFile 脚本执行是按照顺序执行的
3. #表示注释
4. 每一个指令都会创建提交一个新的镜像层，并提交



（3）指令

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050570.awebp)

- FROM

基础的镜像，一切都是从这里开始的

- MAINTAINER

指明镜像是谁写的，写下自己的姓名和邮箱

- RUN

镜像构建的时候需要运行的命令

- ADD

加入某些配置，例如加入 mysql 的压缩包，添加内容

- WORKDIR

镜像的工作目录

- VOLUME

挂载目录

- EXPOSE

暴露端口 和` -p` 是一个效果

- CMD

指定这个容器启动的时候执行的命令，只会是最优一个指令进行生效，会被替代

- ENTRYPOINT

指定这个容器启动的时候执行的命令，可以追加

- ONBUILD

当构建一个被继承的 DockerFIle ，这个时候就会运行 ONBUILD 的指令，触发相应的动作

- COPY

与 ADD 类似，此命令是将文件拷贝到镜像中

- ENV

构建的时候设置环境变量



使用docker部署springboot

```
# 该镜像需要依赖的基础镜像
FROM java:8
# 将当前目录下的jar包复制到docker容器的/目录下
ADD mall-tiny-docker-file-0.0.1-SNAPSHOT.jar /mall-tiny-docker-file.jar
# 运行过程中创建一个mall-tiny-docker-file.jar文件
RUN bash -c 'touch /mall-tiny-docker-file.jar'
# 声明服务运行在8080端口
EXPOSE 8080
# 指定docker容器启动时运行jar包
ENTRYPOINT ["java", "-jar","/mall-tiny-docker-file.jar"]
# 指定维护者的名字
MAINTAINER macrozheng

```











# HuTool

## Convert



```
//转换为字符串
int a = 1;
String aStr = Convert.toStr(a);
//转换为指定类型数组
String[] b = {"1", "2", "3", "4"};
Integer[] bArr = Convert.toIntArray(b);
//转换为日期对象
String dateStr = "2017-05-06";
Date date = Convert.toDate(dateStr);
//转换为列表
String[] strArr = {"a", "b", "c", "d"};
List<String> strList = Convert.toList(String.class, strArr);

```



## DateUtil



```
//Date、long、Calendar之间的相互转换
//当前时间
Date date = DateUtil.date();
//Calendar转Date
date = DateUtil.date(Calendar.getInstance());
//时间戳转Date
date = DateUtil.date(System.currentTimeMillis());
//自动识别格式转换
String dateStr = "2017-03-01";
date = DateUtil.parse(dateStr);
//自定义格式化转换
date = DateUtil.parse(dateStr, "yyyy-MM-dd");
//格式化输出日期
String format = DateUtil.format(date, "yyyy-MM-dd");
//获得年的部分
int year = DateUtil.year(date);
//获得月份，从0开始计数
int month = DateUtil.month(date);
//获取某天的开始、结束时间
Date beginOfDay = DateUtil.beginOfDay(date);
Date endOfDay = DateUtil.endOfDay(date);
//计算偏移后的日期时间
Date newDate = DateUtil.offset(date, DateField.DAY_OF_MONTH, 2);
//计算日期时间之间的偏移量
long betweenDay = DateUtil.between(date, newDate, DateUnit.DAY);

```



## ReflectUtil



```
//获取某个类的所有方法
Method[] methods = ReflectUtil.getMethods(PmsBrand.class);
//获取某个类的指定方法
Method method = ReflectUtil.getMethod(PmsBrand.class, "getId");
//使用反射来创建对象
PmsBrand pmsBrand = ReflectUtil.newInstance(PmsBrand.class);
//反射执行对象的方法
ReflectUtil.invoke(pmsBrand, "setId", 1);
```



## BeanUtil



```
PmsBrand brand = new PmsBrand();
brand.setId(1L);
brand.setName("小米");
brand.setShowStatus(0);
//Bean转Map
Map<String, Object> map = BeanUtil.beanToMap(brand);
LOGGER.info("beanUtil bean to map:{}", map);
//Map转Bean
PmsBrand mapBrand = BeanUtil.mapToBean(map, PmsBrand.class, false);
LOGGER.info("beanUtil map to bean:{}", mapBrand);
//Bean属性拷贝
PmsBrand copyBrand = new PmsBrand();
BeanUtil.copyProperties(brand, copyBrand);
LOGGER.info("beanUtil copy properties:{}", copyBrand);

```



## SecureUtil

```
//MD5加密
String str = "123456";
String md5Str = SecureUtil.md5(str);
LOGGER.info("secureUtil md5:{}", md5Str);
```

## CaptchaUtil

```
//生成验证码图片
LineCaptcha lineCaptcha = CaptchaUtil.createLineCaptcha(200, 100);
try {
    request.getSession().setAttribute("CAPTCHA_KEY", lineCaptcha.getCode());
    response.setContentType("image/png");//告诉浏览器输出内容为图片
    response.setHeader("Pragma", "No-cache");//禁止浏览器缓存
    response.setHeader("Cache-Control", "no-cache");
    response.setDateHeader("Expire", 0);
    lineCaptcha.write(response.getOutputStream());
} catch (IOException e) {
    e.printStackTrace();
}

```



## 字段验证器

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203241050571.awebp)

```
Validator.isEmail("沉默王二");
Validator.isMobile("itwanger.com");
```



## Hutool-http



```
//GET请求
String content = HttpUtil.get(url);

```



```
//POST请求
HashMap<String, Object> paramMap = new HashMap<>();
paramMap.put("city", "北京");

String result1 = HttpUtil.post(url, paramMap);

```







## TreeUtil



```
//配置
TreeNodeConfig treeNodeConfig = new TreeNodeConfig();
// 自定义属性名 都要默认值的
treeNodeConfig.setWeightKey("order");
treeNodeConfig.setIdKey("rid");
// 最大递归深度
treeNodeConfig.setDeep(3);

//转换器
List<Tree<String>> treeNodes = TreeUtil.build(nodeList, "0", treeNodeConfig,
        (treeNode, tree) -> {
            tree.setId(treeNode.getId());
            tree.setParentId(treeNode.getParentId());
            tree.setWeight(treeNode.getWeight());
            tree.setName(treeNode.getName());
            // 扩展属性 ...
            tree.putExtra("extraField", 666);
            tree.putExtra("other", new Object());
        });

```

TreeNode表示一个抽象的节点，也表示数据库中一行数据。 如果有其它数据，可以调用setExtra添加扩展字段。



```
    public List<Tree<String>> getNodeTreeByProjectId(Long projectId) {
        this.getDefaultNode(projectId);
        // 根据 projectId 查询所有节点
        QueryWrapper<ApiModule> wrapperApiModule = new QueryWrapper<>();
        List<ApiModule> apiModules = apiModuleDAO.selectList(wrapperApiModule.eq("projectId", projectId));
        // 配置
        TreeNodeConfig treeNodeConfig = new TreeNodeConfig();
        // 自定义属性名 ，即返回列表里对象的字段名
        treeNodeConfig.setIdKey("id");
        treeNodeConfig.setWeightKey("pos");
        treeNodeConfig.setParentIdKey("parentId");
        // 默认为children不用设置
        treeNodeConfig.setChildrenKey("children");
        // 最大递归深度
//        treeNodeConfig.setDeep(5);
        treeNodeConfig.setNameKey("name");
        //转换器
        List<Tree<String>> treeNodes = TreeUtil.build(apiModules, "0", treeNodeConfig,
                (treeNode, tree) -> {
                    tree.setId(treeNode.getId().toString());
                    tree.setParentId(treeNode.getParentId().toString());
                    tree.setWeight(treeNode.getPos());
                    tree.setName(treeNode.getName());
                    // 扩展属性 ...
                    tree.putExtra("projectId", treeNode.getProjectId());
                    tree.putExtra("level", treeNode.getLevel());
                    tree.putExtra("label", treeNode.getName());
                    tree.putExtra("createTime", treeNode.getCreateTime());
                    tree.putExtra("updateTime", treeNode.getUpdateTime());

                });
        return treeNodes;

    }
```

最后可以使用JSON.toJSON(treenodea)转化为json格式
