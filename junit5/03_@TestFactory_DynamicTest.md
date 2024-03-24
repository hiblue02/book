## @TestFactory
- 동적으로 테스트 코드를 작성해야 할 때 사용한다.
- 런타임에 생성되므로, 특정 조건에 따라 테스트를 생성하고 실행할 수 있다.
- @TestFactory 메소드의 리턴값은 `DynamicNode`, `Stream`, `Collection`, `Iterable`, `Iterator`, array of `DynamicNode`여야 한다. 
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
