# Async Await

### 우선 Sync 란?
1. ``Sync Flow`` 는 메모리에 적재된 명령이 순차적으로 실행된다.
2. 한번 시작하면 관여할 수가 없다. 그래서 처음에 ``Sync Flow Control``을 통해 다른 위치의 명령을 실행할 수 있도록 미리 짜놓는다.
3. ``Sub Flow``는 별도의 명령셋이다. ``Sync Flow`` 안에서 여러번 재활용해서 실행할 수 있다.
  
  
### Blocking 개념

1. ``Sync Flow``가 실행되는 동안 cpu가 다른 일을 할 수 없는 현상이다.
2. 한 명령을 적재하고 실행하는 사이에 아무것도 할 수 없고 명령 실행만 한다. 멀티태스킹이 안된다.
3. ``Blocking``은 불가피하다. 줄이는 방법은 ?
    * ``Sync Flow`` 를 짧게 짠다. (바람직하지만 불가능해)
    * 다른 쓰레드에 ``Sync Flow`` 떠넘긴다. 쓰레드 하나당 ``Blocking`` 시간이 줄어든다. 
    그런데 쓰레드는 별도로 작동하기 때문에 각각 끝나는 시점을 알 수가 없다. (동시성 문제....는 다음 시간에...)


### Non Blocking 개념

1. ``Sync Flow`` 가 납득할 만한 시간 내에 종료되는 것이다.
2. 충분히 짧은 시간에 ..!? 
3. Front-end 기준으로 ``Sync Flow`` 로직이 실행될 때 16m/s 안에 종료된다고 하는 것을 일반적으로 ``Non Blocking``이라고 한다.


### Sync & Async

1. Sync & Async 는 **값을 어떻게 반환하는가**의 차이일 뿐이다. 
2. ``Sync``는 즉시 값을 반환한다.
3. ``Async``는 ``Sub Flow``가 **다른 수단(Promise, Callback Func, Iterations 등)으로 값을 반환한다.**
4. Blocking & Non Blocking 과 관련없다. (두 개념은 조합해서 다양한 인터페이스를 만들 수 있음)


### Async의 단점

1. 호출 결과가 즉시 반환되지 않아서 현재의 ``Sync Flow``는 바로 종료되버린다.
2. ``그 Sync Flow``에 선언되어 있던 지역변수, 인자 등의 그 당시의 상태를 결과 시점에 사용할 수 없다.
3. Async 요청 시의 상태를 별도로 결과 시점에 전달해야한다. 


### Sync의 장점 + Async의 장점

1. Sync 처럼 보이지만 내부는 Async로 작동하게끔 짠다.
2. 그러면 Sync Flow가 어긋나므로 이전 상태를 기억해 이어줄 장치가 필요하다. 
    * Continuation 
    * Continuation Passing Style - Generator, Async, Asynchronous Iterators


### Async & Promise

1. async 함수의 반환값은 promise 이다. async는 promise 생성기이다. 

```javascript
 const promise1 = new Promise(res => res(func1()));
 const promise2 = (async () => func2())();

 promise1.then(console.log);
 promise2.then(console.log);
```


### Async / Await

1. await를 promise 반환하는 함수 앞에 두면 promise.then()을 기다렸다가 진행한다.

```javascript
  (() => {
    timeout(func1, 500).then(v => console.log(v));
  })();
  
  (async () => {
    console.log(await timeout(func1, 500));
  })();
```
  > Javascript ES6는 명령문 하나하나를 객체 Record로 감싼다. <br/>
  > 연속되어 있는 Record를 계속 진행할지, 특정 명령을 담은 Record에서 잠깐 멈췄다가 다시 시작할지 결정할 수 있다.

2. 하나의 await는 하나의 직렬이다. 병렬 수행하려면 Promise.all 또는 race 처럼 함수에 추상화할 수 밖에 없다. Promise로 병렬성을 확보한다.
3. Promise
    * all(*iterable*) : 모든 명령을 기다렸다가 한번에 반환
    * race(*iterable*) : 먼저 완료된 명령을 반환 (주의할 것은 남아있는 명령들을 다 신경써야한다는 것)
    






[참고](https://youtu.be/H_Hb9IF7sfc)
