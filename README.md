# springsourcecode
深入研究spring框架源码
***
# IoC容器的实现

## BeanFactory和ApplicationContext对比

spring框架IoC容器的两个核心工厂接口是BeanFactory和ApplicationContext，其中：

1. BeanFactory：是IoC容器最基本的接口，定义了IoC容器最基本的方法（如getBean()、containsBean()、isSingleton()、getType()等）；

2. ApplicationContext：一方面，它继承了BeanFactory接口体系中的ListableBeanFactory、ListableBeanFactory接口，具备了BeanFactory体系的基本功能；另一方面，它又继承了EnvironmentCapable、MessageSource、ResourcePatternResolver、ApplicationEventPublisher这些接口，增加了更多的功能，如国际化（MessageSource）、资源访问，比如访问URL和文件（ResourcePatternResolver）、应用事件机制（ApplicationEventPublisher）以及其它附加功能等。

可以这么小结：spring IoC容器的继承结构可以分成两大体系：BeanFactory体系和ApplicationContext体系，BeanFactory体系是BeanFactory接口的简单扩展，是基础容器体系；ApplicationContext体系则在此基础上提供了更多的功能，是IoC容器的高级实现。

### BeanFactory的应用场景和设计原理

1.&nbsp;应用场景：BeanFactory定义了IoC容器最基本的形式，定义了一系列与Bean相关的基本操作，不论子类或子接口提供了何种附加的功能，都可以通过BeanFactoy提供的这些方法来对Bean完成不同的检索操作，从而忽略具体的IoC的具体实现。即该接口的作用是为各个子接口或子类提供最基本的容器入口。

2.&nbsp;设计原理：在BeanFactory继承体系中，比较重要的一个类是DefaultListableBeanFactory类，该类实际上是作为一个功能完整的IoC容器来使用。

使用XmlBeanFactory来看BeanFactory继承体系中的类的使用方式：

    ClassPathResource res = new ClassPathResource("bean.xml);
    DefaultListableBeanFactory factory = new DefaultListableBeanFactory();
    XmlBeanDefinitionReader reader = new XmlBeanDefinitionReader(factory);
    reader.loadBeanDefinitions(res);
可以看到方式是：

&nbsp; &nbsp; (1) 先获得一个Resource，这个Resource里包含了BeanDefinition的定义信息;

&nbsp; &nbsp; (2) 创建一个BeanFactory;

&nbsp; &nbsp; (3) 创建一个BeanDefinition的读取器;

&nbsp; &nbsp; (4) 读入配置信息，由读取器来解析和注册Bean，建立IoC容器。

这几个步骤都需要手动操作，较为繁琐，但需要自定义时也较为灵活。

### ApplicationContext的应用场景和设计原理

1.&nbsp;应用场景：ApplicationContext体系继承了BeanFactory接口，不仅有BeanFactory的基本功能，还附加了许多别的功能，同时将资源定位等操作封装好了，所以spring中常用的是ApplicationContext体系。附加的功能如下：

&nbsp; &nbsp; (1) 支持不同的信息源，完成国际化;

&nbsp; &nbsp; (2) 从不同的地方访问资源;

&nbsp; &nbsp; (3) 支持应用事件;

&nbsp; &nbsp; (4) 其它附加服务。

2.&nbsp;设计原理：ApplicationContext的三个具体实现类是FileSystemXmlApplicationContext、ClassPathXmlApplicationContext、XmlWebApplicationContext，分别提供不同的Resource读取功能。（文件系统、Class目录、Web容器）

这三个实现类将BeanFactory的定位Resource资源、载入BeanDefinition、注册Bean等封装好，无需手动操作。

Ioc容器主要包括两个过程：容器的初始化以及容器的依赖注入。

## Ioc容器的初始化过程

Ioc容器的初始化主要包括三个过程：

1. Resource的定位过程；

2. BeanDefinition的载入过程；

3. 向IoC容器注册这些BeanDefinition的过程。

1.&nbsp; Resource的定位过程




## IoC容器的依赖注入


# AOP容器的实现
