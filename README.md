# -JavaScript Questions 154-

@URL :[GitHub - lydiahallie/javascript-questions: A long list of (advanced) JavaScript questions, and their explanations](https://github.com/lydiahallie/javascript-questions)

하루에 한 문제씩 자바스크립트의 중요 개념을 이해하며 문제를 풀고 설명을 작성하고 있습니다. 

**1. What's the output?**

```jsx
function sayHi() {
  console.log(name);
  console.log(age);
  var name = 'Lydia';
  let age = 21;
}

sayHi();
```

- A: `Lydia` and `undefined`
- B: `Lydia` and `ReferenceError`
- C: `ReferenceError` and `21`
- D: `undefined` and `ReferenceError`


<details markdown="1">
<summary>Answer</summary>
D

이 문제는 **변수의 호이스팅을 물어보고 있다.** 자바스크립트의 모든 선언문은 호이스팅을 통해 자신의 스코프의 최상단으로 이동해 런타임 전 가장 먼저 선언된다. var의 경우에는 선언과 동시에undefined로 초기화가 이루어져 출력이 가능하지만 let으로 선언된 변수의 경우 호이스팅을 통해 선언은 되어도 자신의 선언문에 도달했을 때 비로소 초기화가 이루어진다. 그 전까지는 변수에 접근하지 못하게 된다. 이때 선언되고 선언문에 도달하기전까지 접근할 수 없는 이 사각지대를 일시적 사각지대(TDZ)라고 한다. 
</details>



**2. What's the output?**

```jsx
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1);
}

for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1);
}
```

- A: `0 1 2` and `0 1 2`
- B: `0 1 2` and `3 3 3`
- C: `3 3 3` and `0 1 2`



<details markdown="1">
<summary>Answer</summary>
C

이 문제는 **비동기로 작동하게 되는 콜백함수와 클로저에 대한 이해**와 더불어 **블록레벨 스코프, 함수레벨 스코프**에 대한 지식을 필요로 한다. 

 우선 두 반복문 모두 0.001초라는 짧은 시간을 지정해도 for문의 속도가 더 빠르기 때문에 for문이 작동하고 setTimeout함수가 뒤이어 3번을(0,1,2)실행하게 된다. (선언은 코드가 실행되며 for과 같이 되지만 실제로 동작하는 속도가 느린 것 뿐이다.)

 이때 첫번째 반복문의 경우 var로 선언한 변수 i는 함수레벨 스코프이기 때문에 바깥 전역에 변수를 선언하게 되고 반복문 실행 후 결과 3으로 남아있는 상태이다. 세번의 반복동안 생성된 화살표함수내 i는 각자 클로저를 형성하여 본인이 선언되는 시점의 i를 가리키는데 그것은 결국 전역 변수로 선언된 같은 i를 가리키는 것이다. 즉 세 화살표함수 모두 반복문을 마친 3값을 가르키게 되고 실행이 작동할 때는 이미 반복문을 모두 마친 3을 가진 i를 출력하게 된다. 

두번째의 경우 let은 블록레벨스코프로 반복문이 돌때마다 새로운 let을 생성하게 된다. 즉 화살표 함수들은 각자 자신의 선언시점의 for블록안에 선언된 각각의 변수 i를 가리키게 되어 0,1,2를 출력할 수 있게 되는 것이다. 

참조:[https://www.youtube.com/watch?v=RZ3gXcI1MZY](https://www.youtube.com/watch?v=RZ3gXcI1MZY)
</details>




**3. What's the output?**

```jsx
const shape = {
  radius: 10,
  diameter() {
    return this.radius * 2;
  },
  perimeter: () => 2 * Math.PI * this.radius,
};

console.log(shape.diameter());
console.log(shape.perimeter());
```

  

- A: `20` and `62.83185307179586`
- B: `20` and `NaN`
- C: `20` and `63`
- D: `NaN` and `63`



<details markdown="1">
<summary>Answer</summary>
B


이 **문제는 화살표함수에는 this가 없다는 걸 알아야 이해할 수 있다.** this는 실행 컨텍스트가 생성될 때 함께 바인딩 됩니다. 즉 함수를 호출할 때 결정이 된다는 것인데 this는 기본적으로는 전역객체를(브라우저: window, node.js: global)을 가리킵니다. 객체내 메소드의 경우 호출시 객체에 의해 호출되고 this는 객체 shape을 가리키게 됩니다. 즉 this.radius가 10이 되는 것입니다. 하지만 화살표 함수는 this가 존재하지 않고 결국 this를 찾기위해 자신의 스코프체인의 상위로 올라가게 됩니다. 결국 자신의 상위 스코프인 전역으로 가서 this를 찾게 되고 그 this가 의미하는 것은 전역객체이기 때문에 존재하지 않는 radius를 가리키게 됩니다. 따라서 2 * undefined는 NaN이 되는 것입니다. 

-참조:[https://blog.naver.com/websearch/222122306928](https://blog.naver.com/websearch/222122306928)(자바스크립트Math객체 예제)


</details>


**4. What's the output?**

```jsx
+true;
!'Lydia';
```

- A: `1` and `false`
- B: `false` and `NaN`
- C: `false` and `false`


<details markdown="1">
<summary>Answer</summary>
A

문제는 **암묵적 타입변환을 물어보고 있다.** 자바스크립트는 암묵적으로 데이터 타입을 변환하는 경우가 있는데 true의 경우 + 단항 연산자를 만나면서 number타입으로 변환되고 1을 출력합니다.

(true는 1, false는 0을 의미)

두번째의 경우 문자열의 값에 부정을 의미하는 논리연산자 !를 사용하여 문자열의 데이터타입은 암묵적으로 boolen타입이 되고 “”빈값이 아닌 'Lydia'는 truthy를 의미하여 그의 부정이 false가 출력됩니다.
</details>


**5. Which one is true?**

```jsx
const bird = {
  size: 'small',
};

const mouse = {
  name: 'Mickey',
  small: true,
};
```

- A: `mouse.bird.size` is not valid
- B: `mouse[bird.size]` is not valid
- C: `mouse[bird["size"]]` is not valid
- D: All of them are valid


<details markdown="1">
<summary>Answer</summary>
A

이 문제는 **점 표기법과 대괄호 표기법의 차이를 이해해야 한다.** 점 표기법의 경우 그 뒤에 오는 것을 프로퍼티 키로 인식하고 찾게 된다. 즉 1번의 경우 mouse라는 객체안에 bird라는 키에 그 안에 또 size라는 프로퍼티 키를 가진 객체가 있는지를 물어보게되고 이는 존재하지 않기 때문에 유효하지 않다. (밑에 그림을 물어보는 것이라 할 수 있다.)

const mouse = {
name: 'Mickey',
small: true,
bird:{size : undefined}
};

하지만 대괄호 표기법은 []안에서 “”문자열이라는 것을 표시해주지 않으면 객체의 key를 찾는 것이 아니라 식별자로 선언된 것을 찾게 된다. 따라서 B,C의 경우에는 mouse자신 객체 밖에 있는 변수 bird로부터 값을 찾아온 것이다.

- 참고로 대괄호 표기법을 사용할 시에는 키값을 찾을 때 무조건 “”표시를 해줘야 하며 키값이 숫자라면 “”를 생략할 수 있다. 또 어떤 값을 할당하든 프로퍼티 키로 될 때는 암묵적으로 모두 문자열로 타입변환이 이루어진다. 즉 프로퍼티 키는 모두 문자타입이라고 할 수 있다.(Symbol제외)

- 참고:프로퍼티 키가 식별자 네이밍 규칙을 지키지 않는다면 키에 필수적으로 “”를 넣어준다.

예시) 

const mouse = {
  name: 'Mickey',
  “small-bird”: true,
};


</details>




**6. What's the output?**

```jsx
let c = { greeting: 'Hey!' };
let d;

d = c;
c.greeting = 'Hello';
console.log(d.greeting);
```

- A: `Hello`
- B: `Hey!`
- C: `undefined`
- D: `ReferenceError`
- E: `TypeError`

<details markdown="1">
<summary>Answer</summary>
A

이번 문제는 **값의 데이터 타입을 이해해야 한다.** 그중에서도 값이 전달(복사)될 때 값에 의한 전달이 이루어지는 원시타입의 값과 참조에 의한 전달이 이루어지는 객체타입의 값에 대한 이해를 해야한다. 자바스크립트의 객체 타입의 값은 변경가능한 값으로 이루어져있다.

- **여기서 변경 가능하다는 것은 객체 내부의 원시타입의 값에 변화를 말하는 것이지 객체 값 그 자체를 의미하는 것은 아니다.**

즉, 원시타입의 경우 메모리공간의 값이 있는 주소를 그대로 복사하여 넘겨주지만 객체타입의 값의 경우 복사하여 넘겨주는 주소가 값이 들어있는 주소가 아닌 메모리공간 어딘가에 그 객체가 존재하는 곳의 주소를 복사해서 넘겨준다. **쉽게 말해 객체의 주소를 복사해 주는 것이다.** 따라서 객체 타입의 값을 복사하게 되면 **결국 같은 주소를 참조하게 되고 하나의 객체를 가리키는 형태이다.**

![image](https://user-images.githubusercontent.com/98815511/161744226-961a32d3-8da4-46f8-a2e4-996381537d47.png)

</details>



**7. What's the output?**

```jsx
let a = 3;
let b = new Number(3);
let c = 3;

console.log(a == b);
console.log(a === b);
console.log(b === c);
```

- A: `true` `false` `true`
- B: `false` `false` `true`
- C: `true` `false` `false`
- D: `false` `true` `true`
- 
<details markdown="1">
<summary>Answer</summary>
C

이 문제는 **일치연산자(===)와 동등연산자(==)의 개념을 이해해야 한다.** 동등연산자(==)의 경우 비교하는 두 피연산자의 값만 비교하고 타입을 비교하진 않는다. 즉 3==”3” 은 true가 나오는 형태로 암묵적으로 같은 타입으로 만들어 비교하게 된다. 

일치연사자(===)의 경우는 값과 타입을 모두 비교하여 타입도 같아야만 true로 인정하게 된다.

//참조

//NaN === NaN  > false 

//Nan은 자신과 일치하지 않는 유일한 값이다. 따라서 숫자값이 NaN인지 조사하려면

//isNaN(NaN) 빌트인함수 isNaN을 사용하거나 object.is(NaN,NaN) 메서드를 사용하는 것이 좋다.

//object.is(+0,-0), object.is(NaN,NaN)

</details>



**8. What's the output?**

```
class Chameleon {
  static colorChange(newColor) {
    this.newColor = newColor;
    return this.newColor;
  }

  constructor({ newColor = 'green' } = {}) {
    this.newColor = newColor;
  }
}

//{ newColor = 'green' } = {} 에서 = {} 이렇게 추가하면 인수로 아무것도 안넘겨주어도 된다는 의미이다.
//{ newColor = 'green' }의 경우 그냥 매개변수에 기본값(초기값)을 넣어준 것이며 그것을 객체로 표현했다고 보면 된다. ex)constructor(a=3) 이런식으로 인수 준 느낌
const freddie = new Chameleon({ newColor: 'purple' });
console.log(freddie.colorChange('orange'));
```

- A: `orange`
- B: `purple`
- C: `green`
- D: `TypeError`

<details markdown="1">
<summary>Answer</summary>
D


이번 문제는 **클래스 내 정적메소드인 static의 의미를 알아야 한다.** 자바스크립트의 클래스 문법에서 클래스 블록안에서 static으로 정의된 함수는 정적메소드로 입력된다. 즉 클래스 Chameleon이라는 생성자 함수(클래스는 곧 생성자 함수와 같다)자체의 메소드이다. static을 붙이지 않고 메소드 축약표현으로 작성되는 함수는 Chameleon클래스의 프로토타입 메소드가 되고 constructor 함수는 생성자로서 생성하게 되는 인스턴스의 프로퍼티가 입력된다.

정적메소드는 클래스 자체의 프로퍼티 메소드이기 때문에 인스턴스는 상속받아 사용할 수 없다. 

인스턴스가 상속받는 메소드 및 프로퍼티는 오직 프로토타입 체인에 연결이 된 것만 가능하다.

정적메소드를 사용하기 위해서는 인스턴스를 생성하지 않고 바로 Chameleon.colorChange() 하면된다.

- 기본 매개변수 참조:[https://starkying.tistory.com/entry/Javascript-기본-매개변수Default-Function-Parameters](https://starkying.tistory.com/entry/Javascript-%EA%B8%B0%EB%B3%B8-%EB%A7%A4%EA%B0%9C%EB%B3%80%EC%88%98Default-Function-Parameters)

//위에 constructor에 사용된 인수는 기본 매개변수의 새로운 방법으로 배열도 저런식으로 사용 가능하며 위에 문제의 경우 인수를 객체 형태로 주어야 한다. { newColor: 'purple' } 하지만 ={}이 사용된다면 그냥 () 인수없이도 호출할 수 있다.(={}없는데 인수없이 호출하면 에러가 뜨게된다.)
</details>



**9. What's the output?**

```
let greeting;
greetign = {}; // Typo!
console.log(greetign);
```

- A: `{}`
- B: `ReferenceError: greetign is not defined`
- C: `undefined`


<details markdown="1">
<summary>Answer</summary>
A


자세히보면 let으로 선언한 변수와 값을 할당한 변수가 다르다. 즉 greetign으로 잘못 입력한 변수는

let을 통해서 선언된 변수가 아니라 자바스크립트가 **암묵적으로 전역에 생성한 변수**가 된다.

이런걸 **암묵적 전역**이라고 한다. 

즉 변수 window.greetign의 형태가 된다.

</details>


**10. What happens when we do this?**

```
function bark() {
  console.log('Woof!');
}

bark.animal = 'dog';
```

- A: Nothing, this is totally fine!
- B: `SyntaxError`. You cannot add properties to a function this way.
- C: `"Woof"` gets logged.
- D: `ReferenceError`

<details markdown="1">
<summary>Answer</summary>
A


**자바스크립트의 함수는 특별한 형식의 객체이다!** 즉 프로퍼티를 가질수 있다는 말이다.

함수 내용의 추가가 아니라 함수 객체로서의 프로퍼티로 animal:”dog”가 추가된다.

console.log(bark)

결과: `{ [Function: bark] animal: 'dog' }`
</details>


**11. What's the output?**

```
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

const member = new Person('Lydia', 'Hallie');
Person.getFullName = function() {
  return `${this.firstName} ${this.lastName}`;
};

console.log(member.getFullName());
```

- A: `TypeError`
- B: `SyntaxError`
- C: `Lydia Hallie`
- D: `undefined` `undefined`



<details markdown="1">
<summary>Answer</summary>
A

**이 문제는 정적메소드와 프로토타입 메소드**의 개념을 알아야한다. 위에 문제에서 Person.getFullname은 함수 자체 메소드로 getFullname을 등록한 것이다(정적메소드). 즉 프로토타입 체인이 아니라 그냥 함수에 프로퍼티를 추가한게 된다. 따라서 생성된 인스턴스로 접근하는 것이 아니라 함수자체로 호출해야 나온다. 만약 생성된 인스턴스 객체로 호출하고 싶다면 getFullname함수를 person.prototype.getFullname으로 호출하면 가능하다. 
</details>


**12. What's the output?**

```
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

const lydia = new Person('Lydia', 'Hallie');
const sarah = Person('Sarah', 'Smith');

console.log(lydia);
console.log(sarah);
```

- A: `Person {firstName: "Lydia", lastName: "Hallie"}` and `undefined`
- B: `Person {firstName: "Lydia", lastName: "Hallie"}` and `Person {firstName: "Sarah", lastName: "Smith"}`
- C: `Person {firstName: "Lydia", lastName: "Hallie"}` and `{}`
- D: `Person {firstName: "Lydia", lastName: "Hallie"}` and `ReferenceError`

<details markdown="1">
<summary>Answer</summary>
A

이번 문제는 **생성자 함수를 new연산자로 호출했을때와 일반 함수처럼 호출 했을 때**의 차이를 물어보고 있다. 생성자 함수는 특별할 것이 없다. 단지 생성되는 인스턴스와 바인딩되는 this를 식별자(미래의 프로퍼티 키) 앞에 붙여준 것 뿐이다. new 연산자와 함께 호출한다면 새로운 인스턴스 객체를 생성하고 인자로 입력해준  'Lydia', 'Hallie'가 각각 lydia객체의 프로퍼티 값으로 할당이 된다. 하지만 일반함수처럼 호출했을 때는 그냥 평범한 함수처럼 동작하게 된다. 즉 this.firstName, this.lastName
객체 window에 프로퍼티로 등록이 되게 된다. 결국 함수 결과값(return)은 없으므로 undefined가 암묵적으로 반환된다.


</details>


### **13. What are the three phases of event propagation?(이벤트 개념정리 문제)**

- A: Target > Capturing > Bubbling
- B: Bubbling > Target > Capturing
- C: Target > Bubbling > Capturing
- D: Capturing > Target > Bubbling


<details markdown="1">
<summary>Answer</summary>
D

표준 [DOM 이벤트](http://www.w3.org/TR/DOM-Level-3-Events/)에서 정의한 이벤트 흐름엔 3가지 단계가 있습니다.

1. 캡처링 단계 – 이벤트가 하위 요소로 전파되는 단계
2. 타깃 단계 – 이벤트가 실제 타깃 요소에 전달되는 단계
3. 버블링 단계 – 이벤트가 상위 요소로 전파되는 단계

## **Bubbling**:

한 요소에 이벤트가 발생하면, 이 요소에 할당된 핸들러가 동작하고, 이어서 부모 요소의 핸들러가 동작합니다. 가장 최상단의 조상 요소를 만날 때까지 이 과정이 반복되면서 요소 각각에 할당된 핸들러가 동작합니다. 마치 거품이 올라가는 것 같다 해서 버블링이 라고 부른다.
대부분의 이벤트는 거의 기본으로 버블링 되지만 focus이벤트와 같이 안되는 이벤트도 있습니다.

한마디로 정리-**하위에서 상위 요소로의 이벤트 전파방식을 이벤트 버블링이라 한다.**

ex)

<form onclick="alert('form')">FORM     //form창 1개가 나옴
<div onclick="alert('div')">DIV               //div나오고 form창 2개가 나옴
<p onclick="alert('p')">P</p>                //p나오고 div나오고 form창 3개가 나옴
</div>
</form>

![image](https://user-images.githubusercontent.com/98815511/162578949-d28f155f-76e6-4751-82ba-8ec603d093d4.png)

@ 만약 bubbling을 중단하고 싶다면 중단을 원하는 요소의 onclick에 이벤트 객체의 메소드인 **stopPropagation()**메소드를 사용한다.

<body onclick="alert(`버블링은 여기까지 도달하지 못합니다.`)">
<button onclick="event.stopPropagation()">클릭해 주세요.</button>
</body>

@ 추가로 한 요소에 여러 핸들러가 있다면 **event.stopImmediatePropagation()**을 통해 가능합니다.이 메서드를 사용하면 해당 포함되있는 해당 함수만 작동하고 요소에 할당된 다른 이벤트의 처리는  모두 동작하지 않습니다.
**버블링도 막고 해당 요소의 다른 핸들러도 모두 멈추게 합니다.(본인을 포함한 함수는 작동함)
@** 2개 모두 Capturing에도 적용됨.

## **Capturing**:

이벤트 버블링과 반대 방향으로 진행되는 이벤트 전파 방식

`on<event>` 프로퍼티나 HTML 속성, `addEventListener(event, handler)`를 이용해 할당된 핸들러는 캡처링에 대해 **전혀 알 수 없습니다.** 이 핸들러들은 타깃 단계와 버블링 단계만 동작합니다.
만약 캡처링 단계에서 이벤트를 잡아내려면 `addEventListener`의 `capture` 옵션을 `true`로 설정해야 합니다.

```
div.addEventListener("click", logEvent, {
    capture: true // default 값은 false입니다.
  });
```

이렇게 addEventListener인수로 true 혹은 capture:true를 입력해준다.

‣

- `event.target` – 이벤트가 발생한 가장 안쪽의 요소(정확히 어디서 이벤트가 발생했는지)
- `event.currentTarget` (=`this`) – 이벤트를 핸들링 하는 현재 요소 (핸들러가 실제 할당된 요소)
- `event.eventPhase` – 현재 이벤트 흐름 단계(캡처링=1, 타깃=2, 버블링=3)

## **@ event.currentTarget vs event.target**

만약 부모에만 이벤트가 있고 자식에게 없는 상황일때,

**currentTarget은 이벤트 핸들러가 부착된 부모를** 가리킨다!

즉, target은 부모로부터 이벤트가 위임되어 발생하는 자식의 위치, 내가 클릭한 자식 요소를 반환한다. 하지만 currentTarget은 이벤트가 부착된 부모의 위치를 반환한다.


![image](https://user-images.githubusercontent.com/98815511/162578929-3ed9da2e-8097-47fa-8475-072cd48394e9.png)

event.target은 자식 요소인 span을 리턴하고, event.currentTarget은 부모 요소인 button을 반환하게 된다.

전체내용 참조: [https://ko.javascript.info/bubbling-and-capturing](https://ko.javascript.info/bubbling-and-capturing)

</details>




**14. All object have prototypes.**

- A: true
- B: false


<details markdown="1">
<summary>Answer</summary>
B


**자바스크립트의 prototype 객체는 정확히는 함수의 프로퍼티 입니다.**  더 정확히 한다면 caller와 constructor내부슬롯을 가지고 생성되는 함수들에게 암묵적으로 생성되는 프로퍼티 입니다.
****모든 객체들은 __**proto**__ 접근자 프로퍼티를 가지고 있고 그것을 통해 자신의 상위 객체의 프로토타입 객체에 접근할 수 있습니다. 이것을 프로토타입 체인이라고 하며 객체의 최상위에는 결국 object.prototype객체가 존재하고 있습니다.

@ **prototype을 가지고 있지 않은 함수에는 메소드 축약표현과 화살표 함수가 있습니다.**


</details>



**15. What's the output?**

```
function sum(a, b) {
  return a + b;
}

sum(1, '2');
```

- A: `NaN`
- B: `TypeError`
- C: `"12"`
- D: `3`

<details markdown="1">
<summary>Answer</summary>
C


**자바스크립트는 동적타입의 언어입니다.** 즉 변수의 타입을 지정하고 할당하는 것이 아닌 할당되는 데이터 값에 의해 타입이 정해집니다. 위와같은 경우는 계산을 위해서 자바스크립트 자체적으로 1의 숫자타입을 문자열로 변환하게 되는데 이러한 것을 암묵적 변환이라고 부릅니다.

</details>



**16. What's the output?**

```
let number = 0;
console.log(number++);
console.log(++number);
console.log(number);
```

- A: `1` `1` `2`
- B: `1` `2` `2`
- C: `0` `2` `2`
- D: `0` `1` `2`

<details markdown="1">
<summary>Answer</summary>

C


이 문제는 **후치증가 연산자와 전치증가 연산자**의 개념을 물어보고 있습니다. 후치의 ++의 경우 출력후 연산이 이루어지게 되고 전치의 경우 연산 후 값을 출력하게 되어 답은 0 2 2가 됩니다.

</details>



**17. What's the output?**

```
function getPersonInfo(one, two, three) {
  console.log(one);
  console.log(two);
  console.log(three);
}

const person = 'Lydia';
const age = 21;

getPersonInfo`${person} is ${age} years old`;
```

- A: `"Lydia"` `21` `["", " is ", " years old"]`
- B: `["", " is ", " years old"]` `"Lydia"` `21`
- C: `"Lydia"` `["", " is ", " years old"]` `21`

<details markdown="1">
<summary>Answer</summary>

B


자바스크립트의 **함수호출 방법에는 백틱을 사용한 (Tagged Template Literal)방법이 존재한다.**

첫번째 인자는 표현식을 제외한 스트링값의 배열, 두번째 인자는 표현식들이 쓰인만큼 나열
즉 아래 예시의 두 함수호출은 같은 결과를 반환한다.

ex) 

getPersonInfo``${person}is ${age} years old``;
getPersonInfo(["", " is ", " years old"], name, age)

같다!

</details>



**18. What's the output?**

```
function checkAge(data) {
  if (data === { age: 18 }) {
    console.log('You are an adult!');
  } else if (data == { age: 18 }) {
    console.log('You are still an adult.');
  } else {
    console.log(`Hmm.. You don't have an age I guess`);
  }
}

checkAge({ age: 18 });
```

- A: `You are an adult!`
- B: `You are still an adult.`
- C: `Hmm.. You don't have an age I guess`

<details markdown="1">
<summary>Answer</summary>

C


**이 문제는 원시타입과 객체 타입의 값을 참조하고 있는 형태를 알아야 이해할 수 있다.**

원시타입의 경우 바로 값이 들어있는 주소를 참조하고 있지만 객체타입의 경우는 객체 값이 들어있는 주소의 주소를 한단계 더 걸쳐 참조하고 있기 때문에 결국 객체 == 객체 혹은 객체 ===객체는 바로 값을 비교하는게 아니라 참조값(주소)를 비교하고 있다는 말이된다!

</details>


**19. What's the output?**

```
function getAge(...args) {
  console.log(typeof args);
}

getAge(21);
```

- A: `"number"`
- B: `"array"`
- C: `"object"`
- D: `"NaN"`

<details markdown="1">
<summary>Answer</summary>

C


**...args와 같은 것을 스프레드 연산자**라고 한다. 위와 같은 경우 매개변수로 ...args를 지정하면 인수로 들어가는 모든 값을 args라는 식별자의 배열로 저장하게 된다. 배열의 데이터 타입은 객체이므로 답은 object이다.

</details>


**20. What's the output?**

```
function getAge() {
  'use strict';
  age = 21;
  console.log(age);
}

getAge();
```

- A: `21`
- B: `undefined`
- C: `ReferenceError`
- D: `TypeError`

<details markdown="1">
<summary>Answer</summary>

C


이 문제는 **자바스크립트의 strict mode**의 개념을 알아야 한다. 오타나 문법의 지식의 미비로 인한 실수는 언제든 발생할 수 있다. 이 모드는 오류를 줄여 안정적인 코드를 생산하기 위한 해결책이라고 할 수 있다. 예를 들어 아래와 같은 경우가 있다.

function foo(){

x=10;

}

foo();

console.log(x);  // 자바스크립트는 이때 10이라고 하는 결과를 도출한다.
이유는 함수안에 x식별자가 없어 전역인 상위스코프를 확인하게 되고 전역에도 존재하지 않는 x는 자바스크립트가 암묵적으로 전역객체의 프로퍼티로 생성하기 때문이다. 따라서 마치 전역에 선언한 변수처럼 접근이 가능하게 된다. 이는 의도치 않은 결과이므로 이러한 오류들을 줄이기 위해서 strict mode를 사용한다. strict mode는 전역의 선두 또는 함수몸체의 선두에 ‘use strict’를 추가하여 사용한다. ESLint같은 도구를 사용해도 이와 유사하며 더 나은 효과를 얻을 수 있다는 것을 참조하자.

</details>



**21. What's the value of `sum`?**

`const sum = eval('10*10+5');`

- A: `105`
- B: `"105"`
- C: `TypeError`
- D: `"10*10+5"`

<details markdown="1">
<summary>Answer</summary>

A


eval함수는 문자열을 인자로 받아서 실행하는 함수이다. 다만 만약 표현식이 사용되었다면 표현식을 평가한 결과를 반환하게 되는데 그래서 답은 숫자 105가 된다.

</details>



### **22. How long is cool_secret accessible?**

`sessionStorage.setItem('cool_secret', 123);`

- A: Forever, the data doesn't get lost.
- B: When the user closes the tab.
- C: When the user closes the entire browser, not only the tab.
- D: When the user shuts off their computer.

<details markdown="1">
<summary>Answer</summary>

B


**이 문제는 웹 스토리지 객체 localStorage와 sessionStorage를 알아야한다.**
이 객체는 브라우저 내에 키-값 쌍을 저장할 수 있게 도와주는데 session의 경우 페이지 새로고침한 경우만 기억이 남고 기본적으로 탭을 제거했다가 다시 오픈한 경우에는 데이터가 삭제된다. local의 경우 session보다 더 강력하게 브라우저를 껐다가 다시켜도 즉 브라우저나 OS가 재시작해도 데이터가 사라지지 않고 남아있는다. session의 경우 제한적이기에 많이 쓰이지 않으며 같은 페이지라도 탭이 다르면 다른곳에는 저장되지 않는다. 반면 local은 origin이 같다면 모든 탭과 창에서 공유된다.

- 쿠기와는 다른 개념이며 간단히는 네트워크 요청 시 서버로 전달되지 않으며 이런특징으로 쿠키보다 더 많은 자료를 보관할 수 있게된다.웹스토리지 객체 조작은 모두 자바스크립트 내에서 수행된다.
- 참조: [https://ko.javascript.info/localstorage](https://ko.javascript.info/localstorage)

두 스토리지 객체는 동일한 메서드와 프로퍼티를 제공한다.

- `setItem(key, value)` – 키-값 쌍을 보관합니다.
- `getItem(key)` – 키에 해당하는 값을 받아옵니다.
- `removeItem(key)` – 키와 해당 값을 삭제합니다.
- `clear()` – 모든 것을 삭제합니다.
- `key(index)` – 인덱스(`index`)에 해당하는 키를 받아옵니다.
- `length` – 저장된 항목의 개수를 얻습니다.

</details>



### **23. What's the output?**

`var num = 8;
var num = 10;

console.log(num);`

- A: `8`
- B: `10`
- C: `SyntaxError`
- D: `ReferenceError`

<details markdown="1">
<summary>Answer</summary>

B


var 키워드로 변수를 생성한다면 크게 3가지 문제를 겪을 수 있다. 첫쨰는 **중복 선언이 가능**하여 전에 선언된 변수가 있다면 덮어쓰게 된다는 것이고 둘째는 오직 **함수레벨스코프**만 따른다는 것이다. 셋쨰는 호이스팅을 통해 **변수의 선언과 초기화가 동시**에 이루어 지는 것인데 이 문제의 경우 첫번째 이유로 인해 답은 B가 된다.


</details>



### **24. What's the output?**

`const obj = { 1: 'a', 2: 'b', 3: 'c' };
const set = new Set([1, 2, 3, 4, 5]);

obj.hasOwnProperty('1');
obj.hasOwnProperty(1);
set.has('1');
set.has(1);`

- A: `false` `true` `false` `true`
- B: `false` `true` `true` `true`
- C: `true` `true` `false` `true`
- D: `true` `true` `true` `true`


<details markdown="1">
<summary>Answer</summary>


C

ES6의 새로운 자료구조에는 Map과 Set이 존재한다. 둘다 생성자 함수로서 객체를 생성하지만 Map의 경우 키가 있는 데이터를 저장한다는 점에서 객체와 유사하고 Set은 키가 없는 값을 저장한다는 점에서 배열과 유사하다. 둘다 순서를 기억하는 이터러블 객체를 생성하여 반복문을 사용할 수 있다. 특히 이번 문제의 Set의 경우 중복을 허용하지 않는 객체를 생성한다.(파이썬의 집합)

 일반적인 객체의 키 값은 직접 문자열로 입력하지 않아도 내부적으로 문자열입니다. 따라서 “1”도 가능하며 1도 가능합니다. 하지만 set의 경우 인수로 준 이터러블 객체의  요소를 그대로 가져오므로 객체 set에 들어있는 1은 문자열 “1”이 아니다. 

참조 : [https://mine-it-record.tistory.com/473](https://mine-it-record.tistory.com/473)

</details>


### **25. What's the output?**

`const obj = { a: 'one', b: 'two', a: 'three' };
console.log(obj);`

- A: `{ a: "one", b: "two" }`
- B: `{ b: "two", a: "three" }`
- C: `{ a: "three", b: "two" }`
- D: `SyntaxError`

<details markdown="1">
<summary>Answer</summary>

C


만약 객체에 같은 키 이름으로 두개의 값이 들어가게 된다면 처음에 입력된 순서에 있지만 값은 최신것으로 바뀌게 된다. 재할당의 느낌

</details>



### **26. The JavaScript global execution context creates two things for you: the global object, and the "this" keyword.**

- A: true
- B: false
- C: it depends

<details markdown="1">
<summary>Answer</summary>

A

자바스크립트의 전역 실행컨텍스트는 전역객체와 this키워드를 생성한다는 것이 문제의 질문인데 사실 내 개인적인 생각은 false인 것 같다. 왜냐하면 전역객체는 사실 전역컨텍스트에 의해 생성되는 것이 아닌 가장 프로그램 실행시 가장 먼저 실행되게 되고 전역컨텍스트가 이 전역객체를 참조하게 되는 것이기 때문이다. this키워드를 생성하여 바인딩하는 것은 맞다.

</details>



### **27. What's the output?**

```jsx
for (let i = 1; i < 5; i++) {
  if (i === 3) continue;
  console.log(i);
}
```

- A: `1` `2`
- B: `1` `2` `3`
- C: `1` `2` `4`
- D: `1` `3` `4`

<details markdown="1">
<summary>Answer</summary>

C


continue키워드는 반복문에서 내용을 건너뛰고 다음 반복을 진행하라는 의미이다. 따라서 위와 같은 경우 i가 3이 되었을 때는 console.log(i)를 거치지 않고 다음으로 넘어가게 된다.

</details>




### **28. What's the output?**

```jsx
String.prototype.giveLydiaPizza = () => {
  return 'Just give Lydia pizza already!';
};

const name = 'Lydia';

name.giveLydiaPizza();
```

- A: `"Just give Lydia pizza already!"`
- B: `TypeError: not a function`
- C: `SyntaxError`
- D: `undefined`

<details markdown="1">
<summary>Answer</summary>

A

String은 자바스크립트의 빌트인 객체로서 생성자 함수이다. 이 문제에서는 **래퍼객체의 개념과 프로토타입의 개념**을 물어보고 있다. 자바스크립트는 원시타입의 name 식별자를 객체처럼 사용할 때 문자열의 타입을 암묵적으로 객체로 변환시킨다.

(new String("hello")을 호출한 것처럼 string 자료형을 래퍼 객체로 임시로 변환하기 때문입니다.) 

이렇게 객체형태로 원시타입의 값을 감싸게 된 것을 래퍼객체라고 하며 이때 문자열은 프로토타입 체인을 따라 자신이 상속받는 프로퍼티나 메소드를 사용할 수 있게 된다.

- 원시타입에 대응하는 4개의 래퍼객체: String, Number, Boolean, Symbol

</details>



### **29. What's the output?**

```jsx
const a = {};
const b = { key: 'b' };
const c = { key: 'c' };

a[b] = 123;
a[c] = 456;

console.log(a[b]);
```

- A: `123`
- B: `456`
- C: `undefined`
- D: `ReferenceError`

<details markdown="1">
<summary>Answer</summary>

B

객체의 키값으로 등록되는 것들은 모두 암묵적으로 문자열 화가 된다. 만약 그것이 객체라면 “[object Object]”형태의 문자열이 되는데 이때 B,C모두 같은 문자열 키 “[object Object]”의 형태이기 때문에 객체 c를 키로서 전해줄때 프로퍼티키 “[object Object]”의 값이 123에서 456으로 재할당되는 것이다.
즉 b,c 둘다 “[object Object]”를 의미하며 결국 a[b]는  a[“[object Object]”]를 의미하는 것이다.


</details>


### **30. What's the output?**

```jsx
const foo = () => console.log('First');
const bar = () => setTimeout(() => console.log('Second'));
const baz = () => console.log('Third');

bar();
foo();
baz();
```

- A: `First` `Second` `Third`
- B: `First` `Third` `Second`
- C: `Second` `First` `Third`
- D: `Second` `Third` `First`


<details markdown="1">
<summary>Answer</summary>

B


문제는 비동기 작동의 메커니즘을 물어보고 있다. setTimeout은 인수로 전달된 콜백학수를 지연시간후에 작동시키는 함수이다. 설령 위처럼 지연시간이 정해져 있지 않아도 setTimeout()을 호출함과 동시에 일단 bar함수는 stack에서 pop되기 때문에 다른 함수들이 먼저 실행되고 마지막에 ‘second’를 호출하는 콜백함수를 작동시킨다. 아래 그림을 참조하며 실제로 delay로 주어지는 시간은 정확히 말하면 wep api인 setTimeout()이 테스크큐로 함수를 인풋하기까지 걸리는 delay를 말한다.  출력되기 까지의 시간이 아니라는 것이다. 큐에 등록된 함수는 이벤트루프를 통해서 stack이 비게 된다면 그때 stack으로 push되어 비로소 호출된다.

</details>



### **31. What is the event.target when clicking the button?**

```jsx
<div onclick="console.log('first div')">
  <div onclick="console.log('second div')">
    <button onclick="console.log('button')">
      Click!
    </button>
  </div>
</div>
```

- A: Outer `div`
- B: Inner `div`
- C: `button`
- D: An array of all nested elements.

<details markdown="1">
<summary>Answer</summary>

C

 event.target은 클릭한 대상 그자체를 의미하며 이 문제에서는 button을 출력하는 button태그를 말한다.
 
</details>




### **32. When you click the paragraph, what's the logged output?**

```jsx
<div onclick="console.log('div')">
  <p onclick="console.log('p')">
    Click here!
  </p>
</div>
```

- A: `p` `div`
- B: `div` `p`
- C: `p`
- D: `div`

<details markdown="1">
<summary>Answer</summary>

A

이벤트는 크게 캡쳐링 타겟 버블링으로 나누어진다. 그 중 addEventlistener를 통해 3번째 인수를 true로 전달하는 경우가 아니라면 기본적으로 캡쳐링은 일어나지 않는다. 즉 클릭한 타겟과 버블링 현상이 나타나고 그로인해 p div가 출력된다.

</details>


### **33. What's the output?**

```jsx
const person = { name: 'Lydia' };

function sayHi(age) {
  return `${this.name} is ${age}`;
}

console.log(sayHi.call(person, 21));
console.log(sayHi.bind(person, 21));
```

- A: `undefined is 21` `Lydia is 21`
- B: `function` `function`
- C: `Lydia is 21` `Lydia is 21`
- D: `Lydia is 21` `function`

<details markdown="1">
<summary>Answer</summary>

D


bind,call,apply는 모두 함수에 this를 고정시켜줄때 사용할수 있는 메소드들이다. bind는 인자로 (this가 될 객체, ...함수에실제인수로 들어갈 표현식)으로 사용할 수 있으며 call, apply와 다르게 함수 실행 결과가 아니라 binding이 끝난 bound 함수를 다시 리턴한다. call과 apply는 첫번째 인자로 this바인딩을 위한 객체를 넣고 두번째부터는 함수에 실제 인수로 들어갈 표현식이 들어간다. 결국 binding과 똑같다. 하지만 call과 apply의 경우 함수를 실행한 값을 리턴한다. 또한 say.apply의 두번째 인수는 모두 배열로 넣어줘야한다는 차이가 있다.(call과 apply의 차이)

- say.apply(obj, [”seoul”])
- say.call(obj, "seoul")
- student1.print.apply(this, ['male', 'A']); - 실행결과 반환
- var print = student1.print.bind(this, 'female','O'); - binding시킨 student1.print함수 반환


</details>


### **34. What's the output?**

```jsx
function sayHi() {
  return (() => 0)();
}

console.log(typeof sayHi());
```

- A: `"object"`
- B: `"number"`
- C: `"function"`
- D: `"undefined"`

<details markdown="1">
<summary>Answer</summary>

B

이 문제는 sayHi()를 통해 함수의 리턴값을 물어보고 있다. 함수의 리턴 값은 화살표함수의 즉시 실행함수로 이루어져있고 그 결과인 0이라는 숫자값이 나오게 되는데 결국 typeof가 가리키는 대상은 숫자형 0이다.

</details>




### **35. Which of these values are falsy?**

```jsx
0;
new Number(0);
('');
(' ');
new Boolean(false);
undefined;
```

- A: `0`, `''`, `undefined`
- B: `0`, `new Number(0)`, `''`, `new Boolean(false)`, `undefined`
- C: `0`, `''`, `new Boolean(false)`, `undefined`
- D: All of them are falsy 

<details markdown="1">
<summary>Answer</summary>

A

아래는 8가지의 falsy를 의미하는 형태이다.(생성자함수로 만들어진 객체는 truthy이다.)

`undefined`

`null`

`NaN`

`false`

`''` (empty string)

`0`

-`0`

`0n` (BigInt(0))

</details>




### **36. What's the output?**

```jsx
console.log(typeof typeof 1);
```

- A: `"number"`
- B: `"string"`
- C: `"object"`
- D: `"undefined"`

<details markdown="1">
<summary>Answer</summary>

B

첫번쨰 typeof는 “number”를 출력하며 두번째 typeof를 통해 “number”의 타입 string을 출력한다.

</details>


### **37. What's the output?**

```jsx
const numbers = [1, 2, 3];
numbers[10] = 11;
console.log(numbers);
```

- A: `[1, 2, 3, 7 x null, 11]`
- B: `[1, 2, 3, 11]`
- C: `[1, 2, 3, 7 x empty, 11]`
- D: `SyntaxError`

<details markdown="1">
<summary>Answer</summary>

C

문제와 같이 배열에 특정 인덱스에 값을 추가하면 그 크기만큼 배열이 확장되고 그 사이에는 undefined형태의 값이 할당된다.

</details>



### **38. What's the output?**

```jsx
(() => {
  let x, y;
  try {
    throw new Error();
  } catch (x) {
    (x = 1), (y = 2);
    console.log(x);
  }
  console.log(x);
  console.log(y);
})();
```

- A: `1` `undefined` `2`
- B: `undefined` `undefined` `undefined`
- C: `1` `1` `2`
- D: `1` `undefined` `undefined`

<details markdown="1">
<summary>Answer</summary>

A

화살표함수 내부에서 x,y를 선언한 상태에서 두값은 모두 undefined상태이다. 이때 catch의 인수로 전달된 x는 try문에서 전달한 에러를 나타낸다. 즉 catch블록 안에서 별개의 x변수이다. x를 1로 **식별자 결정**에 의해 catch문안에 x에 1이 할당되고 y는 화살표함수에서 생성된 y를 의미하며 2를 할당하게 된다. 결국 catch문 내부 x는 1이 되고 블록 밖으로 나가면 화살표함수내에 x는 그래도 undefined상태이다. y는 catch문 안에서 2로 할당되면서 2를 출력하게 된다.

</details>



### **39. Everything in JavaScript is either a...**

- A: primitive or object
- B: function or object
- C: trick question! only objects
- D: number or object

<details markdown="1">
<summary>Answer</summary>

A

자바스크립트는 기본 원시타입과 객체 타입만이 존재한다. 둘의 차이점은 원시타입이 속성이나 메서드가 없다는 것인데 원시타입의 값은 래퍼 객체의 형태로 사용되는 방법도 존재한다. 하지만 기본적으로 자바스크립트에는 원시타입(기본형), 객체타입 2가지가 존재한다.

- 원시타입에 대응하는 4개의 래퍼객체: String, Number, Boolean, Symbol

(null,undefined제외 나머지)


</details>


### **40. What's the output?**

```jsx
[[0, 1], [2, 3]].reduce(
  (acc, cur) => {
    return acc.concat(cur);
  },
  [1, 2],
);
```

- A: `[0, 1, 2, 3, 1, 2]`
- B: `[6, 1, 2]`
- C: `[1, 2, 0, 1, 2, 3]`
- D: `[1, 2, 6]`

<details markdown="1">
<summary>Answer</summary>

C


**Array.prototype.reduce(), reduce메소드는 배열의 각 요소에 대해 첫번째 인자로 주어진 콜백함수를 실행하고 하나의 결과값을 누적계산하여 반환하는 함수이다.**

### 리듀서 내 콜백함수는 네 개의 인자를 가집니다.

- 누산기 (acc) -- 계산을 통해 계속해서 값이 누적된다.(맨 처음에는 초기값, 초기값 지정이 없다면 인덱스 0번)
- 현재 값 (cur) -- 배열의 각 요소들을 의미 ,reduce함수의 초기값이 없다면 인덱스 0번이 초기값이 되고 인덱스 1번이 cur이 되어
계산을 시작한다.
- 현재 인덱스 (idx) -- 잘 안씀
- 원본 배열 (src) -- 잘 안씀
- 

ex) [0, 1, 2, 3, 4].reduce( (prev, curr) => prev + curr ,10(초기값!!!!));


![image](https://user-images.githubusercontent.com/98815511/167278806-35bc406e-d581-4de6-8946-e915e8d3a260.png)


</details>



### **41. What's the output?**

```jsx
!!null;
!!'';
!!1;
```

- A: `false` `true` `false`
- B: `false` `false` `true`
- C: `false` `true` `true`
- D: `true` `true` `false`

<details markdown="1">
<summary>Answer</summary>


B

부정의 부정 의미를 잘 파악해야한다. false의 부정의 부정은 다시 false이며 true의 부정의 부정 또한 다시 true이다.

</details>



### **42. What does the `setInterval` method return in the browser?**

```jsx
setInterval(() => console.log('Hi'), 1000);
```

- A: a unique id
- B: the amount of milliseconds specified
- C: the passed function
- D: `undefined`

<details markdown="1">
<summary>Answer</summary>

A

setInterval함수는 그 반환값으로 유니크한 고유값을 반환하고 그것을 clearInterval(고유값)이렇게 전달하면 setInterval함수를 종료시킬 수 있다.

</details>



### **43. What does this return?**

```jsx
[...'Lydia'];
```

- A: `["L", "y", "d", "i", "a"]`
- B: `["Lydia"]`
- C: `[[], "Lydia"]`
- D: `[["L", "y", "d", "i", "a"]]`

<details markdown="1">
<summary>Answer</summary>

A


문자열은 유사배열객체로 사용가능 하며 스프레드 연산자를 통해 이터러블한 각각의 요소를 다룰수 있다.


</details>



### **44. What's the output?**

```jsx
function* generator(i) {
  yield i;
  yield i * 2;
}

const gen = generator(10);

console.log(gen.next().value);
console.log(gen.next().value);
```

- A: `[0, 10], [10, 20]`
- B: `20, 20`
- C: `10, 20`
- D: `0, 10 and 10, 20`

<details markdown="1">
<summary>Answer</summary>

C


제너레이터는 **코드블록의 실행을 일시 중지했다가 필요한 시점에 재개**할 수 있는 특수한 함수이다. 제너레이터 함수는 **제너레이터 객체**를 반환하고 그 객체는 이터레이터로서 순환을 위한 **next메소드**를 갖게 된다. 그리고 그 next는 메소드는 다시{value: , done:}의 프로퍼티를 가지는 **이터레이트 리절트 객체**를 생성한다. 포인트는 제너레이터는 1개 이상의 yield 키워드를 가진
표현식을 가지게 되며 next메소드를 통해 yield 키워드를 기준으로 이동하고 다음 yield키워드에 멈추면서 그떄마다 각 상태의 {value: , done:} 프로퍼티를 가지는 이터레이트 리절트 객체를 생성한다. 모든 yield가 다 끝나면 일반함수처럼 작동하면서 return값을 반환한다. (return 이 없으면 일반함수와 같이 undefined 반환)


</details>




**45. What does this return?**

```jsx
const firstPromise = new Promise((res, rej) => {
  setTimeout(res, 500, 'one');
});

const secondPromise = new Promise((res, rej) => {
  setTimeout(res, 100, 'two');
});

Promise.race([firstPromise, seco ndPromise]).then(res => console.log(res));
```

- A: `"one"`
- B: `"two"`
- C: `"two" "one"`
- D: `"one" "two"`

<details markdown="1">
<summary>Answer</summary>


B

Promise.race는 가장먼저 fulfilled 상태가 되어 결과를 반환하는 새로운 프로미스만을 반환하는 함수이다. 보기 처럼 인자는 배열 등의 이터러블을 인수로 전달받는다.

</details>




### **46. What's the output?**

```jsx
let person = { name: 'Lydia' };
const members = [person];
person = null;

console.log(members);
```

- A: `null`
- B: `[null]`
- C: `[{}]`
- D: `[{ name: "Lydia" }]`

<details markdown="1">
<summary>Answer</summary>

D

 이번 문제는 값의 복사,전달의 개념을 이해해야한다. 우선 **객체타입의 값은 참조값을 복사해서 전달한다**. 즉 실제 값이 아닌 주소값을 복사해서 전달하게 되고 이를 받은 변수 member 배열의 첫번쨰 요소로 이 주소가 등록된다. 이후 만약 같은 주소의 객체 요소(프로퍼티)를 교체했다면 person과 member배열의 첫번째 요소의 값은 같은 상태를 유지하면서 변경될 것이다. 왜냐하면 참조하고 있는 주소값이 같기 때문이다. **하지만 문제에서는 아예 person변수의 들어가는 값(주소) 자체를 변경했다.**
**객체타입이 가변하다는 것은 그 프로퍼티 요소를 바꾸었을 때만 성립**되는 것이고 아예 다른 값을 다시 할당하면 메모리공간에 새로운 값을 생성한뒤 그 주소를 참조하게 된다. 따라서 배열의 요소는 변화가 없는것이다.

![image](https://user-images.githubusercontent.com/98815511/169702617-ea2c1aef-c513-41b5-97a2-8307ea86cd62.png)


</details>



### **47. What's the output?**

```jsx
const person = {
  name: 'Lydia',
  age: 21,
};

for (const item in person) {
  console.log(item);
}
```

- A: `{ name: "Lydia" }, { age: 21 }`
- B: `"name", "age"`
- C: `"Lydia", 21`
- D: `["name", "Lydia"], ["age", 21]`

<details markdown="1">
<summary>Answer</summary>

B

```
for~of(배열에 사용 값을 도출)
✔️ 반복 가능한 객체(iterable)를 순회할 수 있도록 해줌
✔️ Array, Map, Set, arguments 등이 해당됨 (Object는 해당 X)
참고로 객체의 값은 이터러블이지만 열거가능한 속성은 아님.
```

```
for ~ in
✔️ 객체의 열거 가능한 '속성들'을 순회할 수 있도록 해줌
✔️ 객체의 key값에 접근 가능, value값에는 직접 접근 불가
✔️ 모든 객체에서 사용 가능
```

</details>



### **48. What's the output?**

`console.log(3 + 4 + '5');`

- A: `"345"`
- B: `"75"`
- C: `12`
- D: `"12"`

<details markdown="1">
<summary>Answer</summary>

B


자바스크립트의 산술연산자의 결합성은 좌항에서 우항으로 이동한다. 즉 3과 4를 먼저 더하고 '5'를더하게 되는데 문자열 '5'를 만날때는 +는 문자열 결합 연산자로 사용이되며 암묵적으로 3과4를 더한 7도 문자열 '7'이 되어 '75'가 된다.


</details>



### **49. What's the value of `num`?**

```jsx
const num = parseInt('7*6', 10);
```

- A: `42`
- B: `"42"`
- C: `7`
- D: `NaN`


<details markdown="1">
<summary>Answer</summary>

C


**parseInt는 string을 정수로 변환한 값을 리턴하는 함수입니다**. 인자는 2개를 가지며(string,radix)
radix에는 2~36의 진법체계가 들어가게 된다. 결과는 모두 10진수로 해석되며 이문제의 경우 10진수로 해석하여 10진수로 결과를 보여준다. 단 **소수는 소수점을 자르고 정수로 표현**되며 **정수로 표현될 수 없는 문자열 값을 만나게 되면 이후는 생략**한다. 이번 문제는 "*"를 만나면서 정수 변환이 불가하여 7까지만 해석한 것이다. 만약 첫글자부터 정수로 변경할 수 없다면 NaN값을 리턴한다.(단 공백은 첫글자로 들어와도 무시되고 다음을 해석할 수 있다.)

- 참조 : [https://hianna.tistory.com/386](https://hianna.tistory.com/386)
</details>




### **50. What's the output?**

```jsx
[1, 2, 3].map(num => {
  if (typeof num === 'number') return;
  return num * 2;
});
```

- A: `[]`
- B: `[null, null, null]`
- C: `[undefined, undefined, undefined]`
- D: `[ 3 x empty ]`



<details markdown="1">
<summary>Answer</summary>

C


[1,2,3]이라는 배열을 map으로 순회하면서 새로운 배열을 만드는 것인데 이떄 if 문을 통해 각각의 요소의 타입이 number인지 찾고 있다 만약 number라면 undefined를 리터하는 형태이며 모든요소가 number타입이기 때문에 답은 C가된다.

</details>


### **51. What's the output?**

```jsx
function getInfo(member, year) {
  member.name = 'Lydia';
  year = '1998';
}

const person = { name: 'Sarah' };
const birthYear = '1997';

getInfo(person, birthYear);

console.log(person, birthYear);
```

- A: `{ name: "Lydia" }, "1997"`
- B: `{ name: "Sarah" }, "1998"`
- C: `{ name: "Lydia" }, "1998"`
- D: `{ name: "Sarah" }, "1997"`

<details markdown="1">
<summary>Answer</summary>

A

문제에서 person의 경우 객체타입으로 참조값을 저장하고 birthyear의 경우 원시타입으로 값자체를 저장한다. 즉 함수가 실행되고 각각 파라메타로 값을 전달했을때 member는 참조값(주소)을 받고, year는 값자체를 받는다. member.name은 똑같은 객체의 name프로퍼티의 값을 바꾸지만 member자체의 참조값이 바뀌지는 않는다. 하지만 year는 새로운 값으로 완전히 갱신되게 된다. 따라서 답은 A이다.

</details>


### **52. What's the output?**

```jsx
function greeting() {
  throw 'Hello world!';
}

function sayHi() {
  try {
    const data = greeting();
    console.log('It worked!', data);
  } catch (e) {
    console.log('Oh no an error:', e);
  }
}

sayHi();
```

- A: `It worked! Hello world!`
- B: `Oh no an error: undefined`
- C: `SyntaxError: can only throw Error objects`
- D: `Oh no an error: Hello world!`

<details markdown="1">
<summary>Answer</summary>

D


자바스크립트의 오류에는 2가지가 있다. 에러와 예외이다.

에러에 경우는 프로그래밍 언어의 문법적인 오류 예를 들면 괄호({[]})를 닫지 않았다거나
할 때 에러라고 부른다.
ex)

```jsx
let a=3;

try{

  console.log(a,b
}catch(e){

  console.log(e.message)
}
// 이 경우 발생한 에러는 예외처리로 처리할 수 없다.
```

예외에 경우는 프로그램을 실행 중 발생하는 오류를 예외라고 한다.

ex)

```jsx
let a=3;

try{

  console.log(a,b)
}catch(e){

  console.log(e.message)
}
// b is not defined를 출력한다 이는 변수 b를 선언하지 않았기 때문이다.
```

예외 : try catch 구문으로 해결할 수 있는 것

에러 : try catch 구문으로 해결할 수 없는 것

- 예외처리는 크게 기본예외처리와 고급예외처리가 있으며 try,catch,finally는 고급예외처리에 해당한다.
- throw문은 예외를 강제적으로 발생시킬수 있는 키워드이며 표현식값이 뒤에 나올 수 있다. catch문은 예외가 발생하면 실행되고 e라는 지역객체를 받아서 작동할 수 있다.
- 에러든 오류든 만약 발생한다면 그 지점에서 모든 작동을 중지하고 예외처리를 찾아낸다. 만약 해당 컨텍스트에 없다면 실행 컨텍스트 stack을 따라 함수를 호출한 곳으로 예외를 전파하며 그 과정에서 처리문(catch)을 찾아내면 그곳에서 처리되고 이후 코드는 다시 처리된 후부터 실행된다. 만약 끝까지 처리문을 찾지 못한다면 에러로서 출력되게 된다.
- 에러는 처리할 수 없다.


</details>


### **53. What's the output?**

```jsx
function Car() {
  this.make = 'Lamborghini';
  return { make: 'Maserati' };
}

const myCar = new Car();
console.log(myCar.make);
```

- A: `"Lamborghini"`
- B: `"Maserati"`
- C: `ReferenceError`
- D: `TypeError`


<details markdown="1">
<summary>Answer</summary>

B 

생성자 함수는 암묵적으로 새로운 인스턴스를 반환한다. 만약 return이 명시적으로 적혀있어도 원시타입인경우는 무시하지만 그 값이 객체라면 인스턴스가 아닌 객체를 반환하게 된다.

</details>




### **54. What's the output?**

```jsx
(() => {
  let x = (y = 10);
})();

console.log(typeof x);
console.log(typeof y);
```

- A: `"undefined", "number"`
- B: `"number", "number"`
- C: `"object", "number"`
- D: `"number", "undefined"`


<details markdown="1">
<summary>Answer</summary>

>Answer: A

즉시 실행함수 내부 x는 함수 레벨 스코프인 let으로 선언이 되었고 선언 키워드가 없는 y의 경우 자바스크립트에서는 암묵적 전역으로 전역 객체의 변수로 등록이 된다. 마치 window.y의 느낌으로 등록된다.


</details>


### **55. What's the output?**

```jsx
class Dog {
  constructor(name) {
    this.name = name;
  }
}

Dog.prototype.bark = function() {
  console.log(`Woof I am ${this.name}`);
};

const pet = new Dog('Mara');

pet.bark();

delete Dog.prototype.bark;

pet.bark();
```

- A: `"Woof I am Mara"`, `TypeError`
- B: `"Woof I am Mara"`, `"Woof I am Mara"`
- C: `"Woof I am Mara"`, `undefined`
- D: `TypeError`, `TypeError`

<details markdown="1">
<summary>Answer</summary>


 A
 
 delete 키워드는 객체의 속성을 삭제하는 키워드이다. 문제의 경우 bark라는 함수를 삭제하게 되면 더 이상 객체의 프로퍼티로 존재하지 않게 되고 undefined인 상태이다. 이때 함수를 호출하는 ()연산자와 함께 사용하면 TypeError: pet.bark is not a function 에러가 나타나게 된다.
 
</details>



**56. What's the output?**

```jsx
const set = new Set([1, 1, 2, 3, 4]);

console.log(set);
```

- A: `[1, 1, 2, 3, 4]`
- B: `[1, 2, 3, 4]`
- C: `{1, 1, 2, 3, 4}`
- D: `{1, 2, 3, 4}`

<details markdown="1">
<summary>Answer</summary>

D

Set은 이터러블을 인수로 받아 그대로 이터러블한 객체를 반환한다. 키가 없는 객체의 형태인 것이 배열과 유사하지만 인덱스도 없다. 하지만 이터러블로 for of의 대상이 된다. 특히 중요한 것은 Set의 경우 중복을 허용하지 않는다는 것이다.

</details>


### **57. What's the output?**

```jsx
// counter.js

let counter = 10;
export default counter;`

// index.js

import myCounter from './counter';

myCounter += 1;

console.log(myCounter);
```

- A: `10`
- B: `11`
- C: `Error`
- D: `NaN`

<details markdown="1">
<summary>Answer</summary>

C

자바스크립트의 import로 모듈로서 가져온 값은 읽기 전용이므로 수정할 수 없다. 해당 모듈에서만 값을 변경할 수 있다.

</details>


### **58. What's the output?**

```jsx
const name = 'Lydia';
age = 21;

console.log(delete name);
console.log(delete age);
```

- A: `false`, `true`
- B: `"Lydia"`, `21`
- C: `true`, `true`
- D: `undefined`, `undefined`


<details markdown="1">
<summary>Answer</summary>

A

delete키워드는 boolean값을 반환하며 객체의 요소를 제거하는 연산자이다. age의 경우 암묵적 전역으로 window 객체의 프로퍼티로 등록되지만 name은 그냥 일반 string type의 변수로
delete키워드는 age에서만 true를 반환하게 된다.

</details>



### **59. What's the output?**

```jsx
const numbers = [1, 2, 3, 4, 5];
const [y] = numbers;

console.log(y);
```

- A: `[[1, 2, 3, 4, 5]]`
- B: `[1, 2, 3, 4, 5]`
- C: `1`
- D: `[1]`


<details markdown="1">
<summary>Answer</summary>

C

리액트에서 특히 많이 사용되는 구조분해 할당문법에 해당하며 numbers배열 안에 첫번째 값을 y라는 변수에 대입한다는 의미이다.(react hook useState)

</details>


### **60. What's the output?**

```
const user = { name: 'Lydia', age: 21 };
const admin = { admin: true, ...user };

console.log(admin);
```

- A: `{ admin: true, user: { name: "Lydia", age: 21 } }`
- B: `{ admin: true, name: "Lydia", age: 21 }`
- C: `{ admin: true, user: ["Lydia", 21] }`
- D: `{ admin: true }`


<details markdown="1">
<summary>Answer</summary>

B

… 스프레드 연산자를 통해서 활용한 문법으로 위와 같이 해당 배열이나 객체의 요소들을 나열할 수 있다.
</details>


### **61. What's the output?**

```jsx
const person = { name: 'Lydia' };

Object.defineProperty(person, 'age', { value: 21 });

console.log(person);
console.log(Object.keys(person));
```

- A: `{ name: "Lydia", age: 21 }`, `["name", "age"]`
- B: `{ name: "Lydia", age: 21 }`, `["name"]`
- C: `{ name: "Lydia"}`, `["name", "age"]`
- D: `{ name: "Lydia"}`, `["age"]`



<details markdown="1">
<summary>Answer</summary>


B

defineProperty 정적메소드는 해당 객체에 새로운 프로퍼티를 추가하거나 기존의 프로퍼티를 수정하는 것이 가능하다. 이때 `writable`, `configurable, enumerable` 등의 프로퍼티 디스크립션을 세번째 인자에 같이 등록이 가능한데 기본적으로 enumerable속성이 default값 false로 지정되기 때문에 Object.keys처럼 객체의 나열 가능한 키값을 추출하는 배열을 돌릴 때 age를 찾아내지 못해서 B가 답이 된 것이다.

- 참조: [https://blog.woolta.com/categories/3/posts/143](https://blog.woolta.com/categories/3/posts/143)

</details>



### **62. What's the output?**

```jsx
const settings = {
  username: 'lydiahallie',
  level: 19,
  health: 90,
};

const data = JSON.stringify(settings, ['level', 'health']);
console.log(data);
```

- A: `"{"level":19, "health":90}"`
- B: `"{"username": "lydiahallie"}"`
- C: `"["level", "health"]"`
- D: `"{"username": "lydiahallie", "level":19, "health":90}"`




<details markdown="1">
<summary>Answer</summary>

A

객체나 일반 타입의 값을 json형식의 문자열로 바꿔주는 json.stringify함수는 2번째 인자로 replacer를 전달할 수 있고 그렇게 되면 해당 키값에 해당하는 값만 json형태로 바꾼 값을 반환한다.

</details>


### **63. What's the output?**

```jsx
let num = 10;

const increaseNumber = () => num++;
const increasePassedNumber = number => number++;

const num1 = increaseNumber();
const num2 = increasePassedNumber(num1);

console.log(num1);
console.log(num2);
```

- A: 10, 10
- B: 10, 11
- C: 11, 11
- D: 11, 12

<details markdown="1">
<summary>Answer</summary>

A

후치 증가 연산자의 첫번째 반환값은 아직 증가하기 전이라는 것을 이해해야 문제를 해결할 수 있다.
</details>


### **64. What's the output?**

```jsx
const value = { number: 10 };

const multiply = (x = { ...value }) => {
  console.log((x.number *= 2));
};

multiply();
multiply();
multiply(value);
multiply(value);
```

- A: `20`, `40`, `80`, `160`
- B: `20`, `40`, `20`, `40`
- C: `20`, `20`, `20`, `40`
- D: `NaN`, `NaN`, `20`, `40`

<details markdown="1">
<summary>Answer</summary>

C

1,2의 호출은 초기값을 설정해 준 상태로 호출할 때마다 새로운 객체를 생성하고 그 프로퍼티는 value객체의 프로퍼티를 복사해온다. value객체의 프로퍼티는 원시타입이기 때문에 값이 복사되고 매번 새로운 값이 복사되는 것이다. 하지만 인자로 value 자체를 넣게 되면 이제 multiply함수가 인자로 받는 x는 value객체와 같은 주소값을 복사하여 하나의 객체를 공유하게 된다.

</details>


### **65. What's the output?**

 

```jsx
[1, 2, 3, 4].reduce((x, y) => console.log(x, y));
```

- A: `1` `2` and `3` `3` and `6` `4`
- B: `1` `2` and `2` `3` and `3` `4`
- C: `1` `undefined` and `2` `undefined` and `3` `undefined` and `4` `undefined`
- D: `1` `2` and `undefined` `3` and `undefined` `4`


<details markdown="1">
<summary>Answer</summary>

D

reduce 함수를 사용하였지만 콜백함수의 반환값이 console.log()로서 즉 undefined이기 때문에 누적값이 계속해서 undefined가 나오게 된다.

</details>



### **66. With which constructor can we successfully extend the `Dog` class?**

```jsx
class Dog {
  constructor(name) {
    this.name = name;
  }
};

class Labrador extends Dog {
  // 1
  constructor(name, size) {
    this.size = size;
  }
  // 2
  constructor(name, size) {
    super(name);
    this.size = size;
  }
  // 3
  constructor(size) {
    super(name);
    this.size = size;
  }
  // 4
  constructor(name, size) {
    this.name = name;
    this.size = size;
  }

};
```

- A: 1
- B: 2
- C: 3
- D: 4


<details markdown="1">
<summary>Answer</summary>

B

클래스를 상속하게 되면 super를 통해 먼저 상위의 클래스를 호출해야하며 이후 상위 클래스를 통해 만들어진 객체(인스턴스)는 상속받은 클래스로 토스되어 과정이 진행된다.


</details>



### **67. What's the output?**

```jsx
// index.js
console.log('running index.js');
import { sum } from './sum.js';
console.log(sum(1, 2));

// sum.js
console.log('running sum.js');
export const sum = (a, b) => a + b;
```

- A: `running index.js`, `running sum.js`, `3`
- B: `running sum.js`, `running index.js`, `3`
- C: `running sum.js`, `3`, `running index.js`
- D: `running index.js`, `undefined`, `running sum.js`



<details markdown="1">
<summary>Answer</summary>

B

import 키워드를 사용하게 되면 가장 먼저 import명령이 걸려있는 파일 먼저 실행되게 된다. 이는 리액트도 마찬가지이며 만약 common js의 require명령어를 사용했다면 이와 달리 실행되는 순서에 맞추어서 작동되게 되어 답은 A처럼 나왔을 것이다.

</details>


### **68. What's the output?**

```jsx
console.log(Number(2) === Number(2));
console.log(Boolean(false) === Boolean(false));
console.log(Symbol('foo') === Symbol('foo'));
```

- A: `true`, `true`, `false`
- B: `false`, `true`, `false`
- C: `true`, `false`, `true`
- D: `true`, `true`, `true`


<details markdown="1">
<summary>Answer</summary>

A

모든 심볼은 그자체로 유니크하다.

</details>


### **69. What's the output?**

```jsx
const name = 'Lydia Hallie';
console.log(name.padStart(13));
console.log(name.padStart(2));
```

- A: `"Lydia Hallie"`, `"Lydia Hallie"`
- B: `" Lydia Hallie"`, `" Lydia Hallie"` (`"[13x whitespace]Lydia Hallie"`, `"[2x whitespace]Lydia Hallie"`)
- C: `" Lydia Hallie"`, `"Lydia Hallie"` (`"[1x whitespace]Lydia Hallie"`, `"Lydia Hallie"`)
- D: `"Lydia Hallie"`, `"Lyd"`,



<details markdown="1">
<summary>Answer</summary>

C


String.prototype의 프로퍼티이다.

- 참조 : [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/padStart](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/padStart)


</details>



### **70. What's the output?**

```jsx
console.log('🥑' + '💻');
```

- A: `"🥑💻"`
- B: `257548`
- C: A string containing their code points
- D: Error


<details markdown="1">
<summary>Answer</summary>

A

문자열 결합 연산자를 통해 두가지 문자열로 인식되는 이모티콘을 결합하게 된다.

</details>



### **71. How can we log the values that are commented out after the console.log statement?**

```jsx
function* startGame() {
  const answer = yield 'Do you love JavaScript?';
  if (answer !== 'Yes') {
    return "Oh wow... Guess we're gone here";
  }
  return 'JavaScript loves you back ❤️';
}

const game = startGame();
console.log(/* 1 */); // Do you love JavaScript?
console.log(/* 2 */); // JavaScript loves you back ❤️
```

- A: `game.next("Yes").value` and `game.next().value`
- B: `game.next.value("Yes")` and `game.next.value()`
- C: `game.next().value` and `game.next("Yes").value`
- D: `game.next.value()` and `game.next.value("Yes")`

<details markdown="1">
<summary>Answer</summary>

C


제너레이터를 이용한 문법으로 처음 yield를 만나면서 함수는 잠시 멈추게되고 그값이 value가 된다. 즉 `game.next().value` = 'Do you love JavaScript?' 인 상태가 되고 이후 다시 함수를 진행시키려면 또 다시 next()함수를 호출하는데 이때 값을 대입하여 value상태를 yes로 만들어야 하는 `game.next("Yes").value`가 나오면 된다.

</details>


### **72. What's the output?**

```
console.log(String.raw`Hello\nworld`);
```

- A: `Hello world!`
- B: `Hello` `world`
- C: `Hello\nworld`
- D: `Hello\n` `world`


<details markdown="1">
<summary>Answer</summary>

C

String.raw는 템플릿리터럴의 태그 함수로 백슬래쉬를 사용하는 기능을 모두 무시할 수 있습니다. 보통 주소를 사용해야하는 경에 사용합니다.

 ex) String.raw`C:\Development\profile\aboutme.html`
 
</details>


### **73. What's the output?**

```jsx
async function getData() {
  return await Promise.resolve('I made it!');

`const data = getData();
console.log(data);`
```

- A: `"I made it!"`
- B: `Promise {<resolved>: "I made it!"}`
- C: `Promise {<pending>}`
- D: `undefined`


<details markdown="1">
<summary>Answer</summary>

C

async함수는 그 자체로 프로미스를 반환하게 된다. 즉 아무리 Promise.resolve가 값이 바로 나와도 그 값을 다시 프로미스로 반환하게 되기 때문에 만약 i made it!을 호출하고 싶다면 data.then(res⇒console.log(res))를 해야한다.

</details>


### **74. What's the output?**

```jsx
function addToList(item, list) {
  return list.push(item);
}

const result = addToList('apple', ['banana']);
console.log(result);
```

- A: `['apple', 'banana']`
- B: `2`
- C: `true`
- D: `undefined`


<details markdown="1">
<summary>Answer</summary>

B

.push 메소드는 해당 배열의 length를 리턴한다. 즉 새로운 apple요소가 추가된 2개의 요소를 가진 배열의 length 2를 출력하게 된다.

</details>


### **75. What's the output?**

```jsx
const box = { x: 10, y: 20 };

Object.freeze(box);

const shape = box;
shape.x = 100;

console.log(shape);
```

- A: `{ x: 100, y: 20 }`
- B: `{ x: 10, y: 20 }`
- C: `{ x: 100 }`
- D: `ReferenceError`

<details markdown="1">
<summary>Answer</summary>

B

freeze 메소드를 사용하게 되면 해당 객체의 요소값을 변경하는 것이 불가능해진다. 만약 해당 객체의 요소 값이 객체라면 그 내부 요소인 객체의 값은 변경이 가능하다 왜냐하면 객체는 값이 아닌 주소값을 전달하고 있기 때문이다.
</details>


### **76. What's the output?**

```jsx
const { name: myName } = { name: 'Lydia' };

console.log(name);
```

- A: `"Lydia"`
- B: `"myName"`
- C: `undefined`
- D: `ReferenceError`

<details markdown="1">
<summary>Answer</summary>

C

구조분해 할당에서 콜론(:)을 사용하게 되면 해당 변수의 이름을 새로 지정하는 것이 된다. 즉 실제로는 변수 myName만 있을뿐 name이라는 변수는 생성된 것이 아니기 때문에 답은 C가된다.

</details>


### **77. Is this a pure function?**

```jsx
function sum(a, b) {
  return a + b;
}
```

- A: Yes
- B: No


<details markdown="1">
<summary>Answer</summary>

A

인풋 입력값이 같다면 side effect없이 늘 같은 값을 반환하는 함수로서 이는 순수함수이다.

</details>


### **78. What is the output?**

```jsx
const add = () => {
  const cache = {};
  return num => {
    if (num in cache) {
      return `From cache! ${cache[num]}`;
    } else {
      const result = num + 10;
      cache[num] = result;
      return `Calculated! ${result}`;
    }
  };
};

const addFunction = add();
console.log(addFunction(10));
console.log(addFunction(10));
console.log(addFunction(5 * 2));
```

- A: `Calculated! 20` `Calculated! 20` `Calculated! 20`
- B: `Calculated! 20` `From cache! 20` `Calculated! 20`
- C: `Calculated! 20` `From cache! 20` `From cache! 20`
- D: `Calculated! 20` `From cache! 20` `Error`



<details markdown="1">
<summary>Answer</summary>

C

addFunction 은 add 함수가 return하는 함수를 값으로 갖게 되며 이제 addFunction을 호출하는 순서에 따라 처음에는 cache객체는 빈객체지만 else문을 돌면서 10 : 20이라는 요소를 갖게된다. 이후 계속 10이라는 요소가 있기 때문에 from cache를 호출하게 되는데 이때 addFunction함수가 add에 있었던 cache라는 함수를 그대로 이어서 사용할 수 있는 이유는 모든 함수는 자신이 생성될 당시에 상위 스코프의 렉시컬 환경을 내부적으로 기억하여 자신의 상위 실행컨텍스트로 지정하기 때문에 결국 호출될 때마다 이 클로저 현상을 통해 객체 cache의 주소를 찾아내게 된다.

</details>


### **79. What is the output?**

```jsx
const myLifeSummedUp = ['☕', '💻', '🍷', '🍫'];

for (let item in myLifeSummedUp) {
  console.log(item);
}

for (let item of myLifeSummedUp) {
  console.log(item);
}
```

- A: `0` `1` `2` `3` and `"☕"` `"💻"` `"🍷"` `"🍫"`
- B: `"☕"` `"💻"` `"🍷"` `"🍫"` and `"☕"` `"💻"` `"🍷"` `"🍫"`
- C: `"☕"` `"💻"` `"🍷"` `"🍫"` and `0` `1` `2` `3`
- D: `0` `1` `2` `3` and `{0: "☕", 1: "💻", 2: "🍷", 3: "🍫"}`


<details markdown="1">
<summary>Answer</summary>

A


for in과 for of 반복문을 구분할 줄 알아야하는 문제로서 for in의 경우 반복불가능한 객체 키값등을 반복(배열의 경우 인덱스) , for of 문은 반복이 가능한 이터러블한 요소를 반복하여 출력하는 차이가 있다.

</details>


### **80. What is the output?**

```jsx
`const list = [1 + 2, 1 * 2, 1 / 2];
console.log(list);`
```

- A: `["1 + 2", "1 * 2", "1 / 2"]`
- B: `["12", 2, 0.5]`
- C: `[3, 2, 0.5]`
- D: `[1, 1, 1]`

<details markdown="1">
<summary>Answer</summary>

C

각각의 표현식이 값이 되어 C처럼 배열에 등록된다.

</details>


### **81. What is the output?**

```jsx
function sayHi(name) {
  return `Hi there, ${name}`;
}

console.log(sayHi());
```

- A: `Hi there,`
- B: `Hi there, undefined`
- C: `Hi there, null`
- D: `ReferenceError`



<details markdown="1">
<summary>Answer</summary>

B

인자로 아무것도 주어지지않으면 자동적으로 undefined가 할당되게 되어 답은 B가 된다.

</details>


### **82. What is the output?**

```jsx
var status = '😎';

setTimeout(() => {
  const status = '😍';

  const data = {
    status: '🥑',
    getStatus() {
      return this.status;
    },
  };

  console.log(data.getStatus());
  console.log(data.getStatus.call(this));
}, 0);
```

- A: `"🥑"` and `"😍"`
- B: `"🥑"` and `"😎"`
- C: `"😍"` and `"😎"`
- D: `"😎"` and `"😎"`

<details markdown="1">
<summary>Answer</summary>

B

첫번째의 호출은 data라고 하는 내부 객체가 this의 대상이 되어 초록 과일을 출력하지만 두번째 호출은 setTimeout에 인자로 주어진 화살표 함수 자체에서 this를 찾게된다. 이때 화살표 함수는 this를 가지고있지않아 스코프 체이닝을 따라 식별자를 찾게 되고 전역객체 window가 가지고 있는 status 선글라스쓴 이모티콘을 출력하게 된다.

</details>


### **83. What is the output?**

```jsx
const person = {
  name: 'Lydia',
  age: 21,
};

let city = person.city;
city = 'Amsterdam';

console.log(person);
```

- A: `{ name: "Lydia", age: 21 }`
- B: `{ name: "Lydia", age: 21, city: "Amsterdam" }`
- C: `{ name: "Lydia", age: 21, city: undefined }`
- D: `"Amsterdam"`
<details markdown="1">
<summary>Answer</summary>

A


person객체에는 city프로퍼티가 존재하지 않아 undefined를 처음에 변수 city에 담게되고 이는 다시 string타입의 ‘Amsterdam’으로 재할당된다. 결국 person객체 자체에는 어떠한 변화도 생기지 않기 때문에 그대로 출력되게 된다.

</details>


### **84. What is the output?**

```jsx
function checkAge(age) {
  if (age < 18) {
    const message = "Sorry, you're too young.";
  } else {
    const message = "Yay! You're old enough!";
  }

  return message;
}

console.log(checkAge(21));
```

- A: `"Sorry, you're too young."`
- B: `"Yay! You're old enough!"`
- C: `ReferenceError`
- D: `undefined`


<details markdown="1">
<summary>Answer</summary>

C

블록레벨 스코프인 const를 통한 변수의 생성은 블록 밖에서 해당 변수를 식별할 수 없기 때문에 선언되지 않은 변수를 return하는 것과 마찬가지여서 답은 C가 된다.

</details>


### **85. What kind of information would get logged?**

```jsx
fetch('https://www.website.com/api/user/1')
  .then(res => res.json())
  .then(res => console.log(res));
```

- A: The result of the `fetch` method.
- B: The result of the second invocation of the `fetch` method.
- C: The result of the callback in the previous `.then()`.
- D: It would always be undefined.

<details markdown="1">
<summary>Answer</summary>

C

문제의 답은 첫번째 then메소드의 리턴값을 두번째 then메소드의 res 매개변수로 받아서 출력하게 되는 형식이다.

</details>


### **86. Which option is a way to set `hasName` equal to `true`, provided you cannot pass `true` as an argument?**

```jsx
function getName(name) {
  const hasName = //
}
```

- A: `!!name`
- B: `name`
- C: `new Boolean(name)`
- D: `name.length`


<details markdown="1">
<summary>Answer</summary>

A

</details>

