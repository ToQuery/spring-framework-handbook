## Bean

- BeanClassLoaderAware

- BeanDefinitionRegistryPostProcessor

在bean注册到ioc后创建实例前修改bean定义或者新增bean注册，这个是在context的refresh方法调用，

- BeanFactoryAware

setBeanFactory

- BeanFactoryPostProcessor

当需要对Bean工厂进行预处理时，可以新建一个实现BeanFactoryPostProcessor接口的类，并将该类配置到Spring容器中。

在Bean实例化前后做一些操作: InstantiationAwareBeanPostProcessor继承自BeanPostProcess接口

InstantiationAwareBeanPostProcessor 里有3 个方法，分别是 postProcessBeforeInstantiation （实例化之前），postProcessAfterInstantiation （实例化之后），postProcessPropertyValues （在处理Bean属性之前），开发者可以在这三个方法中添加自定义逻辑

1、Bean构造出来之前调用postProcessBeforeInstantiation()方法

2、Bean构造出来之后调用postProcessAfterInstantiation()方法

不过通常来讲，我们不会直接实现InstantiationAwareBeanPostProcessor接口，而是会采用继承InstantiationAwareBeanPostProcessorAdapter这个抽象类的方式来使用

实例化—-实例化的过程是一个创建Bean的过程，即调用Bean的构造函数，单例的Bean放入单例池中
初始化—-初始化的过程是一个赋值的过程，即调用Bean的setter，设置Bean的属性


- BeanNameAware

当bean需要获取自身在容器中的id/name时，可以实现BeanNameAware接口。

- BeanPostProcessor 后置处理Bean

针对每一个Bean的生成前后做一些逻辑操作，PostProcessor则帮助我们做到了这一点，并将该类配置到Spring容器中。
BeanPostProcess接口有两个方法，都可以见名知意：
1、postProcessBeforeInitialization：在初始化Bean之前
2、postProcessAfterInitialization：在初始化Bean之后
值得注意的是，这两个方法是有返回值的，不要返回null，否则getBean的时候拿不到对象


- BeanWrapper

- DisposableBean 销毁Bean

当需要在bean销毁之前做些特殊的处理，可以让该bean实现DisposableBean接口。
效果等同于bean的destroy-method属性的使用或者@PreDestory注解的使用。
三种方式的执行顺序：先注解，然后执行DisposableBean接口中定义的方法，最后执行destroy-method属性指定的方法。

- FactoryBean<T> 创建Bean

传统的Spring容器加载一个Bean的整个过程，都是由Spring控制的，换句话说，开发者除了设置Bean相关属性之外，是没有太多的自主权的。FactoryBean改变了这一点，开发者可以个性化地定制自己想要实例化出来的Bean，方法就是实现FactoryBean接口。

1、getObject()方法是最重要的，控制Bean的实例化过程
2、getObjectType()方法获取接口返回的实例的class
3、isSingleton()方法获取该Bean是否为一个单例的Bean

- InstantiationAwareBeanPostProcessor

- InitializingBean 初始化Bean

当需要在bean的全部属性设置成功后做些特殊的处理，可以让该bean实现InitializingBean接口。
效果等同于bean的init-method属性的使用或者@PostContsuct注解的使用。
三种方式的执行顺序：先注解，然后执行InitializingBean接口中定义的方法，最后执行init-method属性指定的方法。



## Core

- ConverterFactory 转换工厂
- Converter 转换器
- Environment

当前应用正在运行环境的接口，通过此接口可以获得配置文件和属性。该接口还继承了PropertyResolver，可以获取placeholder中的属性值。
常用方法

getActiveProfiles 获取当前激活的环境
getDefaultProfiles 获取当前默认环境
acceptsProfiles 检测环境是否处于激活状态
containsProperty 是否包含属性key
getProperty 根据key获取属性
getRequiredProperty 根据key获取属性，若值不存在则抛出IllegalStateException
resolvePlaceholders 处理el表达式，并获取对应值。例如：传入${spring.profiles.active}，就可以获取对应值。跟getProperty不同的是，getProperty需要的是具体key名称，而不是表达式。
resolveRequiredPlaceholders 同resolvePlaceholders，不过值不存在会抛出IllegalStateException
- EnvironmentCapable
- Ordered 排序


## Context

- ApplicationContextAware

当一个类需要获取ApplicationContext实例时，可以让该类实现ApplicationContextAware接口。

- ApplicationEvent
当需要创建自定义事件时，可以新建一个继承自ApplicationEvent抽象类的类。



- ApplicationEventMulticaster

- ApplicationEventPublisher

- ApplicationListener<E extends ApplicationEvent>

当需要监听自定义事件时，可以新建一个实现ApplicationListener接口的类，并将该类配置到Spring容器中。

- ContextLoaderListener

- ImportBeanDefinitionRegistrar
- ImportSelector

- Lifecycle

当前应用正在运行环境的接口，通过此接口可以获得配置文件和属性。该接口还继承了PropertyResolver，可以获取placeholder中的属性值。
常用方法

getActiveProfiles 获取当前激活的环境
getDefaultProfiles 获取当前默认环境
acceptsProfiles 检测环境是否处于激活状态
containsProperty 是否包含属性key
getProperty 根据key获取属性
getRequiredProperty 根据key获取属性，若值不存在则抛出IllegalStateException
resolvePlaceholders 处理el表达式，并获取对应值。例如：传入${spring.profiles.active}，就可以获取对应值。跟getProperty不同的是，getProperty需要的是具体key名称，而不是表达式。
resolveRequiredPlaceholders 同resolvePlaceholders，不过值不存在会抛出IllegalStateException

- ResourceLoaderAware




## Web

- HandlerInterceptorAdapter 

实现自己的拦截器

- HandlerMethodArgumentResolver 方法参数处理
- WebMvcConfigurer    WebMvc 配置

## Aop

- BeanNameAutoProxyCreator

该类是个实现类，一般在这个类的postProcessAfterInitialization方法内对类进行代理增强，该类特殊在于可以指定对IOC容器内的某些名字的类进行增强，并且可以指定增强需要的拦截器.

## Security

- WebSecurityConfigurer<T extends SecurityBuilder<Filter>>