## Table of Contents

> This is menu area

<br />

## JavaScript Core

let 和 const 關鍵字

  * `let` 可以用來宣告變數，用法類似 `var`，但是宣告的變數只在 `let` 所在的區塊內有效
  
    ex :

    ```javascript
    {
        let a = 10;
        var b = 1;
    }

    console.log(a);     // ReferenceError: a is not defined
    console.log(b);     // => 1
    ```

  * `let` 及 `const` 不存在拉升 (_hoisting_)，必須在宣告後才使用，否則會報錯

    ex :

    ```javascript
    console.log(foo);     // => undefined
    console.log(bar);     // ReferenceError: bar is not defined

    var foo = 2;
    let bar = 2;
    ```

  * `let` 及 `const` 所在的區塊，存在暫時性死區 (_temporal dead zone_)，變數宣告後即綁定在該區塊，不受外部所影響

    ex :

    ```javascript
    var tmp = 123;

    if (true) {

        tmp = 'abc';     // ReferenceError: tmp is not defined
        let tmp;
    }
    ```

  * `let` 及 `const` 不允許在同一區塊內重複宣告

    ex :

    ```javascript
    {
        let a = 10;
        var a = 1;          // SyntaxError: Identifier 'a' has already been declared
    }

    {
        let b = 10;
        let b = 99;         // SyntaxError: Identifier 'b' has already been declared
    }

    {
        var age = 25;
        const age = 30;     // SyntaxError: Identifier 'age' has already been declared
    }
    ```

    > 因此不能再函式內重複宣告參數

    ```javascript
    function func(arg) {

        let arg;            // SyntaxError: Identifier 'arg' has already been declared
    }
    ```

  * **區塊範疇 (_block scope_)**：`let` 及 `const` 實際上為 JavaScript 提供了區塊範疇

    ex :

    ```javascript
    {
      let n = 5;
  
      if (true) {
    
          let n = 10;
    
          console.log(n);     // => 10
      }
  
      console.log(n);         // => 5
    }
    ```

  * 允許將函式 (_function_) 定義在區塊之中，且函式宣告的行為類似 `let`，區塊外部不得調用

    ex :

    ```javascript
    function f() {

        console.log('I am outside!');
    }

    (function () {

        if (true) {

            function f() {     // 此函式被隔離於 if loop 區塊之中

                console.log('I am inside!');
            }
        }

        f();                   // => I am outside!
    }());
    ```

  * `const` 關鍵字可宣告一個常數，宣告並初始化後，它的值將是唯讀的，不能更改

    ex :

    ```javascript
    const PI = 3.1415;

    PI = 3;     // TypeError: Assignment to constant variable
    ```

  * `const` 一宣告就必須立即初始化，否則會報錯

    ex :

    ```javascript
    const PI;     // SyntaxError: Missing initializer in const declaration
    ```

  * `const` 的範疇與 `let` 一樣，只在區塊範疇內有效

    ex :

    ```javascript
    if (true) {
    
        const MAX = 5;
    }

    console.log(MAX);     // ReferenceError: MAX is not defined
    ```

  * `var` 及 `function` 在 top-level 宣告的變數，等於全域物件的特性，`let`、`const` 及 `class` 在 top-level 宣告的變數則不是

    ex :

    ```javascript
    var a = 1;

    console.log(window.a);     // => 1

    let b = 1;

    console.log(window.b);     // => undefined
    ```

<br />

## Reference Information

JavaScript ECMAScript 6 Primer, (Author：阮一峰)

<br />