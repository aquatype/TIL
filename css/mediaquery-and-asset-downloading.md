# Media query & Asset downloading

Responsive web을 구현할 때에는 이미지 역시 반응형으로 조절해야 할 때가 많다. 예전에도 [Responsive Image 기술 가이드라인]에 대해 다룬 적이 있었는데, 저기서 어떤 방식을 택하든 일반적으로 추가 마크업 또는 추가 라이브러리가 필요한 편이다. 그래서 내가 주로 쓰는 편법은 미디어쿼리를 이용한 cascade override인데...

```css
section {background-image: url('bg.png'); }

@media only screen and (max-width: 768px) {
	section {background-image: url('bg-mobile.png'); }
}
```

처음 구현 시 크롬 개발자 도구를 통해 미디어쿼리 분기에 따라 필요한 이미지만을 요청하는 것을 확인했고 그래서 그 이후로도 별 고민 없이 이 방법을 주로 써 왔었는데, 문득 내가 크롬을 너무 신봉하는 것이 아닌가 싶어서 리서치를 좀 해 보았다. 역시 성실한 누군가가 이미 [퍼포먼스 테스트]를 하고 그 결과를 공개해 두었다.

정리하자면, 일단 내가 쓰는 방법은 **대체로** 의도한 대로 동작한다. 완벽한 방법은 따로 있고, 그 외의 여러 경우에 대해서는 다음과 같다.

* `<img>` + `display: none` : **절대로 하지 마라**
* `background-image` + `display: none` : **절대로 하지 마라**
* `background-image` + parent `display: none` : 잘 됨
* `background-image` + cascade override(내 방법) : 안되는 브라우저가 많음
* `background-image` + `min-width` cascade override : 완벽한 방법

내 방법이 제대로 동작하지 않는 브라우저는 안드로이드 ~3.0, 블랙베리 6.0, 킨들 3.0, 사파리 4.0 등. 대체로 구 버전 브라우저들이긴 하지만 레거시 지원을 생각한다면 고려는 해야 할 수준이다. 역시 크롬을 너무 믿으면 안 된다.

[Responsive Image 기술 가이드라인]:http://www.css-tricks.com/which-responsive-images-solution-should-you-use/
[퍼포먼스 테스트]:https://timkadlec.com/2012/04/media-query-asset-downloading-results/
