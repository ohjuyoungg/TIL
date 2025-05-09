## Spring에서는 인터페이스 유무에 따라 JDK 동적 프록시와 CGLIB 프록시를 사용합니다.
- 인터페이스가 있는 경우: JDK 동적 프록시 사용
- 인터페이스가 없는 경우: CGLIB 사용

### 왜 인터페이스가 있으면 JDK 동적 프록시를 쓰는가?
- 1) 자바의 기본 원칙: 다형성 활용
    자바에서는 인터페이스를 통한 다형성을 중요하게 생각합니다.
    인터페이스 기반으로 구현하면 유연하게 확장할 수 있고, 구현체를 쉽게 교체할 수 있습니다.
    JDK 동적 프록시는 인터페이스를 구현하여 프록시 객체를 생성하므로 다형성을 자연스럽게 활용할 수 있습니다.

- 2) 인터페이스가 있는 경우 굳이 CGLIB를 쓸 필요가 없다
    CGLIB는 구체 클래스를 상속하여 프록시를 만드는 방식입니다.
    만약 인터페이스가 이미 존재한다면, 인터페이스를 기반으로 가볍고 성능이 좋은 JDK 동적 프록시를 사용하는 것이 더 효율적입니다.
    CGLIB를 사용하면 클래스를 상속해야 하므로 불필요한 비용이 발생할 수 있습니다.
    또한, final 메서드나 final 클래스는 프록시 생성이 불가능하므로 제약이 있습니다.

- 3) Spring의 기본 철학: 인터페이스 기반 개발 권장
    Spring은 인터페이스를 활용하여 유연성을 확보하는 것을 권장합니다.
    인터페이스를 사용하면 다양한 구현체를 쉽게 교체할 수 있으므로 테스트 용이성도 높아집니다.
    이러한 이유로 인터페이스가 있는 경우, Spring은 JDK 동적 프록시를 기본으로 사용합니다.

## 인터페이스가 없는 경우 CGLIB를 사용하는 이유
- 인터페이스 없이도 AOP를 적용해야 할 때 만약 인터페이스가 없는 구체 클래스에 AOP를 적용하려면, JDK 동적 프록시로는 구현할 수 없습니다. 이때 CGLIB를 사용하여 프록시 객체를 만들어야 합니다.
    CGLIB는 클래스 자체를 상속하여 프록시를 생성하기 때문에 인터페이스가 없어도 동작합니다.


## 정리: 인터페이스 유무에 따른 프록시 선택 이유
- 자바 철학 반영 (다형성)
인터페이스가 있는 경우, JDK 동적 프록시를 사용하여 가볍고 효율적으로 프록시를 생성합니다.
인터페이스를 통한 다형성 활용이 가능하며, 구현체 교체가 용이합니다.

- 불필요한 비용 최소화
인터페이스가 있는 경우 굳이 상속 기반 CGLIB를 사용할 필요가 없습니다.
JDK 동적 프록시가 성능 면에서 더 우수합니다.

- 구체 클래스만 있을 때의 대안
인터페이스가 없는 경우 JDK 동적 프록시 사용이 불가능하므로,
CGLIB를 사용하여 구체 클래스를 상속하여 프록시를 생성합니다.

## Spring의 권장 사항
Spring은 인터페이스 기반 개발을 선호하므로, 인터페이스가 있는 경우 JDK 동적 프록시를 우선 사용합니다.
인터페이스가 없으면 CGLIB를 사용하는 방식으로 처리합니다.