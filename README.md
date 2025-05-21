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