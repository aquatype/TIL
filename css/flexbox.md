# Flexbox

기왕 쓰는 거 제대로 알고 쓰기 위해 개념정리부터...

### 개요
플렉스박스 레이아웃(Flexbox Layout;Flexible Box)은 공간(=컨테이너) 분할과 요소(=아이템) 배치에 특화된 CSS3의 레이아웃 모듈이다. 요소의 크기를 알 수 없거나 크기가 동적인 경우 또한 상정하기 때문에 flex라는 이름이 붙은 것.  
플렉스 속성을 주면 해당 컨테이너가 현재 뷰포트 크기에 맞추어 하위 아이템의 크기(width/height)를 자유롭게 변경할 수 있도록 만든다. 즉 가능한 여백 공간을 모두 차지하도록 하위 아이템 크기를 동적으로 늘일 수도 있고, 반대로 오버플로우를 막기 위해 크기를 줄일 수도 있다.  


### 플렉스 컨테이너
아이템들을 담고 있는 공간 그 자체를 말한다. 아래와 같이 컨테이너를 선언하며, 해당 엘리먼트의 다이렉트 칠드런 엘리먼트들은 모두 플렉스 아이템이 된다.

```css
display: flex; /* 또는 inline-flex */
```

플렉스 컨테이너에도 축이 존재하지만 일반적인 x/y 축의 개념이 아니라 **메인 액시스**(main-axis, 주축)과 **크로스 액시스**(cross-axis, 교차축)의 개념으로 동작한다. 이는 메인 액시스 자체의 방향성이 유동적이기 때문이다.

##### 메인 액시스

1. 메인 액시스의 방향성(=아이템이 배치되는 주 방향)은 `flex-direction` 프로퍼티를 통해 지정한다.

  ```css
flex-direction: row | row-reverse | column | column-reverse;
/*
row: 좌에서 우로 (기본값)
row-reverse: 우에서 좌로
column: 위에서 아래로
column-reverse: 아래에서 위로
*/
```


2. 플렉스박스는 기본적으로 1차원 정렬이기 때문에 아이템 크기를 강제로 줄여서라도 한 줄로 표시하는데, `flex-wrap` 속성을 이용해서 오버플로우 랩핑을 지정할 수 있다.

  ```css
flex-wrap: nowrap | wrap | wrap-reverse;
/*
nowrap: 무조건 한 줄로 처리 (기본값)
wrap: 오버플로우될 경우 여러 줄로 처리
wrap-reverse: 여러 줄로, 아래에서 위로 처리
*/
```

  ![flex-wrap]


3. `flex-direction`과 `flex-wrap` 두 가지를 합쳐서 `flex-flow` 숏핸드 프로퍼티로 기술할 수 있다.


4. 메인 액시스에 대한 아이템의 정렬 기준은 `justify-content` 프로퍼티를 통해 지정한다.

  ```css
justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly;
/*
flex-start: 아이템을 컨테이너 앞에서부터 정렬 (기본값)
flex-end: 아이템을 컨테이너 뒤에서부터 정렬
center: 아이템을 컨테이너 중앙에 정렬
space-between: 첫 아이템은 컨테이너 맨 앞, 마지막 아이템은 맨 뒤, 중간 아이템들은 등간격으로 정렬
space-around: 모든 아이템이 같은 '좌우 간격'을 가짐. 즉 중간 아이템들은 2배의 간격을 갖게 됨
space-evenly: 모든 아이템이 등간격으로 배치됨. 다만 아직 미지원 브라우저가 많음에 주의
*/
```

  ![justify-content]

##### 크로스 액시스

1. 크로스 액시스의 방향성(=아이템이 정렬되는 교차 방향)은 `align-items` 프로퍼티를 통해 지정한다.

  ```css
align-items: flex-start | flex-end | center | stretch | baseline;
/*
stretch: 모든 아이템을 크로스 액시스 방향으로 늘임
baseline: 아이템들의 베이스라인에 맞추어 정렬
*/
```

  ![align-items]


2. 크로스 액시스에 대한 아이템의 정렬 기준은 `align-content` 프로퍼티로 지정해줄 수 있다.  
다만 이는 크로스 액시스 방향으로 여분의 공간이 존재할 경우에만 효과가 있으며, 아이템이 한 줄인 경우에도 당연히 효과가 없다.

  ```css
align-content: stretch | flex-start | flex-end | center | space-between | space-around;
```

  ![align-content]


### 플렉스 아이템

플렉스 컨테이너 하위의 다이렉트 칠드런들은 모두 플렉스 아이템이 된다. 당연하지만 `display: flex` 속성은 상속되지 않는다.  
플렉스 아이템에 적용되는 속성군은 크게 **위치**와 **크기**로 나눌 수 있다.

##### 위치

1. `order` 프로퍼티를 이용해 배치 순서를 지정할 수 있다.

  ```css
order: <integer>; /* 기본값은 0, 음수 가능하며 무조건 낮은 수에서 높은 수 순서 */
```

  이걸 이용하면 `float: right` 등을 써서 엘리먼트 배치할 때 소스 상의 선언 순서와 실제 보이는 순서가 불일치하는 문제도 없고, (주로 디자인 목적으로) 좌/우단 번갈아가며 반대로 배치하는 레이아웃 등을 아주 손쉽게 구현할 수 있다.  


2. 단일 아이템에 대해서만 별도의 크로스 액시스 방향성을 적용하고 싶다면(=`align-items`를 오버라이드하려면) `align-self` 프로퍼티를 사용한다.

  ```css
align-self: auto | flex-start | flex-end | center | baseline | stretch;
```

##### 크기

1. 플렉스 아이템이 본래의 크기보다 커져야/줄어들어야 할 경우, `flex-grow` 및 `flex-shrink` 프로퍼티를 이용하여 아이템 간의 성장 비중을 조정할 수 있다.

  ```css
flex-grow: <integer>; /* 기본값은 0, 음수는 불가능 */
flex-shrink: <integer>; /* 동일 */
```

  만약 모든 플렉스 아이템이 `flex-grow: 1`을 가지고 있을 때 특정 아이템만 `flex-grow: 2`로 선언되어 있다면, 이 아이템은 다른 아이템의 2배 공간을 차지하며 늘어난다. `flex-shrink`도 마찬가지.


2. `flex-basis` 프로퍼티를 이용해 (여유 공간을 제외한) 아이템의 기본 크기를 지정할 수 있다.

  ```css
flex-basis: auto | <length>;
/*
auto: 해당 아이템의 width / height 프로퍼티를 참고함 (기본값)
length: 20%나 5rem, 100px 등 모든 길이 단위 사용 가능
*/
```


3. `flex-grow`, `flex-shrink`, `flex-basis`를 합쳐 `flex` 숏핸드 프로퍼티로 기술할 수 있다.  
특이하게 가급적 `flex`를 사용하기를 권장하는데, 다른 값들을 적절하게 설정해주기 때문에 따로 기술하는 것보다 충돌이 적고 편리하다고 한다.

### 사용 시 주의점

1. `flex-wrap`처럼 오버플로우를 처리하기 위한 프로퍼티가 있기 때문에 오해하기 쉽지만, 플렉스박스는 엄연히 **1차원 정렬**이다. 큰 규모의 2차원 정렬 레이아웃을 사용하려면 `display: grid`를 사용하는 것이 맞다.


2. 레거시 브라우저 호환성을 고려해야 하는 경우에도 큰 부담없이 써먹을 수는 있을 정도까지 커버리지가 좋아졌다. unprefixed로도 커버리지 레이트가 93%에 이를 정도.  IE 11 이상만을 타겟한다면 `min-height` 관련 버그 몇 가지만 신경쓰면 된다.  


3. 그래도 아직은 vendor prefix가 필요한 프로퍼티인데, 이는 브라우저마다 박스 모델이 전부 다르기 때문이다. 속편하게 autoprefixer 쓰자.


### Reference:
 * [W3C Specifications]
 * [A Guide To Flexbox]
 * [Codrops CSS Reference: Flexbox]

[flex-wrap]:../images/flex-flex-wrap.png
[justify-content]:../images/flex-justify-content.png
[align-items]:../images/flex-align-items.png
[align-content]:../images/flex-align-content.png
[W3C Specifications]:https://www.w3.org/TR/css-flexbox-1/
[A Guide To Flexbox]:https://css-tricks.com/snippets/css/a-guide-to-flexbox/
[Codrops CSS Reference: Flexbox]:https://tympanus.net/codrops/css_reference/flexbox/
