### HTML의 기본 구조
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

- 위의 코드가 HTML의 기본 구조이다.
- HTML의 기본 구조를 알아야 하는 이유는 이를 알아야 html 태그, head 태그, body 태그를 쉽게 이해할 수 있기 때문이다.
- 그러나 너무 겁 먹을 필요는 없고 html를 사용한 모든 파일은 모두 위의 코드 형태이기 때문에 차근차근 알아가면 된다.
- 위의 코드에 나온 html의 기본 구조를 분석해보자.

```html
<!DOCTYPE html>
```
은 브라우저가 최신 버전의 웹 표준 사양을 준수하고 올바르게 렌더링하도록 하는 역할이다.

```html
<html>부터 </html>까지
```
는 HTML 파일의 시작과 끝을 의미하고 거의 모든 태그는 이 태그들 사이에만 존재해야 한다.
```html
<head>부터 </head>까지
```
는 문자 인코딩, 타이틀, 설명, 스타일시트, 스크립트 등 문서 정보(메타데이터)에 관련된 다양한 요소를 모아둔 태그이다. 이러한 정보들은 화면에 표시되지는 않지만 브라우저, 검색 엔진 및 다른 웹 기술 등 기계적 처리를 위한 정보들이다.

```html
<body>부터 </body> 까지
```
는 HTML 문서의 내용(contents)을 나타내는 태그이다.

### 참고) 인텔리제이에서 html 문서의 기본 구조 세팅
인텔리제이에서 html 문서의 기본 세팅을 할 수 있는 방법은 html 파일에 !을 입력하고 tab 키를 누르는 것이다.
그렇게 되면 아래와 같은 형태의 기본 형태가 나타난다.

![](https://velog.velcdn.com/images/chrios99/post/e497b125-c231-4094-afda-74c46e1f4e4b/image.png)


> 참고 문헌
1. https://codingeverybody.kr/html-doctype-html-%ec%84%a0%ec%96%b8%ec%9d%98-%ec%9d%98%eb%af%b8/
2. https://codingeverybody.kr/html-html-%ED%83%9C%EA%B7%B8-%EC%98%AC%EB%B0%94%EB%A5%B8-%EC%9D%B4%ED%95%B4%EC%99%80-%EC%82%AC%EC%9A%A9-%EB%B0%A9%EB%B2%95/
3. https://codingeverybody.kr/html-body-%ed%83%9c%ea%b7%b8-%ec%98%ac%eb%b0%94%eb%a5%b8-%ec%9d%b4%ed%95%b4%ec%99%80-%ec%82%ac%ec%9a%a9-%eb%b0%a9%eb%b2%95/