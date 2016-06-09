# Injecting linebreak with CSS

[Injecting a Line Break]에서 발췌 요약하여 번역.

CSS를 이용하여 개행하는 여러 방법에 대해 다룬 글이다. 나도 예전에 저자와 비슷한 고민으로 한참 삽질한 끝에 이 글과 비슷한 결론을 내렸었다. 그냥 눈 질끈 감고 ``<br>`` 넣으면 쉽고 빠르다는 것.

### 문제

헤더 안에 ``<span>``이 있고, 이걸 개행을 하고 싶다. 

```html
<h1>
Break right after this
<!-- 여기에 그냥 <br> 넣으면 됨. 근데 CSS로는 안될까? -->
<span>
and before this
</span>
</h1>
```

물론 ``<br>`` 넣으면 되고, 이걸 ``display: none`` 등으로 숨길 수도 있기 때문에 사실 굉장히 유용하긴 하다. **하지만 레이아웃을 위한 개행을 하면서 HTML 마크업을 써야만 하는 건 좀 이상하지 않나?** 그러니 CSS로 개행하는 방법을 찾아보자.

### 해법 1, 블럭 레벨 엘리먼트로 만들기

``<div>``로 대체하던가 ``display: block``을 주던가 하면 해결된다.

근데 대체할 수 없이 ``<span>``을 써야만 할 때도 분명히 있다. 해당 텍스트에 배경색이나 패딩 등을 넣기 위해서라던지. 이 경우 블럭 엘리먼트는 도움이 되지 않는다.

### 해법 2, 슈도 엘리먼트로 개행문자 넣기

```css
h1 span::before {
content: "\A";
}
```

쉽다. 근데 ``<span>``은 인라인 엘리먼트다. 개행문자는 아무 역할도 하지 않는다.

```css
h1 span::before {
content: "\A";
white-space: pre;
}
```

여백을 강제로 preserve해서 개행문자가 동작하게 만들 순 있다. 근데 브라우저 파싱 이슈 때문에 이러면 윗줄에 반각 스페이스 하나 만큼의 인라인 엘리먼트 조각이 남는다.

``<span>``을 인라인 블럭으로 만들면 개행이 그 안에서 일어나버리기 때문에 마찬가지로 의미가 없다. 슈도 엘리먼트를 블럭으로 만들고 ``<span>``은 그대로 두는 것도 의도한 대로 동작하지는 않는다.

### 해법 3, 실제 텍스트를 슈도 엘리먼트로 집어넣기

(일단 원문에 나와 있어서 소개는 하지만 고려할 가치가 없는 방법이므로 자세한 설명은 생략)

```css
h1 span {
display: block;
}
h1 span::before {
content: attr(data-text);
background: black;
padding: 1px 8px;
}
```

### 해법 4, 테이블 레이아웃을 이용하기

가장 간단하면서 완벽한 방법이다. 그냥 ``<span>``을 ``display: table``로 만들면 된다.

```css
h1 span {
display: table;
}
```

물론 우리의 텍스트는 표형 데이터가 아니다. 하지만 상관없다. 이건 시맨틱의 문제가 아니라 테이블의 유니크한 출력 특성만을 가져와서 적용하는 거니까!

### Reference:

* [Injecting a Line Break] (Chris Coyier)
* [Live Demo] (Chris Coyier)

[Injecting a Line Break]:https://css-tricks.com/injecting-line-break/#more-241954
[Live Demo]:http://codepen.io/chriscoyier/pen/LNamvy