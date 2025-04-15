## URL 인코딩

> URL이 ASCII를 사용하는 이유
- HTTP 메시지에서 시작 라인(URL을 포함)과 HTTP 헤더의 이름은 항상 ASCII를 사용해야 한다.
- HTTP 메시지 바디는 UTF-8과 같은 다른 인코딩을 사용할 수 있다.
    - 그렇다면 검색어로 사용하는 /search?q=hello 를 사용할 때 q=가나다 와 같이 URL에 한글을 전달하려면 어떻게 해야할까?

## 퍼센트(%) 인코딩

- 한글을 UTF-8 인코딩으로 표현하면 한 글자에 3byte의 데이터를 사용한다.
- URL은 ASCII 문자만 표현할 수 있으므로, UTF-8 문자를 표현할 수 없다.
- 그래서 한글 "가"를 예를 들면 "가"를 UTF-8 16진수로 표현한 각각의 바이트 문자 앞에 %(퍼센트)를 붙이는 것이다.
  - q=가
  - q=%EA%B0%80
- 이렇게 각각의 16진수 byte를 문자로 표현하고, 해당 문자 앞에 %를 붙이는 것을 퍼센트(%) 인코딩이라 한다.
- % 인코딩 후에 클라이언트에서 서버로 데이터를 전달하면 서버는 각각의 %를 제거하고, EA,B0,8b 이라는 각 문자를 얻는다.
- 그리고 이렇게 얻은 문자를 16진수 byte로 변경한다.
- 이 3개의 byte를 모아서 UTF-8로 디코딩하면 "가" 라는 글자를 얻을 수 있다.

```java
import static java.nio.charset.StandardCharsets.*;

import java.net.URLDecoder;
import java.net.URLEncoder;

public class PercentEncodingMain {

  public static void main(String[] args) {
    String encode = URLEncoder.encode("가", UTF_8);
    System.out.println("encode = " + encode);

    String decode = URLDecoder.decode(encode, UTF_8);
    System.out.println("decode = " + decode);
  }
}
```