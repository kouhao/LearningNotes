## mvc:annotation-driven意义

1、mvc:annotation-driven 配置查看官网https://docs.spring.io/spring/docs/4.3.18.RELEASE/spring-framework-reference/html/mvc.html#mvc-config-enable；注册`RequestMappingHandlerMapping`，`RequestMappingHandlerAdapter`，`ExceptionHandlerExceptionResolver` 以及其他），以支持使用带注释的控制器方法处理请求，例如@RequestMapping，@ ExceptionHandler等注释。

2、JavaBeans  编辑属性的绑定，还可以通过[ConversionService](https://docs.spring.io/spring/docs/4.3.18.RELEASE/spring-framework-reference/html/validation.html#core-convert) 实例实现Spring 3 格式转换。

3、支持通过ConversionService使用@NumberFormat注释格式化数字字段；

4、支持 [formatting](https://docs.spring.io/spring/docs/4.3.18.RELEASE/spring-framework-reference/html/validation.html#format)`@DateTimeFormat`；

5、支持@controller中的输入带有@Valid注解，[validating](https://docs.spring.io/spring/docs/4.3.18.RELEASE/spring-framework-reference/html/mvc.html#mvc-config-validation)

6、@RequestBody`与`@ResponseBody`中的`HttpMessageConverter`，HttpMessageConverter包含的具体MessageConverter

- ByteArrayHttpMessageConverter
- StringHttpMessageConverter
- ResourceHttpMessageConverter ,
- SourceHttpMessageConverter ,
- FormHttpMessageConverter ,
- Jaxb2RootElementHttpMessageConverter ,
- MappingJackson2HttpMessageConverter ,
- MappingJackson2XmlHttpMessageConverter ,
- AtomFeedHttpMessageConverter ,
- RssChannelHttpMessageConverter