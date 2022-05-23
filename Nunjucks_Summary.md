# Nunjucks

### View Engine

DB의 내용 등을 자연스럽게 HTML에 보여줄 수 있도록 하는 엔진

HTML 내부에서 해당 엔진을 활용해 반복문, 조건문 등을 사용 가능

### 연결방법

```
nunjucks.configure('views', {
  express: app,
  watch: true,
});
```

configure의 첫 번째 인수로 views 폴더의 경로, 두 번째 인수로 옵션,

이때, express속성에 app객체를 연결

watch 옵션이 true이면 HTML파일이 변경될 때 템플릿 엔진을 다시 렌더링함

확장자는 html을 그대로 사용 가능(넌적스임을 구분하려면 확장자로 njk를 사용)

확장자로 njk를 사용할 시

`app.set('view engine', 'html');`

에서 html부분도 njk로 바꿔야함

## 넌적스에서의 변수

res.render 호출 시 보내는 변수를 넌적스가 처리함

넌적스 ~ 추후에 변경할 때 용이

```
<h1>{{title}}</h1>
<p>Welcome to {{title}}</p>
<button class="{{title}}" type="submit">전송</button>
<input placeholder="{{title}} 연습" />
```

HTML

```
<h1>Express</h1>
<p>Welcome to Express</p>
<button class="Express" type="submit">전송</button>
<input placeholder="Express 연습" />
```

### 넌적스 내부에서 변수 사용하기

변수를 선언할 때는 {% set 변수이름 = ‘값’ %}

HTML을 이스케이프하고 싶지 않다면 {{ 변수 || safe }}를 사용

## 넌적스에서의 반복문

넌적스에서는 특수한 구문을 {%  %}안에 씀

{%for ~ in ~%}문과  {% endfor %} 사이에 위치

반복문에서 인덱스를 사용하고 싶다면 loop.index라는 변수를 사용({{loop.index}})

## 넌적스에서의 조건문

{% if 변수 %}{% elif %}{% else %}{% endif %}로 이루어져 있음

```
{% if isLoggedIn %} //isLoggedIn이 true일 때
<div>로그인 되었습니다.</div>
{% else %} // isLoggedIn이 false일 때
<div>로그인이 필요합니다.</div>
{% endif %}
```

{{}} 안에서도 사용가능

```
<div>{{'참' if isLoggedIn}}</div> //isLoggedIn이 true일 때
<div>{{'참' if isLoggedIn else '거짓'}}</div> //false일 때
```

## Include

다른 html파일을 넣을 수 있음

헤더나 푸터, 네비게이션처럼 웹 제작 시 공통되는 부분을 따로 관리할 수 있음

`{% include "파일경로" %}`

`{% include "footer.html" %}`

원하는 위치에 따로따로 넣어줌

## Extends와 Block

레이아웃을 정할 수 있음, 공통되는 레이아웃 부분을 따로 관리 가능

레이아웃이 될 파일에는 공통된 마크업을 넣되,

페이지마다 달라지는 부분을 block으로 비움

block은 여러 개 만들어도 됨

block은 {% block [블록명] %} 으로 시작해서 {% endblock %}으로 끝남

나중에 익스프레스에서 res.render(’body’)를 사용해 하나의 html로 합치고 렌더링 가능 —> 같은 이름의 block 부분이 서로 합쳐짐