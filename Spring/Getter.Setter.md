### 자바의 접근 제한자

```java
private : 같은 클래스 내에서만 접근 가능

default : 같은 패키지 내에서만 접근 가능

protected : 같은 패키지내 또는 자손 클래스일 경우 접근 가능

public : 제한 없음
```

### Setter

아래 코드에서 jinhwan이라는 사람의 나이에 -1이 대입되었다.

이상한 값이 age에 들어오면 문제를 파악하기 쉽지 않을 것이다.

```java

class Application{
    public static void main(String[] args){
        Person jinhwan = new Person();
        jinhwan.age = -1;
        jinhwan.howOld();
    }
}

class Person{
    int age;

    void howOld(){
        System.out.println(age);
    }
}

```

이번에는 age를 private으로 해서 직접 값 할당을 못하게 제한하고, 메소드를 이용해서 age에 -1을 대입했다. 

단, 이때는 나이는 음수가 될 수 없으므로 음수가 입력되면 0을 대입하도록 하여 나이에 음수가 대입되는 상황을 처리하였다.

이런식으로 잘못된 입력에 기본 값을 대입하거나, 에러를 출력하고 시스템이 종료되게 하는 것으로 문제를 확인하는 처리가 대표적이다. 

```java

class Application{
    public static void main(String[] args){
        Person jinhwan = new Person();
        jinhwan.setAge(-1);
        jinhwan.howOld();
    }
}

class Person{
    private int age;

    public void setAge(int age) {
        if(age >=0){
            this.age = age;
        }
        else
            this.age = 0;
    }
    
    void howOld(){
        System.out.println(age);
    }
}

```

setter는 이렇게 변수의 값 대입이 여러 곳에서, 제한 없이 가능한 것을 접근 제한자로 막고, 

접근 범위에 한해서 메소드로 대입전 값을 처리 후 대입되게 하기 위해 사용된다.


### Getter / 은닉성

큰 프로젝트에서 엄청 긴 코드를 다룬다고 생각해보자. 다른 사람의 코드 속 모든 변수 값을 가져올 필요도 없을 뿐더러, 가져올 수 있는 것이 마냥 편한 일은 아닐 것이다.

자동차 게임을 개발하는 상황을 가정해보자. 만약 당신이 자동차가 충돌 시 튕겨나가는 이벤트 처리를 만드는 일을 담당한다면, 

충돌하는 자동차의 색상이나, 브랜드를 알야할까? 아마 불필요한 정보에 신경이 빼앗길 것이다.

```java

class Application{
    public static void main(String[] args){
        Person jinhwan = new Person();
    }
}

class Person{
    int age;
    int name;
    String hobby;
    int hobby_id;
    String school;
    int school_id;
    String phoneNumber;
    int gender;
    int pw;
}

```

자동차 객체를 가져다 쓸 때 차종, 색, 휠, 차량 넘버, 제조사 등의 잡다한 정보는 자동차를 구현한 사람들의 몫이고, 다른 사람에게 방해만 될 뿐이다.

위 코드에서 age와 name을 제외하곤, 다른 사람에게 불필요한 정보이다. 

```java

class Person{
    private int age;
    private int name;
    
    private String hobby;
    private int hobby_id;
    private String school;
    private int school_id;
    private String phoneNumber;
    private int gender;
    private int pw;

    public int getAge() {
        return age;
    }

    public int getName() {
        return name;
    }

    public void setAge(int age) {
        if(age >=0){
            this.age = age;
        }
        else
            this.age = 0;
    }
}

이번에는 변수의 접근을 private처리해서 해당 클래스 안에서만 노출되게 바꾸고, 

다른 사람들도 사용할 필요가 있는 주요 변수 age, name만 getter을 이용해서 드러냈다.  

> 이렇게 변수들의 외부 노출을 제한하고, 노출 범위를 정해주는 것이 Getter고, 그러한 속성이 은닉성이다.

```


