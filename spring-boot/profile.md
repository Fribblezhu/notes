# Spring Profile
- spring 5+
- Spring Boot 2+

## Overview
---
In this tutorial, We will focus on introducing Profile in Spring.

## Configuration
 ---
### Use `@Profile` on a Bean
>We annotate an bean with a `dev` profile, and it will only be present in the container during development.
```
@Component
@Profile("dev")
public class AnBean{

}
```
>As a quick sidenote, profile names can also be prefixed with an NOT operateor, e.g., `!dev` to exclude them from a profile.
```
@Component
@Profile("!dev")
public cass AnBean{

}
```

### Declare Profiles In XML
>Profiles can also ben configured in XML, The \<bean> target has a profile attribute, which takes comma-separated value of the applicable profiles:
```
<beans profile="dev">
    <bean id="AnBean" class="com.bean.AnBean.class">
</beans>
```

### Declare in yml file
```
application-dev.yml
application-pro.yml
```

## Active
---

### Programmatically via WebApplicationInitializer Interface
```
@Configuration
public class MyWebApplicationInitializer implements WebApplicationInitializer {

    @Override
    public void onStartup(ServletContext serveletContext) throws ServletException{
        serveletContext.setInitParameter("spring.profiles.active", "dev");
    }
}
```

### Programmatically via ConfigurableEnvironment
```
@Autowired
private ConfigurableEnvironment evn;
...
evn.setActiveProfile("dev");
```
### Context Parameter in web.xml
```
<context-param>
    <param-name>spring.profiles.active</param-name>
    <param-value>dev</param-value>
</context-param>
```
### JVM System parameter
```
-Dspring.profiles.active=dev
```
### JVM Program arguments
```
--spring.profiles.active=test
```
### Environment variable
In a Unix environment, profile also can be activated via the environment variable.
```
export spring_profiles_active=dev
```