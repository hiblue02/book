# @Test
## Test 메소드
1. @Test
    - 메소드 레벨 애노테이션
    - 테스트할 메소드 위에 적는다.
    ~~~java
    class StandardTests {
        @Test
        void succeedingTest() {
        }
    }
    ~~~
2. @RepeatedTest
    - 테스트를 N번 반복한다. `@Execution(SAME_THREAD)`와 함께쓰면 병렬 실행된다.
3. @ParameterizedTest
4. @TestFactory
5. @TestTemplate
    - `TestTemplateInvocationContextProvider`와 함께 사용한다.
    - 어떤 테스트에 특정 기능(`TestTemplateInvocationContextProvider`)을 붙여주고 싶을 때 사용한다.
    - ` @ParameterizedTest`와 `@RepeatedTest`도 TestTemplate이다. 
      ~~~java
      @TestTemplate
      @ExtendWith(MyTestTemplateInvocationContextProvider.class)
        void testTemplate(String fruit) {
        assertTrue(fruits.contains(fruit));
      }
      ~~~
    
6. @Disabled
   - 사용하지 않는 테스트 메소드일 때 사용한다. 
  ~~~java
   @Test
    @Disabled("for demonstration purposes")
    void skippedTest() {
        // not executed
    }
  ~~~
3. 
4. 
## Life Cycle 메소드
### @TestInstance
테스트가 다른 작업에 영향을 받게 하고 싶지 않을때 사용하는 **클래스** 레벨 에노테이션이다. 속성(LifeCycle.class)으로 테스트 분리 단위를 지정할 수 있다. 
분리 단위를 기준으로 인스턴스를 만든다.
### @BeforeAll
클래스의 테스트 메소드가 돌기 전 작업을 실행해주는 **메소드** 레벨 애노테이션이다. @TestInstance(LifeCycle.PER_METHOD)면 사용할 수 없다.
### @AfterAll
클래스의 테스트 메소드가 돈 후 작업을 실행해주는 **메소드** 레벨 애노테이션이다. @TestInstance(LifeCycle.PER_METHOD)면 사용할 수 없다.
### @BeforeEach
클래스 내부의 메소드 1개가 돌기 전 작업을 실행해주는 **메소드** 레벨 애노테이션이다. 
### @AfterEach
클래스 내부의 메소드 1개가 돈 후 작업을 실행해주는 **메소드** 레벨 애노테이션이다. 
