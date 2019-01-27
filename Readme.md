# SpEL (스프링 Expression Language)
## 스프링 EL​이란?
https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#expressions
- 객체 그래프를 조회하고 조작하는 기능을 제공한다
- [Unified EL​](https://docs.oracle.com/javaee/5/tutorial/doc/bnahq.html)과 비슷하지만, 메소드 호출을 지원하며, 문자열 템플릿 기능도 제공한다
- OGNL, MVEL, JBOss EL 등 자바에서 사용할 수 있는 여러 EL이 있지만, SpEL은
  모든 스프링 프로젝트 전반에 걸쳐 사용할 EL로 만들었다
- 스프링 3.0 부터 지원

## SpEL 구성
- ExpressionParser​​ parser = new SpelExpressionParser()
- StandardEvaluationContext context = new Standard​EvaluationContext​​(bean)
- Expression expression = parser.parseExpression(“SpEL 표현식”)
- String value = expression.getvalue(context, String.class)

## 문법
- #{“표현식"}
- ${“프로퍼티"}
- 표현식은프로퍼티를가질수있지만,반대는안됨.
  - #{${my.data} + 1}
- [레퍼런스​](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#expressions-language-ref) 참고

## 실제로 어디서 쓰나?
- @Value 애노테이션
- @ConditionalOnExpression 애노테이션
- [스프링 시큐리티](https://docs.spring.io/spring-security/site/docs/3.0.x/reference/el-access.html)
  - 메소드 시큐리티, @PreAuthorize, @PostAuthorize, @PreFilter, @PostFilter
  - XML 인터셉터 URL 설정
  - ...
- [스프링 데이터](https://spring.io/blog/2014/07/15/spel-support-in-spring-data-jpa-query-definitions)
  - @Query 애노테이션
- [Thymeleaf](https://blog.outsider.ne.kr/997)
- ...

## 예제
```java
@Component
public class AppRunner implements ApplicationRunner {

    @Value("#{1 + 1}")
    int value;

    @Value("#{'hello ' + 'world'}")
    String greeting;

    @Value("#{1 eq 1}")
    boolean trueOrFalse;

    @Value("hello")
    String hello;

    @Value("${my.value}")
    int myValue;

    @Value("#{${my.value} eq 100}")
    boolean isMyValue100;

    @Value("#{'spring'}")
    String spring;

    @Value("#{sample.data}")
    int sampleData;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println("=====================");
        System.out.println(value);
        System.out.println(greeting);
        System.out.println(trueOrFalse);
        System.out.println(hello);
        System.out.println(myValue);
        System.out.println(isMyValue100);
        System.out.println(spring);
        System.out.println(sampleData);

        ExpressionParser parser = new SpelExpressionParser();
        // 이미 Expression이므로 {} 없이 그냥 작성하면
        Expression expression = parser.parseExpression("2 + 100");
        // 해당하는 타입으로 변환할 때 Conversion 서비스를 사용함
        Integer value = expression.getValue(Integer.class);
        System.out.println(value);
    }
}
```