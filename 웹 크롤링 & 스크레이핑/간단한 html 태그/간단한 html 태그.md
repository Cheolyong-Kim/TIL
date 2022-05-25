# 간단한 html 태그 알아보기

<br/>

### html

<br/>

웹페이지를 만드는 프로그래밍 언어

문서 중간마다 있는 **링크**를 통해 다른 문서를 넘나들며 다양한 정보를 취득할 수 있는 것이 특징

<br/>

---

#### 기본적인 태그들

<br/>

태그 : 문서의 태그 안 내용에 역할을 부여. 태그마다 부여하는 역할이 다르다.

<br/>

``<hn></hn>`` : n은 숫자1부터 6까지, 글씨의 굵기와 크기를 조정해준다. 숫자가 클수록 글씨가 작아짐.

<br/>

``<u></u>`` : underline, 즉 밑줄을 그어주는 태그

<br/>

``<br>`` : 줄바꿈 태그

<br/>

``<strong></strong>`` : 글씨를 굵게 해주는 태그

<br/>

``<p></p>`` : 단락 지정 태그

<br/>

``<ol><li></li></ol>`` : 리스트 형태로 보여주는 태그

<br/>

``<div></div>`` : 블록 태그, 바구니라고 생각하면 편하다.

<br/>

``<hr>`` : 수평 가로선을 그어주는 태그

<br/>

``<iframe src=링크></iframe>`` : HTML페이지 내에 HTML페이지를 삽입하는 태그. target 속성으로 새 창에 열 것인지 그냥 열 것인지 등을 선택할 수 있다.

<br/>

주석 표현 : ``<!--주석 내용-->``

<br/>

웹페이지에는 평균 28개 종류의 태그를 사용해서 만들어진다. 어느정도의 태그만 알면 크롤링할 때 문제없이 진행할 수 있다.

``<head>``, ``<body>``, ``<html>``, ``<title>``, ``<meta>``, ``<div>``, ``<a>`` 등의 태그가 자주 사용된다.

<br/>

중요하게 볼 태그로는 ``<a>``가 있다. 해당 태그 뒤에는 링크가 뒤따라오기 때문에 Hypertext를 상징하는 태그라고 할 수 있다.

<br/>

---

#### 간단한 html 문서 만들기

<br/>

![i1](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%9B%B9%20%ED%81%AC%EB%A1%A4%EB%A7%81%20&%20%EC%8A%A4%ED%81%AC%EB%A0%88%EC%9D%B4%ED%95%91/%EA%B0%84%EB%8B%A8%ED%95%9C%20html%20%ED%83%9C%EA%B7%B8/%EA%B0%84%EB%8B%A8%ED%95%9C%20html%20%ED%83%9C%EA%B7%B8%20image/i1.png?raw=true)

<br/>

![i2](https://github.com/Cheolyong-Kim/TIL/blob/master/%EC%9B%B9%20%ED%81%AC%EB%A1%A4%EB%A7%81%20&%20%EC%8A%A4%ED%81%AC%EB%A0%88%EC%9D%B4%ED%95%91/%EA%B0%84%EB%8B%A8%ED%95%9C%20html%20%ED%83%9C%EA%B7%B8/%EA%B0%84%EB%8B%A8%ED%95%9C%20html%20%ED%83%9C%EA%B7%B8%20image/i2.png?raw=true)

- 웹페이지에서 확인한 태그

<br/>

---

#### 알아두면 좋은 CSS

<br/>

CSS는 HTML 문서의 색이나 모양 등 외관을 꾸미는 언어이다. CSS로 작성된 코드를 style sheet라고 부른다.

<br/>

- 셀렉터

  HTML 태그의 모양을 꾸밀 스타일 시트를 선택하는 기능이다.

<br/>

- 태그 이름 셀렉터

  태그 이름이 셀렉터로 사용되는 유형이다. 셀렉터와 같은 이름의 모든 태그에 CSS3 스타일 시트를 적용한다.

  ```html
  h3 { color : brown; }
  
  <h3>Hello, World!</h3>
  
  <!-- Hello, World가 갈색으로 출력됨 -->
  ```

<br/>

- 클래스 셀렉터

  점(.)으로 시작하는 이름의 셀렉터. HTML 태그의 class 속성으로만 지정 가능. 중복되는 형식이 많을 때 주로 사용. (ex. 게시글 제목)

  ```html
  .warning { color : red; }
  body.main { background : aliceblue; }
  
  <body class="main">
      <body>
          <div class="warning">60점 이하는 F!</div></body>
  
  <!-- main 클래스의 배경색이 aliceblue로 적용되고 warning 클래스의 글자색이 빨간색으로 적용된다. -->
  ```

<br/>

- ID 셀렉터

  #으로 시작하는 이름의 셀렉터. HTML 태그의 id 속성으로만 지정 가능. 전체 화면에서 하나만 있을 때 주로 사용.

  ```html
  #list { background : mistyrose; }
  
  <ul id="list">
      <li>HTML5</li>
      <li>JAVASCRIPT</li>
  </ul>
  
  <!-- list 클래스의 배경색이 mistyrose로 적용됨.>
  ```

<br/>

- 자식, 자손 셀렉터

  2개 이상의 셀렉터 조합. 조합에 적절한 HTML 태그에만 적용

  - 자식 셀렉터

    부모 자식 관계인 두 셀렉터를 '>' 기호로 조합

    ``div > strong { color : dodgerblue; }``

  - 자손 셀렉터

    자손 관계인 2개 이상의 태그 나열. 스페이스바로 구분

    ``ul strong { color : dodgerblue; }``

