# @Test
## Test 메소드
### @Test
- 메소드 레벨 애노테이션
- 테스트할 메소드 위에 적는다.
- ~~~java
  class StandardTests {
      @Test
      void succeedingTest() {
      }
  }
  ~~~
### @RepeatedTest
- 테스트를 N번 반복한다. `@Execution(SAME_THREAD)`와 함께쓰면 병렬 실행된다.
### @ParameterizedTest
- 파라미터들로, 테스트를 여러번 실행시켜준다.
- `@ValueSource`, `@CsvSource`, `@EnumSource` 등과 함께 사용한다. 
- ~~~java
  @ParameterizedTest
  @ValueSource(strings = { "racecar", "radar", "able was I ere I saw elba" })
  void palindromes(String candidate) {
       assertTrue(StringUtils.isPalindrome(candidate));
  }
  ~~~
### @TestFactory
- Dynamic Test(동적 테스트, 시나리오 테스트)를 수행한다.
- 앞선 테스트의 데이터를 사용할 수 있다.
- @TestFactory가 붙은 테스트는 DynamicNode, a Stream, Collection, Iterable, Iterator, array of DynamicNode 를 반환해야 한다. 
- ~~~java
     @TestFactory
    Collection<DynamicTest> dynamicTestsFromCollection() {
        return Arrays.asList(
            dynamicTest("1st dynamic test", () -> assertTrue(isPalindrome("madam"))),
            dynamicTest("2nd dynamic test", () -> assertEquals(4, calculator.multiply(2, 2)))
        );
    }
  ~~~
### @TestTemplate
- `TestTemplateInvocationContextProvider`와 함께 사용한다.
- 어떤 테스트에 특정 기능(`TestTemplateInvocationContextProvider`)을 붙여주고 싶을 때 사용한다.
- ` @ParameterizedTest`와 `@RepeatedTest`도 TestTemplate이다. 
- ~~~java
  @TestTemplate
  @ExtendWith(MyTestTemplateInvocationContextProvider.class)
    void testTemplate(String fruit) {
    assertTrue(fruits.contains(fruit));
  }
  ~~~    
### @Disabled
- 사용하지 않는 테스트 메소드일 때 사용한다. 
- ~~~java
 @Test
 @Disabled("for demonstration purposes")
 void skippedTest() {
       // not executed
 }
 ~~~
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
