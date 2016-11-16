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

變數的解構賦值

  * ECMAScript 6 允許按照一定模式，從陣列或物件取值，並賦予至變數中

    ex :

    ```javascript
    let [foo, [[bar], baz]] = [1, [[2], 3]];

    console.log(foo);     // => 1
    console.log(bar);     // => 2
    console.log(baz);     // => 3
    ```
  
  * 解構不成功，變數的值會等於 `undefined`

    ex :

    ```javascript
    var [foo] = [];

    console.log(foo);     // => undefined
    ```

  * 如果等號的右邊不是陣列或不具備 Iterator 接口的結構，將會報錯

  * 解構賦值允許指定預設值，一個位置嚴格等於 (`===`) undefined，預設值就會生效

    ex :

    ```javascript
    var [foo = true] = [];
    
    console.log(foo);     // => true
    
    var [r, x = 'b'] = ['a'];
    var [y, z = 'b'] = ['a', undefined];
    
    console.log(x);       // => 'b'
    console.log(z);       // => 'b'
    ```

  * 預設值可以引用解構賦值的其他變數，但該變數必須已經宣告

    ex :

    ```javascript
    let [x = 1, y = x] = [];

    console.log(x);     // => 1
    console.log(y);     // => 1
    ```

  * 物件的解構賦值不按照位置取值，而是按照物件的特性名稱

    ex :

    ```javascript
    var { bar, foo } = { foo: "aaa", bar: "bbb" };

    console.log(bar);     // => 'bbb'
    console.log(foo);     // => 'aaa'
    ```

    > 如果變數的名稱與特性名稱不一致，需寫成這樣

    ```javascript
    var { foo: baz } = { foo: 'aaa', bar: 'bbb' };

    console.log(baz);     // => 'aaa'
    ```

  * 物件的解構賦值也可以使用預設值

    ex :

    ```javascript
    var {x = 3} = {};

    var {y, z = 5} = {y: 1};

    console.log(x);     // => 3
    console.log(z);     // => 5
    ```

  * 字串的解構賦值

    ex :

    ```javascript
    const [a, b, c, d, e] = 'hello';

    console.log(b);     // => 'e'
    console.log(e);     // => 'o'
    ```

  * 函式參數的解構也可以使用預設值

    ex :

    ```javascript
    function move({x = 0, y = 0} = {}) {

        return console.log([x, y]);
    }

    move({x: 3, y: 8});     // => [3, 8]
    move({x: 3});           // => [3, 0]
    move({});               // => [0, 0]
    move();                 // => [0, 0]
    ```

<br />

## Reference Information

JavaScript ECMAScript 6 Primer, (Author：阮一峰)

<br />