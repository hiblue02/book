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
### @ArgumentsSource
- 기술된 클래스의 provideArguements 메소드를 실행해 파라미터 데이터를 생성한다. (`ArgumentsProvider`를 상속받은 클래스여야 한다.)
- ~~~java
  public class MyArgumentsProvider implements ArgumentsProvider {
  
      @Override
      public Stream<? extends Arguments> provideArguments(ExtensionContext context) {
          return Stream.of("apple", "banana").map(Arguments::of);
      }
  }
  ~~~
- ~~~java
  @ParameterizedTest
  @ArgumentsSource(MyArgumentsProvider.class)
  void testWithArgumentsSource(String argument) {
      assertNotNull(argument);
  }
  ~~~
 ### @CsvSource
- 기술된 csv 형태의 값을 분리해넣어 테스트를 실행한다. (파라미터가 N개인 테스트 메소드를 실행할 수 있다.)
- row(데이터쌍)는 " "로 묶고, common(,)로 분리한다.
- ~~~java
  @ParameterizedTest
  @CsvSource({
      "apple,         1",
      "banana,        2",
      "'lemon, lime', 0xF1",
      "strawberry,    700_000"
  })
  void testWithCsvSource(String fruit, int rank) {
      assertNotNull(fruit);
      assertNotEquals(0, rank);
  }
  ~~~
  - 속성
  - `useHeadersInDisplayName`: CsvSource에 헤더가 있으면 true로 설정한다. (csv의 header를 displayName의 파라미터로 사용할 수 있다. )
  - `textBlock`: textBlock에 csv 데이터를 넣으면 ""이 없이 enter(줄바꿈)을 인신해 데이터 row를 분리한다.
    - ~~~java
        @ParameterizedTest(name = "[{index}] {arguments}")
        @CsvSource(useHeadersInDisplayName = true, textBlock = """
            FRUIT,         RANK
            apple,         1
            banana,        2
            'lemon, lime', 0xF1
            strawberry,    700_000
            """)
        void testWithCsvSource(String fruit, int rank) {
            // ...
        }
      ~~~
    - `delimiter`: CsvSource에 기술된 데이터를 나누는 문자, default는 common(,)다.
    - `nullValues`: CsvSource에 nullValues로 정의된 문자열은 null로 판단한다.
    - `ignoreLeadingAndTrailingWhitespace`: true로 설정하면 trim(앞뒤 공백 제거)된다.
 ### @CsvFileSource 
 - @CsvSource와 같으나, `resources` 속성에 기술된 Csv파일을 불러온다.
 - 속성:
   - `numLinesToSkip`: 정의된 1~N번째 라인을 스킵한다. (헤더를 스킵하고 싶으면, `numLinesToSkip=1`로 선언한다.)
- ~~~java
  @ParameterizedTest
  @CsvFileSource(resources = "/two-column.csv", numLinesToSkip = 1)
  void testWithCsvFileSourceFromClasspath(String country, int reference) {
      assertNotNull(country);
      assertNotEquals(0, reference);
  }
  ~~~    
## 파라미터 변환하기
### 기본 변환 
- https://junit.org/junit5/docs/current/user-guide/#writing-tests-parameterized-tests-argument-conversion-implicit
### String 1개인 팩토리 메소드(생성자)
- 파라미터가 String 1개인 non-private static 팩토리 메소드나, 생성자가 있으면 알아서 junit이 자동으로 사용자 정의 type으로 변환해준다.
- ~~~java
  @ParameterizedTest
  @ValueSource(strings = "42 Cats")
  void testWithImplicitFallbackArgumentConversion(Book book) {
      assertEquals("42 Cats", book.getTitle());
  }
  ~~~
  ~~~java
  public class Book {
  
      private final String title;
  
      private Book(String title) {
          this.title = title;
      }
  
      public static Book fromTitle(String title) {
          return new Book(title);
      }
  }
  ~~~
### 내멋데로 타입 변환하기 (@ConvertWith)
- `ArgumentConverter`를 상속받은 클래스를 만들고 convert를 오버라이딩한다. (내가 쓰고 싶은 변환 로직을 짠다.)
- 테스트 메서드 파라미터 선언 앞에 `@ConverWith`로 사용할 converter를 지정한다.
  - ~~~java
    @ParameterizedTest
    @EnumSource(ChronoUnit.class)
    void testWithExplicitArgumentConversion(
            @ConvertWith(ToStringArgumentConverter.class) String argument) {
    
        assertNotNull(ChronoUnit.valueOf(argument));
    }
    ~~~ 
- `SimpleArgumentConverter`: 파라미터 1개를 여러 타입으로 반환하고 싶을때 사용한다. (String은 Enum이나 String으로 바꾸고 싶을 때)
  - ~~~java
    public class ToStringArgumentConverter extends SimpleArgumentConverter {
        @Override
        protected Object convert(Object source, Class<?> targetType) {
            assertEquals(String.class, targetType, "Can only convert to String");
            if (source instanceof Enum<?>) {
                return ((Enum<?>) source).name();
            }
            return String.valueOf(source);
        }
    }
    ~~~
- `TypedArgumentConverter<Type, Type>`: 파라미터 1개를 1개의 타입으로만 변환하고 싶을 때 사용한다. (타입 체크 로직을 안 쓸 수 있다.)
  - ~~~java
    public class ToLengthArgumentConverter extends TypedArgumentConverter<String, Integer> {

        protected ToLengthArgumentConverter() {
            super(String.class, Integer.class);
        }
    
        @Override
        protected Integer convert(String source) {
            return (source != null ? source.length() : 0);
        }
    
    }
    ~~~ 
## 파라미터 묶어서 받기 
### ArgumentAccessor
- 파라미터를 묶어서 받을 수 있다.
- ~~~java
  @ParameterizedTest
  @CsvSource({
      "Jane, Doe, F, 1990-05-20",
      "John, Doe, M, 1990-10-22"
  })
  void testWithArgumentsAccessor(ArgumentsAccessor arguments) {
      Person person = new Person(arguments.getString(0),
                                 arguments.getString(1),
                                 arguments.get(2, Gender.class),
                                 arguments.get(3, LocalDate.class));
  ...
  }
  ~~~
### 내멋데로 묶기
- `ArgumentsAggregator`를 상속받은 클래스를 만들고 `aggregateArguments` 메소드를 오버라이딩한다. (aggregation 로직을 구현한다.)
- 테스트 메서드 파라미터 선언 앞에 `@AggregateWith`로 사용한 조합 클래스를 지정한다.
- ~~~java
  @ParameterizedTest
  @CsvSource({
      "Jane, Doe, F, 1990-05-20",
      "John, Doe, M, 1990-10-22"
  })
  void testWithArgumentsAggregator(@AggregateWith(PersonAggregator.class) Person person) {
      // perform assertions against person
  }
  ~~~
- ~~~java
  public class PersonAggregator implements ArgumentsAggregator {
      @Override
      public Person aggregateArguments(ArgumentsAccessor arguments, ParameterContext context) {
          return new Person(arguments.getString(0),
                            arguments.getString(1),
                            arguments.get(2, Gender.class),
                            arguments.get(3, LocalDate.class));
      }
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
