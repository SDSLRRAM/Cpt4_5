### 4-1: setInterval을 이용한 타이머 예제

- `setInterval()` 함수를 사용하여 300ms마다 count를 출력
- `clearInterval()`로 일정 조건(count > 4)이 되면 타이머 정지

### 4-2: 콜백 함수를 통한 setInterval 예제

- `setInterval()`의 콜백을 변수 `cbFunc`로 분리
- 함수 재사용성과 디버깅 편의성 증가
- 로직은 `4-1.js`와 동일하지만 코드 구조를 개선한 형태

### 4-3: map() 메서드를 이용한 배열 요소 변환

- 'Array.prototype.map()'을 기반으로 새로운 배열을 반환
- 각 요소에 대해 `currentValue`와 `index`를 콜백 함수 인자로 받아 활용 가능

### 4-4: map()에서 매개변수 순서를 바꿨을 경우우

- `map()`의 콜백 함수는 `(currentValue, index)` 순서로 Argument를 받는다.
- 하지만 이 예제는 `(index, currentValue)`로 잘못 정의되어 결과가 예상과 다르게 출력됨.
- 실제 동작: 첫 번째 인자(index)에 배열 요소가 들어가고, 두 번째 인자(currentValue)는 index로 처리됨
- 이로 인해 출력은 `10 0`, `20 1`, `30 2`로 되고, 결과 배열은 `[5, 6, 7]`이 생성됨

### 4-5: Array.prototype.map() 구현

- `map()` 메서드를 커스터마이즈하여 직접 정의, 콜백 함수에 `call()`을 이용해 `thisArg`를 지정하고, 현재 Arg/index/array 전달
- `thisArg`가 없을 경우 기본값으로 `window` 객체 사용

### 4-6: 콜백 함수와 이벤트에서의 `this`

- `setTimeout` 내의 일반 함수에서의 `this`는 `window` 임
- `forEach` 내 일반 함수도 `this`는 기본적으로 `window`
- `addEventListener` 내의 일반 함수에서는 `this`가 이벤트가 발생한 DOM 요소(button)를 가리킴
- 즉, 함수가 **어떻게 호출되었는가**에 따라 `this`가 다르게 바인딩됨

### 4-7: 객체 메서드를 콜백으로 전달할 때의 this

- `obj.logValues(1, 2)`는 `this`가 `obj`를 가리켜 정상 출력
- 하지만 `[4, 5, 6].forEach(obj.logValues)`처럼 콜백으로 직접 넘기면
  `this` 바인딩이 끊어져 전역 객체(window)가 `this`가 됨
- 메서드가 일반 함수처럼 호출되기 때문으로 보임임
- `bind(obj)` 또는 화살표 함수로 감싸서 명시적으로 `this`를 고정해야 함

### 4-8: self(this)를 활용한 객체 컨텍스트 유지

- `func()` 내부에서 `this`를 `self`로 저장해 클로저에서 참조가 가능
- 따라서 `setTimeout()`의 콜백 내부에서도 `obj1.name`을 정확히 참조 가능, `'obj1'`이 출력됨

### 4-9: setTimeout에서 객체 메서드를 직접 넘겼을 때의 동작

- `setTimeout(obj1.func, 1000)`처럼 메서드를 직접 전달하면 `this`는 바인딩되지 않음
- 하지만 `this`를 사용하지 않고 `obj1.name`을 직접 참조하므로 문제 없음
- 단, `obj1` 참조가 유지되어야 함

### 4-10: 메서드 참조, call(), setTimeout에서의 함수 실행 비교

- `obj2.func = obj1.func`로 참조를 복사했지만 `func` 내부는 `obj1.name`에 직접 접근
- 따라서 `obj2.func()`도 여전히 `'obj1'` 출력
- `callback2`와 `callback3`은 `obj1.func()`와 `obj1.func.call(obj3)`의 실행 결과를 담고 있어
  `setTimeout(callback2, ...)`은 아무 동작도 하지 않음
- `setTimeout()`에는 함수 자체를 전달해야 하며, `callback2`, `callback3`은 함수 호출 결과이므로 `undefined`가 들어감

### 4-11: bind()로 this 고정

- `obj1.func.bind(obj1)`는 `this`를 `obj1`로 고정한 새로운 함수 반환
- `setTimeout()`에 그 반환된 함수를 넘겼기 때문에, 1초 후 `'obj1'` 출력
- `obj1.func.bind(obj2)`는 `this`를 `obj2`로 고정한 새로운 함수 반환. 따라서`'obj2'` 출력
- bind()는 함수 자체는 실행하지 않고, this가 고정된 새 함수를 반환

### 4-12: 콜백 지옥 구조에서의 비동기 처리

- `setTimeout()`을 중첩하여 비동기 순서를 제어
- 각 단계에서 문자열(`coffeeList`)에 커피 이름을 추가하고 출력
- 콜백이 깊게 중첩되며 가독성과 유지보수성이 저하됨됨

### 4-13: 콜백 함수 분리를 통한 가독성 향상

- 이전 예제(4-12.js)의 콜백 지옥 구조를 함수 분리 방식으로 개선
- 각 커피 종류 추가 로직을 독립된 함수로 정의 → 유지보수성 향상
- 실행 순서와 출력 결과는 동일하지만 코드 구조가 훨씬 명확

### 4-14: Promise 체이닝을 이용한 비동기 흐름 제어

- `setTimeout()` 안에서 `resolve()`를 호출하여 다음 `.then()`으로 데이터를 전달
- 각 `.then()` 블록에서 새로운 Promise를 반환하여 순차적으로 로직 실행
- 콜백 지옥 없이 비동기 흐름을 순서대로 표현 가능

### 4-15: 고차 함수로 Promise 체이닝을 일반화한 예제

- `addCoffee(name)`는 `prevName`을 받아 새로운 Promise를 반환하는 고차 함수
- 이 구조는 체이닝 로직을 반복적으로 재사용 가능
- 이전 예제(4-14.js)의 구조를 함수형으로 개선함

### 4-16: Generator를 활용한 비동기 흐름 제어

- `function*`으로 제너레이터 정의 → `yield`를 통해 흐름을 일시 정지 및 재개 가능
- `addCoffee()`는 `setTimeout()` 내에서 `coffeeMaker.next()`를 호출하여 다음 단계로 흐름 전달
- 각 단계에서 누적된 이름이 출력됨

### 4-17: async/await를 사용한 비동기 흐름 제어

- `addCoffee()`는 500ms 후 이름을 resolve하는 Promise 반환
- `coffeeMaker()`는 async 함수로 선언되어 내부에서 await 사용 가능
- `_addCoffee()`는 각 커피 이름을 순차적으로 coffeeList에 추가
- `await`를 사용함으로써 코드의 흐름이 동기적인 형태처럼 자연스럽게 보임

### 5-1: 함수 스코프와 클로저

- `outer()` 함수 내에 지역 변수 `a`와 내부 함수 `inner()`가 존재
- 내부 함수 `inner()`는 외부 스코프의 변수 `a`에 접근 가능 → 클로저(Closure)
- `inner()`를 실행하면 `a`는 1에서 증가되어 `2` 출력

### 5-2: 클로저의 반환값 사용

- `outer()`는 내부에서 `inner()`를 실행 후후 그 결과를 반환
- 내부 함수 `inner()`는 외부 스코프의 변수 `a`에 접근하고 이를 증가시켜 반환
- `outer2 = outer()`는 `2`를 반환하여 저장하고, `console.log(outer2)`는 2 출력

### 5-3: 클로저를 활용한 상태 유지

- `outer()`는 `inner` 함수를 반환 → 반환된 함수는 `a`에 대한 접근 권한을 유지
- `outer2()`를 여러 번 호출해도 `a`는 독립적으로 증가
- 클로저는 함수가 선언 당시의 스코프 체인을 기억하여 외부 변수를 참조하기 때문

### 5-4: 클로저 + IIFE 구조

- 첫 번째 IIFE:
- `a`는 외부에서 접근 불가능. IIFE 내부에서 `a`와 `intervalId`를 정의하였기 때문에.
- `setInterval()`로 1초마다 `inner()` 호출 → `a`가 10 이상이 되면 자동 종료
- 클로저를 통해 `inner()`가 `a`와 `intervalId`를 지속적으로 참조

- 두 번째 IIFE:
- 버튼 생성 후 클릭 이벤트 등록
- 이벤트 핸들러 내부에서 `count`는 `addEventListener()`가 선언된 시점의 렉시컬 스코프에 묶여 있음 → 클로저로 카운트 상태 유지
- 출력: 클릭할 때마다 `1 times clicked`, `2 times clicked`, ...

### 5-5: 클로저 해제와 메모리 관리

- 클로저는 외부 변수의 참조를 유지하기 때문에 불필요한 참조가 남으면 메모리 낭비 할 수 있음

- 1. 반환 함수의 참조 제거
- `outer = null`로 클로저(즉, `inner`)에 대한 참조 제거

- 2. setInterval 내부 함수 참조 제거
- `inner = null`을 통해 클로저를 더 이상 참조하지 않도록 처리
- `clearInterval()` 호출 후 반드시 참조를 끊는 것이 메모리 누수 방지에 중요

- 3. 이벤트 핸들러 해제
- `removeEventListener()`로 등록된 이벤트 제거 후, 핸들러 자체 참조도 제거
- 버튼 클릭 10회 이후 클로저를 제거하여 메모리 정리 완료

### 5-6: 클로저를 이용한 forEach 내 이벤트 핸들러 작성

- `fruits.forEach()` 내에서 각 `fruit` 값에 대해 `<li>` 요소 생성
- 각 항목에 `click` 이벤트 핸들러를 등록하면서 `fruit`을 클로저로 캡처
- 클릭 시 `alert('your choice is ' + fruit)`가 정확히 해당 항목에 대한 메시지 출력
- 실행 시점에 `fruit` 값이 변경되지 않도록 클로저가 각 `fruit` 값을 기억하고 있음

### 5-7: 공통 핸들러 함수 사용 시 클로저 사용하지 않을 경우 문제제

- `alertFruit(fruit)`은 외부에서 직접 호출할 때는 정확히 동작함  
- 하지만 `addEventListener('click', alertFruit)`으로 이벤트 등록 시 `alertFruit`은 매개변수를 전달받지 못해 `undefined` 출력됨
- `alertFruit`은 `fruit` 값을 전달받아야 정상 동작하지만, 이벤트 핸들러로 직접 참조만 넘기면 이벤트 객체만 전달됨

### 5-8: bind()로 argument를 고정한 이벤트 핸들러

- `alertFruit.bind(null, fruit)`는 `fruit` 값을 고정한 새 함수를 반환
- 이렇게 생성된 함수는 클릭 이벤트 시에도 고정된 `fruit`을 인자로 전달받음
- 클로저를 직접 사용하지 않아도, 각 항목별로 독립된 함수 생성 효과를 얻음
- `bind()`는 첫 번째 인자로 `this`를 고정하는데, 이 경우 `null`을 넘겨 `this`는 필요하지 않음

### 5-9: 클로저를 활용한 이벤트 핸들러 빌더

- `alertFruitBuilder(fruit)`는 `fruit`을 캡처한 새 함수를 반환
- `addEventListener()`에 이 반환된 함수를 넘기면, 클릭 시 올바른 과일 이름이 출력됨
- 각 `fruit` 값은 해당 함수의 렉시컬 환경에 저장되므로, 고유한 값 유지

### 5-10: this를 활용한 객체(자동차차) 상태 제어 예제

- 객체 `car`는 `fuel`, `power`,`moved` 를 속성으로 가짐
- `run()` 메서드는 무작위 거리(`km`)만큼 이동 후, 연료가 부족하면 "이동불가" 출력
- 이동 가능 시: 연료 소모(`km / power`), 연료 차감 후 이동 거리 누적
- `this` 키워드를 통해 메서드 내부에서 객체 자신의 속성(`this.fuel`, `this.moved`)에 접근

### 5-11: 클로저를 활용한 객체 내부 상태 은닉

- `createCar()`는 내부 변수 `fuel`, `power`, `moved`를 지역 스코프에 정의하고 외부에 노출하지 않음
- 반환된 객체는  `run()` 메서드를 통해 주행 시도, `moved` 는 getter로 읽기만 가능하게 정의
- 주행 로직은 연료가 충분할 경우 `moved` 증가, 연료 차감

### 5-12: Object.freeze()를 이용한 불변 객체(immutable objec) 생성

- `createCar()`는 내부 상태(`fuel`, `power`, `moved`)를 클로저로 은닉
- `publicMembers` 객체는 외부에서 접근 가능한 메서드와 getter만 포함
- `Object.freeze(publicMembers)`은 속성 추가/변경/삭제 불가능 메서드 오버라이드 및 컨트롤 차단.

### 5-13: bind()와 arguments를 활용한 부분 적용

- `add()` 함수는 전달된 모든 argument들을 합산
- `add.bind(null, 1, 2, 3, 4, 5)`로 앞의 다섯 argument를 고정한 새로운 함수 `addPartial` 생성
- `addPartial(6, 7, 8, 9, 10)` 실행 시 총 10개의 인자가 `add()`에 전달됨

### 5-14: 사용자 정의 partial() 함수 구현

- `partial(func, ...args)`는 첫 번째 인자를 함수로 받아, 이후 argument들을 고정한 새로운 함수를 반환
- 반환된 함수는 추가 argument를 받아 기존 argument와 결합하여 `func.apply()`로 실행
- 내부적으로 `arguments`, `slice`, `concat` 사용하여 argument 처리

### 5-15: Placeholder(_)를 지원하는 partial 함수 구현

- `Object.defineProperty(window, '_', ...)`를 통해 `_`를 글로벌 상수로 정의
- 값: `'EMPTY_SPACE'` (변경 불가, 열거 불가)

- `partial2(func, ...args)`: 인자 목록 중 `_`이 있는 자리를 나중에 전달받은 인자로 대체 `shift()`를 통해 순서대로 빈칸을 채우고, 남은 인자는 끝에 concat을 수행

### 5-16: 디바운스 함수 구현

- `debounce(eventName, func, wait)`는 이벤트 이름, 콜백 함수, 지연 시간(ms)을 받아,
  이벤트가 연속 발생할 경우 최종 이벤트만 일정 시간 후 실행되도록 제어
- 이벤트가 발생할 때마다 기존 타이머 제거 (`clearTimeout`)
- 새 타이머 등록 → 지정 시간(`wait`) 동안 이벤트가 없으면 실행됨
- `func.bind(this, event)`를 통해 원래 context 및 이벤트 객체 유지

### 5-17: 커링(Currying)을 이용한 이항 함수 분리 호출

- `curry3(func)`는 두 인자를 받는 함수 `func(a, b)`를 `curry3(func)(a)(b)` 형식으로 호출할 수 있도록 변환
- `curry()`를 통해 여러 개의 인자를 받는 함수를 단일 인자 함수로 변환할 수 있음