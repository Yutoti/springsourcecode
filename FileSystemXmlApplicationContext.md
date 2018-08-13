FileSystemXmlApplicationContext的代码分析——IoC容器的初始化
***
首先，在new出FileSystemXmlApplicationContext对象时，通过传入的String，设置xml文件的位置（支持多个xml）；接着调用refresh()方法，该方法在AbstractApplicationContext中定义，是FileSystemXmlApplicationContext容器的核心方法，资源定位、加载Bean定义以及注册Bean都是在该方法中完成的。

	public FileSystemXmlApplicationContext(String[] configLocations, boolean refresh, ApplicationContext parent)
			throws BeansException {

		super(parent);
		setConfigLocations(configLocations);
		if (refresh) {
			refresh();
		}
	}
  
  setConfigLocations(configLocations)可以简单理解为设置xml文件的位置。
  
  接着来看refresh()方法。
  
    @Override
    public void refresh() throws BeansException, IllegalStateException {
      synchronized (this.startupShutdownMonitor) {
        // 做一些前期准备工作（设置起始时间等），不用管
        prepareRefresh();

        // 在这个方法里就完成了资源的定位和加载Bean定义的工作，重点分析这个方法！！
        ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();

        // Prepare the bean factory for use in this context.
        prepareBeanFactory(beanFactory);

        try {
          // Allows post-processing of the bean factory in context subclasses.
          postProcessBeanFactory(beanFactory);

          // Invoke factory processors registered as beans in the context.
          invokeBeanFactoryPostProcessors(beanFactory);

          // Register bean processors that intercept bean creation.
          registerBeanPostProcessors(beanFactory);

          // Initialize message source for this context.
          initMessageSource();

          // Initialize event multicaster for this context.
          initApplicationEventMulticaster();

          // Initialize other special beans in specific context subclasses.
          onRefresh();

          // Check for listener beans and register them.
          registerListeners();

          // Instantiate all remaining (non-lazy-init) singletons.
          finishBeanFactoryInitialization(beanFactory);

          // Last step: publish corresponding event.
          finishRefresh();
        }

        catch (BeansException ex) {
          logger.warn("Exception encountered during context initialization - cancelling refresh attempt", ex);

          // Destroy already created singletons to avoid dangling resources.
          destroyBeans();

          // Reset 'active' flag.
          cancelRefresh(ex);

          // Propagate exception to caller.
          throw ex;
        }
      }
    }

  下面来重点分析obtainFreshBeanFactory()方法。
  
  先看FileSystemXmlApplicationContext的getResource()方法的调用关系图，弄清楚这个方法是什么时候调用的：
  
  //待插入
  
  可以看到，在AbstractApplicationContext方法中的refresh()方法调用时，通过一系列的调用链，最终调用到getResource()方法，而在这个调用链当中，loadBeanDefinition赫然在列！我们具体看AbstractRrefrshableApplicationContext中的refreshBeanFactory()方法：
  
    @Override
    protected final void refreshBeanFactory() throws BeansException {
        ...
        DefaultListableBeanFactory beanFactory = createBeanFactory();
        ...
        loadBeanDefinitions(beanFactory);
        ...
    }
    
再看AbstractXmlApplicationContext中的两个loadBeanDefinition(...)方法：

    @Override
    protected void loadBeanDefinitions(DefaultListableBeanFactory beanFactory) throws BeansException, IOException {
        ...
         XmlBeanDefinitionReader beanDefinitionReader = new XmlBeanDefinitionReader(beanFactory);
         ...
         loadBeanDefinitions(beanDefinitionReader);
     } 
以及：

    protected void loadBeanDefinitions(XmlBeanDefinitionReader reader) throws BeansException, IOException {
	  	  Resource[] configResources = getConfigResources();
        ...
			  reader.loadBeanDefinitions(configResources);
        ...
		    String[] configLocations = getConfigLocations();
        ...
			  reader.loadBeanDefinitions(configLocations);
        ...
		}
    
这就很清楚了，和DefaultListableBeanFactory相同，这里也有获取资源Resources、定义读取器reader、加载Bean定义、由reader注册Bean的一系列操作。spring通过继承关系良好的多个父类子类定义，自动完成了这些操作。
