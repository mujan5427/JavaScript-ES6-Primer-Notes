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

字串的擴充

  * `includes`、`startsWith`、`endsWith` 函式

    - includes()：回傳布林值，表示是否找到參數中指定的字串

    - startsWith()：回傳布林值，表示參數中指定的字串，是否存在源字串的起始處

    - endsWith()：回傳布林值，表示參數中指定的字串，是否存在源字串的結尾處

    ex :

    ```javascript
    var str = 'Hello world!';

    console.log(str.startsWith('Hello'));     // => true
    console.log(str.endsWith('!'));           // => true
    console.log(str.includes('o'));           // => true
    ```

  * `repeat` 函式

    ex :

    ```javascript
    console.log('x'.repeat(3));         // => 'xxx'
    console.log('hello'.repeat(2));     // => 'hellohello'
    console.log('na'.repeat(0));        // => ''
    ```

    > 參數如果是浮點數，會被自動取整

    ```javascript
    console.log('na'.repeat(2.9));     // => 'nana'
    ```

    > 參數如果是 `Infinity` 或 負數，將會報錯

    ```javascript
    console.log('na'.repeat(Infinity));     // => RangeError: Invalid count value
    console.log('na'.repeat(-1));           // => RangeError: Invalid count value
    ```

  * **模板字串字面值 (_template string_)**：用反引號 `` ` `` 標示，可以用來定義普通字串，也可以定義多行字串，或者在字串中嵌入變數

    ex :

    ```javascript
    var str1 = `In JavaScript is a line-feed.`;

    var str2 = `In JavaScript this is
     not legal.`;
    
    var name = 'Bob', time = 'today';
    
    var str3 = `Hello ${name}, how are you ${time}?`;
    
    console.log(str1);     // => 'In JavaScript is a line-feed.'
    console.log(str2);     // => 'In JavaScript this is
                           //     not legal.'
    console.log(str3);     // => 'Hello Bob, how are you today?'
    ```

  * 模板字串字面值支援跳脫字元

    ex :

    ```javascript
    var str1 = `\`Yo\` World!`;

    console.log(str1);     // => '`Yo` World!'
    ```

  * 嵌入變數需將變數寫入 `${}` 中，可以放入運算式、引用物件特性 或 調用函式

    ex :

    ```javascript
    var x = 1;
    var y = 2;
    var obj = {x: 1, y: 2};
    
    function fn() {
      
      return 'Hello World';
    }
    
    var exp1 = `${x} + ${y * 2} = ${x + y * 2}`;
    var exp2 = `${obj.x + obj.y}`;
    var exp3 = `foo ${fn()} bar`;
    
    
    console.log(exp1);     // => '1 + 4 = 5'
    console.log(exp2);     // => '3'
    console.log(exp3);     // => 'foo Hello World bar'
    ```

<br />

數值的擴充

  * `Number.isFinite()`、`Number.isNaN()`，非數值一律回傳 `false`

    - Number.isFinite()：檢查一個數值是否為有限的 (_finite_)

    - Number.isNaN()：檢查一個值是否為 `NaN`

    ex :

    ```javascript
    // Number.isFinite()

    console.log(Number.isFinite(15));           // => true
    console.log(Number.isFinite(0.8));          // => true
    console.log(Number.isFinite(NaN));          // => false
    console.log(Number.isFinite(Infinity));     // => false

    // Number.isNaN()

    console.log(Number.isNaN(NaN));             // => true
    console.log(Number.isNaN(15));              // => false
    console.log(Number.isNaN('15'));            // => false
    console.log(Number.isNaN(true));            // => false
    ```

  * `Number.isInteger()`：檢查一個值是否為整數

    ex :

    ```javascript
    console.log(Number.isInteger(25));       // => true
    console.log(Number.isInteger(25.0));     // => true
    console.log(Number.isInteger(25.1));     // => false
    console.log(Number.isInteger("15"));     // => false
    console.log(Number.isInteger(true));     // => false
    ```

  * `Number.isSafeInteger()`：檢查一個數值是否在，JavaScript 能夠準確表示的整數範圍內 `-2^53` 到 `2^53` 之間 (不包含前後兩端點)

    ex :

    ```javascript
    console.log(Number.isSafeInteger(9007199254740990));     // => true
    console.log(Number.isSafeInteger('a'));                  // => false
    console.log(Number.isSafeInteger(null));                 // => false
    console.log(Number.isSafeInteger(NaN));                  // => false
    console.log(Number.isSafeInteger(Infinity));             // => false
    console.log(Number.isSafeInteger(-Infinity));            // => false
    ```

  * `Math.trunc()`：去除一個數值的小數部分，回傳整數部分

    ex :

    ```javascript
    console.log(Math.trunc(4.1));         // => 4
    console.log(Math.trunc(4.9));         // => 4
    console.log(Math.trunc(-4.1));        // => -4
    console.log(Math.trunc(-4.9));        // => -4
    console.log(Math.trunc(-0.1234));     // => -0
    ```

  * `Math.sign()`：檢查一個數值是否為正數、負數、還是零

    ex :

    ```javascript
    console.log(Math.sign(-5));        // => -1
    console.log(Math.sign(5));         // => +1
    console.log(Math.sign(0));         // => +0
    console.log(Math.sign(-0));        // => -0
    console.log(Math.sign(NaN));       // => NaN
    console.log(Math.sign('foo'));     // => NaN
    console.log(Math.sign());          // => NaN
    ```

<br />

陣列的擴充

  * Array.from()：可以將物件轉換成陣列，主要為下列兩種

    - 類似陣列的物件 (_array-like object_)

    - 可遍歷的物件 (_iterable object_)

    ex :

    ```javascript
    let arrayLikeObject = {
        '0': 'a',
        '1': 'b',
        '2': 'c',
        length: 3
    };
    
    let newArray = Array.from(arrayLikeObject);
    
    console.log(newArray);     // => ['a', 'b', 'c']
    ```

  * find()、findIndex()：找出陣列中第一個符合條件的元素

    - find()：回傳該元素的值，都不符合則回傳 `undefined`

    - findIndex()：回傳該元素的 index，都不符合則回傳 `-1`

    ex :

    ```javascript
    // find()

    var ans = [1, 4, -5, 10].find(
    
      function (value) {
        
        return value < 0;
      }
    );
    
    console.log(ans);     // => -5

    // findIndex()

    var ans = [1, 5, 10, 15].find(
      
      function(value, index, arr) {
        
      return value > 9;
    });
    
    console.log(ans);     // => 10
    ```

  * fill()：將值填滿於陣列中

    ex :

    ```javascript
    var arr1 = ['a', 'b', 'c'].fill(7);
    var arr2 = new Array(3).fill(7);
    
    console.log(arr1);     // => [7, 7, 7]
    console.log(arr2);     // => [7, 7, 7]
    ```

  * keys()、values()、entries()：用於遍歷陣列

    - keys()：回傳 index

    - values()：回傳 value

    - entries()：回傳 key 和 value

    ex :

    ```javascript
    for (let index of ['a', 'b'].keys()) {
      
      console.log(index);            // => 0
    }                                //    1
    
    for (let value of ['a', 'b'].values()) {
      
      console.log(value);            // => 'a'     * 只有 Safari 10 有實作
    }                                //    'b'
    
    for (let [index, value] of ['a', 'b'].entries()) {
      
      console.log(index, value);     // => 0 'a'
    }                                //    1 'b'
    ```

  * ES6 才新增的函式，會將陣列中的空元素轉成 `undefined`

    ex :

    ```javascript
    console.log(Array.from(['a',,'b']));     // => ['a', undefined, 'b']
    ```

<br />

函式的擴充

  * 允許為函式的參數設定預設值

    ex :

    ```javascript
    function sayHi(x, y = 'World') {
      
      console.log(x + ' ' + y);
    }
    
    sayHi('Hello');              // => 'Hello World'
    sayHi('Hello', 'China');     // => 'Hello China'
    sayHi('Hello', '');          // => 'Hello '
    ```

  * 函式參數預設值如果是一個變數，則它的的範疇一樣是函式範疇

    ex :

    ```javascript
    var x = 1;
    
    function f(x, y = x) {
      
      console.log(y);
    }
    
    f(2);     // => 2
    ```

  * rest 參數：透過 `...` 來取得函式多餘的引數，rest 參數搭配的變數將回傳一組陣列，且之後不可以再有參數

    ex :

    ```javascript
    function add(str, ...values) {
    
      console.log(values);
    }
    
    add('Hi', 2, 5, 3);     // => [2, 5, 3]
    ```

  * **擴展運算子 (_spread operator_)**：透過 `...` 將一組陣列轉成用逗號分隔的參數序列

    ex :

    ```javascript
    function add(x, y) {
      
      return x + y;
    }
    
    var numbers = [4, 38];
    
    console.log(add(...numbers));     // => 42
    ```

  * 函式參數有設定預設值，則該函式不得使用 `嚴格模式`，否則會報錯

    ex :

    ```javascript
    function doSomething(a, b = a) {
      'use strict';     // => SyntaxError
      
    }
    ```

  * 函式的 `name` 特性，會回傳該函式的名稱

    ex :

    ```javascript
    function foo() {}
    var func1 = function () {};
    
    console.log(foo.name);       // => 'foo'
    console.log(func1.name);     // => 'func1'
    ```

  * 允許使用 `=>` 定義函式，但有下列限制

    - 函式內的 `this` 不可指定，是固定的

    - 不可當作建構式，用 `new` 則會報錯

    - 不可使用 `arguments`、`super`、`new.target`

    - 不可使用 `yield`，也就是不能成為 Generator 函式

    ex :

    ```javascript
    var func = (value) => value;

    // 上面的寫法實際上等於下面這種

    var func = function(value) {

      return value;
    };
    ```

<br />

物件的擴充

  * 使用物件字面值定義特性時，允許直接寫入變數及函式

    ex :

    ```javascript
    var birth = '1866/11/12';
    
    var person = {
    
      university: '德明',
      birth,
      hello() { return this.university; }
    };
    
    console.log(person.birth);       // => '1866/11/12'
    console.log(person.hello());     // => '德明'
    ```

  * 使用物件字面值定義特性時，特性名稱可以使用運算式

    ex :

    ```javascript
    let propKey = 'foo';
    
    let obj = {
      
      [propKey]: true,
      ['a' + 'bc']: 123
    };
    
    console.log(obj['foo']);     // => true
    console.log(obj['abc']);     // => 123
    ```

  * Object.assign()：用於合併物件的可列舉特性

    ex :

    ```javascript
    var target  = { a: 1, b: 1 };
    var source1 = { b: 2, c: 2 };
    var source2 = { c: 3 };
    
    Object.assign(target, source1, source2);
    
    console.log(target);     // => { a:1, b:2, c:3 }
    ```

  * ES6 一共有 5 種方式可以遍歷物件的特性

    - for...in：遍歷物件自有的、繼承的可列舉特性 (不含 Symbol 特性)

    - Object.keys(object)：回傳一個陣列，包含物件自有的可列舉特性 (不含 Symbol 特性)

    - Object.getOwnPropertyNames(object)：回傳一個陣列，包含物件自有的所有特性 (不含 Symbol 特性)

    - Object.getOwnPropertySymbols(object)：回傳一個陣列，包含物件自有的 Symbol 特性

    - Reflect.ownKeys(object)：回傳一個陣列，包含物件自有的特性 (含 Symbol 特性、不可列舉特性)

  * 物件的 `__proto__` 特性：用來讀取或設定該物件的 prototype 屬性，僅瀏覽器可實作，定義為內部屬性理論上不應對外揭露

  * Object.setPrototypeOf(object, prototype)：設定指定物件的 prototype 屬性

  * Object.getPrototypeOf(object)：取得指定物件的 prototype 屬性

    ex :

    ```javascript
    var prototypeObject = { a:1, b:2 };
    var target = {};
    
    Object.setPrototypeOf(target, prototypeObject);
    
    console.log(target.__proto__);                  // => { a:1, b:2 }
    console.log(Object.getPrototypeOf(target));     // => { a:1, b:2 }
    ```

<br />

Symbol

  * 新增一種新的資料型別 Symbol，用來表示獨一無二的值，不得與其他資料型別進行運算

    ex :

    ```javascript
    var symbol1 = Symbol('justin');
    var symbol2 = Symbol('justin');
    var symbol3 = Symbol('abc');
    
    console.log(symbol1.toString());      // => 'Symbol(justin)'
    console.log(symbol2.toString());      // => 'Symbol(justin)'
    console.log(symbol3.toString());      // => 'Symbol(abc)'
    
    console.log(symbol1 === symbol2);     // => false
    ```

  * 因為它獨一無二的特質，可以用來定義物件的特性名稱，防止特性被 override

    > Symbol 值作為物件特性名稱時，不能使用 `.` 運算子

    ex :

    ```javascript
    var varSymbol = Symbol();
    
    var obj = {
      
      [varSymbol]: 'Hello!'
    };
    
    console.log(obj[varSymbol]);     // => 'Hello!'
    ```

<br />

Proxy、Reflect

  * Proxy：攔截某些物件的操作行為，進而修改它的預設行為

    ex :

    ```javascript
    var person = {
      
      name: '張三'
    };
    
    var handler = {
      
      get: function(target, property) {
        
        if (property in target) {
          
          return target[property];
          
        } else {
          
          throw new ReferenceError(`Property '${property}' does not exist.`);
        }
      }
    };
    
    var proxy = new Proxy(person, handler);
    
    console.log(proxy.name);     // => '張三'
    console.log(proxy.age);      // => ReferenceError: Property 'age' does not exist.
    ```

  * Proxy 支持的攔截操作行為

    - get(target, propKey, receiver)：攔截物件特性的讀取

    - set(target, propKey, value, receiver)：攔截物件特性的設定

    - has(target, propKey)：攔截 `in` 運算子 及 hasOwnProperty()

    - deleteProperty(target, propKey)：攔截 `delete` 特性的操作

    - ownKeys(target)：攔截 Object.keys()

    - getOwnPropertyDescriptor(target, propKey)：攔截 Object.getOwnPropertyDescriptor()

    - defineProperty(target, propKey, propDesc)：攔截 Object.defineProperty()

    - preventExtensions(target)：攔截 Object.preventExtensions()

    - getPrototypeOf(target)：攔截 Object.getPrototypeOf()、Object.prototype.__proto__、Object.prototype.isPrototypeOf()、Object.getPrototypeOf()、Reflect.getPrototypeOf()、`instanceof`

    - isExtensible(target)：攔截 Object.isExtensible()

    - setPrototypeOf(target, proto)：攔截 Object.setPrototypeOf()

    - apply(target, object, args)：攔截函式調用、call() 和 apply()

    - construct(target, args)：攔截 `new` 實例的操作

  * Reflect：為了操作物件而提供的新API，將 Object 物件的一些明顯屬於語言內部的函式，放到 Reflect 物件中，某些方法同時存在 Object、Reflect 物件中，未來新方法將僅部署至 Reflect 物件中

<br />

## Reference Information

JavaScript ECMAScript 6 Primer, (Author：阮一峰)

<br />