# 콜백 함수

1. 콜백 함수(callback function)란

   - 다른 코드의 인자로 넘겨주는 함수(인자로 넘겨줌으로써 제어권도 위임)

2. 제어권

   - 콜백 함수의 제어권을 넘겨받은 코드는 콜백 함수 호출 시점에 대한 제어권을 갖는다.

   ```javascript
   var count = 0;
   var callbackFunc = function () {
     console.log(count);
     if (++count > 10) clearInterval(timer);
   }
   var timer = setInterval(callbackFunc, 500);
   ```

3. 인자

   - 콜백 함수의 제어권을 넘겨받은 코드는 콜백 함수를 호출할 때 인자에 어떤 값들을 어떤 순서로 넘길 것인지에 대한 제어권을 갖는다.

4. this

   - 기본적으로 this는 전역객체를 참조한다. (단, 제어권을 넘겨받을 코드에서 콜백 함수에 별도로 this가 될 대상을 지정한 경우 제외)
   - 콜백 함수 내부의 this에 다른 값 바인딩
     - this를 변수에 담아 this 대신 변수를 사용
     - bind 메서드 사용

5. 콜백 "함수"

   - 콜백 함수로 어떤 객체의 메서드를 전달하더라도 그 메서드는 메서드가 아닌 함수로서 호출된다.

   ```javascript
   var obj = {
     vals: [1, 2, 3],
     logVals: function(v, i) {
       console.log(this, v, i);
     }
   };
   obj.logVals(1, 2);
   [7, 8, 9].forEach(obj.logVals);
   ```

6. 콜백 지옥(callback hell) & 비동기(asynchronous) 제어
   - 콜백 지옥은 콜백 함수를 익명 함수로 전달하는 과정이 반복되어 코드의 들여쓰기 수준이 감당하기 힘들 정도로 깊어지는 현상이다.
   - 비동기적인 일련의 작업을 동기적으로, 혹은 동기적인 것처럼 보이게끔 제어하기 위해 ES6에서 Promise, Generator ES2017에서 async/await이 도입되었다.
