# Spring 使用过程中常用的接口

[TOC]

## Bean

### BeanClassLoaderAware


- void setBeanClassLoader(ClassLoader classLoader);

设置 Bean 的 ClassLoader

### BeanDefinitionRegistryPostProcessor

在bean注册到ioc后创建实例前修改bean定义或者新增bean注册，这个是在context的refresh方法调用，

- void postProcessBeanDefinitionRegistry(BeanDefinitionRegistry registry) throws BeansException;

### BeanFactoryAware

- void setBeanFactory(BeanFactory beanFactory) throws BeansException;


### BeanFactoryPostProcessor

当需要对Bean工厂进行预处理时，可以新建一个实现BeanFactoryPostProcessor接口的类，并将该类配置到Spring容器中。

- void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException;

### BeanNameAware

当bean需要获取自身在容器中的id/name时，可以实现BeanNameAware接口。

- void setBeanName(String name);

### BeanPostProcessor 后置处理Bean

针对每一个Bean的生成前后做一些逻辑操作，PostProcessor则帮助我们做到了这一点，并将该类配置到Spring容器中。
BeanPostProcess接口有两个方法，都可以见名知意。值得注意的是，这两个方法是有返回值的，不要返回null，否则getBean的时候拿不到对象。

- @Nullable default Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException { return bean;}

在初始化Bean之前

- @Nullable default Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException { return bean;}

在初始化Bean之后

### BeanWrapper

### DisposableBean 销毁Bean

当需要在bean销毁之前做些特殊的处理，可以让该bean实现DisposableBean接口。
效果等同于bean的destroy-method属性的使用或者@PreDestory注解的使用。
三种方式的执行顺序：先注解，然后执行DisposableBean接口中定义的方法，最后执行destroy-method属性指定的方法。

- void destroy() throws Exception;

### FactoryBean<T> 创建Bean

传统的Spring容器加载一个Bean的整个过程，都是由Spring控制的，换句话说，开发者除了设置Bean相关属性之外，是没有太多的自主权的。FactoryBean改变了这一点，开发者可以个性化地定制自己想要实例化出来的Bean，方法就是实现FactoryBean接口。

- @Nullable T getObject() throws Exception;

控制Bean的实例化过程

- @Nullable Class<?> getObjectType();

获取接口返回的实例的class

- default boolean isSingleton() { return true;}

获取该Bean是否为一个单例的Bean


### InstantiationAwareBeanPostProcessor

继承自`BeanPostProcess`接口。InstantiationAwareBeanPostProcessor 里有3 个方法，分别是 postProcessBeforeInstantiation （实例化之前），postProcessAfterInstantiation （实例化之后），postProcessPropertyValues （在处理Bean属性之前）。不过通常来讲，我们不会直接实现InstantiationAwareBeanPostProcessor 接口，而是会采用继承 InstantiationAwareBeanPostProcessorAdapter 这个抽象类的方式来使用。

实例化—-实例化的过程是一个创建Bean的过程，即调用Bean的构造函数，单例的Bean放入单例池中
初始化—-初始化的过程是一个赋值的过程，即调用Bean的setter，设置Bean的属性

- @Nullable default Object postProcessBeforeInstantiation(Class<?> beanClass, String beanName) throws BeansException { return null;}

Bean构造出来之前调用 postProcessBeforeInstantiation() 方法

- default boolean postProcessAfterInstantiation(Object bean, String beanName) throws BeansException { return true;}

Bean构造出来之后调用 postProcessAfterInstantiation() 方法

- @Nullable default PropertyValues postProcessPropertyValues(PropertyValues pvs, PropertyDescriptor[] pds, Object bean, String beanName) throws BeansException { return pvs;}


### InitializingBean 初始化Bean

当需要在bean的全部属性设置成功后做些特殊的处理，可以让该bean实现InitializingBean接口。
效果等同于bean的init-method属性的使用或者@PostContsuct注解的使用。
三种方式的执行顺序：先注解，然后执行InitializingBean接口中定义的方法，最后执行init-method属性指定的方法。

- void afterPropertiesSet() throws Exception;

## Core

### ConverterFactory<S, R> 转换工厂

- <T extends R> Converter<S, T> getConverter(Class<T> targetType);

### Converter 转换器
### Environment

当前应用正在运行环境的接口，通过此接口可以获得配置文件和属性。该接口还继承了`PropertyResolver`可以获取placeholder中的属性值。

- String[] getActiveProfiles();

获取当前激活的环境

- String[] getDefaultProfiles();

获取当前默认环境

- @Deprecated boolean acceptsProfiles(String... profiles);
- boolean acceptsProfiles(Profiles profiles);

检测环境是否处于激活状态

### EnvironmentCapable

- Environment getEnvironment();

### Ordered 排序

- int getOrder();

## Context

### ApplicationContextAware

当一个类需要获取ApplicationContext实例时，可以让该类实现ApplicationContextAware接口。

- void setApplicationContext(ApplicationContext applicationContext) throws BeansException;

### ApplicationEvent

当需要创建自定义事件时，可以新建一个继承自ApplicationEvent抽象类的类。



### ApplicationEventMulticaster

### ApplicationEventPublisher

### ApplicationListener<E extends ApplicationEvent>

当需要监听自定义事件时，可以新建一个实现ApplicationListener接口的类，并将该类配置到Spring容器中。

- void onApplicationEvent(E event);

### ContextLoaderListener

### ImportBeanDefinitionRegistrar
### ImportSelector

### Lifecycle

### ResourceLoaderAware


## Web

### HandlerInterceptorAdapter 

实现自己的拦截器

### HandlerMethodArgumentResolver 方法参数处理

### `WebMvcConfigurer` WebMvc配置

- default void configurePathMatch(PathMatchConfigurer configurer) {}
- default void configureContentNegotiation(ContentNegotiationConfigurer configurer) {}
- default void configureAsyncSupport(AsyncSupportConfigurer configurer) {}
- default void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {}
- default void addFormatters(FormatterRegistry registry) {}
- default void addInterceptors(InterceptorRegistry registry) {}
- default void addResourceHandlers(ResourceHandlerRegistry registry) {}
- default void addCorsMappings(CorsRegistry registry) {}
- default void addViewControllers(ViewControllerRegistry registry) {}
- default void configureViewResolvers(ViewResolverRegistry registry) {}
- default void addArgumentResolvers(List<HandlerMethodArgumentResolver> resolvers) {}
- default void addReturnValueHandlers(List<HandlerMethodReturnValueHandler> handlers) {}
- default void configureMessageConverters(List<HttpMessageConverter<?>> converters) {}
- default void extendMessageConverters(List<HttpMessageConverter<?>> converters) {}
- default void configureHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {}
- default void extendHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {}
- @Nullable default Validator getValidator() { return null;}
- @Nullable default MessageCodesResolver getMessageCodesResolver() { return null;}



## Aop

### BeanNameAutoProxyCreator

该类是个实现类，一般在这个类的postProcessAfterInitialization方法内对类进行代理增强，该类特殊在于可以指定对IOC容器内的某些名字的类进行增强，并且可以指定增强需要的拦截器.

## Security

### `WebSecurityConfigurer<T extends SecurityBuilder<Filter>>`