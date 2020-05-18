---
title: 'JavaScript \| 객체와 프로토타입 그리고 상속'
excerpt: '프로토타입 기반 언어(prototype-based language)인 JavaScript에서 객체를 어떻게 생성하고 상속은 어떻게 받는지, 그리고 프로토타입이란 무엇인지를 알아보았습니다.'
date: 2020-05-19
last_modified_at: 2020-05-19T02:00:02

category:
 - JavaScript

tags:
 - JavaScript
 - ECMAScript
 - JS
 - ES6
 - prototype
 - class
 - object
 - instance
 - inheritance
---

JavaScript는 **프로토타입 기반(prototype-based)**의 객체 지향 프로그래밍(Object Oriented Programming; OOP)을 지원합니다.

흔히 OOP는 Java로 입문하는데 클래스 기반 언어(class-based language)인 Java에 익숙해져서 JavaScript와 같은 프로토타입 기반 언어들을 공부할 때 혼란스러운 경우가 많은 것 같습니다.

이번 글은 JavaScript에서 객체를 생성하는 방법부터 프로토타입, 상속 등을 폭 넓게 다뤄보고자 합니다.

양이 매우 길어져서 한번에 다 읽기 어려울 수도 있겠지만, 해당 주제들이 워낙 연관돼 있어서 한번에 다루는 것이 전체적인 흐름 파악에 더 도움이 될 것이라 생각했습니다.


## Shortcuts
[프로토타입이란?](#prototype)

[프로토타입 체인이란?](#prototype-chain)

[상속 예시 코드 - ES5](#retriever)

[상속 예시 코드 - ES6](#es6에서의-상속)



## 객체 생성
JavaScript에서 객체를 생성하기 위해선 다음의 세 가지 방법을 사용합니다.

1. [Object Literal](#object-literal)
1. [Constructor](#constructor)
1. [`Object.create()`](#objectcreate)

클래스 기반의 Java에서 객체를 만들기 위해선 `new` 키워드를 사용해서 인스턴스(instance)를 생성하는 것이 가장 보편적입니다. Java에서 객체라 한다면 대부분 인스턴스를 말하는 것이었고 학교에서도 OOP 수업 초반에 객체와 인스턴스의 정의를 가르칠 때 빼고는 객체와 인스턴스 혼용에 크게 개의치 않았던 걸로 기억합니다. (저는 거의 parameter와 argument 급으로 미묘한 차이를 갖고 있다고 받아들이고 있습니다.)

> **객체(Object)**는 프로그래밍에서 구현하고자 하는 사물, 개념을 component 단위로 구현화한 것이며 attributes와 behaviors를 포함합니다.
>
> **클래스(Class)**는 object를 통일된 규격으로 찍어내기 위한 일종의 설계도이며, class라는 설계도로 찍어낸 개별 object를 **인스턴스(Instance)**라고 합니다.

하지만 굳이 constructor를 거치지 않고도 객체를 생성할 수 있는 JavaScript의 문법을 보니 *객체는 클래스가 인스턴스화된 것*이라는 저의 지식에 대치되는 것 같아서 혼동이 오는데요, 한편으론 클래스가 없는 프로토타입 기반 언어에서 굳이 생성자로만 객체를 만들 필요도 없지 않나 싶습니다. 다른 방법들이 주는 편리함도 분명 있고요.



### Object Literal
**Object Initializer**라고도 불리는 **Object Literal**(객체 리터럴) 방식은 중괄호 `{}`를 이용해 객체를 생성하고 객체가 갖고있는 값들을 그 안에 적는 방식입니다. 

Java에선 attirbute 혹은 field라 부르던 객체들의 속성들을 JavaScript에선 주로 **property**라 부르는 것 같습니다. property와 property에 해당하는 값들을 `key : value` 형태로 `{}` 안에 써넣고 property 끼리는 `,`로 구분합니다.

> Object literals enable us to write code that supports lots of features yet still provide a relatively straightforward process for implementers of our code.
> No need to invoke constructors directly or maintain the correct order of arguments passed to functions.
> Object literals are also useful for unobtrusive event handling;
> they can hold the data that would otherwise be passed in function calls from HTML event handler attributes.
>
> [https://www.dyn-web.com/tutorials/object-literal/](https://www.dyn-web.com/tutorials/object-literal/)

```js
let person = {
  firstName: "Junghyuk",
  lastName: "Park",
  sex: "M",
  sayHello: function(name) {
    return "Hello, " + name + "!";
  }
}

console.log(typeof person);       // object
console.log(person.lastName);     // Park
person.sayHello("world");         // "Hello, world!"
```


### Constructor
클래스 기반 언어에서 사용하던 익숙한 그 방식입니다. `new` 키워드와 **생성자(Constructor)**를 사용하여 instance를 만들어냅니다.

```js
let emptyObj = new Object();
```

단, JavaScript에서는 **함수를 constructor로 사용**합니다. instance를 만들기 위해 정의한 함수가 아니더라도 `new`와 함께 쓰인다면 constructor처럼 쓰일 수 있다는 것입니다. 그래서 constructor와 일반 함수를 구분하기 위해 constructor는 대문자로 시작하는 것이 관례입니다.

```js
function Person(firstName, lastName, sex) {
  this.firstName = firstName;
  this.lastName = lastName;
  this.sex = sex;
  this.sayHello = function(name) {
    return "Hello, " + name + "!";
  }
}

let author = new Person("Junghyuk", "Park", "M");
console.log(typeof author);       // object
console.log(author.lastName);     // Park
author.sayHello("world");         // "Hello, world!"


// constructor가 아니더라도 'new' 키워드로 객체를 만들 수는 있습니다.
function add(x, y) {
  return x + y;
}
let obj = new add(3, 5);
JSON.stringify(obj)       // "{}"
```

이때 `this`의 바인딩에 대해서 한 가지 주의사항이 있습니다. 함수 내부의 `this`는 호출이 발생하는 순간의 Execution Context에 따라 결정됩니다. 그래서 똑같은 함수라 하더라도 function call을 할 때와 constructor call을 할 때의 this는 값이 다릅니다.

```js
function Foo() {
  console.log(this);
}

Foo();                // Window 혹은 global과 같은 전역 객체
let f = new Foo();    // Foo {}
```


### Object Literal vs. Constructor
> Both Object Literals and Instance Objects have their place and purpose in JavaScript.
> The power of Object Literals is in their lightweight syntax, ease of use, and the facility with which they allow you to create name-spaced code.
> Instance Objects are powerful because they are derived from a function, they provide private scope when they are created, and expressions can be executed on instantiation.
>
> [What is the difference between an Object Literal and an Instance Object in JavaScript ? \| Kevin Chisholm - Blog](https://blog.kevinchisholm.com/javascript/difference-between-object-literal-and-instance-object/)


객체 리터럴 방식으로 생성된 객체는 private scope를 갖지 않습니다. 모든 property 와 method 가 public 이기 때문에 dot `.` 과 bracket `[]` 으로 접근이 가능합니다.

생성자를 통해 생성된 객체는 private scope를 갖습니다. constructor 내부에서 `this`는 생성될 instance를 가리킵니다. `this`를 사용해 추가된 property와 method는 public이며 그 외에 함수 내부에서 선언된 변수와 함수는 모두 private입니다.

참고로, closure를 활용하여 객체에 선언된 private member에 접근할 수 있습니다. 이렇게 private member에 접근하는 method들을 Java에서 accessor method / mutator method 라 불렀다면 JavaScript에서는 **privilleged method**라고 부릅니다.

```js
function Person(firstName, lastName, sex) {
  this.firstName = firstName;
  this.lastName = lastName;
  this.sex = sex;
  let age = 28;   // private

  this.sayHello = function(name) {
    return "Hello, " + name + "!";
  }
  this.getAge = function() {
    return age;
  }

  function getAddress() {
    return "Suwon";
  }   // private
  this.livesIn = function() {
    return "I live in " + getAddress();
  }
}
let author = new Person("Junghyuk", "Park", "M");

author.age;             // undefined
author.getAge();        // 28
author.getAddress();    // Uncaught TypeError: author.getAddress is not a function
author.livesIn();       // "I live in Suwon"
```


### `Object.create()`
`Object.create()` 메소드는 입력받은 프로토타입 객체 및 속성을 갖는 새로운 객체를 반환합니다.

```js
let newObj = Object.create(null);
console.log(typeof newObj);       // object
```

프로토타입에 대한 설명은 바로 아래에 이어지기 때문에 `Object.create()`는 그때 더 자세히 다루겠습니다.

일단은 `Object.create()`로 새로운 객체를 생성할 수 있다는 정도까지만 이해하고 넘어가면 되겠습니다.



## Prototype
알다시피 Prototype은 '원형(原型)'이란 뜻 입니다. 

JavaScript는 **프로토타입 기반의 언어(prototype based language)**이고, JavaScript에서는 **class란 개념이 존재하지 않습니다.**

대신, prototype이 객체들의 **원형**이 되어 클래스 기반 언어의 클래스가 그랬던 것처럼 객체들이 어떤 식으로 초기화 되는지, 무엇을 상속받는지 등을 나타냅니다.

물론 JavaScript에서는 모든 것이 다 객체이며 프로토타입 또한 객체입니다. 좀 더 정확히는 어떤 객체가 다른 객체의 prototype이 된다고 할 수 있겠습니다.

이렇게 다른 객체의 프로토타입이 되는 객체들을 **프로토타입 객체(prototype object)**라 부릅니다.

결국, 클래스 기반 언어에서 클래스의 인스턴스는 객체가 되었던 것처럼, 프로토타입 기반의 언어인 JavaScript에서 (프로토타입) 객체의 인스턴스는 객체가 되는 것입니다.

JavaScript에서 모든 객체는 자신의 프로토타입을 나타내는 프로퍼티를 갖고 있습니다.

브라우저의 개발자 콘솔에서 객체에 `__proto__`라는 프로퍼티를 다들 한 번쯤 본 적이 있을 것입니다.

![01][proto in console]

콘솔에서 `__proto__`를 통해 객체의 프로토타입이 무엇인지 확인할 수 있습니다. 참고로, 객체 리터럴 방식을 통해 생성된 객체들의 프로토타입은 `Object`가 됩니다.

![02][proto detail]

`__proto__`를 통하여 객체의 프로토타입을 지정하는 일 또한 가능합니다.

```js
let a = {
  prop_a : "value_a"
}

let b = {
  prop_b : "value_b"
}

b.__proto__ = a;

b.prop_a;       // "value_a"
```

![03][b proto a]

객체 `b`의 프로토타입을 객체 `a`로 지정해줬기 때문에 `b` 안에 `prop_a`가 없어도 `a`에 있는 `prop_a`에 접근해 그 값을 가져온 것입니다.

그렇다고 함부로 `__proto__`를 사용하는 것은 지양해야 합니다.

`__proto__` 프로퍼티는 JavaScript 내부에서만 접근할 수 있는 `[[prototype]]` 프로퍼티를 외부에서도 접근하고 조작할 수 있는 일종의 **accessor property** 입니다.

좀 더 엄밀히 말하자면, JavaScript의 객체는 **internal slot**과 **internal method**라는 종류의 private한 성격을 가진 property와 method를 갖고 있습니다. 이들은 외부에 노출되지 않고 내부적으로 사용되기만 하며 그 중 하나가 객체의 프로토타입을 나타내는 `[[prototype]]`라는 프로퍼티인 것입니다.

현재의 왠만한 브라우저는 모두 `__proto__`을 지원하지만 사실 `__proto__`는 ECMAScript language specification에 없었던 비표준 기능이며 ECMAScript2015(ES6)에 와서야 웹 브라우저와의 호환을 위해 추가된 일종의 **legacy feature**입니다.

그래도 여전히 직접 `__proto__`로 프로토타입에 접근하는 것보단 `Object.getPrototypeOf()`의 사용을 권장하고 있습니다.  그리고 `Object.setPrototypeOf()`도 있지만 애초에 `[[prototype]]`의 값을 수정하는 작업은 성능 면에서 굉장히 느리기 때문에 어플리케이션의 최적화를 위해서 객체를 생성한 이후 `[[prototype]]`을 변경하는 것은 고민해 봐야할 문제입니다.

`__proto__` 와 `[[prototype]]`에 관한 정보는 아래의 링크에서 더 자세히 얻을 수 있습니다.

> [Object.prototype.\_\_proto\_\_ - JavaScript \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/proto)
>
> [What is an "internal slot" of an object in JavaScript? - Stack Overflow](https://stackoverflow.com/questions/33075262/what-is-an-internal-slot-of-an-object-in-javascript)
>
> [ECMAScript® 2021 Language Specification](https://tc39.es/ecma262/#sec-ordinary-and-exotic-objects-behaviours)



## 함수의 Prototype
JavaScript에서 함수 또한 객체라는 것은 모두 알고있을 것입니다. 때문에 함수도 `__proto__` 프로퍼티가 있으며 `Function.prototype` 객체를 프로토타입으로 삼고 있습니다. 모든 함수가 `Fuction.prototype`에 있는 `call()`, `apply()`, `bind()`를 호출할 수 있었던 건 바로 이 이유에서 입니다.

![04][function]

```js
function Foo() {}

Object.getPrototypeOf(Foo) === Function.prototype;  // true
```

여기서 `Function`은 익숙하겠지만 뒤에 붙은 `.prototype`은 다소 생소해 보입니다.

사진에서도 보이듯 **함수는** `prototype`**이란 프로퍼티를 갖고 있습니다.** `prototype`은 함수가 생성될 때 JavaScript 엔진이 함수 객체에 추가로 부여하는 프로퍼티이며 함수가 생성자로 쓰일 때 프로토타입이 되는 객체를 가리킵니다. 그러니깐 `new Foo()`로 생성된 객체의 프로토타입은 `Foo.prototype`이 되는 것입니다.

이는 Built-in Object들에게도 동일하게 적용되어 `new Object(...)`로 생성된 객체의 프로토타입은 `Object.prototype`, `new Array(...)`로 생성된 배열의 프로토타입은 `Array.prototype` 등등이 됩니다.

![05][function prototype]

```js
function Foo() {}
let foo = new Foo();

Object.getPrototypeOf(foo) === Foo.prototype;       // true
```

그런데 `Foo.prototype`을 보면 `constructor`라는 프로퍼티가 있고 이 `constructor`의 값은 `Foo`로 되어 있습니다. `constructor` 프로퍼티는 `Foo.prototype`을 프로토타입으로 갖는 객체를 만들기 위한 생성자이기 때문입니다. 모든 프로토타입 객체는 이같이 `constructor`란 프로퍼티를 갖고 있으며 `constructor` 프로퍼티에는 해당 프로토타입 객체의 인스턴스를 만들기 위한 생성자가 들어갑니다.

결국 함수를 선언하면 그 안에 `prototype` 프로퍼티가 있으며, `prototype` 프로퍼티에는 `constructor` 프로퍼티가 있고, `constructor` 프로퍼티는 맨 처음 선언한 함수가 들어가는 것입니다.

이를 그림으로 표현하면 다음과 같습니다.

![06][prototype constructor]

```js
function Foo() {}

Foo.prototype.constructor === Foo;      // true
Foo.prototype.constructor.prototype === Foo.prototype;      // true
```

여기에 생성자 `Foo()`의 프로토타입인 `Function.prototype`과 `Foo()`의 인스턴스 객체 `foo`와의 관계까지 함께 표현하면 다음과 같은 그림으로 표현할 수 있습니다.

![07][prototype]



## 상속과 Prototype Chain
상속이란 간단하게 상위 객체의 특징을 하위 객체에게 넘겨주는 것으로 OOP의 주요 특징입니다.

Java에서는 클래스를 정의할 때 `extends` 키워드를 사용함으로써 상속을 구현할 수 있습니다. 하지만 앞서 언급했듯이, 프로토타입 기반의 JavaScript는 클래스가 존재하지 않으며 당연히 클래스를 통한 상속 또한 불가능합니다.

JavaScript는 대신 **프로토타입을 활용하여 상속**을 구현합니다.

일단 코드를 보겠습니다.


### Animal
```js
function Animal(age, weight) {
  this.age = age;
  this.weight = weight;
}

Animal.prototype.cries = function() {
  return "";
}
```
`Animal`이라는 생성자를 만들어 봤습니다.

그런데 `Animal`의 메소드 `cries()`를 생성자 안에서 `this.cries = function() {...}`으로 선언하지 않고 `Animal.prototype` 객체 안에 정의하였습니다.

`this.cries = function() {...}`와 `Animal.prototype.cries = function() {...}`의 차이는 `new Animal(...)`로 생성되는 각 인스턴스 객체가 자신의 프로퍼티로 `cries()`를 직접 갖느냐 아님 `Animal.prototype`이 대신 갖느냐로 갈립니다. 왜 위 코드와 같이 프로토타입에 정의해줘야 하는지에 대한 자세한 이유는 아래의 링크에서 확인할 수 있습니다.

> [Javascript Prototype methods vs Object methods](https://veerasundar.com/blog/2014/02/javascript-prototype-methods-vs-object-methods/)

메소드와 관련해서 한 가지만 더 짚고 넘어가겠습니다. 만약 `cries()`를 static method로 선언하고 싶다면 메소드를 `Animal`에 직접 추가해줍니다.

```js
// static method
Animal.cries = function() {
  return "";
}
```


### Dog
```js
function Dog(age, weight) {
  Animal.call(this, age, weight);
}

Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

Dog.prototype.cries = function() {
  return "bark";
}

Dog.prototype.__proto__ === Animal.prototype                // true
Animal.prototype === Object.create(Animal.prototype);       // false
```

이 코드의 `Dog` 생성자가 생성한 객체는 `Animal`을 상속받습니다.

우선 생성자 함수 `Dog()` 내부에서 `this`를 바인딩한 생성자 함수 `Animal()`을 호출합니다. Java에서 자식 클래스의 생성자 안에서 부모 생성자 `super()`를 호출했던 것처럼 말입니다. 참고로 여기서 `this`는 `Dog.prototype`의 인스턴스로 생성되는 객체 자신을 의미합니다.

그리고나서 `Dog`의 프로토타입 값을 조정합니다. 

일단 함수 `Dog()`을 정의하고 나서 `Dog.prototype`에 아무것도 하지 않았을 때 `Dog.prototype` 객체의 상태는 아래와 같을 것입니다.

`Dog.prototype`
: `constructor: Dog()`
: `__proto__: Object.prototype`

<!--![08][no child prototype]-->

여기서 `Dog`과 `Animal`이 프로토타입적으로 이어져서 상속관계를 형성하기 위해 우리가 바라는 모습은 다음과 같습니다.

`Dog.prototype`
: `constructor: Dog()`
: <mark style="background-color:BlanchedAlmond"><code>__proto__: Animal.prototype</code></mark>

<!--![09][child prototype]-->

이를 `Object.create()`를 통해 수행합니다. `Object.create()`는 argument로 받은 **객체를 프로토타입으로 삼는 새로운 객체를 반환**합니다. 말이 조금 복잡한데, `Dog.prototype`은 `Animal.prototype`을 프로토타입으로 삼는 어떤 객체라는 것입니다. (`Object.create(Animal.prototype)`은 `Animal.prototype` 자체가 아닙니다.) 마찬가지로 `new Dog(...)`로 생성되는 객체들의 프로토타입은 `Animal.prototype`을 프로토타입으로 갖는 어떤 객체인 것이고 그 객체를 `Dog.prototype`에 할당했으니 `Dog.prototype`이란 레퍼런스로 접근할 수 있는 것입니다.

![10][no constructor]

그런데 `Dog.prototype = Object.create(Animal.prototype)`을 하고나면 `Dog.prototype`에는 프로토타입만 `Animal.prototype`으로 설정된 빈 객체가 할당된다는 문제가 있습니다.

`Dog.prototype`
: `__proto__: Animal.prototype`

이 상태로 `Dog.prototype.constructor`에 접근한다면 `Dog.prototype`에는 `constructor`가 없으므로 프로토타입을 따라 `Animal.prototype`에 있는 `constructor`를 가져오게 됩니다. 

따라서, 우리가 원하는 함수 객체인 

`Dog.prototype`
: `constructor: Dog()`
: `__proto__: Animal.prototype`

를 완성하기 위해 직접 `Dog.prototype.constructor`를 `Dog`으로 지정해 줍니다.


### Retriever
```js
function Retriever(age, weight) {
  Dog.call(this, age, wieght);
  this.temperament = ["intelligent". "affectionate"];
}

Retriever.prototype = Object.create(Dog.prototype);
Retriver.prototype.constructor = Retriever;

Retriever.prototype.cries = function() {
  return Dog.prototype.cries.call(this) + ": Ruff!";
}

let myDog = new Retriever(2, 70);
```

이번에는 `Dog`를 상속받는 `Retriever`를 만들어 보았습니다.

`Dog.prototype.cries()`와 `Retriever.prototype.cries()`는 각각 상속받는 프로토타입의 `cries()`를 method overriding 했습니다.


### Prototype Chain
최종적으로 위에서 정의된 `Animal`과 `Dog`과 `Retriever`의 관계를 도식화하면 다음과 같습니다.

![11][inheritance]

`Retriever.prototype`의 인스턴스인 `myDog`부터 시작해서 부모 객체의 프로토타입을 정리해봤습니다.

* `myDog`의 프로토타입 = `Retriver.prototype`
* `Retriver.prototype`의 프로토타입 = `Dog.prototype`
* `Dog.prototype`의 프로토타입 = `Animal.prototype`
* `Animal.prototype`의 프로토타입 = `Object.prototype`

상속 관계의 객체 사이에는 자식이 부모의 프로토타입을 참조하는 관계가 성립합니다.

이렇게 자식에서부터 부모에게로 타고 올라가는 프로토타입의 연결 구조를 **프로토타입 체인(Prototype Chain)**이라고 합니다.

프로토타입 체인을 따라 계속해서 올라가다보면 결국에는 모든 객체의 프로토타입인 `Object.prototype`이 나오며 이 `Object.prototype.__proto__`는 `null`을 가리키고 있습니다. 그러니 모든 프로토타입의 종점은 `null`인 셈입니다.

그리고 한 객체에서 특정 프로퍼티를 조회할 때, 객체가 그 프로퍼티를 갖고 있지 않다면 객체의 프로토타입 체인을 따라 상위 프로토타입으로 올라가며 해당 프로퍼티가 있는지 확인합니다. 이는 메소드를 호출할 때도 마찬가지 입니다. 함수, 배열 그리고 모든 객체들이 `Function`, `Array`, `Object` 등 built-in object에 정의된 메소드를 사용할 수 있는 이유는 바로 이런 메커니즘을 따르기 때문입니다.

이처럼 객체가 프로토타입 체인을 따라 연쇄적으로 프로퍼티나 메소드를 탐색하고 접근하는 것을 **프로토타입 체이닝(Prototype Chaining)**이라고 합니다.



## ES6에서의 상속
ES6에 들어서, 이전에 알던 클래스 기반 언어의 문법과 아주 유사한 형태로 상속을 구현할 수 있게 됐습니다. 

추가된 `class`, `constructor`, `static`, `extends`, `super` 키워드를 사용해 이전 장의 상속을 구현하였습니다.

```js
class Animal {
  constructor(age, weight) {
    this.age = age;
    this.weight = weight;
  }

  cries() {
    return "";
  }
}

class Dog extends Animal {
  constructor(age, weight) {
    super(age, weight);
  }

  cries() {
    return "bark";
  }
}

class Retriever extends Dog {
  constructor(age, weight) {
    super(age, weight);
    this.temperament = ["intelligent". "affectionate"];
  }

  cries() {
    return super() + ": Ruff!";
  }
}
```

그렇다고 JavaScript가 클래스 기반 프로그래밍이 가능해진 것은 아닙니다.

여전히 클래스는 없고 프로토타입으로 상속 관계를 형성합니다. 위 코드들은 단지 사용자의 편의를 위한 syntactic sugar일 뿐, 내부적으로는 생성자함수에 prototype과 constructor가 지정되고 `[[prototype]]` 값이 부여되는 등 일련의 과정이 동일하게 작동됩니다.

또한 `class`는 인스턴스 생성보다 앞서 정의해야 합니다. 생성자 함수의 경우에는 hoisting 덕분에 인스턴스를 생성하는 코드 이후에 함수를 정의했더라도 정상적으로 작동하지만 `class`는 에러를 발생시킵니다.

```js
let func = new Func();
let cls = new Cls();    // Uncaught ReferenceError: Cls is not defined

function Func() {}
class Cls {}
```



## References
### 객체 생성
> [Working with objects - JavaScript \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_Objects)
>
> [Object \| PoiemaWeb](https://poiemaweb.com/js-object)
>
> [JavaScript Object Literal](https://www.dyn-web.com/tutorials/object-literal/)  
>
> [What is the difference between an Object Literal and an Instance Object in JavaScript ? \| Kevin Chisholm - Blog](https://blog.kevinchisholm.com/javascript/difference-between-object-literal-and-instance-object/)
>
> [Details of the object model - JavaScript \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Details_of_the_object_model)

### Prototype
> [Prototype \| PoiemaWeb](https://poiemaweb.com/js-prototype)
>
> [Inheritance and the prototype chain - JavaScript \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)
>
> [Object prototypes - Learn web development \| MDN](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object_prototypes)
>
> [Object.prototype.\_\_proto\_\_ - JavaScript \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/proto)
>
> [What is an "internal slot" of an object in JavaScript? - Stack Overflow](https://stackoverflow.com/questions/33075262/what-is-an-internal-slot-of-an-object-in-javascript)
>
> [쉽게 이해하는 자바스크립트 프로토타입 체인 : TOAST Meetup](https://meetup.toast.com/posts/104)

### 함수의 Prototype
> [JavaScript Prototype and Prototype Chain explained.](https://medium.com/@chamikakasun/javascript-prototype-and-prototype-chain-explained-fdc2ec17dd04)

### 상속과 Prototype Chain
> [Javascript Prototype methods vs Object methods](https://veerasundar.com/blog/2014/02/javascript-prototype-methods-vs-object-methods/)
>
> [Object.create() - JavaScript \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create)

### ES6에서의 상속
> [Class \| PoiemaWeb](https://poiemaweb.com/es6-class)
>
> [Hoisting - 용어 사전 \| MDN](https://developer.mozilla.org/ko/docs/Glossary/Hoisting)



[proto in console]: /assets/images/2020-05-05-js-prototype/prototype01-proto-in-console.png
[proto detail]: /assets/images/2020-05-05-js-prototype/prototype02-proto-detail.png
[b proto a]: /assets/images/2020-05-05-js-prototype/prototype03-b-proto-a.png
[function]: /assets/images/2020-05-05-js-prototype/prototype04-function.png
[function prototype]: /assets/images/2020-05-05-js-prototype/prototype05-function-prototype.png
[prototype constructor]: /assets/images/2020-05-05-js-prototype/prototype06-prototype-constructor.jpg
[prototype]: /assets/images/2020-05-05-js-prototype/prototype07-prototype.jpg
[no child prototype]: /assets/images/2020-05-05-js-prototype/prototype08-no-child-prototype.png
[child prototype]: /assets/images/2020-05-05-js-prototype/prototype09-child-prototype.png
[no constructor]: /assets/images/2020-05-05-js-prototype/prototype10-no-constructor.png
[inheritance]: /assets/images/2020-05-05-js-prototype/prototype11-inheritance.png