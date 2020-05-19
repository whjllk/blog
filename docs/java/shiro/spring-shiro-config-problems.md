#### sprinng + springmvc + mybatis + shiro 
##### 问题：
sprinng + springmvc + mybatis 功能都没问题，但是与shiro集成后，shiro功能就乱了，完全不起作用。
控制台上报错：基本上SpringShiroWeb.java、SpringDatasource.java中，注册的所有bean都会有提示, ××× is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)

##### 分析：
据说是
1、@Configuration会最先实例化，也就是在spring启动完成之前
2、shiroFilter中会初始化业务Bean，如UserService、UserDao，两者初始化之前，spring还未初始化完成，因此無法使用spring的相關功能。
（感觉不太对，因为目前shiro连基本的登录认证都不能正常使用，更别说其他功能了。网上其他人的问题都是基本功能正常使用，事务不正常）
3、努力了3天了，先放放，日后在看。
    a、Spring容器对BeanPostProcessor、Bean的装载顺序？
    b、xml方式bean的加载，与纯java配置方式bean的加载顺序有什么不同？

##### 參考
1、出现这个问题，一般是在开发过程中，因为不清楚Spring容器对BeanPostProcessor、Bean的装载顺序，从而导致有时候我们需要提前用到Bean的功能；可以用实现ApplicationContextAware接口的方式，如何使用？
2、参考：https://www.jianshu.com/p/b1209cd3686d?tdsourcetag=s_pcqq_aiomsg
还是不知道如何做？ /尴尬！
