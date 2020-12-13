# 클래스

1. 클래스(class)와 인스턴스(instance)
   - 클래스: 어떤 사물의 공통 속성을 모아 정의한 추상적인 개념
   - 인스턴스: 클래스의 속성을 지니는 구체적인 예시

2. 자바스크립트의 클래스

   - 생성자 함수를 new 연산자와 함께 호출하면 인스턴스가 생성된다. ex) var a = new Array()
   - 이때 Array를 일종의 클래스라고 하면, Array의 prototype 객체 내부 요소들이 인스턴스에 상속된다고 볼 수 있다.(프로토타입 체이닝에 의한 결과 => prototype 프로퍼티를 제외한 나머지는 인스턴스에 상속되지 않음)
   - 인스턴스에 상속되는지 여부에 따라 스태틱 멤버(static member, 인스턴스가 직접 호출 불가)와 인스턴스 멤버(instance member)로 나뉜다.

   ```javascript
   var Rectangle = function (width, height) {
     this.width = width;
     this.height = height;
   };
   Rectangle.prototype.getArea = function () {
     return this.width * this.height;
   };
   Rectangle.isRectangle = function (instance) {
     return instance instanceof Rectangle && instance.width > 0 && instance.height > 0;
   };

   var a = new Rectangle(5, 6)
   console.log(a.getArea());
   console.log(a.isRectangle(a)); // error
   console.log(Rectangle.isRectangle(a));
   ```

3. ES5와 ES6의 클래스

   ```javascript
   var ES5 = function (name) {
     this.name = name;
   };
   ES5.staticMethod = function () {
     return this.name + ' staticMethod';
   };
   ES5.prototype.method = function () {
     return this.name + ' method';
   };
   var es5Instance = new ES5('es5');
   console.log(ES5.staticMethod());
   console.log(es5Instance.method());

   var ES6 = class {
     constructor (name) {
       this.name = name;
     }
     static staticMethod () {
       return this.name + ' staticMethod';
     }
     method () {
       return this.name + ' method';
     }
   };
   var es6Instance = new ES6('es6');
   console.log(ES6.staticMethod());
   console.log(es6Instance.method());
   ```

