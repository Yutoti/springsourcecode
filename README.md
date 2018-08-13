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


### ApplicationContext的应用场景和设计原理



# AOP容器的实现
