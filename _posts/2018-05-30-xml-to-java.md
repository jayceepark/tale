---

title: 스프링 설정 xml에서 자바(클래스)로 변환하기
author: 박종철
category: spring
description: xml설정 파일을 자바로 변환하는 방법.

---

대부분의 스프링 환경에서는 설정을 xml로 하는데, 자바 클래스로도 설정이 가능하다.

자바 클래스로 설정 시 장점은
1. 오타와 오류를 컴파일 단계에서 오류 검증 가능. xml은 런타임 시 검증 가능
2. ide 도움을 받을 수 있다.
3. 디버깅이 쉽다.

자바로 바꿔보면 확실히 편하다. stack overflow에서 관련 좋은 내용이 있어서 올려본다.

원본 xml

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="mailSender" class="org.springframework.mail.javamail.JavaMailSenderImpl">
        <property name="host" value="mail.csonth.gov.uk"/>
    </bean>

    <bean id="registrationService" class="com.foo.SimpleRegistrationService">
        <property name="mailSender" ref="mailSender"/>
        <property name="velocityEngine" ref="velocityEngine"/>
    </bean>

    <bean id="velocityEngine" class="org.springframework.ui.velocity.VelocityEngineFactoryBean">
        <property name="velocityProperties">
            <value>
                resource.loader=class
                class.resource.loader.class=org.apache.velocity.runtime.resource.loader.ClasspathResourceLoader
            </value>
        </property>
    </bean>

</beans>
```

1. @Configuration 어노테이션으로 자바 클래스를 생성하고, 각각의 빈을 선언한다.

``` java
@Configuration
public class MyConfiguration {

    @Bean
    public JavaMailSenderImpl mailSender() {
        JavaMailSenderImpl mailSender = new JavaMailSenderImpl();
        mailSender.setHost("mail.csonth.gov.uk");
        return mailSender;
    }

    @Bean
    public SimpleRegistrationService registrationService(JavaMailSenderImpl mailSender, VelocityEngineFactoryBean velocityEngine) {
        SimpleRegistrationService registrationService = new SimpleRegistrationService();
        registrationService.setMailSender(mailSender);
        registrationService.setVelocityEngine(velocityEngine); 
        return registrationService; 
    }

    @Bean
    public VelocityEngineFactoryBean velocityEngine() {
        VelocityEngineFactoryBean velocityEngine = new VelocityEngineFactoryBean();
        Properties velocityProperties = new Properties();
        velocityProperties.put("resource.loader", "class");
        velocityProperties.put("class.resource.loader.class", "org.apache.velocity.runtime.resource.loader.ClasspathResourceLoader");
        velocityEngine.setVelocityProperties(velocityProperties);
        return velocityEngine;
    }
}
```
2. 생성된 빈들은 스프링 컨테이너에 등록할 수 있도록 설정해준다. 다음과 같은 두가지 방법이 있다.
  - 컴포넌트 스캔 어노테이션을 이용(Component scanning)  
   ``` java
   @ComponentScan("your.package.name")
   ```
  - AnnotationConfigApplicationContext를 이용하여 등록  
   ``` java
   AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext();  
   context.register(MyConfiguration.class);  
   context.refresh();  
   ```


출처: <a href="https://stackoverflow.com/questions/45880027/how-to-convert-spring-bean-integration-from-xml-to-java-annotation-based-config" target="_blank">stack overflow - How to convert spring bean integration From XML to Java annotation based Config</a>
