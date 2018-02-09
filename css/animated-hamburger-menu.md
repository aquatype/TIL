# Animated Hamburger Menu

언젠가는 쓸 일이 있지 않을까 해서 만들어봄.  
엘리먼트 크기 및 각종 포지셔닝 수치들에 매직넘버가 난무하는데 좀 깔끔하게 다듬을 필요가 있다

HTML
```html
<a class='hamburger menu' href='#'>
  <span>Menu</span>
</a>
```

SCSS
```scss
a.hamburger {
  position: relative;
  display: inline-block;
  width: 24px;
  height: 24px;

  span,
  &:before,
  &:after {
    position: absolute;
    left: 0;
    right: 0;
    height: 2px;
    background-color: #000;
    font-size: 0;
    transition: all 0.2s linear;
  }

  &:before {
    content: '';
    top: 2px;
  }

  span {
    top: calc(50% - 1px);
  }

  &:after {
    content: '';
    bottom: 2px;
  }
}

a.hamburger.expanded {
  span {
    opacity: 0;
  }

  &:before {
    transform: rotate(45deg);
    top: calc(50% - 1px);
    left: -2px;
    right: -2px;
  }

  &:after {
    transform: rotate(-45deg);
    top: calc(50% - 1px);
    bottom: initial;
    left: -2px;
    right: -2px;
  }

}
```

* [Live Demo]

[Live Demo]:https://codepen.io/aquatype/pen/aVRRYG
