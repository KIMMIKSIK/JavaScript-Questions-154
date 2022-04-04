# -154-

@URL :[GitHub - lydiahallie/javascript-questions: A long list of (advanced) JavaScript questions, and their explanations](https://github.com/lydiahallie/javascript-questions)

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

- A: `Lydia` and `undefined`
- B: `Lydia` and `ReferenceError`
- C: `ReferenceError` and `21`
- D: `undefined` and `ReferenceError`

>Answer : D

이 문제는 **변수의 호이스팅을 물어보고 있다.** 자바스크립트의 모든 선언문은 호이스팅을 통해 자신의 스코프의 최상단으로 이동해 런타임 전 가장 먼저 선언된다. var의 경우에는 선언과 동시에undefined로 초기화가 이루어져 출력이 가능하지만 let으로 선언된 변수의 경우 호이스팅을 통해 선언은 되어도 자신의 선언문에 도달했을 때 비로소 초기화가 이루어진다. 그 전까지는 변수에 접근하지 못하게 된다. 이때 선언되고 선언문에 도달하기전까지 접근할 수 없는 이 사각지대를 일시적 사각지대(TDZ)라고 한다. 

**2. What's the output?**

```jsx
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1);
}

for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1);
}
```

- A: `0 1 2` and `0 1 2`
- B: `0 1 2` and `3 3 3`
- C: `3 3 3` and `0 1 2`

>Answer :  C

이 문제는 **비동기로 작동하게 되는 콜백함수와 클로저에 대한 이해**와 더불어 **블록레벨 스코프, 함수레벨 스코프**에 대한 지식을 필요로 한다. 

 우선 두 반복문 모두 0.001초라는 짧은 시간을 지정해도 for문의 속도가 더 빠르기 때문에 for문이 작동하고 setTimeout함수가 뒤이어 3번을(0,1,2)실행하게 된다. (선언은 코드가 실행되며 for과 같이 되지만 실제로 동작하는 속도가 느린 것 뿐이다.)

 이때 첫번째 반복문의 경우 var로 선언한 변수 i는 함수레벨 스코프이기 때문에 바깥 전역에 변수를 선언하게 되고 반복문 실행 후 결과 3으로 남아있는 상태이다. 세번의 반복동안 생성된 화살표함수내 i는 각자 클로저를 형성하여 본인이 선언되는 시점의 i를 가리키는데 그것은 결국 전역 변수로 선언된 같은 i를 가리키는 것이다. 즉 세 화살표함수 모두 반복문을 마친 3값을 가르키게 되고 실행이 작동할 때는 이미 반복문을 모두 마친 3을 가진 i를 출력하게 된다. 

두번째의 경우 let은 블록레벨스코프로 반복문이 돌때마다 새로운 let을 생성하게 된다. 즉 화살표 함수들은 각자 자신의 선언시점의 for블록안에 선언된 각각의 변수 i를 가리키게 되어 0,1,2를 출력할 수 있게 되는 것이다. 

참조:[https://www.youtube.com/watch?v=RZ3gXcI1MZY](https://www.youtube.com/watch?v=RZ3gXcI1MZY)

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

  

- A: `20` and `62.83185307179586`
- B: `20` and `NaN`
- C: `20` and `63`
- D: `NaN` and `63`

>Answer B

이 **문제는 화살표함수에는 this가 없다는 걸 알아야 이해할 수 있다.** this는 실행 컨텍스트가 생성될 때 함께 바인딩 됩니다. 즉 함수를 호출할 때 결정이 된다는 것인데 this는 기본적으로는 전역객체를(브라우저: window, node.js: global)을 가리킵니다. 객체내 메소드의 경우 호출시 객체에 의해 호출되고 this는 객체 shape을 가리키게 됩니다. 즉 this.radius가 10이 되는 것입니다. 하지만 화살표 함수는 this가 존재하지 않고 결국 this를 찾기위해 자신의 스코프체인의 상위로 올라가게 됩니다. 결국 자신의 상위 스코프인 전역으로 가서 this를 찾게 되고 그 this가 의미하는 것은 전역객체이기 때문에 존재하지 않는 radius를 가리키게 됩니다. 따라서 2 * undefined는 NaN이 되는 것입니다. 

-참조:[https://blog.naver.com/websearch/222122306928](https://blog.naver.com/websearch/222122306928)(자바스크립트Math객체 예제)
