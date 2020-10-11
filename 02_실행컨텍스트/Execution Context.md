# 실행 컨텍스트

1. 실행 컨텍스트란?
   - 실행할 코드에 제공할 환경 정보들을 모아놓은 객체
   
   - 들어가기 전에
     - 스택
       - 스택은 우물 저장할때 순서를 출력할때 반대로 출력 ex) a,b,c,d -> d,c,b,a
     - 큐
       - 큐는 양쪽이 열려있는 파이프 한쪽으로 순서대로 출력 ex) a,b,c,d -> d,c,b,a or a,b,c,d



2. 콜 스택

   function a()
   {
      console.log(1);
   }
   function b()
   {
      console.log(2);
   }
   a();
   b();

   - 위처럼 함수가 실행될 경우
                                    function b
                    function a      function a    function a
   전역 컨텍스트    전역 컨텍스트    전역 컨텍스트   전역 컨텍스트

   - 해당 컨텍스트에 관련된 코드들을 실행하는 데 필요한 환경 정보들을 수집해서 객체에 저장 이 객체 담기는 정보들에 대해 알아봅시다
      - Variable environment : 현재 컨텍스트 내의 식별자들에 대한 정보 + 외부 환경 정보 선언 시점의 Lexical Environment의 스냅샷으로 변경 사항은 반영되지 않음
      - Lexical Environment : 처음에는 Lexical Environment와 같지만 변경 사항이 실시간으로 반영됨
      - ThisBinding : 식별자가 바라봐야 할 대상 객체



3. environmentRecord와 호이스팅
   1. 현재 컨텍스트와 관련된 코드의 식별자 정보들이 저장됨
   2. 컨텍스트 내부 전체를 처음부터 끝까지 순서대로 수집
   3. 수집 과정을 마쳐도 아직 실행 컨텍스트가 관여할 코드들은 실행되기 전에 상태 실행전이여도 이미 환경에 속한 코드의 변수명들을 모두 알고있음 
    - 식별자들을 최상단으로 끌어올리고 코드를 실행한다
    - 끌어올리다는 의미의 host + ing

   4. 매개변수와 변수에 대한 호이스팅

   function test1(x) { // 수집 대상 1 - 매개변수
      console.log(x);
      var x;
      console.log(x); // 수집 대상 2 변수선언
      var x = 2;
      console.log(x); // 수집 대상 3 변수선언
   }
   test1(1);

   결과값은?



   function test2() { // 수집 대상 1 - 매개변수
      var x = 1;
      console.log(x);
      var x;
      console.log(x); // 수집 대상 2 변수선언
      var x = 2;
      console.log(x); // 수집 대상 3 변수선언
   }
   test2();

   결과값은?


   function test3() {
      var x; // 수집 대상 1 의 변수 선언 부분
      var x; // 수집 대상 2 의 변수 선언 부분
      var x; // 수집 대상 3 의 변수 선언 부분

      x = 1;
      console.log(x);
      console.log(x);
      x = 2;
      console.log(x);
   }
   test3(1);

   결과값은?


   function test4() {
      console.log(b);
      var b = 'bbb';
      console.log(b);

      function b () {}
      console.log(b);
   }
   test4();

   결과값은?



4. 함수를 정의하는 세 가지 방식
   - 함수 선언문 함수명 a가 곧 변수명
   ex) function a () {}  a(); // 실행 ok

   - 변수명 b가 곧 함수명
   ex) var b = function () {} b(); // 실행 ok

   - 기명 함수 표현식 c는 변수명 d는 함수명
   ex) var c = function d () {} c(); // 실행 ok d(); // 에러



5. 함수 선언문의 위험성
   console.log(sum(3,4));

   function sum(x, y) {
      return x + y;
   }

   var a = sum(1, 2);

   function sum(x, y) {
      return x + '+' + y + '=' + (x + y);
   }

   var c = sum(1, 2);
   console.log(c);

   결과값은?

   상대적으로 함수 표현식이 안전하다.

   console.log(sum(3, 4));

   var sum = function (x, y){
      return x + y;
   };

   var a = sum(1, 2);

   var sum = function (x, y){
      return x + '+' + y + '=' + (x + y);
   }

   var c = sum(1, 2);
   console.log(c);

   결과값은?


   
6. 스코프, 스코프 체인
   - 식별자에 대한 유효범위 a외부에서 선언한 경우 a내부에서 사용가능하지만 a내부에서 선언한 경우 외부에서 사용 불가
   - 전역공간을 제외하면 함수에 의해서만 스코프가 생성 (es5까지)
   - 식별자의 유효범위를 안에서 밖으로 차례로 검색하는것을 스코프 체인이라고 한다.
   - 무조건 스코프 체인 상에서 가장 먼저 발견된 식별자에만 접근 가능
   
   var a = 1;
   var outer = function () {
      var inner = function () {
         console.log(a);
         var a =3;
      };
      inner();
      console.log(a);
   };
   outer();
   console.log(a);

   결과값은? 

   전역  -> outer -> inner


7. 전역변수 지역변수
   - 전역 공간에서 선언한 변수는 전역변수, 함수 내부에서 선언한 변수는 무조건 지역변수