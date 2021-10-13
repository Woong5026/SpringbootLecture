# JPA

### @OneToMany 속성에는 다음과 같은 것들이 있습니다.

+ targetEntity : 관계를 맺을 Entity Class를 정의합니다.
+ cascade : 현 Entity의 변경에 대해 관계를 맺은 Entity도 변경 전략을 결정합니다.
+ fetch : 관계 Entity의 데이터 읽기 전략을 결정
+ mappedBy : 양방향 관계 설정시 관계의 주체가 되는 쪽에서 정의
+ orphanRemoval : 관계 Entity에서 변경이 일어난 경우 DB 변경을 같이 할지 결정합니다. cascade와 다른것은 cascade는 JPA 레이어 수준이고 이것은 DB레이어에서 처리합니다. 기본은 false입니다.




