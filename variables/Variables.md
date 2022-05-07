# Variables

- **Use less `var`**
    
    
    ```jsx
    var name = 'name1';
    var name = 'name2';
    var name = 'name3';
    ```
    
    ```jsx
    console.log(name); // undefined
    var name = '이름';
    ```
    
    `var`를 사용하면 동일한 변수 이름을 여러 번 선언이 가능하다. 재할당과 재선언(중복선언)이 매우 자유롭다.
    
    ```jsx
    // Syntax Error
    let name = 'name1';
    let name = 'name2';
    let name = 'name3';
    ```
    
    ```jsx
    // Syntax Error
    const name = 'name1';
    const name = 'name2';
    const name = 'name3';
    ```
    
    `let` 과 `const`는 동일한 이름으로 변수를 선언하려고하면 error를 낸다.
    
    ```jsx
    let name;
    name = 'name1';
    name = 'name2';  // OK
    ```
    
    ```jsx
    const name;  //Syntax Error
    const name = 'name1';
    name = 'name2';  //Syntax Error
    ```
    
    `let` 과 `const`의 차이는 재할당 가능의 여부이다.
    
- **Function scope & Block scope**
    
    ```jsx
    var global = 'global';
    
    if (global === 'global') {
    	var global = 'local';
      console.log(global);  // local
    }
    
    console.log(global);  // local
    ```
    
    `var`는 함수 단위 scpoe이다 `if` 문은 block 단위이므로 scope가 침범된다.
    
    ```jsx
    let global = 'global';
    
    {
    	let global = 'local';
      console.log(global);  // local
    }
    
    console.log(global);  // global
    ```
    
    ```jsx
    const global = 'global';
    
    {
    	const global = 'local';
      console.log(global);
    }
    
    console.log(global);
    ```
    
    `let`, `const`는 block 단위 scope이므로 scope가 침범되지 않는다.
    
    ```jsx
    const person = {
      name: 'lee',
      age: 23,
    }
    
    // Syntax Error
    person = {
      name: 'lee2',
      age: 24,
    }
    ```
    
    그래도 `let` 보다는 `const`를 쓰자. 재할당이 금지되어 있는데 왜??
    
    ```jsx
    const person = {
      name: 'lee',
      age: 23,
    }
    
    person.name = 'kim';
    person.age = 22;
    console.log(person)  // { name: 'kim', age: 22 }
    ```
    
    직접적인 재할당이 금지되어 있지만 그 안에 있는 property는 변경이 가능하다.
    
    ```jsx
    const person = [{
      name: 'lee',
      age: 23,
    }]
    
    person.push({
      name: 'lee2',
      age: 24,
    })
    
    console.log(person)  // [ { name: 'lee', age: 23 }, { name: 'lee2', age: 24 } ]
    ```
    
    리스트에서도 push를 통해서 값을 추가하면서 변경이 가능하다.
    
    이처럼 재할당이 금지 되어있을뿐, 객체나 배열같은 래퍼런스 객체들의 조작에는 이상이 없다.
    
- **Minimize Global**
    
    전역 공간(최상위) 사용을 지양하하는 말을 어디서 들어봤을까?
    
    > 1. 경험
    2. 누군가 혹은 자바스크립트 상태계
    3. 강의 혹은 책
    4. 회사 혹은 멘토
    5. Lint
    > 
    
    전역 공간은 두가지로 나뉜다.
    
    > 1. Window (브라우저 환경)
    2. Global (Node.js 환경)
    > 
    
    그럼 왜 전역 공간의 사용을 자제해야할까?
    
    ```jsx
    var globalVar = 'global';
    console.log(globalVar);
    console.log(window.globalVar);
    ```
    
    ```jsx
    console.log(window.globalVar);
    // 심지어 이것도 된다
    console.log(globalVar);
    ```
    
    index1.js 에서 선언한 globalVar가  index2.js에서도 접근이 된다...!
    
    ```jsx
    // 헤더
    
    var globalVar = 'global';
    console.log(globalVar);
    
    var setTimeout = 'setTimeout';
    function setTimeout() {}
    ```
    
    ```jsx
    // 바디
    
    // 아래 코드는 동작하지 않는다.
    setTimeout(() => {
      console.log('1초');
    },  1000);
    ```
    
    index2에서 아무런 기능을 하지 않는 코드를 작성하는 동안에는 오류가 발생하지 않는다.
    
    심지어 index1에서는 변수 이름으로 함수를 만드는데도 아무런 오류가 없다.
    
    ```jsx
    const array = [1, 2, 3];
    
    for (var index = 0; index < array.length; index++) {
      const element = array[index];
    }
    ```
    
    ```jsx
    console.log(window.index)  // 3
    ```
    
    index1.js에서 var로 선언된 index가 index2.js에서도 접근되어 3이라는 값을 얻어온다.
    
    전역 공간을 사용한다는 것은 어디서나 접근이 가능한 상황을 만들어 스코드 분리가 되지 않는 위험한 상황이 된다.
    
    이렇게 전역 공간을 더럽히는 일을 많이 줄이기 위해서 우리는 다음과 같은 것을 참고하면 좋다.
    
    > 1. IIFE, Module, Closure 사용
    2. 전역변수 사용 X → 지역변수 사용 O
    3. window, global 조작 X
    4. const, let
    > 
    
    따라서 전역변수를 사용하지 않고, 지역변수만 만드는 것이 중요하다.
    
    또한 window, global 을 조작해서 
    
- **Remove Temporary variable**
    
    ```jsx
    function getElements() {
      const result = {};  // 임시 객체
    
      result.title = document.querySelector('.title');
      result.text = document.querySelector('.text');
      result.value = document.querySelector('.value');
    
      return result;
    }
    ```
    
    위 코드에서 `const result`는 getElement 함수내에서 전역변수로서 사용이 된다.
    
    함수의 사이즈가 크다면 임시변수로 만들어 놓은 이 변수들도 위험해 진다.
    
    또한 만약 다른 사람이 result 안에 뭔가를 넣고, 접근 할 수 있다는 것이다.
    
    그렇다면 어떻게 이런 임시 변수들 사용하지 않고 좋은 방법으로 CRUD 할까?
    
    ```jsx
    function getElements() {
      return {
        title: document.querySelector('.title'),
        text: document.querySelector('.text'),
        value: document.querySelector('.value'),
      };
    }
    ```
    
    위와 같은 코드로 바꾸면 임시변수 자체를 삭제함으로서 CRUD의 유혹을 원천에 차단한다.
    
    또 다른 예시를 보자.
    
    ```jsx
    function getDateTime(targetDate) {
      let month = targetDate.getMonth();
      let day = targetDate.getDate();
      let hour = targetDate.getDay();
    
      month = month >= 10 ? month : '0' + month;
      day = day >= 10 ? day : '0' + day;
      hour = hour >= 10 ? hour : '0' + hour;
    
      return {
        month, day, hour
      };
    }
    ```
    
    이 함수를 수정해 추가적인 기능을 구현 한다고 할때, 꼼꼼하게 확인하지 못하는 경우 이 함수를 사용하는 여러가지 코드들에서 많은 일들이 일어 날 수 있다.
    
    ```jsx
    function getDateTime(targetDate) {
      const month = targetDate.getMonth();
      const day = targetDate.getDate();
      const hour = targetDate.getDay();
    
      return {
        month: month >= 10 ? month : '0' + month,
        day: day >= 10 ? day : '0' + day,
        hour: hour >= 10 ? hour : '0' + hour,
      };
    }
    ```
    
    먼저 let으로 선언된 변수르 const로 바꾸어서 재할당의 위험을 제거하고, 바로 반환을 하게 만들어서 CRUD를 최소하는 방법으로 코드를 작성한다. 이렇게 함수를 추상화 시키면 다음과 같은 코드가 가능하다.
    
    ```jsx
    function getDateTime(targetDate) { ... }
    
    function getDateTime() {
      const currentDateTime = getDateTime(new Date());
    
      return {
        month: currentDateTime.month + "월",
        day: currentDateTime.day + "분",
        hour: currentDateTime.hour + "시",
      }
    }
    ```
    
    함수를 추상화 시켰기 때문에 새로운 가능을 부담없이 추가 할 수 있게 된다.
    
    극단적인 예시를 보자면 다음과 같은 코드에서 dot을 추가하는 경우이다.
    
    ```jsx
    function getRandomNumber(min, max) {
      const randomNumber = Math.floor(Math.random() * (max + 1) + min);
    	
    	randomNumber.something...  // 끔찍하다.
      
    	return randomNumber;
    }
    ```
    
    이것도 위와 마찬가지로 바로 return 해주는 방법으로 해결이 가능하다.
    
    ```jsx
    function getRandomNumber(min, max) {
      return Math.floor(Math.random() * (max + 1) + min);
    }
    ```
    
    정리하자면 임시변수의 제거는 명령형으로 가득한 로직을 줄이고 디버깅의 용이함을 얻기 위함이다.
    
    또한 이 임시변수를 이용해서 추가적인 코드를 작성하는 유혹을 제거해 유지보수를 쉽게 할 수 있다.
    
    해결책은 다음과 같다.
    
    > 1. 함수 나누기
    2. 간단한건 바로 반환
    3. 고차함수(map, filter, reduce)
    4. 선언형 코드로 작성하기
    > 
- **Be careful of hoisting**
    
    먼저 호이스팅이 무엇인가? 
    
    간단하게 말하면 런타임시에 선언이 최상단으로 끌어올려지는 것이다. 
    
    따라서 예상한 스코프가 런타임에 우리의 예상대로 작동하지 않을 수 있다.
    
    이런 현상을 우리는 호이스팅이라고 하는데 `var`로 선언한 변수가 초기화가 제대로 되어 있지 않았을 때 `undifined`로 최상단으로 끌어올려 줄 수 있는 것을 뜻한다. 이런 경우 선언부만 최상단으로 끌어 올려준다.
    
    `let`이나 `const`를 쓰면 이런일이 잘 나타나지 않겠지만, `var`로 선언한 경우 이런 경우가 많다. 예시를 보자.
    
    ```jsx
    var global = 0;
    
    function outer() {
      console.log(global);  // undefined <- 호이스팅이 동작한 사례
      var global = 5;
      
      function inner() {
        var global = 10;
        console.log(global);  // 10
      }
    
      inner();
    
      global = 1;
    
      console.log(global);  // 1
    }
    
    outer();
    ```
    
    ```jsx
    function duplicatedVar() {
      var a;
      console.log(a);  // undefined
    
      var a = 100;
      console.log(a);  // 100
    }
    
    duplicatedVar();
    ```
    
    이렇게 원하는 변수가 제대로 원하는대로 불려지지 않을 수 있다.
    
    이런 현상은 변수뿐 아니라 함수에서도 나타난다.
    
    ```jsx
    var sum;
    
    sum = function () {
      return 1 + 2;
    };
    
    console.log(sum());  // 3
    ```
    
    ```jsx
    var sum;
    
    function sum() {
      return 1 + 2;
    }
    
    console.log(sum());  // 3
    ```
    
    ```jsx
    var sum;
    
    console.log(sum());  // 3  <- 함수도 호이스팅 된다.
    
    function sum() {
      return 1 + 2;
    }
    ```
    
    위의 예시들을 보면 함수도 호이스팅 되는것을 알 수 있다. 뭐가가 이상하게 동작한다...
    
    ```jsx
    var sum;
    
    console.log(sum());  // 10 <- 가장 마지막 sum이 불린다.
    
    function sum() {
      return 1 + 2;
    }
    
    function sum() {
      return 1 + 2 + 3;
    }
    
    function sum() {
      return 1 + 2 + 3 + 4;
    }
    ```
    
    마지막 sum 함수가 불리는 것으로 봐서, 모든 선언이 다 끌어올려지는것을 볼 수 있다.
    
    따라서 함수를 만들때 `const`를 이용해서 만드는 것이 좋다. 예시를 보자.
    
    ```jsx
    console.log(sum())  // Cannot access 'sum' before initialization
    
    const sum = function() {
      return 1 + 2;
    };
    ```
    
    이렇게 `const` 변수에 익명 함수를 할당해서 사용하면 호이스팅을 막을 수 있다
    
    (정확히는 선언은 되는데 값이 없어서 오류가 난다.)
    
    ```jsx
    console.log(sum())  // Cannot access 'sum' before initialization
    
    let sum = function() {
      return 1 + 2;
    };
    ```
    
    `let`을 사용해도 동일한 결과를 얻을 수 있다.