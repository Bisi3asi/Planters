![](https://velog.velcdn.com/images/chrios99/post/034fce20-84e6-4284-9087-b2c368b68e8c/image.png)

> 사진 출처 : https://namu.wiki/w/HTML

### HTML의 기본 구조

head 태그에 대해 알아보기 전에 HTML 문서의 기본 구조를 다시 한 번 보자.

```html
<!DOCTYPE html>
<html>
    <head>
        <!-- 문서의 정보(메타데이터) -->
    </head>
    <body>
        <!-- 문서의 내용 -->
    </body>
</html>

```
위의 코드가 HTML의 기본 구조이다.

### head 태그의 위치

head 태그는
```html
<head>
        <!-- 문서의 정보(메타데이터) -->
    </head>
```
이 코드를 의미한다.

HTML 기본구조 중에서 head 태그는 어디에 위치해 있을까? 아주 금방 찾아낼 수 있을 텐데

```html
<!DOCTYPE html>
<html>
    <head>
        <!-- 문서의 정보(메타데이터) -->
    </head>
    <body>
        <!-- 문서의 내용 -->
    </body>
</html>

```

HTML 기본구조를 살펴보면 head 태그는 html 태그와 body 사이에 위치한다는 것을 알 수 있다.

### head 태그란?

head 태그란 문서의 문자 인코딩, 타이틀, 설명, 스타일시트, 스크립트 등 문서 정보(메타데이터)에 관련된 다양한 요소를 모아둔 태그이다. 이러한 정보들은 화면에 표시되지는 않지만 브라우저, 검색 엔진 및 다른 웹 기술 등 기계적 처리를 위한 정보들이다. 간단히 말하면 head 태그란 문서의 머리를 나타내는 태그이다. head 태그에는 브라우저 화면에 직접적으로 보이지 않는 숨은 데이터를 정의하는 태그들이 들어간다. 또한 head 태그 내에서 외부 스타일시트, JavaScript 파일 등을 연결할 수 있다.

### 메타데이터란?
메타데이터란 해당 문서에 대한 정보이다. 더 자세히 말하자면, 메타데이터란 문자 인코딩, 타이틀, 설명, 스타일시트, 스크립트 등의 HTML 문서를 다루고 기술적으로 처리하기 위해 필요한 코드화된 추가 정보를 말한다.

### head 태그의 주된 목적
head 태그의 주된 목적은 브라우저, 검색 엔진 및 다른 웹 기술 등 기계적 처리를 위한 HTML 문서 정보들을 모아두는 것이다. 앞서 말했듯이 head 태그는 화면에 표시되지 않는다. 만약, 문서의 최상위 제목이나 작성자 정보 등이 화면에 보여야 할 필요가 있다면 header 태그를 사용하면 된다.

### head 태그의 특징
- head 태그는 html 태그의 첫 번째 자식 요소여야 한다.
- head 태그는 HTML의 기본 구조에 반드시 필요한 필수 태그이다.
- HTML 문서에는 하나의 head 태그만 존재해야 한다.
- head 태그의 필수 자식 요소는 title 태그이다.
- head 태그는 반드시 body 태그를 형제 요소로 두고 있어야 하며, 이때, head 태그가 먼저 위치해 있어야 한다.
  **참고)** 형제 요소로 두고 있어야 한다는 말은 간단히 말하면 body 태그 내에 위치하면 안된다는 것이다.

### head 태그의 자식 요소
head 태그에서 자식 요소로 사용할 수 있는 태그는

- title 태그
- style 태그
- base 태그
- link 태그
- meta 태그
- script 태그
- noscript 태그

가 있다.

**참고)**  script 태그와 noscript 태그는 head 태그 뿐만 아니라 body 태그에서도 사용할 수 있다.


### head 태그를 잘못 마크업한 사례 - 1. 올바르지 않은 HTML 구조
```html
<!DOCTYPE html>
<html>
    <body>
        <head> <!-- 올바르지 않은 HTML 구조 -->
            <!-- 문서의 정보(메타데이터) -->
        </head>
        <!-- 문서의 내용 -->
    </body>
</html>
```

head 태그는 기본 구조에 반드시 필요한 필수 태그이며, 반드시 body 태그를 형제 요소로써 존재해야 한다. 그러므로 head 태그가 body 태그의 자식 요소로 존재할 수는 없다.

### head 태그를 잘못 마크업한 사례 - 2. 자식 요소로 title 태그가 반드시 있어야 함

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <!-- 문서의 타이틀을 나타내는 title 요소가 없음 -->
        <meta name="description" content="이 페이지의 짧은 설명입니다.">
        <link rel="stylesheet" href="style.css">
        <script src="script.js"></script>
    </head>
    <body>
        <!-- 문서의 내용 -->
    </body>
</html>
```

head 태그에 반드시 필요한 요소는 title 태그라고 하였다. 이번 파트의 코드를 보면 title 태그가 없다.

### 왜 title 태그가 반드시 필요한 것인가?

- title 태그는 웹 브라우저 탭에 표시되는 문서의 제목을 지정한다.
- 사용자가 웹페이지를 즐겨찾기나 북마크에 추가할 때, title 태그의 내용이 기본적으로 제목으로 사용된다.
- 검색 엔진은 title 태그를 사용하여 페이지의 내용을 요약하고 검색 결과에서 해당 페이지의 제목으로 사용한다.
- 소셜 미디어 플랫폼에서 웹 페이지를 공유할 때, 대부분의 플랫폼은 title 태그의 내용을 사용하여 페이지의 제목을 표시한다.

이 4가지 이유로 인해 웹 문서의 head 태그 내에 title 태그를 포함하는 것은 사용자 경험, 검색 엔진 최적화, 그리고 웹 페이지의 식별성을 높이는 데 매우 중요하다. 그러므로 head 태그에 title 태그를 반드시 포함해줘야 한다.

> 참고 문헌:
1. https://ofcourse.kr/html-course/head-%ED%83%9C%EA%B7%B8
2. https://codingeverybody.kr/html-head-%ed%83%9c%ea%b7%b8-%ec%98%ac%eb%b0%94%eb%a5%b8-%ec%9d%b4%ed%95%b4%ec%99%80-%ec%82%ac%ec%9a%a9-%eb%b0%a9%eb%b2%95/
3. https://www.tcpschool.com/html-tags/head#google_vignette
4. https://floz.tistory.com/entry/HTML4-head-%EC%9A%94%EC%86%8C%EB%9E%80-title-style-link-meta-script
5. https://codingeverybody.kr/html-html-%ED%83%9C%EA%B7%B8-%EC%98%AC%EB%B0%94%EB%A5%B8-%EC%9D%B4%ED%95%B4%EC%99%80-%EC%82%AC%EC%9A%A9-%EB%B0%A9%EB%B2%95/