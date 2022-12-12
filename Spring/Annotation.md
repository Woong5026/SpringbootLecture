### @pathvariable

URL 경로에 변수를 넣어주는것(경로의 특정 위치 값이 고정되지 않고 달라질 때 사용)

@RequestMapping 어노테이션 값으로 {템플릿변수} 를 사용합니다.

@PathVariable 어노테이션을 이용해서 {템플릿 변수} 와 동일한 이름을 갖는 파라미터를 추가하면 됩니다.

<br/>

### @Transactional

데이터베이스의 상태를 변경하는 작업 또는 한번에 수행되어야 하는 연산들을 의미한다.

begin, commit 을 자동으로 수행해준다.

예외 발생 시 rollback 처리를 자동으로 수행해준다.

적용된 범위에서는 트랜잭션 기능이 포함된 프록시 객체가 생성되어 자동으로 commit 혹은 rollback을 진행해준다.

<br/>


### @Optional

Java8부터 Optional<T>클래스를 사용해 NullPointerException(이하 NPE)를 방지할수 있도록 했다.

Optional<T> 클래스는 Integer나 Double클래스처럼 T타입의 객체를 포장해주는 래퍼클래스이다.

Optional<T>는 null이 올수 있는 값을 감싸는 Wrapper클래스로, 참조하더라도 NPE가 발생하지 않도록 도와준다. <br/>
즉, 예상치못한 NPE예외를 제공되는 메소드로 간단히 회피할 수 있어 복잡한 조건문 없이도 null값으로 인해 발생하는 예외를 처리할 수 있다.
  
<br/>
  
### @Value   

DB 연결에 필요한 정보(계정 정보)나 노출되기 민감한 값들을 하드 코딩하게 된다면, 여러 가지 이슈에 휘말릴 수 있다. <br/>
(깃허브 같이 공유 레퍼지토리에 그대로 코드와 함께 유출될 것이다.) 
  
또한 개발 시엔 로컬에 맞는 환경으로 세팅을 했지만, <br/>
클라우드 서버에 올린다거나 배포 환경으로 전환될 때, 직접 해당 코드를 수정해야 하는 번거로움이 있다. <br/>
이러한 이슈들을 막기 위해 민감한 정보나, 메타정보들은 파일로 따로 빼두어 관리하게 된다. (수정과 관리가 용이하기 때문) <br/>
이러한 이유로 따로 빼둔 설정 파일을 필요한 곳에 주입시켜주는 어노테이션이 @value 다.  
  
  
* 사용법
  
```java
  
  @RestController
public class ValueController {

    @Value("${admin.name}") 
    private String adminName;

   //... 이하 코드 생략
   
}
  
```  
  
그러나 위 코드만 가지고는 admin.name의 값을 가져올 수 없다.

왜냐하면 @value은 주입을 시켜주는 친구지 찾아주는 친구가 아니다. <br/>
즉, 해당 어노테이션한테 '이런 설정 파일에 admin.name 키값이 있네?'라는 것을 알려주기 위한 사전 작업이 필요하다.   
  
  
```java  
  
  @RestController
  @PropertySource("classpath:sample.properties") // 이부분
  public class ValueController {

    @Value("${admin.name}") 
    private String adminName;

   //... 이하 코드 생략
   
}
  
  
```  
  
위처럼 직접 사용할때는 application.properties과 같은 파일의 경로를 정해줘야 한다
  
  
<br/>
  
  
아래처럼 엔티티에 두고 toString 메서드를 사용해서 사용하는 것도 가능
  
* application.properties
  
```java
  
custom.myname=blackdog
custom.myage=10
custom.mytel=01000001122  
  
  
```

  
  *  User 클래스
  
```java


import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component
public class User {
    @Value("${custom.myname}")
    private String name;

    @Value("${custom.myage}")
    private int age;

    @Value("${custom.mytel}")
    private String tel;

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", tel='" + tel + '\'' +
                '}';
    }
}
  
  
```
  
* 테스트코드
  
```java
  

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class SpringbootTutorialApplicationTests {

	@Autowired
	User user;

	@Test
	void contextLoads() {
	}


	@Test
	public void userTest(){
		System.out.println(user.toString());
		System.out.println(new User().toString());
	}

}
  
  
```
  
  
  
* 결과

User{name='blackdog', age=10, tel='01000001122'} <br/>
User{name='null', age=0, tel='null'}
