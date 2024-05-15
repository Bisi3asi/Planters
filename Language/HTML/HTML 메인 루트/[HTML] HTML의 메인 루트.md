## HTML의 메인 루트

### HTML의 기본 구조

HTML의 메인 루트에 대해 알아보기 전에 HTML 문서의 기본 구조를 다시 한 번 보자.

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

### HTML 메인 루트란?
- 위의 코드에서 메인 루트를 찾아보면 아래의 것을 말하는데

```html
<html>
	
</html>
```

이를 html 태그라고 한다.

- html 태그는 메인 루트, 루트 요소, 최상단 요소라고 부른다.
- 이렇게 부르는 이유는 이 태그가 HTML 문서의 구조상 가장 기본이 되는 요소이기 때문입니다.
- HTML 문서의 구조상 가장 기본이 되는 요소인 이유는 이 태그의 시작 태그는 HTML 문서의 시작을, 닫는 태그는 HTML 문서의 끝을 나타내기 떄문이다.
- 하나의 HTML 파일에는 하나의 html 태그를 기술해야 한다.
- 하나의 HTML 파일에는 하나의 html 태그를 작성해야 하는 이유는 웹 브라우저나 다른 소프트웨어가 HTML 파일을 읽고 해석할 때 혼동을 방지하기 위함이다.
- html 태그 외의 다른 모든 태그는 html 태그의 자손이어야 한다.
- 이 문장의 의미는 여는 html 태그와 닫는 html 태그 사이에만 다른 요소(태그)들이 존재할 수 있다는 것을 의미한다.

### html 태그의 형태

```html
<html>
    <head>
        <!-- 문서의 정보(메타데이터) -->
    </head>
    <body>
        <!-- 문서의 내용 -->
    </body>
</html>
```

- html 태그는 여는 html 태그와 닫는 html 태그가 있으며 이 두 태그 사이에 head 태그와 body 태그가 존재한다.
- head 태그는 HTML 문서의 문자 인코딩, 타이틀, 설명, 스타일시트, 스크립트 등 문서 정보(메타데이터)에 관련된 다양한 요소를 모아둔 태그이다.
- body 태그는 HTML 문서의 내용(contents)을 나타내는 태그이다.
- head 태그, body 태그는 차차 학습하자.


### html 태그의 속성(lang 속성)
- html 태그에 사용할 수 있는 속성으로 대표적인 속성이 lang 속성이다.
- lang 속성을 html 태그에 기술해줌으로써 해당 문서에 대한 기본 언어를 지정할 수 있게 된다.
- lang 속성을 지정해주는 이유는 스크린 리더나 브라우저, 검색 엔진 등에 사용해야 하는 언어 정보를 제공해주기 때문이다.
- 아래의 코드는 문서의 기본 언어를 한국어로 지정하는 코드이다.

```html
<html lang="ko">
	<head>
        <!-- 문서의 정보(메타데이터) -->
    </head>
    <body>
        <!-- 문서의 내용 -->
    </body>
</html>
```

- 한국어 외에도 여러 언어를 문서의 기본 언어로 지정할 수 있다.

> 참고 문헌 :
1. https://codingeverybody.kr/html-html-%ED%83%9C%EA%B7%B8-%EC%98%AC%EB%B0%94%EB%A5%B8-%EC%9D%B4%ED%95%B4%EC%99%80-%EC%82%AC%EC%9A%A9-%EB%B0%A9%EB%B2%95/
2. https://developer.mozilla.org/ko/docs/Web/HTML/Element
3. https://developer.mozilla.org/ko/docs/Web/HTML/Element/html
4. https://codingeverybody.kr/html-lang-%ec%86%8d%ec%84%b1-%ec%98%ac%eb%b0%94%eb%a5%b8-%ec%9d%b4%ed%95%b4%ec%99%80-%ec%82%ac%ec%9a%a9-%eb%b0%a9%eb%b2%95/
 