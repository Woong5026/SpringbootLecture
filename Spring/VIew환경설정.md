```java
@Controller
public class HelloController {

    @GetMapping("hello")
    public String hello(Model model){
        model.addAttribute("data","hello");
        return "hello";
    }
}

```
model에 데이터를 실어서 view 에 넘기겠다 , name이 date란 이름의 값을 hello로 넘기겠다
return: templates 밑에 생성

타임리프 기본 경로
resources:templates/ +{ViewName}+ .html

서버사이드 랜더링 없이 순수하게 정적인 페이지를 구동하고 싶으면 static 폴더에 생성하면 된다
