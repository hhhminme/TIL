[https://www.youtube.com/watch?v=36HrZHzPeuY](https://www.youtube.com/watch?v=36HrZHzPeuY)

해당 개념정리는 드림코딩 by 엘리님의 영상을 가지고 정리 했습니다. 

**es6(2015)**

es6 이후는 internelt explorer에서 지원이 안됩니다. 그래서 babel을 사용해야 합니다.

**babel이란?** 

babel은 먼저 말씀드리면 컴파일러 입니다. 하지만 이에 대해서 아래서 한번 더 설명을 드리겠습니다.

**Interpreter**

자바스크립트는 인터프리터 언어입니다. C,Java와 같이 고급언어로 쓰인 언어가 기계가 알아들을 수 있는 언어로 번역되어 실행되는 걸 컴파일러 언어라 부르고, 소스코드 자체가 수행되는 방식을 인터프리터라고 합니다. 이 둘의 가장 큰 차이는 pre-processin, 컴파일 유무라고 정리할 수 있습니다. 


**V8 엔진의 시작**

그러면 컴파일러가 왜 필요한가? 의 시작은 V8 엔진에 시작에 있습니다. javascript의 목적은 HTML을 동적으로 만드는 것에 목표가 있었지만, 이후 많은 user-interaction이 필요했고 이를 커버할 수 있는 더 좋은 엔진이 필요했습니다. 


javascript engine은 엔진 내부에서 컴파일 과정이 일어나고 다음으로 Interpreter가 코드를 읽으며 실행됩니다. 이때 코드를 수행하는 과정에서 profiler가 지켜보며 최적화할 수 있는 코드를 compiler에게 전달해줍니다. 주로 반복해서 실행되는 코드블록을 compile(Optimizer) 합니다. 

코드를 우선 interpreter 방식으로 실행하고 필요할 때 컴파일 하는 방법을 JIT(Just-In-Time) 컴파일러라고 부릅니다. 

결론은 자바스크립트는 실행되는 플랫폼에 따라 인터프리팅과 컴파일이 혼합되어 사용됩니다. 이 방식은 자바스크립트의 성능을 크게 향상 시켰습니다.

**Transpile과 Compile**

**Babel is a JavaScript compiler**

그러면 Babel은 무엇일까요? 앞전에 말씀드렸듯이 babel은 컴파일러입니다.

Babel is a toolchain that is mainly used to convert ECMAScript 2015+ code into a backwards compatible version of JavaScript in current and older browsers or environments. 

한 마디로 es6+ 문법은 구형 브라우저(internet expoler)에서 작동되지 않습니다. 그래서 babel을 통해 최신 문법을 레거시로 변환해주는 친구가 필요합니다. 그런데 Transpile은 무엇일까요?

**Compile** 

컴파일의 경우는, 한 언어로 작성된 소스 코드를 다른 언어로 변환하는 것을 의미합니다. 

Java → bytecode , C → assembly 와 같은 예가 대표적입니다.

**Transpile**

트랜스파일의 경우는 한 언어로 작성된 소스 코드를 비슷한 수준의 추상화를 거친 다른 언어로 변환하는 것을 말합니다. 예를 들어, 트랜스 파일하는 경우는 아래와 같습니다. 

- es6 코드 → es5 코드로 변환하는 경우
- c++ → c
- typescript → javascript

**결론적으로 말하면 Transpiler 는 source to source compiler라고 말할 수 있습니다. 즉 컴파일러가 가장 큰 범주에 있고 그 아래 트랜스파일러가 있다고 봐도 무방합니다.** 

**Shorthand poperty names**

With Shorthand Properties, whenever you have a variable which is **the same name as a property on an object, when constructing the object, you can omit the property name.**

예시를 보면 아래의 parameter와 property가 동일합니다. 그렇기 때문에 property의 key name을 생략해서 작성할 수 있습니다. 이는 함수에도 동일하게 적용할 수 있습니다.

```jsx
function formatMessage (name, id, avatar) {
  return {
    name: name,
    id: id,
    avatar: avatar,
    timestamp: Date.now()
		save: function () {
      // save message
    }
  }
}

function formatMessage (name, id, avatar) {
  return {
    **name,
    id,
    avatar,**
    timestamp: Date.now()
		save () {
      //save message
    }
  }
}

```

```jsx
/**
 * Shorthand property names
 * https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Object_initializer
 *
 */

{
  const ellie1 = {
    name: 'Ellie',
    age: '18',
  };

	//상단에 Object 안에 property와 동일한 key name의 property가 있다면 
	//property 의 값은 상단에 선언된 내용을 따릅니다. 
	
  const name = 'Ellie';
  const age = '15';

  // 💩
  const ellie2 = {
    name: name,
    age: age,
  };

  // ✨
  const ellie3 = {
    name,
    age,
  };

  console.log(ellie1, ellie2, ellie3);
  console.clear();
}
```

```jsx
{name: 'Ellie', age: '18'} {name: 'Ellie', age: '15'} {name: 'Ellie', age: '15'}
```

**Destructuring assignment(구조 분해 할당)**

The **destructuring assignment** syntax is a JavaScript expression that makes it possible to unpack values from arrays, or properties from objects, into distinct variables.

```jsx
/**
 * Destructuring Assignment
 * https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment
 *
 */
{
  // object
  const student = {
    name: 'Anna',
    level: 1,
  };

  // 💩
  {
    const name = student.name;
    const level = student.level;
    console.log(name, level);
  }

  // ✨
  {
    const { name, level } = student;
    console.log(name, level); // Anna, 1

    const { name: studentName, level: studentLevel } = student;
    console.log(studentName, studentLevel); //변수 명을 다르게 해줬으니 console도 다르게 찍어줘야 합니다.
  }

  // array
  const animals = ['🐶', '😽'];

  // 💩
  {
    const first = animals[0];
    const second = animals[1];
    console.log(first, second);
  }

  // ✨
  {
    const [first, second] = animals; //array도 동일하게 적용할 수 있습니다. 
    console.log(first, second);
  }
  console.clear();
}
```

배열이 선언 되었으나 green, blue에는 참조할 수 있는 값이 없으므로 undefined가 뜹니다. 이럴땐 Default value를 사용할 수 있습니다. 

```jsx
const foo = ['one', 'two'];

const [red, yellow, green, blue] = foo;
console.log(red); // "one"
console.log(yellow); // "two"
console.log(green); // undefined
console.log(blue);  //undefined

const foo = ['one', 'two'];

const [red, yellow, green="three", blue="four"] = foo;
console.log(red); // "one"
console.log(yellow); // "two"
console.log(green); // "three"
console.log(blue);  // "four"
```

**Spread syntax**

**Spread syntax** (`...`) allows an iterable such as an array expression or string to be expanded in places where zero or more arguments (for function calls) or elements (for array literals) are expected, or an object expression to be expanded in places where zero or more key-value pairs (for object literals) are expected.

**Spread syntax can be used when all elements from an object or array need to be included in a list of some kind**

```jsx
/**
 * Spread Syntax
 * https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax
 *
 */
{
  const obj1 = { key: 'key1' };
  const obj2 = { key: 'key2' };
  const array = [obj1, obj2];

  // array copy
	// 복사된 arrayCopy와 array는 동일한 객체를 참조하고 있기 때문에 같은 값이 출력됩니다.
  const arrayCopy = [...array];
  console.log(array, arrayCopy);

  const arrayCopy2 = [...array, { key: 'key3' }];
  obj1.key = 'newKey';
  console.log(array, arrayCopy, arrayCopy2);

  // object copy
  const obj3 = { ...obj1 };
  console.log(obj3);

  // array concatenation
  const fruits1 = ['🍑', '🍓'];
  const fruits2 = ['🍌', '🥝'];
  const fruits = [...fruits1, ...fruits2];
  console.log(fruits); //['🍑', '🍓', '🍌', '🥝']

  // object merge
  const dog1 = { dog: '🐕' };
  const dog2 = { dog: '🐶' };
  const dog = { ...dog1, ...dog2 }; // 동일한 key 값이 존재한다면 뒤에 있는 value가 앞에 덮어 씌워 집니다.
  console.log(dog); // 🐶
  console.clear();
}
```

**Default parameters**

```jsx
/**
 * Default parameters
 * https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/Default_parameters
 */
{
  // 💩
  {
    function printMessage(message) {
      if (message == null) {
        message = 'default message';
      }
      console.log(message);
    }

    printMessage('hello');
    printMessage();
  }

  // ✨
  {
    function printMessage(message = 'default message') { //파라메터의 기본 값을 지정하여 null값을 체크하지 않아도 됩니다.
      console.log(message);
    }

    printMessage('hello');
    printMessage(); //default message
  }
  console.clear();
}
```

**Conditional(Ternary) Operator**

The **conditional (ternary) operator** is the only JavaScript operator that takes three operands: a condition followed by a question mark (`?`), then an expression to execute if the condition is [truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) followed by a colon (`:`), and finally the expression to execute if the condition is [falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy). This operator is frequently used as an alternative to an `[if...else](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else)` statement.

**이거 맞아 ? 응 : 아니** 라고 생각하고 생각하면 편하다.

```jsx
/**
 * Ternary Operator
 * https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Conditional_Operator
 */
{
  const isCat = true;
  // 💩
  {
    let component;
    if (isCat) {
      component = '😸';
    } else {
      component = '🐶';
    }
    console.log(component);
  }

  // ✨
  {
    const component = isCat ? '😸' : '🐶';
    console.log(component);
    console.log(isCat ? '😸' : '🐶');
  }
  console.clear();
}
```

이를 이용해서 Handling null values를 해줄 수 있다.

```jsx
let greeting = person => {
    let name = person ? person.name : `stranger`return `Howdy, ${name}`}

console.log(greeting({name: `Alice`}));  // "Howdy, Alice"
console.log(greeting(null));
```

**Template Literals**

불편하게 + 로 문자열을 표현할 것 없이 `(벡틱) 안에 ${}안에 변수를 넣어 표현할 수 있다.

```jsx
/**
 * Template Literals
 * https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Template_literals
 */
{
  const weather = '🌤';
  const temparature = '16°C';

  // 💩
  console.log(
    'Today weather is ' + weather + ' and temparature is ' + temparature + '.'
  );

  // ✨
  
  console.log(`Today weather is ${weather} and temparature is ${temparature}.`);
```

**es11(2020)**

**Optional chaining**

The **optional chaining** operator (**`?.`**) enables you to read the value of a property located deep within a chain of connected objects without having to check that each reference in the chain is valid.

The `?.` operator is like the `.` chaining operator, except that instead of causing an error if a reference is [nullish](https://developer.mozilla.org/en-US/docs/Glossary/Nullish) (`[null](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/null)` or `[undefined](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined)`), the expression short-circuits with a return value of `undefined`. When used with function calls, it returns `undefined` if the given function does not exist.

This results in shorter and simpler expressions when accessing chained properties when the possibility exists that a reference may be missing. It can also be helpful while exploring the content of an object when there's no known guarantee as to which properties are required.

Optional chaining cannot be used on a non-declared root object, but can be used with an undefined root object.

**The optional chaining operator provides a way to simplify accessing values through connected objects when it's possible that a reference or function may be `undefined` or `null`.**

`let nestedProp = obj.first?.second;` 는 풀어 쓰게 되면 아래와 동일한 의미를 갖는다.

```jsx
let temp = obj.first;
let nestedProp = ((temp === null || temp === undefined) ? undefined : temp.second);
```

```jsx
/**
 * 관련 강의 영상: https://youtu.be/36HrZHzPeuY
 */
/**
 * Optional Chaining (ES11)
 * https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Optional_chaining
 */
{
  const person1 = {
    name: 'Ellie',
    job: {
      title: 'S/W Engineer',
      manager: {
        name: 'Bob',
      },
    },
  };
  const person2 = {
    name: 'Bob',
  };

  // 💩💩💩💩💩💩
  {
    function printManager(person) {
      console.log(person.job.manager.name);
    }
    printManager(person1);
    // printManager(person2); 매니저가 존재하지 않아 문제가 발생
  }

  // 💩💩💩
	//Ternary Operation 을 이용
	//코드의 반복이 존재하기 때문에 불편함.
  {
    function printManager(person) {
      console.log(
        person.job
          ? person.job.manager
            ? person.job.manager.name
            : undefined
          : undefined
      );
    }
    printManager(person1);
    printManager(person2);
  }

  // 💩
	// 이또한 코드의 반복이 계속적으로 반복되어서 보기 안좋다.
  {
    function printManager(person) {
      console.log(person.job && person.job.manager && person.job.manager.name);
    }
    printManager(person1);
    printManager(person2);
  }

  // ✨
	//optional Chaining
  {
    function printManager(person) {
      console.log(person.job?.manager?.name);
    }
    printManager(person1);
    printManager(person2);
  }
  console.clear();
}
```

**Nullish coalescing operator(??)**

The **nullish coalescing operator (`??`)** is a logical operator that returns its right-hand side operand when its left-hand side operand is `[null](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/null)` or `[undefined](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined)`, and otherwise returns its left-hand side operand.

Logical OR operator 에서 false 는 아래의 것들이 포함된다. 

- `null`;
- `NaN`;
- `0`;
- empty string (`""` or `''` or ````);
- `undefined`.

```jsx
const foo = null ?? 'default string';
console.log(foo);
// expected output: "default string"

const baz = 0 ?? 42;
console.log(baz);
// expected output: 0
```

```jsx
/**
 * Nullish Coalescing Operator (ES11)
 * https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator
 */
{
  // Logical OR operator
  // false: false, '', 0, null, undefined
  {
    const name = 'Ellie';
    const userName = name || 'Guest';
    console.log(userName);
  }

  {
    const name = null;
    const userName = name || 'Guest';
    console.log(userName);
  }

  // 💩
  {
    const name = ''; //false
    const userName = name || 'Guest';
    console.log(userName); //Guest

    const num = 0; //false로 간주
    const message = num || 'undefined';
    console.log(message); //undefined
  }

  // ✨
  {
    const name = '';
    const userName = name ?? 'Guest'; //이름에 이름이 있다면 이름을 쓰고 없다면 Guest를 쓰자.
    console.log(userName);

    const num = 0;
    const message = num ?? 'undefined';
    console.log(message);
  }
}
```
