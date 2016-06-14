# Form Validation in HTML and CSS

HTML5의 ``required``, ``pattern``, ``type`` 어트리뷰트 및 CSS3의 ``:valid``, ``:invalid``, ``:placeholder-shown`` 등의 셀렉터를 이용한 입력 검증 UX 예제. 즉 자바스크립트를 쓰지 않는 방법이다.

다만 ``:placeholder-shown``은 아직 IE나 파이어폭스 등에서 지원되지 않으므로 당장 활용하기엔 조금 요원함..

저자가 폼 시맨틱 마크업 관련해서 하나 주의를 주고 있는데, 인풋의 ``placeholder`` 어트리뷰트를 ``<label>``처럼 쓰지 말라는 것이다(= placeholder visual pattern). ``placeholder``는 말 그대로 '올바른 입력 예시'이기 때문. 따라서 'Name' 레이블이 달린 입력창에 'John'같은 플레이스홀더 텍스트를 넣는 것이 시맨틱한 마크업이다.

로그인처럼 너무 간단하고 정형화된 폼이라서 비주얼 패턴을 꼭 써야 한다면, 그냥 ``<label>``을 쓰되 포지션 조정해서 플레이스홀더처럼 보이게 만들면 된다. 뭐 이렇게.

```html
<form>

  <div>
    <input type="text" id="first_name" name="first_name">
    <label for="first_name">First Name</label>
  </div>
  
  ...
  
</form>
```

```css
form > div {
  position: relative;
}
  
form > div > label {
  opacity: 0.3;
  position: absolute;
  top: 22px;
  left: 20px;
}
```


### Reference:

 * [Form Validation UX in HTML and CSS] (Chris Coyier)
 * [CodePen]

[Form Validation UX in HTML and CSS]:https://css-tricks.com/form-validation-ux-html-css/#more-242123
[CodePen]:http://codepen.io/chriscoyier/pen/JXgKjb