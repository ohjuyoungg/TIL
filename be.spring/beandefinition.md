## BeanDefinition 이란?

> BeanDefinition은 스프링 컨테이너에 등록될 빈 객체의 설계도 역할이다. 
> 이 객체는 어떤 클래스이고, 어떤 방식으로 만들고, 어떤 의존성을 주입받아야하는지 알려주는 메타정보이다.


### BeanDefinition 의 주요 요소들
- Class - 어떤 클래스를 인스턴스화 할 것인지
- Scope - 싱글톤? 프로토타입? (기본값은 singleton)
- Constructor args - 생성자 주입에 필요한 값들
- Properties - 세터 주입을 위한 값들
- Lazy init 여부 - 지연 초기화할지 말지
- Depends-on - 이 빈이 의존하는 다른 빈들
- init method / destroy method - 초기화, 소멸 메서드 지정

### BeanDefinition 이 어떻게 쓰이는지
1. 설정 파일(@Configuration, @Component 등)을 분석해서 모든 BeanDefinition을 미리 만들어 컨테이너 내부에 등록해둠
2. 누군가가 어떤 빈을 요청하면, 해당 BeanDefinition을 꺼내서 보고, 그 정보를 바탕으로 객체를 생성함

### 설계도에 비유
- BeanDefinition = 건물을 짓기 위한 설계도
- 스프링 컨테이너 = 건설 회사
- 빈 객체 = 실제 지어진 건물
> 건설 회사(스프링)는 미리 준비된 설계도(BeanDefinition)를 보고 건물을 짓는 것 처럼, 스프링은 BeanDefinition을 참고해 실제 객체를 만들어낸다.

1. 소스코드
   - @Component, @Bean 등으로 Bean 후보 등록

2. BeanDefinition 생성
   - 스프링이 각 Bean에 대한 "설계도" 만들고 컨테이너에 등록

3. 실제 객체 생성
   - 설계도 보고 BeanFactory가 객체 생성, DI, 초기화 등 수행
