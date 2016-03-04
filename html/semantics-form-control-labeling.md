# Semantics: form control labeling

폼컨트롤에 `<label>`을 붙일 때의 보편적인 마크업은 이렇다.

```html
<label for='name'>Your name:</label>
<input type='text' id='name'>
```

이 방식을 explicit labeling이라고 한다. 문제는 `for` 어트리뷰트가 `id`만을 받기 때문에 마크업이 지저분해진다는 것인데, 그래서인지 HTML5부터는 `label` 안에 폼컨트롤을 네스팅하는 방식도 많이 쓴다.

```html
<label>
	Your name:
    <input type='text'>
</label>
```

이건 implicit labeling이라고 한다. `for`를 지정해 줄 필요가 없다. 음 좋아 아름답다.

근데 레이블이 인풋의 sibling이라면 모를까 parent가 될 수 있나? 엥 이거 완전 편법 아니냐? 싶어 [레퍼런스]를 뒤져보았는데, 된단다. 심지어 예제 코드도 전부 저 방식으로 기재해 놓았다.

>  If the `for` attribute is not specified, but the `label` element has a labelable element descendant, then the first such descendant in tree order is the `label` element's labeled control.

이 방식에 대한 반대도 많은데, 나처럼 `label > input`의 의존관계가 시맨틱하지 않다고 생각하는 사람들도 있고 Web Accessibility를 고려하면 써선 안된다는 의견도 있다. 2007년 이전에 나온 스크린 리더는 암묵적 레이블을 제대로 읽지 못한다고 한다.
또한 모바일 사파리를 비롯한 일부 브라우저에서는 event propagation 우선순위 판정에 문제가 있다. 이 방식으로 구현하면 parent인 `<label>`의 이벤트 판정이 child인 `<input>`보다 무조건 우선하는 것 같은데, 이메일이나 연락처 등을 복수의 필드로 나누어 구현하는 등의 경우 어떤 필드를 클릭해도 전부 첫 번째 필드로 포커스가 가게 된다.

### Reference:

 * [HTML5 Accessibility Chops: form control labeling]
 * [Web Usability - Accessible Forms 1: Labels and identification]

[레퍼런스]:https://www.w3.org/TR/html5/forms.html#the-label-element
[HTML5 Accessibility Chops: form control labeling]:https://www.paciellogroup.com/blog/2011/07/html5-accessibility-chops-form-control-labeling/
[Web Usability - Accessible Forms 1: Labels and identification]:http://usability.com.au/2013/04/accessible-forms-1-labels-and-identification/