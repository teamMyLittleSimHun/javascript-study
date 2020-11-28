# This

1. This In Javascript
   1. 일반적인 객체 지향 언어에서 사용하는 경우
      1.  *this*는 인스턴스를 가르킴
      2.  클래스 에서 사용 가능함
   2. 자바스크립트의 경우 
      1. 모든 곳에서 사용 가능함
      2. 상황에 따라 *this*의 대상이 다름

## 상황에 따른 this

* *this*는 실행 컨텍스트가 생성될때 결정됨 = 함수가 호출할때 결정
* 호출의 주체가 들어감

### 전역공간에서의 this
 
* 전역 공간에서 *this*는 전역 객체를 가르킴
  * 브라우저 환경에서는 `window` 
  * Node 환경에서는 `global`  

#### 참고 ####
* 자바스크립트의 모든 변수는 특정 객체의 프로퍼티로 작동함
* `var`로 선언된 변수는 글로벌 객체의 프로퍼티임
* [확인](https://jsfiddle.net/acidf0x/qnay75ze/1/)
* [delete](https://jsfiddle.net/acidf0x/9dsb2rh0/5/)
* 함수로써 호출됬냐 메서드로 호출됬느냐는 함수앞에 점(.) 으로 부분이 가능함
* (대괄호 표기법은 메서드로 호출)

### 메서드로 호출할때 메서드 내부의 this
* 메서드 내부에서 호출시
* 우리가 생각하는 대로 작동함  
[참고](https://jsfiddle.net/acidf0x/L13nqs2u/5/)

### 함수로서 호출할 때 내부에서의 this

* 먼저 아래의 코드에서 this가 가르키는 대상은 무엇일까?
```javascript
var obj1 = {
    outer: function () {
        console.log(this);  //? (1)
        var innerFunc = () => {
            console.log(this);
        ;}
        innerFunc(); //? (2)
        var obj2 = {
            innerMethod: innerFunc
        };
        obj2.innerMethod(); //? (3)
    }
};
obj1.outer();
```

> 참고 부분을 다시 한번 확인해보자


```
this 바인딩에 관하여 함수를 실행 하는 당시의 환경은 중요하지 않고  
호출하는 구문이 중요하다는것을 알 수 있다.
``` 

#### 메서드 내부 함수에서 this를 우회하는법

1. this를 저장하기
2. 화살표 함수 사용하기

#### 콜백 함수 호출시 그 함수 내부의 this
* 콜백 함수도 함수이므로 기본적으로 *this*가 전역객체를 참조함
* 제어권을 받은 함수에서 콜백 함수에 별도로 *this*를 지정하는 경우도 있음

```javascript
setTimeout(function () {
	console.log(this); //?
}, 300);

[1, 2, 3, 4, 5].forEach(function (x) {
  console.log(this, x); //?
});

document.body.innerHtml = '<button id="a">클릭</button>';
document.body.querySelector('#a').addEventListener('click', function (e) {
	console.log(this); //?
});
```


 정답 : 전역객체, 전역객체, 클릭된 버튼

## 명시적으로 바인딩 하기
* 위의 규칙을 무시하고 명시적으로 this를 바인딩 하는 방법이 존재함
  
### Call Method
* call 메서드는 메서드의 호출 주체인 함수를 즉시 실행 하도록 하는 명령
* 첫번째 인자로 this를 바인딩 하고 이후 인자는 호출한 함수의 매게변수로 사용됨

```javascript
var func = function (a, b, c) {
  console.log(this, a, b, c);
};

fun(1,2,3);  //?
func.call({x: 1}, 4, 5, 6); //?
```

### Apply Method
* Call 메서드와 동일한 기능을 하는 함수
* 호출할 함수에 매개 변수를 전달하는 스타일 만 다름

```javascript
var func = function (a, b, c) {
  console.log(this, a, b, c);
};

func.apply({x: 1}, {4, 5, 6});
```

## Call / Apply 메서드의 활용

### 유사배열객체에 배열 메서드 적용

* 유사배열 객체는 배열처럼 사용하지만 배열 메서드를 활용할수가없다.  
  [유사배열 객체란?](https://www.zerocho.com/category/JavaScript/post/5af6f9e707d77a001bb579d2)

```javascript
var obj = {
  0: 'a',
  1: 'b',
  2: 'c',
  length: 3
}

Array.prototype.push.call(obj, 'b');
console.log(obj);

var arr = Array.prototype.slice.call(obj);
console.log(arr);
```

* 다른 배열 메서드들을 사용 가능하지만 에러가 발생하거나 정상작동 안하는 경우가 있음
* ES6에서는 유사배열객체를 배열로 쉽게 전환해주는 `Array.from`메서드가 도입됨

### 생성자 내부에서 다른 생성자를 호출
```javascript
function Person(name, gender) {
  this.name = name;
  this.gender = gender;
}

function Student(name, gender, school)
{
  Person.call(this, name, gender);
  this.school = school;
}

function Employee(name, gender, company)
{
   Person.apply(this, [name, gender]);
   this.company = company;
}

var by = new Student('보영', 'female', '단국대');
var jn = new Employee('재난', 'male', ' 구골');
```

### 여러 인수를 묶어서 하나의 배열로 전달하고 싶을때
* ES6에서는 펼치기 연산자로 사용기 가능하다.
```javascript
var numbers = [1, 2, 3, 4, 5];
console.log(Math.max.apply(null, numbers))
console.log(Math.max(... numbers))
```

### Bind 메서드
- ES5에서 추가된 기능
- `call`과 유사하지만 즉시호출되지않음
- `name`프로퍼티에 자동으로 `bound`가 붙음  
  
[name](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/name)

```javascript
var func = function() {
  console.log(this);
}

func();

var bindFunc1 = func.bind({x: 1});
bindFunc1();

console.log(func.name);
console.log(bindFunc1.name);
```

### 화살표 함수
- ES6의 추가됨
- `this`를 바인딩 하지 않으므로 가장 가까운 `this`가 출력됨

