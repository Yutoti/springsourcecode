# springsourcecode
深入研究spring框架源码
***
# IoC容器的实现

## BeanFactory和ApplicationContext对比

spring框架IoC容器的两个核心工厂接口是BeanFactory和ApplicationContext，其中：

1. BeanFactory：是IoC容器最基本的接口，定义了IoC容器最基本的方法（如getBean()、containsBean()、isSingleton()、getType()等）；

2. ApplicationContext：一方面，它继承了BeanFactory接口体系中的ListableBeanFactory、ListableBeanFactory接口，具备了BeanFactory体系的基本功能；

另一方面，它又继承了EnvironmentCapable、MessageSource、ResourcePatternResolver、ApplicationEventPublisher这些接口，增加了更多的功能，

如国际化（MessageSource）、资源访问，比如访问URL和文件（ResourcePatternResolver）、应用事件机制（ApplicationEventPublisher）以及其它附加功能等。

可以这么小结：spring IoC容器的继承结构可以分成两大体系：BeanFactory体系和ApplicationContext体系，BeanFactory体系是BeanFactory接口的简单扩展，

是基础容器体系；ApplicationContext体系则在此基础上提供了更多的功能，是IoC容器的高级实现。

### BeanFactory的应用场景和设计原理

1.&nbsp;应用场景：BeanFactory定义了IoC容器最基本的形式，定义了一系列与Bean相关的基本操作，不论子类或子接口提供了何种附加的功能，都可以

通过BeanFactoy提供的这些方法来对Bean完成不同的检索操作，从而忽略具体的IoC的具体实现。即该接口的作用是为各个子接口或子类提供最基本的容器入口。

2.&nbsp;设计原理：在BeanFactory继承体系中，比较重要的一个类是DefaultListableBeanFactory类，该类实际上是作为一个功能完整的IoC容器来使用。

使用XmlBeanFactory来看BeanFactory继承体系中的类的使用方式：

    ClassPathResource res = new ClassPathResource("bean.xml);
    DefaultListableBeanFactory factory = new DefaultListableBeanFactory();
    XmlBeanDefinitionReader reader = new XmlBeanDefinitionReader(factory);
    reader.loadBeanDefinitions(res);
可以看到方式是：

1. 先获得一个Resource，这个Resource里包含了BeanDefinition的定义信息;

2. 创建一个BeanFactory;

3. 创建一个BeanDefinition的读取器;

4. 读入配置信息，由读取器来解析Bean，建立IoC容器。

### ApplicationContext的应用场景和设计原理





# AOP容器的实现
