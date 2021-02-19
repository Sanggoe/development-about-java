# Jsoup (크롤링)

<br/>

### Jsoup 이란?

- Java로 만들어진 HTML 파서 라이브러리이다.
- URL, 파일, 문자열을 Source로 HTML을 파싱할 수 있다.
- DOM 구조 추적 및 CSS 선택자를 사용해 데이터를 추출할 수 있다.
- 문서 내 HTML 요소, 속성, 텍스트를 조작할 수 있다.
- 정돈된 형태의 html을 출력할 수 있다.

<br/>

### Jsoup 주요 요소

* Document
  * Jsoup을 통해 얻어온 최상위 노드
* Element
  * Document가 가지는 HTML 요소
* Elements
  * Element의 집합. 반복문 사용이 가능
* Connection
  * 연결을 위한 정보를 담고있는 객체
* Response
  * Jsoup으로 URL에 접속해 얻어온 결과로, Document와 다르게 status의 코드, 메시지, charset 등의 헤더 메시지와 쿠키 등도 포함한다.

<br/>

### Jsoup 사용 과정

#### 사용 준비

* jar 파일을 다운로드 받아서 class path에 추가하는 방법도 있다.
* Maven을 사용중이라면, 다음 의존성을 추가한다.

```xml
<dependency>
	<groupId>org.jsoup</groupId>
	<artifactId>jsoup</artifactId>
	<version>1.10.3</version>
</dependency>
```

* Gradle을 사용중이라면, build.gradle 파일에 다음 의존성을 추가한다.

```java
implementation 'org.jsoup:jsoup:1.13.1'
```

<br/>

#### 수행 과정

* Jsoup을 사용해 파싱하는 과정은 이렇다.
  * Connection 객체를 통해 URL에 접속한다.
    * 또는, local file이나 문자열에 접근한다.
  * Response 객체에서 세션 ID 등의 쿠키, HTML Document를 얻어온다.
  * Document의 Element들을 파싱한다.
* 파싱의 대상은 string, local file, URL 등을 source로 할 수 있다.

<br/>

#### 1. URL에서 문서 파싱

* 웹에서 HTML 문서를 가져와서 구문을 분석하고, 파싱하는 방법
* **Jsoup.connect(String url)**
  * URL을 인자로 주어 해당 주소에 접속하여, Response를 반환한다.
  * 이어서 Response의 .get() 메소드를 통해 리턴값으로 Document를 얻어온다.
  * 이를 축약하여 바로 connect().get() 으로 사용할 수도 있다.

``` java
Document doc = Jsoup.connect("http://example.com/").get();
String title = doc.title();
```

* URL을 가져오는 동안 오류 발생을 방지하기 위한 IOException을 처리해주어야 한다.
* 또한, Builder 패턴 형식으로 여러 정보들을 직접 넣어 .post() 메소드를 이용해 Document를 얻을 수도 있다.
  * 이 방법은 웹 URL(http, https)만 지원한다.
  * 파일에서 로드하는 경우, 2번의 parse(File in, String charsetName) 을 사용한다.

```java
Document doc = Jsoup.connect("http://example.com")
  .data("query", "Java")
  .userAgent("Mozilla")
  .cookie("auth", "token")
  .timeout(3000)
  .post();
```

* 그 후, 이 Document 객체를 이용해 원하는 Element 등을 파싱하여 사용할 수 있다.

<br/>

#### 2. 파일에서 문서 파싱

* Jsoup.parse(File in, String charsetName, String baseUri)

```java
File input = new File("/tmp/input.html");
Document doc = Jsoup.parse(input, "UTF-8", "http://example.com/");
```

<br/>

#### 3. 문자열에서 문서 파싱

```java
// 문서 전체를 가진 문자열에서 html을 파싱하는 경우
String html = "<html><head><title>First parse</title></head>"
  + "<body><p>Parsed HTML into a doc.</p></body></html>";
Document doc = Jsoup.parse(html);

// 웹 페이지 URL로부터 얻어오는 경우
Document doc = Jsoup.parse(String html, String baseUri)
```

* Jsoup.parse(String html)
  * string 형태로 존재하는 html에서 Document 객체를 얻어오기 위한 메소드
* Jsoup.parse(String html, String baseUri)

<br/>

#### 4. 본문 일부를 가진 문자열에서 파싱

```
Document doc = Jsoup.connect("URL").get();
log(doc.title());
Elements newsHeadlines = doc.select("tag type");
for (Element headline : newsHeadlines) {
    log("%s\n\t%s", headline.attr("title"), headline.absUrl("href"));
}
```

<br/>

<br/>

### 데이터 추출

<br/>

[참고 - tistory 블로그1](https://offbyone.tistory.com/116)

[참고 - tistory 블로그2](https://partnerjun.tistory.com/42)

[참고 - tistory 블로그3](https://hyeran-story.tistory.com/120)

[공식 레퍼런스](https://jsoup.org/)

