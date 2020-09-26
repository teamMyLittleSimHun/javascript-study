# 데이터 타입

1. 데이터 타입의 종류
   - Primitive type
     - Number, String, Boolean, null, undefined, Symbol(ES6~)
   - Reference type
     - Object
       - Array, Function, Date, RegExp, Map, WeakMap, Set, WeakSet



2. 식별자(identifier)와 변수(variable)
   - 변수: 변할 수 있는 데이터
   - 식별자: 어떤 데이터를 식별하는 데 사용하는 이름(변수명)



3. 변수 선언과 데이터 할당

   var foo;

   foo = 'bar';

   1. 변수 영역에서 빈 공간 확보
   2. 확보한 공간의 식별자를 foo로 지정
   3. 데이터 영역에서 빈 공간에 문자열 bar를 저장
   4. 변수 영역에서 foo라는 식별자를 검색
   5. 3에서 저장한 공간의 주소를 1에서 만든 공간에 대입



4. 기본형 데이터와 참조형 데이터
   1. 불변값
      - 기본형 데이터: Number, String, boolean, null, undefined, Symbol
   2. 가변값
      - 참조형 데이터: 기본적으로 가변값인 경우가 많고, 변경 불가능한 경우도 존재하며 아예 불변값으로 활용도 가능



5. 불변 객체
   - 얕은 복사(shallow copy): 바로 아래 단계의 값만 복사
   - 깊은 복사(deep copy): 내부의 모든 값들을 하나하나 찾아서 전부 복사



6. undefined와 null
   - undefined: 어떤 변수에 값이 존재하지 않을 경우 자바스크립트 엔진이 자동으로 부여
     - 값을 대입하지 않은 변수에 접근 시
     - 객체 내부의 존재하지 않는 프로퍼티에 접근 시
     - return 문이 없거나 호출되지 않는 함수의 실행 결과
   - null: 사용자가 명시적으로 '없음'을 표현하기 위해 대입
   - undefined 와 null 비교 시 일치 연산자(identity operrator, ===) 사용



문제1: ()는 바로 아래 단계의 값만 복사하는 방법이고, ()는 내부의 모든 값들을 하나하나 찾아서 전부 복사하는 방법이다.

문제2: typeof null 의 결과값은? ()