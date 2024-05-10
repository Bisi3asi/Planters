## [HTML] DOCTYPE 선언
### HTML 문서의 기본 구조

DOCTYPE에 대해 알아보기 전에 HTML 문서의 기본 구조를 다시 한 번 보자.

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

### DOCTYPE??
- DOCTYPE은 document type의 줄임말이다.
- 이는 웹 문서가 어떠한 형식으로 작성되었는지 최상단에 문서의 형식을 선언하는 것을 의미한다.

### DOCTYPE 선언이란?
- 위의 HTML 문서의 기본구조 코드에서
```html
<!DOCTYPE html>
```
가 DOCTYPE 선언이다.

- 이는 html 5에서 사용하는 DOCTYPE 선언이다.
- DOCTYPE 선언은 반드시 HTML 문서에서 최상단에 위치해야 한다.
- 즉, 모든 HTML 문서는 DOCTYPE 선언으로 시작되어야만 한다.
- DOCTYPE 선언의 형식은 HTML 태그는 아니다.
- 그러나 HTML 문서의 버전을 웹 브라우저에 알린다.
- 또한 대소문자를 구분하지 않지만 보통은 대문자로 사용한다.

### DOCTYPE 선언의 목적
- 각 브라우저가 HTML 문서를 동일하게 인식할 수 있도록 하고, 문서 간의 호환성을 높이기 위해 선언을 한다.

### DOCTYPE 선언의 역할
- DOCTYPE 선언은 브라우저가 최신 버전의 웹 표준 사양을 준수하고 올바르게 렌더링하도록 하는 역할을 한다.

### DOCTYPE 선언이 생략된다면?
- 생략되면, 보통 브라우저는 일부 사양과 호환되지 않는 다른 렌더링 모드를 사용한다.
- 따라서 HTML 문서 최상단에 <!DOCTYPE html>를 포함하면 브라우저가 관련 사양을 최선의 노력으로 준수하려고 시도하는 것을 보장한다.

### 버전마다 DOCTYPE 선언을 사용할때 주의할 점
- 브라우저는 현대적인 최신의 웹 표준 사양을 지원하지만, 과거에 사용하던 여러 사양의 이전 버전의 렌더링 방식을 함께 지원한다.
- 이러한 이유는 과거에 제작되어 있는 웹과 호환성을 유지하기 위해서이다.
- 현대적인 최신의 웹 표준 사양은 과거에 사용하던 방식과는 다른 작동원리가 많다.
- 웹은 매우 발달했고, 현재 진행형이다.
- 그러므로, 과거의 버전들은 현대 웹 표준과 호환되지 않을 수 있다.

### HTML5 이전 버전을 폐지하지 않고 유지하는 이유
- HTML5 이전 버전을 폐지하지 않고 유지하는 이유는 과거 웹 자료의 보존하기 위해서다.


> 참고 문헌:
1. https://sdsupport.cafe24.com/reference/html/doctype.html
2. https://ssd0908.tistory.com/entry/HTML-DOCTYPE-%EC%84%A0%EC%96%B8-%EC%9D%98%EB%AF%B8-%EB%B0%8F-%ED%98%84%EC%9E%AC%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94%EA%B0%80
3. https://rypro.tistory.com/187
4. https://www.w3schools.com/tags/tag_doctype.ASP