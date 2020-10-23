#  RxJS

## 특징

### 1. 옵저버 패턴

![옵저버 패턴 UML 다이어그램](https://camo.githubusercontent.com/7e3b8cba69be744a525aa363fc74db58b099c0fe/68747470733a2f2f75706c6f61642e77696b696d656469612e6f72672f77696b6970656469612f636f6d6d6f6e732f7468756d622f382f38642f4f627365727665722e7376672f3132383170782d4f627365727665722e7376672e706e67)

+ 관찰하는 역할의 옵저버 객체들을 서브젝트라는 객체에 등록 후, 서브젝트 객체의 상태 변경이 일어나면 여기에 의존성 있는 옵저버들의 메서드를 호출해서 알림
+ 이벤트를 관찰하다가 이벤트가 발생하면 리스너가 호출되는 addEventListner 및 removeEventListener 메서드로 이벤트를 전파하는 방식과 유사한데 특정 이벤트를 관찰할 옵저버(리스너)를 등록 후 해당 이벤트가 발생했을 때 알려줌

### 2. 옵저버블 

+ 옵저버블은 특정 객체를 관찰하는 옵저버에게 여러 이벤트나 값을 보내는 역할을 함
+ 옵저버블 객체 안에서 여러 개의 값이나 이벤트를 취급하고, 옵저버의 함수를 호출해 필요한 값이나 이벤트를 보내는 방식

#### 옵저버블의 라이프사이클

1. 옵저버블 생성(Create)
2. 옵저버블 구독(Subscribing)
3. 옵저버블 실행(Executing)
4. 옵저버블 구독 해제(Disposing)

### 3. 마블 다이어그램

+ 연산자를 쉽게 이해하는 데 도움을 주고, 개발자가 생각하는 흐름을 그림으로 도식화하는데 유용

+ **0부터 4까지의 수 중 홀수 값을 찾는 filter 연산자의 마블 다이어그램**

  ![filter 마블다이어그램](https://camo.githubusercontent.com/ddd1d779231fb84c3fff0e6753fd0f692e74567f/68747470733a2f2f72786a732d6465762e66697265626173656170702e636f6d2f6173736574732f696d616765732f6d6172626c652d6469616772616d732f66696c7465722e706e67)

  1. 왼쪽에서 오른쪽으로 향하는 가로 줄은 시간에 따라 함수에서 발행하는 값들을 표시. 입력(input) 옵저버블이라고도 함.
  2. 동그란 모양이고 그 안에 실제 값이 표시되어 있음. 이렇게 가로줄이 하나의 옵저버블
  3. 제일 마지막에 있는 수직선(|)은 구독 완료(complete 함수 호출)를 의미
  4. 가운데 쓰여 있는 박스는 연산자를 가리킴
  5. 가운데 연산자 아래 있는 옵저버블은 연산자를 이용해서 생성한 옵저버블. 출력(output) 옵저버블이라고도 함. 각 값들은 색깔이나 숫자로 구분해서 어떤 입력값으로 어떤 발행 값을 만들었는지 구분할 수 있게 함. 동그라미, 세모, 네모를 사용해서 값을 구분하기도 함
  6. X 표시는 에러가 발생해 옵저버블 실행을 종료했음을 뜻함

+ 참고: [https://rxmarbles.com/](https://rxmarbles.com/)

### 4. 파이퍼블 연산자

+ 생성 함수로 만들어진 옵저버블 인스턴스를 pipe 함수 안에서 다룰 수 있는 연산자

+ 기본적으로 파이퍼블 연산자는 rxjs/operators(s가 뒤에 붙어 있어 rxjs/operator와 혼동 X) 아래에서 불러올 수 있음

  ```
  <옵저버블 인스턴스>.pipe(연산자1 (), 연산자2(), ...)
  ```

  ```
  <옵저버블 인스턴스>.pipe(연산자1 ()).pipe(연산자2 ())
  ```

<br />

## 장점

+ RxJS의 장점은 이벤트 구독을 취소하고, 모든 이벤트를 하나의 스트림으로 제어 가능

<br />

## 패턴

+ 옵저버블의 변수 스타일은 이름 뒤에 `const click$`처럼 `$`을 뒤에 붙여주는 것이 정형화되어 있음
+ `subscribe`는 `next, error, complete`를 파라미터로 받음

<br />

## API 

### 📕 함수(rxjs/index)

#### 01. of​ :star:

+ args 순서대로 값을 반환

+ 생성자

+ 여러 타입을 담을 수 있음(?)

  ```js
  import { of } from 'rxjs';
  
  /* 숫자 반환 */
  const source = of(1, 2, 3, 4, 5);
  // output: 1,2,3,4,5
  
  /* object, array, function 반환 */
  const source = of({ name: 'Brian' }, [1, 2, 3], function hello() {
    return 'Hello';
  });
  // output: {name: 'Brian'}, [1,2,3], function hello() { return 'Hello' }
  ```

#### 02. from :star:

+ 아래의 타입을 옵저버블로 변환

  + Observable
  + Array
  + Promise
  + Iterable
  + String
  + ArrayLike

  ```js
  import { from } from 'rxjs';
  
  /* 배열을 옵저버블로 변환 */
  const arraySource = from([1, 2, 3, 4, 5]);
  // output: 1,2,3,4,5
  
  /* Promise를 옵저버블로 변환 */
  const promiseSource = from(new Promise(resolve => resolve('Hello World!')));
  // output: 'Hello World'
  
  /* 컬렉션을 옵저버블로 변환 */
  const map = new Map();
  map.set(1, 'Hi');
  map.set(2, 'Bye');
  const mapSource = from(map);
  // output: [1, 'Hi'], [2, 'Bye']
  
  /* 문자열을 옵저버블로 변환 */
  const source = from('Hello World');
  //output: 'H','e','l','l','o',' ','W','o','r','l','d'
  ```

#### 03. fromEvent

+ EventEmitter 클래스의 객체와 조합하거나 브라우저의 이벤트를 옵저버블로 바꿀 때 사용

  ```js
  import { fromEvent } from 'rxjs';
  import { map } from 'rxjs/operators';
  
  // 클릭 이벤트를 생성하고 옵저버블로 변환
  const source = fromEvent(document, 'click');
  
  // event.timeStamp를 문자열로 매핑
  const example = source.pipe(map(event => `Event time: ${event.timeStamp}`));
  // output (example): 'Event time: 7276.390000000001'
  
  // document를 클릭할 때마다 콘솔 창에 timeStamp 출력 확인
  const subscribe = example.subscribe(val => console.log(val));
  ```

#### 04. defer

+ 팩토리 함수로 옵저버블을 생성 후 구독하는 시점에 팩토리 함수를 호출해 이미 생성한 옵저버블을 리턴받아 구독

+ from과 defer의 차이점

  | from                                                    | defer                                                        |
  | ------------------------------------------------------- | ------------------------------------------------------------ |
  | 프로미스 내부 구현부가 언제 실행되던지 상관 없을 때     | 옵저버블을 구독하는 시점에 프로미스를 생성해 구현부가 실행되어야 할 때 |
  | 이미 실행 중이거나 완료한 프로미스를 옵저버블로 만들 때 | 프로미스 객체 생성 시점이 구독하는 시점이어야 할 때          |

  ```js
  import { defer, of, timer, merge } from 'rxjs';
  import { switchMap } from 'rxjs/operators';
  
  /* 구독 시 현재 Date 가져오기 */
  const s1 = of(new Date()); // ⓑ 현재 Date 
  const s2 = defer(() => of(new Date())); // ⓒ 구독 시점의 Date
  
  console.log(new Date()); // ⓐ 현재 Date
  
  timer(2000)
    .pipe(switchMap(_ => merge(s1, s2)))
    .subscribe(console.log);
  
  // output 
  // Fri Oct 16 2020 11:38:29 GMT+0900 ⓐ 첫 번째 콘솔 로그의 현재 Date
  // Fri Oct 16 2020 11:38:29 GMT+0900 ⓑ 옵저버블 생성 시점의 Date
  // Fri Oct 16 2020 11:38:31 GMT+0900 ⓒ 구독 시점의 Date
  ```

#### 05. range

+ 범위 지정 후 그 값을 순서대로 발행

+ 반복문이 필요할 때 사용

  ```js
  import { range } from 'rxjs';
  
  /* 1부터 10까지의 숫자 구하기 */
  const source = range(1, 10);
  // output: 1,2,3,4,5,6,7,8,9,10
  ```

#### 06. interval

+ ms 단위로 값을 발행

  ```js
  import { interval } from 'rxjs';
  
  /* 1초마다(1000ms) 숫자 3까지 오름차순 숫자를 출력 */
  const numbers = interval(1000);
  const takeFourNumbers = numbers.pipe(take(4));
  takeFourNumbers.subscribe(x => console.log('Next: ', x));
  
  // output
  // Next: 0
  // Next: 1
  // Next: 2
  // Next: 3
  ```

#### 07. timer

+ 파라미터가 하나일 경우 ms 이후에 한 번 값을 발행

  ```js
  import { timer } from 'rxjs';
  
  /* 1초 후에 숫자 하나를 출력 후 완료 */
  const source = timer(1000);
  // output: 0
  ```

+ 파라미터가 두 개일 경우 ms 이후에 두 번째 파라미터만큼 주기적으로 값을 발행

  ```js
  import { timer } from 'rxjs';
  
  /* 3초 후 첫 번째 값을 방출하고 1초마다 오름차순 숫자를 출력 */
  const source = timer(3000, 1000);
  // output: 0,1,2,3,4,5......
  ```

#### 08. empty

+ 값 발행 후 중간에 멈춰야 하는 상황에 사용

+ 단독으로 사용하지 않고 다른 함수나 연산자와 조합하여 complete 함수를 호출해야 할 때 사용

+ 즉, **바로 구독을 완료해야 할 때 사용**

  ```js
  import { empty } from 'rxjs';
  
  /* 비어 있는 즉시 구독 완료 */
  const subscribe = empty().subscribe({
    next: () => console.log('Next'),
    complete: () => console.log('Complete!')
  });
  // output: 'Complete!'
  ```

  ```js
  import { empty } from 'rxjs';
  import { startWith } from 'rxjs/operators';
  
  /* 숫자 7을 내보낸 다음 구독 완료 */
  const result = empty().pipe(startWith(7));
  result.subscribe(x => console.log(x));
  ```

#### 09. throw

+ 구독 시 에러를 발생시킬 때 사용

  ```js
  import { throwError } from 'rxjs';
  
  /* 구독 시 에러 발생 */
  const source = throwError('This is an error!');
  const subscribe = source.subscribe({
    next: val => console.log(val),
    complete: () => console.log('Complete!'),
    error: val => console.log(`Error: ${val}`)
  });
  // output: 'Error: This is an error!'
  ```

#### 11. throwError

+ 옵저버블로 값을 발행하다가 에러를 발생시키고 종료해야 하는 상황에 사용

  ```js
  import { throwError, concat, of } from 'rxjs';
  
  /* 숫자 7을 출력 후 에러를 발생 */
  const result = concat(of(7), throwError(new Error('This is an error!')));
  result.subscribe(x => console.log(x), e => console.error(e));
  // output
  // 7
  // Error: This is an error!
  ```

#### 12. concat :star:

+ 완료 순서대로 옵저버블 구독

  ```js
  import { of, concat } from 'rxjs';
  
  concat(
    of(1, 2, 3),
    // 첫 번째 완료 후 구독
    of(4, 5, 6), 
    // 두 번째 완료 후 구독
    of(7, 8, 9)
  )
  .subscribe(console.log);
  // output: 1, 2, 3, 4, 5, 6, 7, 8, 9
  ```

+ 완료되지 않으면 후속 옵저버블이 실행되지 않음

  ```js
  import { interval, of, concat } from 'rxjs';
  
  // 미완료로 인한 옵저버블 미실행
  concat(
    interval(1000), 
    of('This', 'Never', 'Runs')
  )
  .subscribe(console.log);
  // output: 1,2,3,4.....
  ```

#### 13. combineLatest :star:

+ 여러 옵저버블을 결합하여 각 옵저버블에서 마지막으로 방출된 값에서 값을 방출

  ```js
  import { fromEvent, combineLatest } from 'rxjs';
  import { mapTo, startWith, scan, tap, map } from 'rxjs/operators';
  
  /* 2개의 버튼에서 이벤트 결합 */
  const redTotal = document.getElementById('red-total');
  const blackTotal = document.getElementById('black-total');
  const total = document.getElementById('total');
  
  const addOneClick$ = id =>
    fromEvent(document.getElementById(id), 'click').pipe(
      // 모든 클릭을 1로 매핑
      mapTo(1),
      // 누계 유지
      scan((acc, curr) => acc + curr, 0),
      startWith(0)
    );
  
  combineLatest(addOneClick$('red'), addOneClick$('black'))
    .subscribe(([red, black]: any) => {
      redTotal.innerHTML = red;
      blackTotal.innerHTML = black;
      total.innerHTML = red + black;
    });
  ```

#### 14. iif

+ 조건에 따라 첫 번째 또는 두 번째 관찰 항목을 구독

  ```js
  import { iif, of, interval } from 'rxjs';
  import { mergeMap } from 'rxjs/operators';
  
  const r$ = of('R');
  const x$ = of('X');
  
  interval(1000)
    .pipe(mergeMap(v => iif(() => v % 4 === 0, r$, x$)))
    .subscribe(console.log);
  
  // output: R, X, X, X, R, X, X, X, e
  ```





<br />

### 📗 연산자(rxjs/operator)

#### 01. startWith :star:

+ 주어진 값을 먼저 내보냄

  ```js
  import { startWith } from 'rxjs/operators';
  import { of } from 'rxjs';
  
  /* 숫자 시퀀스 */
  const source = of(1, 2, 3);
  const example = source.pipe(startWith(0));
  // output: 0,1,2,3
  const subscribe = example.subscribe(val => console.log(val));
  ```

#### 02. filter :star:

+ 제공에 충족하는 값을 출력

+ 주로 파이퍼블 연산자(pipe 함수 안에서 다룰 수 있는 연산자)와 연결해서 사용

  ```js
  import { from } from 'rxjs';
  import { filter } from 'rxjs/operators';
  
  /* 1부터 5까지에서 짝수 출력 */
  const source = from([1, 2, 3, 4, 5]);
  const example = source.pipe(filter(num => num % 2 === 0));
  // output: "Even number: 2", "Even number: 4"
  const subscribe = example.subscribe(val => console.log(`Even number: ${val}`));
  ```

#### 03. first

+ 처음으로 일치하는 값을 출력 

  ```js
  import { from } from 'rxjs';
  import { first } from 'rxjs/operators';
  
  /* 첫 번째 값 출력 */
  const source = from([1, 2, 3, 4, 5]);
  const example = source.pipe(first());
  // output: "First value: 1"
  const subscribe = example.subscribe(val => console.log(`First value: ${val}`));
  ```

+ 두 번째 인자로 기본값을 줄 수 있음

  ```js
  import { from } from 'rxjs';
  import { first } from 'rxjs/operators';
  
  /* 기본값 출력 */
  const source = from([1, 2, 3, 4, 5]);
  const example = source.pipe(first(val => val > 5, 'Nothing'));
  // output: 'Nothing'
  const subscribe = example.subscribe(val => console.log(val));
  ```

#### 04. last

+ 마지막으로 일치하는 값을 출력

  ```js
  import { from } from 'rxjs';
  import { last } from 'rxjs/operators';
  
  /* 마지막 값 출력 */
  const source = from([1, 2, 3, 4, 5]);
  const example = source.pipe(last());
  // output: "Last value: 5"
  const subscribe = example.subscribe(val => console.log(`Last value: ${val}`));
  ```

+ first와 마찬가지로 두 번째 인자로 기본값을 줄 수 있음

#### 05. take :star:

+ 정해진 갯수만큼 구독하고 구독을 해지

  ```js
  import { of } from 'rxjs';
  import { take } from 'rxjs/operators';
  
  /* take 안의 개수만큼 출력 */
  const source = of(1, 2, 3, 4, 5);
  const example = source.pipe(take(3));
  // output: 1, 2, 3
  const subscribe = example.subscribe(val => console.log(val));
  ```

+ interval과 같이 무한 반복이 실행되는 연산자와 함께 사용

  ```js
  import { interval } from 'rxjs';
  import { take } from 'rxjs/operators';
  
  /* 5초마다 숫자 5개까지 출력 */
  const interval$ = interval(5000);
  const example = interval$.pipe(take(5));
  // output: 0,1,2,3,4
  const subscribe = example.subscribe(val => console.log(val));
  ```

#### 06. takeUntil :star:

+ 특정 이벤트가 발생할 때까지 옵저버블을 구독해야할 때 사용

  ```js
  import { interval, timer } from 'rxjs';
  import { takeUntil } from 'rxjs/operators';
  
  /* 타이머가 방출될 때까지 값 출력 */
  const source = interval(1000);
  // 작업수행 시간: 7초
  const timer$ = timer(7000);
  // 1초마다 0부터 7초 동안 6개의 숫자가 출력
  const example = source.pipe(takeUntil(timer$));
  //output: 0,1,2,3,4,5
  const subscribe = example.subscribe(val => console.log(val));
  ```

  ```js
  import { fromEvent, interval } from 'rxjs';
  import { takeUntil } from 'rxjs/operators';
  
  /* 첫 번째 클릭이 발생할 때까지 매 초마다 숫자 출력 */
  const source = interval(1000);
  const clicks = fromEvent(document, 'click');
  const result = source.pipe(takeUntil(clicks));
  result.subscribe(x => console.log(x));
  ```

#### 07. takeWhile

+ take와 filter가 합쳐진 연산자

  ```js
  import { of } from 'rxjs';
  import { takeWhile } from 'rxjs/operators';
  
  /* 4를 찾을 때까지 출력 */
  const source = of(1, 2, 3, 4, 5);
  const example = source.pipe(takeWhile(val => val <= 4));
  // output: 1,2,3,4
  const subscribe = example.subscribe(val => console.log(val));
  ```

  ```js
  import { of } from 'rxjs';
  import { takeWhile, filter } from 'rxjs/operators';
  
  const source$ = of(1, 2, 3, 9);
  // 포함 플래그를 사용하면 술어가 false를 리턴하도록 값 내보내기 가능
  source$.pipe(takeWhile(val => val <= 3, true)).subscribe(console.log);
  // output: 1, 2, 3, 9
  ```

+ takeWhile과 filter의 차이점

  ```js
  import { of } from 'rxjs';
  import { takeWhile, filter } from 'rxjs/operators';
  
  // emit 3, 3, 3, 9, 1, 4, 5, 8, 96, 3, 66, 3, 3, 3
  const source = of(3, 3, 3, 9, 1, 4, 5, 8, 96, 3, 66, 3, 3, 3);
  
  /* takeWhile은 3일 때까지 출력 */
  // output: [3, 3, 3]
  source
    .pipe(takeWhile(it => it === 3))
    .subscribe(val => console.log('takeWhile', val))
  
  /* filter는 3을 찾아 모두 출력 */
  // output: [3, 3, 3, 3, 3, 3, 3]
  source
    .pipe(filter(it => it === 3))
    .subscribe(val => console.log('filter', val));
  ```

#### 08. takeLast

+ 파라미터 수 만큼 저장해뒀다가 구독 완료 시에 일괄적으로 출력

  ```js
  import { of } from 'rxjs';
  import { takeLast } from 'rxjs/operators';
  
  /* 완료 전에 마지막으로 내보낸 2개의 값 가져오기 */
  const source = of('Ignore', 'Ignore', 'Hello', 'World!');
  const example = source.pipe(takeLast(2));
  // output: Hello, World!
  const subscribe = example.subscribe(val => console.log(val));
  ```

  ```js
  import { range } from 'rxjs';
  import { takeLast } from 'rxjs/operators';
  
  /* 숫자가 높은 마지막 3개의 값을 가져오기 */
  const many = range(1, 100);
  const lastThree = many.pipe(takeLast(3));
  // output: 98, 99, 100
  lastThree.subscribe(x => console.log(x)); 
  ```

+ 발행하는 값이 `[0, 2, 4, 6, 8, 10]`일때 `takeLast(3)`일 경우 내부에 저장된 값은 다음과 같음

  | 발행값 | 내부배열   |
  | ------ | ---------- |
  | 0      | [0]        |
  | 2      | [0, 2]     |
  | 4      | [0, 2, 4]  |
  | 6      | [6, 2, 4]  |
  | 8      | [6, 8, 4]  |
  | 10     | [6, 8, 10] |

#### 09. skip

+ 이름 그대로 n개 만큼의 출력을 건너 뜀

  ```js
  import { interval } from 'rxjs';
  import { skip } from 'rxjs/operators';
  
  /* 1초마다 방출하지만 0부터 4까지 5개의 값을 건너 뛰고 방출 */
  const source = interval(1000);
  const example = source.pipe(skip(5));
  //output: 5...6...7...8........
  const subscribe = example.subscribe(val => console.log(val));
  ```

+ filter보다 간결한 코드로 동일한 결과값을 낼 수 있음

  ```js
  import { from } from 'rxjs';
  import { skip, filter } from 'rxjs/operators';
  
  const numArrayObs = from([1,2,3,4,5,6,7,8,9,10]);
  
  /* skip 사용 */
  const skipObs = numArrayObs.pipe(skip(2))
      .subscribe(console.log);
  // output: 3, 4, 5...
  
  /* filter 사용*/
  const filterObs = numArrayObs.pipe(
      filter((val, index) => index > 1)
    )
    .subscribe(console.log);
  // output: 3, 4, 5... 
  ```

#### 10. skipUntil

+ takeUntil과 반대로 옵저버블이 실행될 때까지 건너 뜀

  ```js
  import { interval, timer } from 'rxjs';
  import { skipUntil } from 'rxjs/operators';
  
  /* 옵저버블 방출시 까지 건너 뛰기 */
  const source = interval(1000);
  // 옵저버블이 방출될 때(6초)까지 방출 값을 건너 뜀
  const example = source.pipe(skipUntil(timer(6000)));
  // output: 5...6...7...8........
  const subscribe = example.subscribe(val => console.log(val));
  ```

  ```js
  import { interval, fromEvent } from 'rxjs';
  import { skipUntil } from 'rxjs/operators';
  
  /* 사용자가 페이지 아무 곳이나 클릭할 때까지 모든 방출 값을 건너 뜀 */
  const intervalObservable = interval(1000);
  const click = fromEvent(document, 'click');
  
  const emitAfterClick = intervalObservable.pipe(
    skipUntil(click)
  );
  // clicked at 4.6s. output: 5...6...7...8........ or
  // clicked at 7.3s. output: 8...9...10..11.......
  const subscribe = emitAfterClick.subscribe(value => console.log(value));
  ```

#### 11. skipWhile

+ 조건을 만족하지 않는 순간부터 값을 출력

  ```js
  import { interval } from 'rxjs';
  import { skipWhile } from 'rxjs/operators';
  
  /* 5초 이후 발행 */
  const source = interval(1000);
  const example = source.pipe(skipWhile(val => val < 5));
  // output: 5...6...7...8........
  const subscribe = example.subscribe(val => console.log(val));
  ```

#### 12. distinct

+ 중복은 제거하고 출력

+ 값 비교에는 === 연산자 사용

  ```js
  import { of } from 'rxjs';
  import { distinct } from 'rxjs/operators';
  
  /* 중복 제거 */
  of(1,2,3,4,5,1,2,3,4,5)
    .pipe(distinct())
    .subscribe(console.log);
  // output: 1,2,3,4,5
  ```

  ```js
  import { from } from "rxjs";
  import { distinct } from "rxjs/operators";
  
  /* key 선택으로 중복 구분 */
  const obj1 = { id: 3, name: "name 1" };
  const obj2 = { id: 4, name: "name 2" };
  const obj3 = { id: 3, name: "name 3" };
  const vals = [obj1, obj2, obj3];
  
  from(vals)
    .pipe(distinct(e => e.id))
    .subscribe(console.log);
  
  /*
  output:
  {id: 3, name: "name 1"}
  {id: 4, name: "name 2"}
  */ 
  ```

#### 13. distinctUntilChanged :star:

+ 중복값이 연속으로 발행된 경우에만 제거

  ```js
  import { from } from 'rxjs';
  import { distinctUntilChanged } from 'rxjs/operators';
  
  /* 연속으로 발행된 중복값 제거 */
  const source$ = from([1, 1, 2, 2, 3, 3]);
  
  source$
    .pipe(distinctUntilChanged())
    .subscribe(console.log);
  // output: 1,2,3
  ```

#### 14. distinctUntilKeyChanged

+ 객체 속성을 기준으로 중복값을 비교

+ 비교기 기능이 제공되면 해당 값을 내보내야하는지 여부를 테스트하기 위해 각 항목에 대해 호출

+ 비교기 기능이 제공되지 않으면 기본적으로 동등성 검사가 사용

  ```js
  import { from } from 'rxjs';
  import { distinctUntilKeyChanged } from 'rxjs/operators';
  
  /* 마지막 방출된 이름 속성을 기준으로 고유한 값만 출력 */
  const source$ = from([
    { name: 'Brian' },
    { name: 'Joe' },
    { name: 'Joe' },
    { name: 'Sue' }
  ]);
  
  source$
    .pipe(distinctUntilKeyChanged('name'))
    // output: { name: 'Brian }, { name: 'Joe' }, { name: 'Sue' }
    .subscribe(console.log);
  ```

  ```js
  import { fromEvent } from 'rxjs';
  import { distinctUntilKeyChanged, pluck } from 'rxjs/operators';
  
  /* 키보드 이벤트 */
  const keys$ = fromEvent(document, 'keyup')
    .pipe(
      distinctUntilKeyChanged<KeyboardEvent>('code'),
      pluck('key')
    );
  
  keys$.subscribe(console.log);
  ```

#### 15. pluck

+ 맵처럼 동작하지만 소스 옵저버블에서 객체를 리턴할 때 객체의 property를 뽑아냄

  ```js
  import { from } from 'rxjs';
  import { pluck } from 'rxjs/operators';
  
  /* 객체의 속성 값 뽑아내기 */
  const source = from([{ name: 'Joe', age: 30 }, { name: 'Sarah', age: 35 }]);
  const example = source.pipe(pluck('name'));
  // output: "Joe", "Sarah"
  const subscribe = example.subscribe(val => console.log(val));
  ```

  ```js
  import { fromEvent } from 'rxjs';
  import { pluck } from 'rxjs/operators';
  
  /* 클릭할 때마다 클릭한 타겟 요소의 tagName에 매핑 */
  const clicks = fromEvent(document, 'click');
  const tagNames = clicks.pipe(pluck('target', 'tagName'));
  // output: HTML, div, h1, li, a ...
  tagNames.subscribe(x => console.log(x));
  ```

#### 16. concatAll

+ 소스에서 방출된 모든 옵저버블을 직렬 방식으로 결합

+ 이전 내부 옵저버블이 완료된 후에 각 내부 옵저버블을 구독하고 모든 값을 반환된 옵저버블에 병합

  ```js
  import { map, concatAll } from 'rxjs/operators';
  import { of, interval } from 'rxjs';
  
  /* 2초마다 추가된 값에서 오름차순으로 출력 */
  const source = interval(2000);
  const example = source.pipe(
    map(val => of(val + 10)),
    concatAll()
  );
  // output: 'Example with Basic Observable 10', 'Example with Basic Observable 11'...
  const subscribe = example.subscribe(val =>
    console.log('Example with Basic Observable:', val)
  );
  ```

  ```js
  import { interval, of } from 'rxjs';
  import { take, concatAll } from 'rxjs/operators';
  
  /* 옵저버블을 구독하고 완료되면 다음 옵저버블을 구독 */
  const obs1 = interval(1000).pipe(take(5));
  const obs2 = interval(500).pipe(take(2));
  const obs3 = interval(2000).pipe(take(1));
  const source = of(obs1, obs2, obs3);
  const example = source.pipe(concatAll());
  /*
    output: 0,1,2,3,4,0,1,0
    obs1: 0,1,2,3,4 (complete)
    obs2: 0,1 (complete)
    obs3: 0 (complete)
  */
  
  const subscribe = example.subscribe(val => console.log(val));
  ```

#### 17. scan :star:

+ reduce와 비슷하지만 소스가 값을 방출할 때마다 현재 누적값을 방출

  ```js
  import { of } from 'rxjs';
  import { scan } from 'rxjs/operators';
  
  /* 누적 합계 구하기 */
  const source = of(1, 2, 3);
  // 0부터 시작
  const example = source.pipe(scan((acc, curr) => acc + curr, 0));
  // log accumulated values
  // output: 1,3,6
  const subscribe = example.subscribe(val => console.log(val));
  ```

  ```js
  import { fromEvent } from 'rxjs';
  import { scan, mapTo } from 'rxjs/operators';
  
  /* 클릭 이벤트 수 계산 */
  const clicks = fromEvent(document, 'click');
  const ones = clicks.pipe(mapTo(1));
  const seed = 0;
  const count = ones.pipe(scan((acc, one) => acc + one, seed));
  count.subscribe(x => console.log(x));
  ```

#### 18. mergeMap :star:

+ 옵저버블에 매핑하고 값을 내보냄

+ 내부 옵저버블을 평면화하고 싶지만 내부 구독 수를 수동으로 제어하려는 경우에 가장 적합

  ```js
  import { of, interval } from 'rxjs';
  import { mergeMap, map } from 'rxjs/operators';
   
  /* 1초마다 각 문자를 옵저버블에 매핑하고 평평하게 만듦 */
  const letters = of('a', 'b', 'c');
  const result = letters.pipe(
    mergeMap(x => interval(1000).pipe(map(i => x+i))),
  );
  result.subscribe(x => console.log(x));
   
  // Results in the following:
  // a0
  // b0
  // c0
  // a1
  // b1
  // c1
  // continues to list a,b,c with respective ascending integers
  ```

#### 19. catchError :star:

+ 새로운 옵저버블을 반환하거나 에러를 발생시켜 처리할 옵저버블에서 오류를 잡음

  ```js
  import { throwError, of } from 'rxjs';
  import { catchError } from 'rxjs/operators';
  
  /* 옵저버블에서 에러 잡기 */
  const source = throwError('This is an error!');
  // 오류 메세지와 함께 옵저버블 값을 반환하여 오류를 정상적으로 처리
  const example = source.pipe(catchError(val => of(`I caught: ${val}`)));
  // output: 'I caught: This is an error'
  const subscribe = example.subscribe(val => console.log(val));
  ```

  ```js
  import { of } from 'rxjs';
  import { map, catchError } from 'rxjs/operators';
  
  /* 오류가 있을 때 다른 옵저버블로 계속 */
  of(1, 2, 3, 4, 5).pipe(
      map(n => {
    	   if (n === 4) {
  	       throw 'four!';
        }
       return n;
      }),
      catchError(err => of('I', 'II', 'III', 'IV', 'V')),
    )
    .subscribe(x => console.log(x));
  // output: 1, 2, 3, I, II, III, IV, V
  ```

#### 20. tap :star:

+ 소스에서 각 방출을 가로채고 함수를 실행

+ 오류가 발생하지 않는 한 소스와 동일한 출력을 반환

  ```js
  import { of } from 'rxjs';
  import { tap, map } from 'rxjs/operators';
  
  /* 탭을 이용한 로깅 */
  const source = of(1, 2, 3, 4, 5);
  // 소스의 값을 투명하게 기록
  const example = source.pipe(
    tap(val => console.log(`BEFORE MAP: ${val}`)),
    map(val => val + 10),
    tap(val => console.log(`AFTER MAP: ${val}`))
  );
  
  // 값을 변환하지 않음
  //output: 11...12...13...14...15
  const subscribe = example.subscribe(val => console.log(val));
  ```

#### 21. share :star:

+ 여러 구독자의 소스를 공유

+ 구독자가 하나 이상있으면 옵저버블을 구독할 수 있고 데이터를 방출

+ 모든 구독자가 구독을 취소하면 옵저버블 소스에서 구독을 취소

  ```js
  import { timer } from 'rxjs';
  import { tap, mapTo, share } from 'rxjs/operators';
  
  /* 소스를 공유하는 여러 구독자 */
  const source = timer(1000);
  
  const example = source.pipe(
    tap(() => console.log('***SIDE EFFECT***')),
    mapTo('***RESULT***')
  );
  
  /*
    ***NOT SHARED, SIDE EFFECT WILL BE EXECUTED TWICE***
    output:
    "***SIDE EFFECT***"
    "***RESULT***"
    "***SIDE EFFECT***"
    "***RESULT***"
  */
  const subscribe = example.subscribe(val => console.log(val));
  const subscribeTwo = example.subscribe(val => console.log(val));
  
  //share observable among subscribers
  const sharedExample = example.pipe(share());
  /*
    ***SHARED, SIDE EFFECT EXECUTED ONCE***
    output:
    "***SIDE EFFECT***"
    "***RESULT***"
    "***RESULT***"
  */
  const subscribeThree = sharedExample.subscribe(val => console.log(val));
  const subscribeFour = sharedExample.subscribe(val => console.log(val));
  ```

#### 22. merge :star:

+ 옵저버블이 내보낸 각 값에 주어진 함수를 적용 후 결과 값을 옵저버블로 내보냄

  ```js
  import { merge } from 'rxjs/operators';
  import { interval } from 'rxjs';
  
  /* 2개의 옵저버블 병합하여 인스턴스 메소드 사용 */
  // 2.5초마다 실행
  const first = interval(2500);
  // 1초마다 실행
  const second = interval(1000);
  // 인스턴스 메소드로 사용
  const example = first.pipe(merge(second));
  // output: 0,1,0,2....
  const subscribe = example.subscribe(val => console.log(val));
  ```

#### 23. withLatestFrom :star:

+ 기존 옵저버블을 다른 옵저버블과 결합하여 기존 옵저버블을 방출할 때만 각각의 최신 값에서 계산되는 옵저버블을 생성

  ```js
  import { withLatestFrom, map } from 'rxjs/operators';
  import { interval } from 'rxjs';
  
  /* 더 빠른 두 번째 소스의 최신 값 구하기 */
  // 5초마다 실행
  const source = interval(5000);
  // 1초마다 실행
  const secondSource = interval(1000);
  const example = source.pipe(
    withLatestFrom(secondSource),
    map(([first, second]) => {
      return `First Source (5s): ${first} Second Source (1s): ${second}`;
    })
  );
  /*
    "First Source (5s): 0 Second Source (1s): 4"
    "First Source (5s): 1 Second Source (1s): 9"
    "First Source (5s): 2 Second Source (1s): 14"
    ...
  */
  const subscribe = example.subscribe(val => console.log(val));
  ```

#### 24. debounce

+ 다른 옵저버블에 의해 결정된 특정 시간 범위가 다른 소스 방출 없이 경과한 경우만 소스 옵저버블에서 값을 방출

  ```js
  import { fromEvent, interval } from 'rxjs';
  import { debounce } from 'rxjs/operators';
  
  /* 여러 번 클릭 후 가장 최근 클릭만 내보내기 */
  const clicks = fromEvent(document, 'click');
  const result = clicks.pipe(debounce(() => interval(1000)));
  result.subscribe(x => console.log(x));
  ```

#### 25. debounceTime :star:

+ 특정 시간 범위가 지난 후에만 옵저버블에서 값을 방출

+ delay와 비슷하지만 가장 최근 값만 전달

  ```js
  import { fromEvent } from 'rxjs';
  import { debounceTime, map } from 'rxjs/operators';
  
  /* 입력 사이의 시간을 기반으로 디바운싱 */
  // 요소 참조
  const searchBox = document.getElementById('search');
  
  // streams
  const keyup$ = fromEvent(searchBox, 'keyup')
  
  // 현재 값을 방출하기 위해 keyup 사이에 0.5초를 기다림 
  keyup$.pipe(
    map((i: any) => i.currentTarget.value),
    debounceTime(500)
  )
  .subscribe(console.log);
  ```

#### 26. concatMap

+ 스트림의 순서를 지킨 채로 각 소스의 값을 옵저버블에 투영

+ 옵저버블은 출력 옵저버블에 병합되며, 다음 소스를 병합하기 전에 각 소스가 완료될 때까지 직렬화됨

+ merge는 처리량에 관심사, concat은 순서에 관심사

  ```js
  import { fromEvent, interval } from 'rxjs';
  import { concatMap, take } from 'rxjs/operators';
  
  const clicks = fromEvent(document, 'click');
  const result = clicks.pipe(
    concatMap(ev => interval(1000).pipe(take(4)))
  );
  result.subscribe(x => console.log(x));
  
  // Results in the following:
  // (results are not concurrent)
  // For every click on the "document" it will emit values 0 to 3 spaced
  // on a 1000ms interval
  // one click = 1000ms-> 0 -1000ms-> 1 -1000ms-> 2 -1000ms-> 3
  ```

#### 27. every

+ 완료되기 전 모든 값이 술어(조건)을 통과하면 true를 내보내고 그렇지 않으면 false

  ```js
  import { every } from 'rxjs/operators';
  import { of } from 'rxjs';
  
  /* 모든 수가 짝수인지 판별하기 */
  // 5개의 값을 내보냄
  const source = of(1, 2, 3, 4, 5);
  const example = source.pipe(
    // 모든 값이 짝수인지 판별
    every(val => val % 2 === 0)
  );
  // output: false
  const subscribe = example.subscribe(val => console.log(val));
  ```

#### 28. defaultEmpty

+ 완료 전에 아무것도 내보내지 않으면 주어진 기본 값을 내보냄

  ```js
  import { defaultIfEmpty } from 'rxjs/operators';
  import { of } from 'rxjs';
  
  /* 빈 값의 기본값 */
  // 모든 소스의 값이 비어있는 경우 'Observable.of() Empty!'를 내보냄
  const exampleOne = of().pipe(defaultIfEmpty('Observable.of() Empty!'));
  // output: 'Observable.of() Empty!'
  const subscribe = exampleOne.subscribe(val => console.log(val));
  ```

<br />

## 참고

+ rxjs-hooks: [https://github.com/LeetCode-OpenSource/rxjs-hooks](https://github.com/LeetCode-OpenSource/rxjs-hooks)

