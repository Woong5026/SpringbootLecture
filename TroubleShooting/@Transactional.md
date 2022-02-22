하지만 자세히 읽어보니 actual transaction available for current thread 라는 워딩이 있었고, 내가 하는 작업에 트랜잭션 선언을 했었나 확인해보니 역시나..

@Transactional 어노테이션을 빼먹었었다.


**기본적으로 JPA는 transaction을 기반으로 작동**하게 되어있다.


transaction 단위에 따라 1차캐시영역에 있는 객체들이 db에 flush되어 영속화되기 때문이다.

 

하지만 그러한 영속작업을 하는 persist() 메소드에 객체가 들어갔으나 가능한 transaction이 존재하지 않았기에 저런 에러를 낸것이다.

 

고로 서비스 혹은 클래스에 미리 @Transactional을 선언해두자!!

 

클래스에는 @Transactional(readOnly = true) / 메소드에는 @Transactional 을 붙여 read 트랜잭션과 write 트랜잭션을 구분하는 것도 잊지말자!
