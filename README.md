####@Component 已经可以支持声明一个 bean 了，为何还要再弄个 @Bean 出来？
Spring帮助我们管理bean可以分为两步：注册bean、装配bean  
@Component提供自动装配的方式，表明一个类会作为组件类，并告知Spring要为这个类创建Bean。  
@Configuration+@Bean提供的方式相当于是半自动的。@Bean注解告诉Spring这个方法将会返回一个对象，这个对象要注册为Spring应用上下文中的bean。  
但两者的目的是一样的，都是注册bean到Spring容器中。  
假如我想要将第三方的库中的组件装配到我的应用中，在这种情况下，没有办法在它的类上加上@Component注解，因此加@Component注解的方式是无法满足情况的。但是我们可以使用@Bean将第三方的组件装配到我的应用中,代码如下：
```
@Configuration
public class WireThirdLibClass {
    @Bean
    public ThirdLibClass getThirdLibClass() {
        return new ThirdLibClass();
    }
}
```
另外一种情况，当我们想装配的组件可能根据不同的状态创建不同的bean时，直接用@Component方法无法实现。此时用@Configuration和@Bean可以实现。代码如下：
```
@Configuration
public class WireThirdLibClass {
    @Bean
    public OneService oneService(int status) {
        switch (status) {
            case 1:
                return new ServiceImp1();
            break;
            case 2:
                return new ServiceImp2();
            break;
            case 3:
                return new ServiceImp3();
            break;
        }
    }
}
```

