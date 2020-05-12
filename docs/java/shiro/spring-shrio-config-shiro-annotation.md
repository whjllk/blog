### spring-shiro 开启注解
1、shiro 采用spring AOP 式方法级别的权限检查  
2、开启shiro注解（如：@RequiresRoles @RequiresPermissions），需借助SpringAOP扫描shiro注解修饰的类，并在必要的时候进行安全验证  
3、通过注解将权限硬编码到程序中，不推荐使用  
4、开启shiro注解，需在spring-mvc.xml中配置，加载两个类 DefaultAdvisorProxyCreator、AuthorizationAttributeSourceAdvisor  
```
<bean class="org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator">
    <property name="proxyTargetClass" value="true"/>
</bean>
<bean class="org.apache.shiro.spring.security.interceptor.AuthorizationAttributeSourceAdvisor">
    <property name="securityManager" ref="securityManager"/>
</bean>
```
★★★ 若发现shiro注解不起作用的时候，需要检查上面这两个bean是否在spring-mvc.xml中被扫描，在applicationContext.xml中扫描不起作用。
原因：shiro注解一般是在controller中使用的，若配置在applicationContext.xml中，则会导致shiro注解无法被扫描，故不起作用
