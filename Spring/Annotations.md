# 스프링 애노테이션들

## @Configuration

어떤 클래스가 하나 이상의 @Bean 메소드를 정의하고 있고, 스프링 컨테이너가 이를 처리해 빈을 찾아야 할 때 사용한다.

예:

```
// 설정 클래스
@Configuration
public class AppConfig {
  @Bean
  public MyBean myBean() {
      // 빈을 인스턴스화하고 설정해서 반환함
  }
}

// 사용하는 방법
AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext();
ctx.register(AppConfig.class);
ctx.refresh();
MyBean myBean = ctx.getBean(MyBean.class);
// 빈을 사용한다
```
