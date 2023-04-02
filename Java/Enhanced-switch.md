# 향상된 switch 문
Java 12에서 switch 문을 더 간결하게 쓸 수 있도록 시험 업데이트를 해주었고, Java 14에서 yield 문과 함께 정식 기능으로 추가되었다.

이를 설명하기 위해 다음 열거형 타입을 전제로 한다.
```java
enum Day { MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY }
```
## Arrow case label
업데이트 이전, switch 문은 다음과 같았다.
```java
switch (day) {
    case MONDAY:
    case FRIDAY:
    case SUNDAY:
        System.out.println(6);
        break;
    case TUESDAY:
        System.out.println(7);
        break;
    case THURSDAY:
    case SATURDAY:
        System.out.println(8);
        break;
    case WEDNESDAY:
        System.out.println(9);
        break;
}
```
Arrow case label을 이용해 아래와 같이 변화 시킬 수 있다.
```java
switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> System.out.println(6);
    case TUESDAY -> System.out.println(7);
    case THURSDAY, SATURDAY -> System.out.println(8);
    case WEDNESDAY -> System.out.println(9);
}
```
사용법은 람다식과 비슷하다. 

두 줄 이상 표현하려면 화살표 이후 중괄호로 묶어주어야 한다. 마지막에 break; 을 사용한 것과 같은 효과를 준다.

## Arrow case label을 이용한 변수 대입
변수에 switch 문에 따른 값을 대입하는 코드는 아래와 같았다.
```java
int numLetters;
switch (day) {
    case MONDAY:
    case FRIDAY:
    case SUNDAY:
        numLetters = 6;
        break;
    case TUESDAY:
        numLetters = 7;
        break;
    case THURSDAY:
    case SATURDAY:
        numLetters = 8;
        break;
    case WEDNESDAY:
        numLetters = 9;
        break;
    default:
        numLetters = -1;
}
```
Arrow case label을 이용해 아래와 같이 변화시킬 수 있다.
```java
int numLetters = switch (day) {
    case MONDAY, FRIDAY, SUNDAY	-> 6;
    case TUESDAY -> 7;
    case THURSDAY, SATURDAY -> 8;
    case WEDNESDAY -> 9;
};
```

## yield
yield는 switch 문의 return 같은 것이다. 아래 코드를 보자.
```java
int numLetters = switch (day) {
    case MONDAY, FRIDAY, SUNDAY	-> {
        System.out.print("Six");
        yield 6;
    }
    case TUESDAY -> 7;
    case THURSDAY, SATURDAY -> 8;
    case WEDNESDAY -> 9;
};
```
day가 MONDAY, FRIDAY 또는 SUNDAY 일 경우, numLetters 값은 6으로 초기화된다.

yield는 중괄호 내에서만 사용이 가능하다. 그래서 나머지 case에는 따로 붙이지 않았다.

참고로, case: 에서도 사용 가능하다.
```java
int numLetters = switch (day) {
    case MONDAY, FRIDAY, SUNDAY:
        System.out.print("Six ");
        yield 6;
    case TUESDAY:
        yield 7;
    case THURSDAY, SATURDAY:
        yield 8;
    case WEDNESDAY:
        yield 9;
};
```

# 열거형을 전제한 이유
향상된 switch 문에서는 default 문이 없을 경우, 값이 나올 수 있는 모든 경우를 case로 커버해야 한다. 무슨 말이냐면, 만약 switch(day) 에서 day가 String 타입이었다면 default가 없을 경우, 모든 String 값으로 case를 만들어야 한다는 뜻이다. 다른 예시로 int 형이었으면 [-2^31 ~ 2^31 - 1]을 case로 전부 커버해야 한다.

따라서 열거형으로 만들면 이것이 해결된다.
# 참조
* [[Java 14] 개선된 switch 문(Enhanced Switch Expressions)](https://congcoding.tistory.com/73)
- - -
#### 최초 생성: 2023-04-02