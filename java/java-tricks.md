
## 基于控制台的输入输出编写断言

```xml
<dependency>
  <groupId>com.github.stefanbirkner</groupId>
  <artifactId>system-rules</artifactId>
  <version>1.18.0</version>
</dependency>
```

```java
@RunWith(BlockJUnit4ClassRunner.class)
public class TestTest {
    @Rule
    public final TextFromStandardInputStream systemInMock = TextFromStandardInputStream.emptyStandardInputStream();

    @Rule
    public final SystemErrRule systemErrRule = new SystemErrRule().enableLog();

    @Test
    public void writesTextToSystemErr() {
        System.err.print("hello world");
        systemErrRule.clearLog();
        System.err.print("foo");
        assertEquals("foo", systemErrRule.getLog());
    }

    @Test
    public void summarizesTwoNumbers() {
        systemInMock.provideLines("1", "2");
        assertEquals(3, Summarize.sumOfNumbersFromSystemIn());

        System.out.println("123");
    }
}
```

## 生成随机数
> 参考 Apache Commons Lang 的 RandomStringUtils

```java
@Test
public void givenUsingApache_whenGeneratingRandomAlphanumericString_thenCorrect() {
    String generatedString = RandomStringUtils.randomAlphanumeric(10);

    System.out.println(generatedString);
}
```

