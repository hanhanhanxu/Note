### DispatcherServlet

spring boot为我们自动配置了一个开箱即用的DispatcherServlet，映射路径为‘/’，所以我们不需要配置DispatcherServlet了。

#### 修改DispatcherServlet默认拦截路径

在启动类添加代码：

```java
/**
     * 修改DispatcherServlet默认配置
     *
     * @param dispatcherServlet
     * @return
     * @author SHANHY
     * @create  2016年1月6日
     */
    @Bean
    public ServletRegistrationBean dispatcherRegistration(DispatcherServlet dispatcherServlet) {
        ServletRegistrationBean registration = new ServletRegistrationBean(dispatcherServlet);
        registration.getUrlMappings().clear();
        registration.addUrlMappings("*.do");
        registration.addUrlMappings("*.json");
        return registration;
    }
```



#### 配置多个DispatcherServlet

https://www.jianshu.com/p/be2dafc8c644

