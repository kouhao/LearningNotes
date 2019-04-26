## Spring配置项\<context:annotation-config>的解释说明

1、\<context:annotation-config>在spring 容器中注册AutowiredAnnotationBeanPostProcessor，CommonAnnotationBeanPostProcessor，PersistenceAnnotationBeanPostProcessor,RequiredAnnotationBeanPostProcessor这4个BeanPostProcessor.让spring 容器识别对应的注解，对应关系如下：

@Resource 、@PostConstruct、@PreDestroy 对应CommonAnnotationBeanPostProcessor；

@PersistenceContext 对应 PersistenceAnnotationBeanPostProcessor；

@Autowired 对应 AutowiredAnnotationBeanPostProcessor ；

@Required 对应 RequiredAnnotationBeanPostProcessor



2、\<context:annotation-config> 对应xml 配置

```xml
<bean class="org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor "/>
```

但是，就一般而言，这些注解我们是经常使用，比如Antowired,Resuource等注解，如果总是按照传统的方式一条一条的配置，感觉比较繁琐和机械。于是Spring给我们提供了<context:annotation-config/>的简化的配置方式，自动帮助你完成声明，并且还自动搜索@Component, @Controller , @Service , @Repository等标注的类。

```xml
<context:component-scan base-package="com.**.impl"/>
```

3、\<context:component-scan/> 与 \<context:annotation-config>  具有包含关系，因此可以移除\<context:annotation-config>

