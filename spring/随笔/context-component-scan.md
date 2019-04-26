## \<context:component-scan> 配置

默认情况下，\<context:component-scan>查找使用构造型（stereotype）注解所标注的类，如@Component(组件)，@Service（服务），@Controller（控制器），@Repository（数据仓库）我们具体看下\<context:component-scan>的一些属性，以下是一个比较具体的\<context:component-scan>配置。

```xml
<context:component-scan base-package="com.wjx.betalot" <!-- 扫描的基本包路径 -->
                        annotation-config="true" <!-- 是否激活属性注入注解 -->
name-generator="org.springframework.context.annotation.AnnotationBeanNameGenerator"  <!-- Bean的ID策略生成器 -->
resource-pattern="**/*.class" <!-- 对资源进行筛选的正则表达式，这边是个大的范畴，具体细分在include-filter与exclude-filter中进行 -->
scope-resolver="org.springframework.context.annotation.AnnotationScopeMetadataResolver" <!-- scope解析器 ，与scoped-proxy只能同时配置一个 -->
scoped-proxy="no" <!-- scope代理，与scope-resolver只能同时配置一个 -->
use-default-filters="false" <!-- 是否使用默认的过滤器，默认值true -->
                                  >
            <!-- 注意：若使用include-filter去定制扫描内容，要在use-default-filters="false"的情况下，不然会“失效”，被默认的过滤机制所覆盖 -->                   
            <!-- annotation是对注解进行扫描 -->
   <context:include-filter type="annotation" expression="org.springframework.stereotype.Component"/> 
            <!-- assignable是对类或接口进行扫描 -->
    <context:include-filter type="assignable" expression="com.wjx.betalot.performer.Performer"/>
    <context:include-filter type="assignable" expression="com.wjx.betalot.performer.impl.Sonnet"/>
            
 <!-- 注意：在use-default-filters="false"的情况下，exclude-filter是针对include-filter里的内容进行排除 -->
   <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
   <context:exclude-filter type="assignable" expression="com.wjx.betalot.performer.impl.RainPoem"/>
   <context:exclude-filter type="regex" expression=".service.*"/> 

</context:component-scan>
```

- **back-package**：标识了\<context:component-scan>元素所扫描的包，可以使用一些通配符进行配置

- **annotation-config**:\<context:component-scan>元素也完成了\<context:annotation-config>元素的工作，开关就是这个属性，false则关闭属性注入注解功能。
- **name-generator**:这个属性指定你的构造型注解，注册为Bean的ID生成策略，这个生成器基于接口BeanNameGenerator实现generateBeanName方法，你可以自己写个类去自定义策略。这边，我们可不显示配置，它是默认使用org.springframework.context.annotation.AnnotationBeanNameGenerator生成器，也就是类名首字符小写的策略，如Performer类，它注册的Bean的ID为performer.并且可以自定义ID，如@Component("Joy").这边简单贴出这个默认生成器的实现。
- **resource-pattern**：对资源进行筛选的正则表达式，这边是个大的范畴，具体细分在include-filter与exclude-filter中进行。
- **scoped-proxy**: scope代理，有三个值选项，no(默认值)，interfaces(接口代理)，targetClass（类代理），那什么时候需要用到scope代理呢，举个例子，我们知道Bean的作用域scope有singleton，prototype，request,session,那有这么一种情况，当你把一个session或者request的Bean注入到singleton的Bean中时，因为singleton的Bean在容器启动时就会创建A，而session的Bean在用户访问时才会创建B，那么当A中要注入B时，有可能B还未创建，这个时候就会出问题，那么代理的时候来了，B如果是个接口，就用interfaces代理，是个类则用targetClass代理。这个例子出处：http://www.bubuko.com/infodetail-1434289.html。
- **scope-resolver**:这个属性跟name-generator有点类似，它是基于接口ScopeMetadataResolver的，实现resolveScopeMetadata方法，目的是为了将@Scope(value="",proxyMode=ScopedProxyMode.NO,scopeName="")的配置解析成为一个ScopeMetadata对象,Spring这里也提供了两个实现，我们一起看下。首先是org.springframework.context.annotation.AnnotationScopeMetadataResolver中，
- **\<context:include-filter> ：**用来告知哪些类需要注册成Spring Bean,使用type和expression属性一起协作来定义组件扫描策略。type有以下5种

| 过滤器类型 |                             描述                             |
| ---------- | :----------------------------------------------------------: |
| annotation | 过滤器扫描使用注解所标注的那些类，通过expression属性指定要扫描的注释 |
| assignable |       过滤器扫描派生于expression属性所指定类型的那些类       |
| aspectj    | 过滤器扫描与expression属性所指定的AspectJ表达式所匹配的那些类 |
| custom     | 使用自定义的org.springframework.core.type.TypeFliter实现类，该类由expression属性指定 |
| regex      | 过滤器扫描类的名称与expression属性所指定正则表示式所匹配的那些类 |

