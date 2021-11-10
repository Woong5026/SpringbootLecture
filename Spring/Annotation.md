### @pathvariable

URL 경로에 변수를 넣어주는것(경로의 특정 위치 값이 고정되지 않고 달라질 때 사용)

@RequestMapping 어노테이션 값으로 {템플릿변수} 를 사용합니다.

@PathVariable 어노테이션을 이용해서 {템플릿 변수} 와 동일한 이름을 갖는 파라미터를 추가하면 됩니다.

### 

### @Transactional

데이터베이스의 상태를 변경하는 작업 또는 한번에 수행되어야 하는 연산들을 의미한다.

begin, commit 을 자동으로 수행해준다.

예외 발생 시 rollback 처리를 자동으로 수행해준다.

적용된 범위에서는 트랜잭션 기능이 포함된 프록시 객체가 생성되어 자동으로 commit 혹은 rollback을 진행해준다.


### @Optional

Java8부터 Optional<T>클래스를 사용해 NullPointerException(이하 NPE)를 방지할수 있도록 했다.

Optional<T> 클래스는 Integer나 Double클래스처럼 T타입의 객체를 포장해주는 래퍼클래스이다.

Optional<T>는 null이 올수 있는 값을 감싸는 Wrapper클래스로, 참조하더라도 NPE가 발생하지 않도록 도와준다.

즉, 예상치못한 NPE예외를 제공되는 메소드로 간단히 회피할 수 있어 복잡한 조건문 없이도 null값으로 인해 발생하는 예외를 처리할 수 있다.
  

  
