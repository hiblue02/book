## @TestFactory
- 동적으로 테스트 코드를 작성해야 할 때 사용한다.
    - 하나의 테스트에 대해서 입력값을 여러개 넣어줘야 할때 사용하면 유용하다 (ex @ParameterizedTest)
    - 테스트는 람다식으로 구현하는데, DisplayName과 DisplayTest를 쌍으로 넣어줘야 한다. (시나리오 별로 테스트를 묶을 수 있어 테스트 가독성이 올라간다.)
- 런타임에 생성되므로, 특정 조건에 따라 테스트를 생성하고 실행할 수 있다.
- @TestFactory 메소드의 리턴값은 `DynamicNode`, `Stream`, `Collection`, `Iterable`, `Iterator`, array of `DynamicNode`여야 한다.
- @BeforeEach와 @AfterEach와 같은 생명주기 관련 기능은 개별로 실행되지 않는다. (TestFactory 메소드 최초/최종 실행될때 1번만 동작한다.) 
~~~java
@DisplayName("시나리오 테스트")
@TestFactory
Stream<DynamicTest> dynamicTestsFromCollection() {
    return Stream.of(
        dynamicTest("객체를 생성한다", () -> {
            // when
            createObject("name1");
            createObject("name2");

            // then
            List<Object> objects = getObjects();
            assertThat(objects.size()).isEqualTo(2);
        }),
    
        dynamicTest("객체 목록을 불러온다.", () -> {
            // when
            List<Object> objects = getObjects();
 
            // then
            assertThat(objects.size()).isEqualTo(2);
        }),
      
        ...
    );
}
~~~
https://tecoble.techcourse.co.kr/post/2020-07-31-dynamic-test/

~~~java
    @TestFactory
    Stream<DynamicTest> testFactory() {
        Iterator<String> strings = List.of("첫번째", "두번째", "세번째", "네번째").iterator();

        return DynamicTest.stream(strings, (input) -> input + " 테스트",
                (input) -> Assertions.assertThat(input).isNotBlank());
    }
~~~
~~~
testFactory()
ㄴ 첫번째 테스트
ㄴ 두번째 테스트
ㄴ 세번째 테스트
ㄴ 네번째 테스트
~~~
