## 11. 버그와 오류에 대처하는 방법  
### 11.1 버그에 대처하기  
  - 버그의 원인  
    논리적인 버그  
    오타  
    실행 환경의 변화  
    
  - Strict 모드 사용  
    Strict 모드는 자바스크립트 언어의 사양 중에서 버그를 일으키기 쉬운 부분을 제거한다.   
    Strict 모드를 사용하려면 스크립트 첫머리 또는 함수 첫머리 앞에 "use strict";를 입력한다. (strict 모드는 스크립트 단위로 반영된다)  
    
    Strict 모드의 주요 제약 사항  
    - 변수는 모두 선언해야 한다. 선언되지 않은 변수, 함수, 함수의 인자에 값을 대입하면 ReferenceError가 발생한다.  
    - 함수를 직접 호출할 때, 함수 안의 this 값이 undefined가 된다.  
    - with문은 사용할 수 없다.  
    - 함수 정의문에 같은 이름의 인수가 있으면 문법 오류가 발생한다.  
    - 객체에 같은 이름의 프로퍼티가 있으면 문법 오류가 발생한다.  
    - NaN, Infinity, undefined를 표기하면 TypeError가 발생한다.  
    - agruments[i]는 호출되었을 때의 인수 값을 유지한다. (비 Strict 모드에서는 arguments[i]가 인자의 별명이다. 따라서 한쪽을 수정하면 다른 쪽도 바뀐다.)  
    - arguments.callee를 읽을 수 없다. (TypeError 발생)  
    - eval로 실행한 코드는 호출자의 유효 범위 안에 새로운 변수나 함수를 선언할 수 없다.  
    
  - console 디버깅  
    디버깅 방법 중에는 Console 객체를 사용하는 방법이 있다. 다음 두 메서드를 가장 많이 사용한다.  
    - console.log (변수 값을 표시)   
    - console.dir (객체의 프로퍼티 목록 표시)  
  
  - 개발자 도구 실행
  
### 11.2 예외처리  
  - 예외  
    프로그램을 실행하는 도중에 예기치 않은 오류가 발생할 수도 있고, 오류는 아니지만 어떤 대처가 필요한 예외적인 상황이 발생할 수 있다.  
    예외란 오류 및 예외 조건이 발생한 사실을 알려주는 신호이다. 예외를 받아서 처리하는 부분을 가리켜 예외 처리기라고 하고, 예외를 받는 작업을 가리켜 예외를 잡는다라고 한다.  
    자바스크립트에서는 throw 문으로 예외를 던지고 try/catch/finally 문으로 예외를 잡아서 처리한다.  
    
 <!--20210220 기록 마침--> <!--20210221 기록 시작--> 
  - throw문  
    throw문은 예외를 던진다. 'throw 표현식;'과 같이 사용하며, 표현식으로는 어떤 타입의 값도 지정할 수 있다.  
    사용자에게 표시할 오류 메시지가 포함된 문자열이나 오류 코드를 의미하는 숫자도 허용된다. 하지만 일반적으로 Error 객체나 Error 객체를 상속받은 객체를 지정한다.  
    예외를 던지면 자바스크립트 인터프리터는 프로그램의 실행을 중단하고 바깥 블록에서 예외를 처리하는 예외처리기를 찾는다. (try/catch/finally 문의 catch문)  
    예외 처리기가 없으면 프로그램을 종료한다.  
    
  - Error 객체  
    자바스크립트에는 예외를 표현하기 위한 내장 객체가 일곱가지 있다. 그 중에 Error 객체는 범용적인 예외를 표현하기 위한 객체고, 나머지 여섯 개는 특정 예외가 발생했을 때 표현하기 위한 객체이다.  
    |생성자|생성하는 인스턴스|
    |:----|:---------------|
    |Error|범용적인 예외 객체|
    |EvalError|eval 함수와 관련해서 발생한 예외 객체|
    |RangeError|숫자 값이 허용 범위를 벗어났을 때 발생하는 예외 객체|
    |ReferenceError|잘못된 참조를 만났을 때 발생하는 예외 객체|
    |SyntexError|자바스크립트 문법에 어긋나는 구문을 만났을 때 발생하는 예외 객체|
    |TypeError|변수 및 인수 타입이 유효하지 않을 때 발생하는 예외 객체|
    |URIError|encodeURI와 decodeURI 메서드에 잘못된 인수가 전달되었을 때 발생하는 예외 객체|
    
    자바스크립트 인터프리터는 요류가 발생했을 때 오류에 따라 예외 객체를 던진다. 아래 예를 보자.  
    ```javascript
      x++; //ReferenceError: x is not defined
      if(a>0) {a++; //SyntaxError: Unexpected end of input
      var obj = {};
      obj.join(); //TypeError: obj.join is not a function
      var pi = 3.141592;
      pi.toFixed(1000); //RangeError: toFixed() digits argument must be between 0 and 100
      decodeURIComponent("%");  //URIError: URI malformed    
    ```
    
    예외를 표현하는 모든 내장 객체는 Error.prototype의 프로퍼티와 메서드를 상속 받는다.  
    Error.prototype의 프로퍼티는 다음과 같다.  
      - message : 오류 메시지를 뜻하는 문자열 
      - name : 오류 이름을 뜻하는 문자열  
    
    Error.prototype의 메서드는 다음과 같다.  
      - toString : 지정된 객체를 표현하는 문자열을 반환  
    
    아래 예와 같이 사용할 수 있다.  
    ```javascript
      var error = new TypeError("is not an array type");
      console.log(error.message); //is not an array type
      console.log(error.name); //TypeError
      console.log(error.toString()); //TypeError: is not an array type
    ```
  <!--20210222 기록 시작-->
  - try/catch/finally 문  
    try/catch/finally 문은 예외가 던져졌을 때 그것을 잡아 처리한다. 문법은 다음과 같다.  
    ```javascript
      try{
        // 실행할 코드를 적는다 (예외가 발생할 수 있는 코드)
        // try 안에서 예외가 발생하면 곧장 catch 블록 안으로 실행 흐름이 바뀐다.
        //(try 블록의 예외 발생한 부분의 아래에 위치한 코드는 실행안함)
      }catch{
       // try 블록에서 예외가 발생했을 때 실행된다
      }finally{
        //try 블록 코드와 catch 블록 코드가 실행된 이후 반드시 실행된다
      }
    
    ```
    - 예외가 여러 개 발생했을 때 대처하는 방법 : catch 블록 안에 예외 유형별로 처리를 작성한다.  
      ```javascript
        try{
          //오류 발생
        }catch(e){
          if (e instanceof TypeError){
            //TypeError가 발생했을 때 처리를 작성
          }else if(e instanceof ReferenceError){
            //ReferenceError가 발생했을 때의 처리를 작성
          }else{
            // 그 외의 예외가 발생했을 때의 처리를 작성
          }
        }
      
      ```
    - 예외의 전파  
      예외는 호출 스택을 거슬러 올라가며 전파된다.  
      ```javascript
        try{
          f();
        }catch(e){
          console.log("Catch Error -> " +e);
        }
        function f(){ g(); }
        function g(){ h(); }
        function h(){ throw new Error("Error occured in function h");}
        //Catch Error -> Error: Error occured in function h
      ```
      
    - 비동기 처리의 콜백 함수가 던진 예외의 처리  
      비동기 처리의 콜백 함수가 던진 예외는 콜백 함수를 넘긴 함수로 전파되지 않는다. 다음 예로 확인해보자.  
      ```javascript
        try{
          setTimeout(function throwError(){
            throw new Error("Error occured");
          }, 1000);
        }catch(e){
          console.log("Catch Error", e);
        }
        //Uncaught Error: Error occured at throwError
      ```
      위 코드에서는 예외가 캐치되지 않는 상태에서 프로그램이 종료된다. 예외를 던지는 콜백 함수 throwError는 함수 정의가 try 블록 안에 있을 뿐 try 블록 안에서 호출된 것이 아니다.  
      이 콜백 함수를 호출한 주체는 타이머 이벤트이며 try 블록 안에서 발생한 예외가 아니다. 그래서 예외를 잡을 수 없다.  
      위처럼 비동기 처리의 콜백 함수에서 발생한 예외는 바깥의 try/catch 문으로 잡을 수 없다. 
      
      하지만 ECMAScript 6부터 추가된 제너레이터를 활용하면 비동기 처리 중에서 발생한 예외를 잡을 수 있다. (skip)  
    
    - 반복문에서 빠져나오기  
      예외 메커니즘을 중첩 반복문처럼 깊은 반복문 안에서 탈출하는 용도로 이용할 수 있다.  
      ```javascript
        var a = [0,1,2,3,4,5,6,7,8,9];
        try{
          a.forEach(function(v,i,a){
            if(i>5) throw false;
            return a[i] = v*v;
           });
         }catch(e){
          if(e) throw e;
         }
          console.log(a); //[0, 1, 4, 9, 16, 25, 6, 7, 8, 9]
      
      ```
