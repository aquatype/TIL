# Throttling vs. Debouncing

스로틀과 디바운스 모두 과도한 리퀘스트를 줄이기 위한 퍼포먼스 해킹 기법이지만, 그 효과와 용법은 조금 다르다.

### Throttling

함수의 단위시간 당 최대 호출횟수를 제한한다. 드래그, 스크롤, 마우스 위치 추적, 자동완성 등 burst input이 들어오는 DOM 이벤트 계열의 최적화에 적용 가능.

![throttling]

leading call/trailing call은 각각 처음/마지막 리퀘스트에 대한 반응 여부를 지정한다.

```
var throttle = function(func, wait, leading, trailing) {
	var context, args, result;
	var timeout = null;
	var previous = 0;

	var later = function() {
		previous = leading === false ? 0 : timestamp();
		timeout = null;
		result = func.apply(context, args);
		if (!timeout) context = args = null;
	};


	return function() {
		var now = timestamp();
		if (!previous && leading === false) previous = now;
		var remaining = wait - (now - previous);
		context = this;
		args = arguments;

		if (remaining <= 0 || remaining > wait) {
			if (timeout) {
				clearTimeout(timeout);
				timeout = null;
			}
			previous = now;
			result = func.apply(context, args);

			if (!timeout) context = args = null;
		}
		else if (!timeout && trailing !== false) {
			timeout = setTimeout(later, remaining);
		}

		return result;
	};
};
```

### Debouncing

함수가 최종 호출 후 일정 시간이 지나기 전까지는 실행되지 않도록 막는다. 또 하나의 막강한 장점으로, 디바운싱을 사용하면 여러 개의 연속적인 call을 단일 리퀘스트로 **그룹화**하는 것이 가능해진다. 브라우저 리사이징에 따른 엘리먼트 재배치, AJAX 호출, 폼서브밋 등 이벤트 종료 뒤 1회만 실행되는 것이 이상적인 액션에 적용 가능.

![debouncing-trailing]

![debouncing-leading]

trailing call과 leading call 각각의 동작 방식.

```
debounce = function(func, wait, immediate) {
	var timeout, args, context, timestamp, result;

	var later = function() {
		var last = timestamp() - timestamp;

		if (last < wait && last >= 0) {
			timeout = setTimeout(later, wait - last);
		}
		else {
			timeout = null;
			if (!immediate) {
				result = func.apply(context, args);
				if (!timeout) context = args = null;
			}
		}
	};

	return function() {
		context = this;
		args = arguments;
		timestamp = timestamp();
		var callNow = immediate && !timeout;
		if (!timeout) timeout = setTimeout(later, wait);
		if (callNow) {
			result = func.apply(context, args);
			context = args = null;
		}

		return result;
	};
};
```


### Reference:
* [Throttling vs Debouncing (Chris London)]
* [The Difference Between Throttling and Debouncing (Chris Coyier)]
* [Debouncing and Throttling Explained Through Examples]

[throttling]:http://www.chrislondon.co/wp-content/uploads/2013/08/Screen-Shot-2013-08-30-at-6.46.08-AM.png
[debouncing-trailing]:http://www.chrislondon.co/wp-content/uploads/2013/08/Screen-Shot-2013-08-30-at-6.46.16-AM.png
[debouncing-leading]:http://www.chrislondon.co/wp-content/uploads/2013/08/Screen-Shot-2013-08-30-at-6.46.30-AM.png
[Throttling vs Debouncing (Chris London)]:http://www.chrislondon.co/throttling-vs-debouncing/
[The Difference Between Throttling and Debouncing (Chris Coyier)]:https://css-tricks.com/the-difference-between-throttling-and-debouncing/
[Debouncing and Throttling Explained Through Examples]:https://css-tricks.com/debouncing-throttling-explained-examples/