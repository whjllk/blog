#### <url-patrern/> 中 "/"、"/*"、"/**"、"" 的区别
##### /**
    /** 是拦截所有的文件夹及里面的子文件夹
##### <url-pattern>/*</url-pattern>
    /* 是拦截所有的文件夹，不包含子文件夹
    The /* on a servlet overrides all other servlets, including all servlets provided by the servletcontainer such as the default servlet and the JSP servlet. 
    Whatever request you fire, it will end up in that servlet. This is thus a bad URL pattern for servlets.
    Usually, you'd like to use /* on a Filter only. It is able to let the request continue to any of the servlets listening on a more specific URL pattern by calling FilterChain#doFilter().
    总结：/* 会拦截所有url，一般用在filter的url-pattern上
##### <url-pattern>/</url-pattern>
    The / doesn't override any other servlet. 
    It only replaces the servletcontainer's builtin default servlet for all requests which doesn't match any other registered servlet. 
    this is normally only invoked on static resources (CSS/JS/image/etc) and directory listings. 
    The servletcontainer's builtin default servlet is also capable of dealing with HTTP cache requests, media (audio/video) streaming and file download resumes. 
    Usually, you don't want to override the default servlet as you would otherwise have to take care of all its tasks, 
    which is not exactly trivial (JSF utility library OmniFaces has an open source example). 
    This is thus also a bad URL pattern for servlets. 
    As to why JSP pages doesn't hit this servlet, it's because the servletcontainer's builtin JSP servlet will be invoked, which is already by default mapped on the more specific URL pattern *.jsp.
    总结：
##### <url-pattern></url-pattern>
    Then there's also the empty string URL pattern . This will be invoked when the context root is requested. 
    This is different from the <welcome-file> approach that it isn't invoked when any subfolder is requested. 
    This is most likely the URL pattern you're actually looking for in case you want a "home page servlet". 
    I only have to admit that I'd intuitively expect the empty string URL pattern  and the slash URL pattern / be defined exactly the other way round, so I can understand that a lot of starters got confused on this. But it is what it is.

##### Front Controller
    In case you actually intend to have a front controller servlet, then you'd best map it on a more specific URL pattern like *.html, *.do, /pages/*, /app/*, etc. You can hide away the front controller URL pattern and cover static resources on a common URL pattern like /resources/*, /static/*, etc with help of a servlet filter. See also How to prevent static resources from being handled by front controller servlet which is mapped on /*. Noted should be that Spring MVC has a builtin static resource servlet, so that's why you could map its front controller on / if you configure a common URL pattern for static resources in Spring. See also How to handle static content in Spring MVC?

##### 参考
https://stackoverflow.com/questions/4140448/difference-between-and-in-servlet-mapping-url-pattern