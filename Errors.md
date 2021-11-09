### could not initialize proxy

요 에러는 말그대로 지연 로딩을 하려는데 이미 세션이 사라져서 지연 로딩을 못할 때 발생하는 에러다.

그래서 보통은 @*ToMany 관계로 되어 있어 기본 FetchType이 Lazy인 멤버를 읽어올 때 발생하며, 그냥 쉬운 해결 법으로는 FetchType.EAGER를 명시해서 즉시 로딩으로 읽어오게 하면 해결될 수도 있다.

하지만, 비즈니스 요구가 아니라 단순히 LazyInitialization 에러를 피하기 위해 즉시 로딩을 선택하는 것은 좋은 해법이 아니다.

요는 지연 조회 시점까지 세션을 유지 시켜주면 되는데 가장 쉽게는 해당 조회가 포함된 메서드에 @Transactional(조회일 때는 @Transactional(readOnly = true))를 붙여주면 된다.
