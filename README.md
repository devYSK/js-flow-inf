# js-flow-inf
인프런 Javascript 핵심 개념 알아보기 - JS Flow 정재남님 강의


## 기본형과 참조형의 종류 및 차이점

* var a; // 선언과정
    * a = 10; // 할당 과정

* var b = 'abc'; // 선언 및 할당과정 
    * b = false; // 재할당 과정(덮어쓰기)

* var c = b; // 기본형(primitive type)이기 때문에 새 메모리에 b값(false)를 저장한다. b의 주소를 참조하는것이 아님. 
    * c = 20; // false를 20으로 덮어씌움.




* 참조형 데이터
```javascript
var obj = {
    a: 1,
    b: 'b'
};

```

![](/images/2021-08-04-18-10-04.png)

* obj2가 obj의 주소를 참조한다.
    * obj2.a의 값을 변경하면, obj가 참조하는 .a의 값도 바뀌게 된다

* obj === obj2이다.


* 참조형 데이터 안에 또 참조형 데이터가 있는 경우를 `nested`하다고 한다.

![](/images/2021-08-04-18-13-14.png)

* 만약 여기서 obj3.a = 'new'; 라는 새 데이터를 대입하게 되면, 링크가 끊기게 된다.

![](/images/2021-08-04-18-14-16.png)

* 가비지컬렉터가 1326, 1327, 1328 을 수거해서가서 초기화한다.

---


## 함수 - Hoisting(끌어올리다)
변수 '선언'과 함수 '선언'을 끌어올린다. 

![](/images/2021-08-04-18-16-00.png)

* 언뜻 보기에 선언 전에 호출함으로써 오류를 뿜어낼 것 같지만,   
  자바스크립트 엔진이 오른쪽 박스처럼 함수를 먼저 끌어 올려서, 오류를 없앤다.

--- 

## 함수선언문과 함수표현식
표현방식이 각각 다르다.


![](/images/2021-08-04-18-18-03.png)

* 기명 함수표현식처럼 이름을 쓰게 되면 오류가 났을 때 bb()함수라는것을 알 수 있어 디버깅 할때 용이하다.

* 하지만 최근에 브라우저는 함수명이 비어있을 경우 자동으로 변수명을 네임 프로퍼티에 할당해서 기명함수 표현식을 잘 사용하지 않는다.

* 익명함수표현식이 선언되고 정의되는 방식의 순서
    * 1. 변수 c선언
    * 2. 익명함수 선언
    * 3. 변수 c에 익명함수 할당 

* `함수 선언문과 함수 표현식의 차이는 할당 하는것과 할당 하지 않는것`

* 할당을 하지 않으면 코드 전체가 호이스팅(위 예제처럼 끌어올리는것)의 대상이 되고,  
  할당을 하면 변수만 호이스팅 대상이 되어 함수는 밑에자리에 그대로 있고, 변수만 위로 호이스팅 된다.

* 함수 선언문보다는 무조건 표현식을 사용하는것이 좋다고 한다. 

---

### 함수 스코프, 실행컨텍스트 

* 스코프(scope) : 유효범위(변수)
    * 스코프는 함수가 `정의`될 때 결정된다.

* 실행 컨텍스트(execution context) : 실행되는 코드 덩어리(추상적 개념)
    * 실행컨텍스트는 함수가 `실행`될 때 결정된다.
    * 실행 컨텍스트에는 `호이스팅`, `this 바인딩` 등의 정보가 담긴다.
    * 즉, 사용자가 함수를 호출했을때 내부적으로 해당 함수를 실행하기 위해 정보를 모아놓은 집합체.


* 이 코드를 해석해보자 

```javascript
var a = 1;
function outer() {
    console.log(a);

    function inner() {
        console.log(a);
        var a = 3;
    }

    inner();

    console.log(a);
}

outer();
console.log(a);

/*
 * 예상 출력 : 1, 1, 1, 1  
 */
```

* 실제 실행되는 순서

![](/images/2021-08-04-18-34-01.png)

* 실제 출력 순서 : 1, undefined, 1, 1


![](/images/2021-08-04-18-39-38.png)



---
## 메소드
메소드의 형태는 객체.함수(); 
  * ex obj.sum();

함수와 메소드의 차이점 
  * this에 obj 바인딩 하느냐 안하느냐. 바인딩 하면 메소드 
  * obj.b()를 실행하면 this 는 obj를 의미 

![](/images/2021-08-04-18-48-16.png)


---

## 콜백함수
call / back : 뒤로 부르다


![](/images/2021-08-04-18-53-52.png)

* 콜백함수의 제어권을 setInterval 함수로 넘겨준것. 

### 콜백함수의 특징

* 다른 함수(A)의 매개변수로 콜백함수 (B)를 전달하면, A가 B의 제어권을 갖게 된다.

* 특별한 요청(bind)이 없는 한 A에 미리 정해진 방식에 따라 B를 호출한다.

* 미리 정해진 방식이란?
    * this에 무엇을 바인딩할지
    * 매개변수에는 어떤값들을 지정할지,
    * 어떤 타이밍에 콜백을 호출할지 등이다. 



---

## this

* 전역공간에서
    * 전역 객체 , window, global
    * ![](/images/2021-08-05-10-16-21.png)

* 함수 내부에서
    * window
    * 기본적으로 전역객체를 가르킴. 디폴트 : 전역객체

* 메소드 호출시
    * 메소드 호출한 주체(메소드명 앞)
    * a.b() -> a객체가 주체
    * a.b.c() -> a.b객체가 주체
    * 함수는 전역객체의 메소드라고 생각하면 편하다. 
    * 내부함수에서의 우회방법 
        * ![](/images/2021-08-05-10-18-55.png)
        * c의 주체는 b인데 b가 a가 없으므로 a를 찾아 호출한다.
        * ![](/images/2021-08-05-10-20-16.png)
            * 이런식으로 우회해서 스코프를 정할 수 있다.

        
* callback에서
    * 기본적으로 함수내부에서와 동일
    * callback함수에서의 this는 기본적으로 전역객체. 
    * bind함수를 사용하면 this를 다른 객체로 지정가능 
    * ## 정리
        * 제어권을 가진 함수가 callback의 this를 명시한 경우 그에 따른다.
        * 개발자가 this를 바인딩한 채로 callback을 넘기면 그에 따른다.

## `!! call, apply, bind 메소드에 대하여`
* ![](/images/2021-08-05-10-22-56.png)
    * call과 apply의 차이는 두번째부터 쭉 나열해서 매개변수들을 받느냐, 아니면 두번째 인자 하나에 매개변수를 배열로 합쳐서 받느냐 이 차이 
    * call과 apply는 즉시 호출하지만, bind는 새로운 함수를 생성하지 호출하지는 않는다. 
   
 

* 생성자함수에서
    * 인스턴스를 가리킴.


---

## 클로저

> A closure is the combination of a function and the lexical environment within which that function was declared.
* lexical environment : 선언 당시의 환경에 대한 정보를 담는 객체(구성 환경)
* 함수가 그 함수가 선언될 당시의 환경 정보 사이의 조합
    * 즉 스코프와 관련되있다. 

* ## `함수 내부에서 생성한 데이터와 그 유효범위로 인해 발생하는 특수한 현상/상태`
    * 클로저란, 이미 생명주기가 끝난 외부함수의 변수를 참조하는 함수 라고도 할 수 있다.

* 클로저 이점
    * 접근 권한 제어

    * 지역 변수 보호

    * 데이터 보존 및 활용

---


## 지역변수 만들기 (클로저로 private member 만들기)

![](/images/2021-08-05-10-42-28.png)
* 왼쪽의 안전하지 않은 객체를 오른쪽 방법으로 안전하게 수정.
    * 바뀐 코드를 잘 보자. return 하고 안하고 차이가 있따.


* 클로저를 이용해서 private 멤버와 public 멤버를 구분하는 방법
    1. 함수에서 지역변수 및 내부함수 등을 생성한다.
    2. 외부에 노출시키고자 하는 멤버들로 구성된 객체를 return한다.
        * return한 객체에 포함되지 않은 멤버들은 private 하다.
        * return한 객체에 포함된 멤버들은 pubiic 하다.

---

## prototype과 constructor, __prote__

* ![](/images/2021-08-05-10-53-11.png)
* 생성자를 new 키워드를 이용하여 인스턴스를 생성하면, 생성자의 prototype 프로퍼티가 인스턴스의 __proto__로 전달이 된다.
    * 즉 생성자 함수의 prototyper과 인스턴스의 __proto__는 같은 객체를 `참조` 한다.
* __proto__는 생략이 가능하다.

* ![](/images/2021-08-05-10-58-48.png)

---

## 메소드 상속 및 동작 원리

```javascript
function Person(n, a) {
    this.name = n;
    this.age = a;
}

Person.prototype.setOlder = function() {
    this.age += 1;
}

Person.prototype.getAge = function() {
    return this.age;
}

var gomu = new Person('고무곰', 30);
var iu = new Person('아이유', 25);

iu.__proto__.setOlder(); // 이러면 NaN이 출력된다. 메서드들의 this가 iu가 아닌 iu.__proto__이기 때문


// 그러나 __proto__는 생략이 가능하다
iu.setOlder(); // 이러면 this가 iu를 가리키기 때문에 사용이 가능하다. 

```

---

## prototype static 메소드 및 static 프로퍼티

![](/images/2021-08-05-11-48-46.png)

![](/images/2021-08-05-11-56-48.png)
* 인스턴스에서는 prototype method에는 다이렉트로 접근 가능하지만, 생성자 함수 내부에 있는 스태틱 메소드나 프로퍼티는 접근이 불가하다.

```javascript
function Person(name, age) {
    this._name = name;
    this._age = age;
}

// static method
Person.getInformations = function(instance) {
    return {
        name : instance._name,
        age : instance._age
    };
}


// (prototype) method
Person.prototype.getName = function() {
    return this._name;
}

// (prototype) method
Person.prototype.getAge = function() {
    return this._age;
}

```

![](/images/2021-08-05-12-01-13.png)




### class 상속 구현

![](/images/2021-08-05-12-04-06.png)

* Employee 클래스가 Person 클래스를 상속 받으려면?

* ![](/images/2021-08-05-12-05-58.png)
    * 하위 클래스의 프로토타입을 상위 클래스로 인스턴스화 한다. 
    * 하위 클래스의 프로토타입의 컨스트럭터를 하위클래스로 

```javascript

function Person(name, age) {
    this.name = name || '이름없음';
    this.age = age || '나이모름';
}

Person.prototype.getName = function() {
    return this.name;
}

Person.prototype.getAge = function() {
    return this.age;
}

function Employee(name, age, position) {
    this.name = name || '이름없음';
    this.age = age || '나이모름';
    this.position = position || '직책모름';    
}

Employee.prototype = new Person();
Employee.prototype.constructor = Employee;
Employee.prototype.getPosition = function() {
    return this.position;
}

```

* 그러나 속성을 뜯어보면 다음과 같은 문제가 발생한다.

* ![](/images/2021-08-05-12-14-59.png)
    * gomu.__proto__ 와 같은 속성에 age 와 name같은 속성(프로퍼티)가 있는것이 마땅하지 않다.
    * 불필요한 프로퍼티들을 제거해야 한다.

* 반드시 Person(상위클래스)의 인스턴스가 필요한 것이 아니다. 
    * `Person(상위클래스).prototype을 상속받는 별도의 인스턴스가 있고, 그 객체에는 아무런 프로퍼티가 존재하지 않게 하면 된다.`

* 이런 방식으로 해결
    * ![](/images/2021-08-05-12-20-55.png)


```javascript

function Person(name, age) {
    this.name = name || '이름없음';
    this.age = age || '나이모름';
}

Person.prototype.getName = function() {
    return this.name;
}

Person.prototype.getAge = function() {
    return this.age;
}

function Employee(name, age, position) {
    this.name = name || '이름없음';
    this.age = age || '나이모름';
    this.position = position || '직책모름';    
}

function Bridge() {}

Bridge.prototype = Person.prototype; // 이부분 변경 

Employee.prototype = new Bridge(); // 이부분 변경
Employee.prototype.constructor = Employee;

Employee.prototype.getPosition = function() {
    return this.position;
}


// 이런식으로 함수화해서 사용도 가능하다!!!
// 클로저 함수를 이용한것 
var extendClass = (function() {
    function Bridge(){}

    return function(Parent, Child) {
        Bridge.prototype = Parent.prototype;
        Child.prototype = new Bridge();
        Child.prototype.constructor = Child;
    }
});


// 다음과 같이 name, age 프로퍼티를 구현할 수도 있다.

function Person(name, age) {
    this.name = name || '이름없음';
    this.age = age || '나이모름';
}

function Employee(name, age, position) {
    this.superClass(name, age);
    this.position = position || '직책 모름';
}


var extendClass = (function() {
    function Bridge(){}

    return function(Parent, Child) {
        Bridge.prototype = Parent.prototype;
        Child.prototype = new Bridge();
        Child.prototype.constructor = Child;
        Child.prototype.superClass = Parent;
   }
});

extendClass(Person, Employee);




```

* `## 그러나 es6에서는 내장 명령어만으로 다음과 같이 쉽게 구현할 수 있다.`

```javascript

class Person {
    constructor(name, age) {
        this.name = name || '이름없음';
        this.age = age || '나이모름';
    }

    getName() {
        return this.name;
    }

    getAge() {
        return this.age;
    }

}

class Employee extends Person {
    constructor(name, age, position) {
        super(name, age);
        this.position = position || '직책모름';
    }

    getPosition() {
        return this.position;
    }
}


```

* 참조형 데이터가 어떤식으로 변수를 저장하는지, 알 수 있따. 


---
## 데이터타입

* 자바스크립트의 두 데이터타입의 차이

* Primitive Type(기본형타입)
  * Number
  * String
  * Boolean
  * null
  * undefined

* Reference Type(참조형 타입)
  * Object
    * Array
    * Function
    * RegExp(정규표현식)
    * ... 이외 등 






   