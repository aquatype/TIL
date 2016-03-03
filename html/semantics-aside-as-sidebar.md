# Semantics: aside as sidebar

사이트 전체의 사이드바를 구현해야 하는데, HTML5 시맨틱 태그 중에는 헤더 푸터 심지어 메인은 있어도 사이드바라는 개념은 없다. 가장 개념적으로 근접해보이는 것으로 `<aside>`가 있는데, 이를 사이드바 마크업으로 사용해도 괜찮을지 궁금해서 리서치해봄.

일단 [W3의 정의]는 다음과 같다.

>  The `aside` element represents a section of a page that consists of content that is tangentially related to the content around the `aside` element, and which could be considered separate from that content.

보다시피 아주 두리뭉실한 정의이기 때문에 원본주의자와 해석주의자 간의 [키배]도 많았는데, 다행히도 W3가 이를 인지하고 '사이트 전체의 사이드바로 써도 된다'고 [컨펌]을 해 주었다고 한다. ~~고민 끝 행복 시작~~ 하지만 여기서 다시 한 번 키배가 벌어지는데, 이번엔 '그럼 무엇이 사이트와 tangential하게 관련된 컨텐츠이냐'가 쟁점이다. '메인 로고' 및 '메인 내비게이션', 그리고 '다른 연관 컨텐츠'로 구성된 사이드바를 마크업하는 경우를 예로 들면,

* 원본주의자들은 로고나 내비게이션은 컨텐츠가 아닌 독립된 **엘리먼트**이므로 `<aside>` 내에 들어가서는 안 된다고 말한다. 따라서 그들의 마크업은 아래와 같다.

```html
<body>
<div class="sidebar">
    <h1>Title of the website</h1>
    <nav>
        <h1>Main navigation</h1>
        <ul>
            <li><a href="#">1</a></li>
            <li><a href="#">2</a></li>
            <li><a href="#">3</a></li>
        </ul>
    </nav>
    <!-- aside inside sidebar -->
    <aside>
        <h1>My last tweets</h1>
        <ul>
            <li><a href="#">quoque... </a></li>
            <li><a href="#">inleceb... </a></li>
            <li><a href="#">locaaer... </a></li>
        </ul>
    </aside>
</div><!-- /sidebar -->
```

* 반대로 해석주의자들은 로고나 내비게이션 역시 사이트 그 자체에 종속적인 **컨텐츠**로 볼 수 있으므로 `<aside>` 내에 넣을 수 있다고 주장한다.

```html
<body>
<!-- aside as sidebar -->
<aside>
    <h1>Title of the website</h1>
    <nav>
        <h1>Main navigation</h1>
        <ul>
            <li><a href="#">1</a></li>
            <li><a href="#">2</a></li>
            <li><a href="#">3</a></li>
        </ul>
    </nav>
    <section class="tweets">
        <h1>My last tweets</h1>
        <ul>
            <li><a href="#">quoque... </a></li>
            <li><a href="#">inleceb... </a></li>
            <li><a href="#">locaaer... </a></li>
        </ul>
    </section>
</aside><!-- /sidebar -->
```

사이드바를 컨텐츠와 디자인 중 어느 쪽의 개념으로 볼 지에 따른 이슈인데, 논리적으로는 전자가 맞다고 생각하면서도 개인적으로는 뒤의 마크업이 좀더 마음에 든다.

### Reference:

 * [Sidebar and Aside are different]

[w3의 정의]:https://www.w3.org/TR/html5/sections.html#the-aside-element
[키배]:http://html5doctor.com/understanding-aside/
[컨펌]:http://html5doctor.com/aside-revisited/
[Sidebar and Aside are different]:http://aastudio.fr/Sidebar-and-Aside-are-different.html