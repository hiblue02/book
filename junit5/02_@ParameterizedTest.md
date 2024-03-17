## ParameterizedTest
- 서술된 파라미터들을 1개씩 넣어 테스트를 실행시켜주는 기능이다.
  
## 파라미터 셋팅하기
### @ValueSource
- short, byte, int, long, float,  double,  char, boolean, java.lang.String, java.lang.Class 를 사용할 수 있다. 
- ~~~java
  @ParameterizedTest
  @ValueSource(ints = { 1, 2, 3 })
  void testWithValueSource(int argument) {
      assertTrue(argument > 0 && argument < 4);
  }
  ~~~
### @NullSource, @EmptySource, @NullAndEmtpySource
- @NullSource : 파라미터에 null을 넣어 테스트한다.
- @EmptySource : 파라미터에 빈 객체(ex: 빈 컬랙션, 빈 문자열(""))을 넣어 테스트한다.
- @NullAndEmtpySource : null과 빈 객체를 넣어 테스트한다. 
- @ValueSource와 함께 쓸 수 있다.
- ~~~java
  @ParameterizedTest
  @NullAndEmptySource
  @ValueSource(strings = { " ", "   ", "\t", "\n" })
  void nullEmptyAndBlankStrings(String text) {
      assertTrue(text == null || text.trim().isEmpty());
  }
  ~~~
### @EnumSource
- 기술된 Enum 클래스의 값을 파라미터로 넣어 테스트를 실행한다.
- 속성 - `names`: 기술된 이름의 Enum만 넣어 실행한다. (mode 값에 따라 포함할지, 제외할지가 결정된다.)
- 속성 - `mode`:
    - EXCLUDE : names를 제외한 Enum만 넣어 실행한다.
    - MATCH_ALL : names의 문자열(정규식)과 일치하는 Enum만 넣어 실행한다.
    - INCLUDE : mode의 default. names에 기술된 Enum만 넣어 실행한다.
- ~~~java
  @ParameterizedTest
  @EnumSource(mode = EXCLUDE, names = { "ERAS", "FOREVER" })
  void testWithEnumSourceExclude(ChronoUnit unit) {
      assertFalse(EnumSet.of(ChronoUnit.ERAS, ChronoUnit.FOREVER).contains(unit));
  }
  ~~~
### @MethodSource
- 기술된 팩토리 메소드의 반환 값을 파라미터로 넣어 테스트를 실행한다. (팩토리 메소드는 Stream이나 Collection, Array를 반환해야하고, static해야 한다.)
- ~~~java
  @ParameterizedTest
  @MethodSource("stringProvider")
  void testWithExplicitLocalMethodSource(String argument) {
      assertNotNull(argument);
  }
  
  static Stream<String> stringProvider() {
      return Stream.of("apple", "banana");
  }
  ~~~
## 메소드 이름 표시하기
~~~java
@ParameterizedTest(name = "For example, year {0} is not supported.")
@ValueSource(ints = { -1, -4 })
void if_it_is_negative(int year) {
}
~~~
~~~
For example, year -1 is not supported. [OK]
For example, year -4 is not supported. [OK]
~~~
