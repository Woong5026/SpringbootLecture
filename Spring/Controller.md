### String

+ Spring + View template을 사용할 때 흔히 사용하는 타입입니다.

```java
@GetMapping("/test")
public String test(Model model) {
    model.addAttribute("data", data);
    return "/test/data";
}
```

Model 안에 데이터를 key, value 값으로 담고, return 타입을 String 값으로 뷰의 이름을 지정해주면 뷰로 데이터가 전송되게 됩니다. 
뷰에서는 해당 데이터의 key값을 객체 이름으로 하여 그 안에 데이터를 조회합니다.

<br>

### ModelAndView

ModelAndView는 Model과 View를 동시에 설정가능한 객체입니다. Controller는 ModelAndView 객체만을 반환하지만 Model과 View 모두를 가지고 반환합니다. 
생성자로 뷰의 이름을 저장하거나 setViewName() 매서드를 사용하여 뷰 네임을 지정하고, addObject() 메서드로 데이터를 저장합니다.

```java
@GetMapping("/test")
public ModelAndView test() {
    ModelAndView mav = new ModelAndView("test/viewPage");
    modelAndView.addObject("data", "Baeldung");
    return mav;
}
```
<br>

### redirect

redirect: 접두어를 붙이게 되는 경우 지정한 페이지로 리다이렉트가 되게 됩니다. 리다이렉트는 두 가지 방식으로 입력할 수 있습니다.

redirect: /api/test -> 현재 서블릿 컨텍스트에 대한 **상대적인 경로**로 리다이렉트를 하게 됩니다.
redirect: http//:localhost:8080/api/test -> 와 같이 전체 경로를 적는 경우 **절대 경로**로 리다이렉트를 하게 됩니다.

<br>
### void

Spring에서는 뷰의 이름을 지정해주지 않아도 Spring이 해당 url을 보고 뷰 네임을 자동으로 결정하는 기능을 제공


```java
@GetMapping("/test/address")
public void void(Model model) {
    model.addAttribute("user", data);
}
```

이 클래스는 요청 url에서 제일 앞의 '/'와 확장자를 제외한 나머지 부분을 뷰 네임으로 지정하게 됩니다
<br>

### Map

Map 형태로도 리턴이 가능합니다. 모델과는 조금 다른 점은 개별로 모델이 등록되기 때문에 뷰에서 key.value 형태로 데이터에 접근했던 
Model과는 다르게 value만으로 데이터에 접근해야 합니다

```java
@GetMapping("/test")
public Map<String, String> test() {
    return data;
}
```
<br>

```java
<!DOCTYPE html>
<html>
    <head>
        <title>Document</title>
    </head>
    <body>
        <p>${name}</p>
        <p>${age}</p>
    </body>
</html>
```

<br>

### RestController
RestController는 Restful Controller의 준말로써, Spring MVC Controller에 @ResponseBody가 추가된 것입니다.

@Controller 대신 @RestController 애노테이션을 붙여줌으로써 @Controller와 @ResponseBody 애노테이션 두 가지를 모두 사용하는 효과를 낼 수 있습니다.
제이슨 데이터를 받을 때 사용

```java
@RestController
@RequestMapping("/test")
public class TestController {

    @GetMapping
    public Account test(Account account) {
        return account;
    }
}
```


