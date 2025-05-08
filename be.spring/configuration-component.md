## @Configuration과 CGLIB
 - @Configuration 어노테이션을 사용하면 Spring은 해당 클래스를 CGLIB을 사용하여 프록시 객체로 등록합니다.
 - CGLIB를 사용하는 이유는 빈의 싱글톤 보장을 위해서입니다.
 - Spring은 @Configuration 이 붙은 클래스의 메서드를 호출할 때 프록시 객체를 통해 호출하여 중복 빈 생성을 방지합니다.

```java
@Configuration
public class AppConfig {
    @Bean
    public MyService myService() {
        return new MyService();
    }
}
```
 - 위 코드에서 myService() 메서드를 직접 호출하더라도 프록시 객체를 통해 호출되어 항상 같은 인스턴스를 반환합니다.
 - 즉, CGLIB를 사용하여 해당 클래스 자체를 상속한 프록시 객체를 생성합니다.

## @Component 와 프록시 생성 방식
 - @Component 는 기본적으로 프록시를 사용하지 않고 빈으로 등록합니다.
 - 하지만 AOP와 같은 프록시 기반 기능이 포함된 경우, Spring은 다음 기준에 따라 프록시를 생성합니다.
   - 인터페이스가 있는 경우: JDK 동적 프록시 사용
   - 인터페이스가 없는 경우: CGLIB 사용
   
```java
@Component
public class MyComponent {
    public void doSomething() {
        System.out.println("Doing something");
    }
}
```

## 핵심 정리
- @Configuration 은 무조건 CGLIB로 프록시를 생성하여 싱글톤을 보장합니다.
- @Component 는 기본적으로 프록시를 사용하지 않지만, AOP나 트랜잭션 관리와 같은 프록시가 필요한 경우 인터페이스 유무에 따라 JDK 동적 프록시 또는 CGLIB를 사용합니다.

## JDK 동적 프록시란?
- JDK 동적 프록시는 Java 표준 라이브러리에서 제공하는 기능으로, 런타임에 인터페이스를 기반으로 프록시 객체를 동적으로 생성할 수 있는 방식입니다.
- JDK 동적 프록시는 인터페이스 기반으로 동작하여, 인터페이스가 없는 클래스에는 사용할 수 없습니다. 반면, CGLIB는 클래스를 직접 상속하여 프록시를 생성합니다. Spring AOP에서는 인터페이스가 있을 때 JDK 동적 프록시를, 없을 때 CGLIB를 사용하여 최적의 성능과 유연성을 제공합니다.

