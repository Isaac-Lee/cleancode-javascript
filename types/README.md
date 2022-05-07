# Types

- **Type Check**
    
    일단 자바스크립트는 동적으로 타입을 할당하는 언어이다. 따라서 타입검사가 까다롭다.
    
    흔히 사용되는 `typeof` 로 모든 타입의 검사가 가능하지 않다. (무적이 아니다...!)
    
    Primtive value에 대해서는 검사 가능하지만  Reference value에 대해서는 원하는대로 나오지 않을 수 있다.
    
    다음은 Reference 객체를 typeof로 검사하는 예시이다.
    
    ```jsx
    function myFunction() {}
    class MyClass {}
    
    console.log(typeof myFunction)  // function
    console.log(typeof MyClass)  // function
    console.log(typeof str)  // object
    console.log(typeof null)  // object <- 언어적인 오류
    ```
    
    Reference 검사에 또 다른 어려운 점은 동적으로 변화하는 타입이다.
    
    여기서 instanceof 라는 함수가 있다는 것을 알고가면 좋다.
    
    ```jsx
    function Person(name, age) {
      this.name = name;
      this.age = age;
    }
    
    const isaac = new Person('isaac', 23)
    const i = { name: 'isaac', age: 23 }
    
    console.log(isaac instanceof Person)  // true
    console.log(i instanceof Person)  // false
    ```
    
    조금 더 정확하게 검사하는 방법으로는 `Object.prototype.toString.call()` 이 있다.
    
    ```jsx
    const arr = [];
    const func = () => {};
    const date = new Date();
    
    Object.prototype.toString.call(arr)  // [object Array]
    Object.prototype.toString.call(func)  // [object Function]
    Object.prototype.toString.call(date)  // [object Date]
    ```
    
    좋은 타입검사 방법은  javascript is ____ 로 원하는 타입을 어떻게 검사하는지를 하나하나 찾는 것이 좋다.
    
    스택오버플로우에서 upvote가 가장 많은 것을 고르되, 의견이 언제 제시되었는지도 확인해보는 것이 중요하다.
    
- `**undefined` & `null`**
    
    `null`의 특징은 다음과 같다
    
    ```jsx
    !null  // true
    !!null  // false
    
    null === false  // false
    !null === true  // true
    
    null + 123  // 123
    ```
    
    `undefined`의 특징은 다음과 같다. 
    
    ```jsx
    // 선언은 했지만 값은 정의(할당)되지 않음
    let varb;
    typeof varb;  // undefined
    
    undefined + 10  // NaN
    
    !undefined  // true
    undefined === null  // false
    !undefined === !null  // true
    ```
    
    undefined, null은 값이 (명시적으로)없거나 정의되지 않은 것들이다.
    
    `undefined` → NaN → type undefined
    
    `null` → $\emptyset$ → type object
    
    자세한 내용은 ECMA script reference를 보면 더 명확해질 것 같다. 결국 정의된 sementics에 따라서 동작되는 것이기 때문이다. (개발자가 language reference를 봐야하는 이유..!)
    
- **Reduce `eqeq`**
    
    일반 동등 연산자(`==`)를 이용하면 type casting이 일어난다. 그래서 다음과 같은 현상이 발생한다.
    
    ```jsx
    '1' == 1  // true
    1 == true  // true
    ```
    
    그래서 우리는 strict eqeq(`===`)를 써야한다.
    
    ```jsx
    '1' === 1  // false
    1 === true  // false
    ```
    
    하지만 이런 eqeq의 허점을 역이용해서 type casting을 바로 이용하는 사람도 있을 것이다. 이런 시도는 좋지 않다. 팀원들에게 어떠한 여지를 주어서도 안된다. 즉, eqeq의 사용은 되도록 피해야한다.
    
    ```jsx
    ticketNum.value == 0  // 이런 코드는 되도록 쓰지말자
    
    Number(ticketNum.value) == 0  // 완전하게 형변환 후에 사용하자
    
    ticketNum.valueAsNumber == 0  // 또는 이렇게 하면 좋다
    ```
    
    ESLint 에서는 `===`와 `!==` 를 요구할만큼 이는 중요한 문제이다.
    
- **Aware of Type Casting**
    
    ```jsx
    11 + 'concat with string'  // '11 concat with string'
    
    !!'string'  // true
    !!''        // false
    
    parseInt('9.9999', 10)  // 9 (뒤에 10 안쓰면 10진수로 안바뀔 수 있음)
    ```
    
    아래 `parseInt`와 위 세줄의 차이는 무엇일까? 바로 형변환을 명시적으로 해주었느냐의 차이이다.
    
    따라서 위의 세줄은 아래와 같이 고쳐서 사용하도록 하자.
    
    ```jsx
    String(11 + ' concat with string')
    
    Boolean('string')
    Boolean('')
    
    Number('11')  // 숫자 변환도 이렇게..!
    ```
    
    JS는 암묵적으로 타입이 변환할 수 있다. 따라서 사람이 헷갈리지 않도록 명시적으로 변환하는 습관을 들이자.
    
    (물론 ECMA 스크립트를 다 알아서 암묵적인 변환을 다 알면 좋다.)
    
- `**isNaN**`
    
    ```jsx
    Number.MAX_SAFE_INTEGER  // 9007199254740991 <- 가장 큰 정수
    Number.isInteger  // 숫자인지 검사하는 함수
    ```
    
    이런 기능들이 제공되는 이유는 JS에는 이와 같이 수 많은 숫자가 제공되기 때문이다. 다른 말로 숫자를 판별하기가 어렵다는 것이다..
    
    typeof 말고 숫자인지 검사하는 함수로는 isNaN이 있다. 두가지 방법이 있는데 확인해보자.
    
    ```jsx
    isNaN(123 + 'test')         // true   <- 느슨한 검사
    Number.isNaN(123 + 'test')  // false  <- 엄격한 검사
    ```
    
    언뜻보면 이름이 같아서 구현체가 같다고 생각할 수 있지만, Number를 한번 더 붙여준 것이 더 엄격한 검사를 한다. 엄격한 검사는 eqeqeq와 마찬가지로 아주 중요하기 때문에 Number를 사용해서 isNaN을 사용하자.