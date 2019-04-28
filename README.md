## Table of Contents

[JavaScript Core](#javascript-core)

  1. [let、const](#let-and-const)
  2. [Destructuring](#destructuring)
  3. [New String Features](#new-string-features)
  4. [New Number Features](#new-number-features)
  5. [New Array Features](#new-array-features)
  6. [New Function Features](#new-function-features)
  7. [New Object Features](#new-object-features)
  8. [Symbols](#symbols)
  9. [Proxy、Reflect](#proxy-reflect)
  10. [Sets、Maps](#sets-and-maps)
  11. [Iterators、for-or loop](#iterators-and-for-or-loop)
  12. [Generators](#generators)
  13. [Promises](#promises)
  14. [Classes](#classes)
  15. [Module](#module)

[Reference Information](#reference-information)

<br />

## JavaScript Core

<a name="let-and-const"></a>
let、const

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

**[⬆ back to top](#table-of-contents)**

<br />
<br />

<a name="destructuring"></a>
Destructuring

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

**[⬆ back to top](#table-of-contents)**

<br />
<br />

<a name="new-string-features"></a>
New String Features

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

**[⬆ back to top](#table-of-contents)**

<br />
<br />

<a name="new-number-features"></a>
New Number Features

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

**[⬆ back to top](#table-of-contents)**

<br />
<br />

<a name="new-array-features"></a>
New Array Features

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

  * **陣列擴展運算子 (_array spread operator_)**：透過 `...` 將一組陣列轉成用逗號分隔的參數序列

    ex :

    ```javascript
    function add(x, y) {

      return x + y;
    }

    var numbers = [4, 38];

    console.log(add(...numbers));     // => 42
    ```

**[⬆ back to top](#table-of-contents)**

<br />
<br />

<a name="new-function-features"></a>
New Function Features

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

  * **rest 參數 (_rest parameters_)**：透過 `...` 來取得函式多餘的引數，rest 參數搭配的變數將回傳一組陣列，且之後不可以再有參數

    ex :

    ```javascript
    function add(str, ...values) {

      console.log(values);
    }

    add('Hi', 2, 5, 3);     // => [2, 5, 3]
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

      > 箭頭函式 `this` 的值，取決於箭頭函式在哪定義，而不是箭頭函式執行的上下文環境

      > 箭頭函式沒有自己的 `this`，導致內部的 `this` 就是外層程式碼區塊的 `this`

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

  * 不適用箭頭函式的場合

    - 定義方法，且該方法內部包含 `this`

    ex :

    ```JavaScript
    var cat = {
      lives: 9,
      jumps: () => {
        console.log(Object.prototype.toString.call(this));   // [object Window]
      }
    }

    cat.jumps();
    ```

    > 如果使用一般函式，`this` 將指向 cat 物件

    - Event Handler

    ex :

    ```JavaScript
    var btn = document.querySelector('button');

    btn.addEventListener('click', () => {
      console.log(Object.prototype.toString.call(this));   // [object Window]
    });
    ```

    > 如果使用一般函式，`this` 將指向該元素

  * 箭頭函式允許和 Destructuring 結合使用

    ex :

    ```javascript
    var user = {
      firstName: 'Justin',
      lastName: 'Ho'
    };

    var getName = ({firstName, lastName}) => {
      console.log(`${ firstName } ${ lastName }`);
    };

    getName(user);   // => 'Justin Ho'
    ```

**[⬆ back to top](#table-of-contents)**

<br />
<br />

<a name="new-object-features"></a>
New Object Features

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

  * **物件擴展運算子 (_object spread operator_)** : 透過 `...` 取出物件的所有可列舉特性

    ex :

    ```JavaScript
    var obj1 = { user1: 'John', user2: 'Justin' };
    var obj2 = { ...obj1 };   // {user1: "John", user2: "Justin"}
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

**[⬆ back to top](#table-of-contents)**

<br />
<br />

<a name="symbols"></a>
Symbols

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

**[⬆ back to top](#table-of-contents)**

<br />
<br />

<a name="proxy-reflect"></a>
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

    - getPrototypeOf(target)：攔截 Object.getPrototypeOf()、Object.prototype.\_\_proto\_\_、Object.prototype.isPrototypeOf()、Object.getPrototypeOf()、Reflect.getPrototypeOf()、`instanceof`

    - isExtensible(target)：攔截 Object.isExtensible()

    - setPrototypeOf(target, proto)：攔截 Object.setPrototypeOf()

    - apply(target, object, args)：攔截函式調用、call() 和 apply()

    - construct(target, args)：攔截 `new` 實例的操作

  * Reflect：為了操作物件而提供的新API，將 Object 物件的一些明顯屬於語言內部的函式，放到 Reflect 物件中，某些方法同時存在 Object、Reflect 物件中，未來新方法將僅部署至 Reflect 物件中

**[⬆ back to top](#table-of-contents)**

<br />
<br />

<a name="sets-and-maps"></a>
Sets、Maps

  * ECMAScript 6 提供了新的數據結構 Set，它類似陣列但是各個元素的值都是唯一的，Set 本身是個建構式

  ex :

  ```javascript
  var set1 = new Set();

  [2, 3, 5, 4, 5, 2, 2].map(x => set1.add(x));

  for (let i of set1) {

    console.log(i);     // => 2 3 5 4
  }
  ```

  * Set 結構的實例擁有下列特性、方法

    - Set.prototype.size：回傳該 Set 實例的元素總數

    > 操作方法

    - add(value)：新增值

    - delete(value)：刪除某個值，並回傳一個布林值，表示成功與否

    - has(value)：回傳一個布林值，表示該值是否存在

    - clear()：清除所有值

    > 遍歷方法

    - keys()：回傳鍵名

    - values()：回傳值

    - entries()：回傳鍵、值

    - forEach()：使用回呼函式遍歷每個元素

    ex :

    ```javascript
    var set1 = new Set();

    set1.add(1).add(2).add(2);

    console.log(set1.size);          // => 2

    console.log(set1.has(1));        // => true
    console.log(set1.has(2));        // => true
    console.log(set1.has(3));        // => false
    console.log(set1.delete(2));     // => true
    console.log(set1.has(2));        // => false

    set1.clear();

    console.log(set1.size)           // => 0
    ```

    > 由於 Set 結構沒有鍵名，因此 keys() 和 values() 行為一樣

    ```javascript
    var set = new Set(['red', 'green', 'blue']);

    for (let item of set.keys()) {

      console.log(item);     // => 'red' 'green' 'blue'
    }

    for (let item of set.values()) {

      console.log(item);     // => 'red' 'green' 'blue'
    }

    for (let item of set.entries()) {

      console.log(item);     // => ['red', 'red'] ['green', 'green'] ['blue', 'blue']
    }
    ```

  * WeakSet 結構與 Set 類似，差別在於它的值只能是物件，且內含的物件都是弱引用

  * Map 結構：類似物件也是「鍵-值」的集合，但是鍵的範圍不限是字串，可以是各種型別的值 (包含物件)

    ex :

    ```javascript
    var map1 = new Map();
    var object1 = { p: 'Hello World' };

    map1.set(object1, 'content');

    console.log(map1.get(object1));     // => 'content'
    console.log(map1.has(object1));     // => true

    map1.delete(object1);

    console.log(map1.has(object1));     // => false
    ```

  * WeakMap 結構與 Map 類似，差別在於它的鍵只能是物件，鍵所指向的物件，不計入垃圾回收機制

**[⬆ back to top](#table-of-contents)**

<br />
<br />

<a name="iterators-and-for-or-loop"></a>
Iterators、for-or loop

  * 原有的數據結構有陣列 (_Array_)、物件 (_Object_)，ECMAScript 6 新增了 Set 和 Map

  * Iterator 接口：一種為了遍歷各數據結構而制定的統一接口

  * 預設的 Iterator 接口部署在 Symbol.iterator 特性，該特性指向一個遍歷器函式

  * 有 4 種數據結構原生具備 Iterator 接口，陣列、某些類似陣列的物件、Set 和 Map 結構

    ex :

    ```javascript
    let arr = ['a', 'b', 'c'];
    let iter = arr[Symbol.iterator]();

    console.log(iter.next());     // => { done: false , value: 'a' }
    console.log(iter.next());     // => { done: false , value: 'b' }
    console.log(iter.next());     // => { done: false , value: 'c' }
    console.log(iter.next());     // => { done: true , value: undefined }
    ```

  * 一個數據結構只要部署了 Symbol.iterator 特性，就可以用 for...of 遍歷它的元素，for...of 實際上是呼叫它的遍歷器函式

    ex :

    ```javascript
    const arr = ['red', 'green', 'blue'];
    let iterator  = arr[Symbol.iterator]();

    for(let value of arr) {

      console.log(value);     // => 'red' 'green' 'blue'
    }

    for(let value of iterator) {

      console.log(value);     // => 'red' 'green' 'blue'
    }
    ```

  * for...of 可直接取到元素的值，而 for...in 只能取到元素的 index

  * for...of 不能直接遍歷**物件**，需要部署 Iterator 接口才行，for...in 可以直接遍歷物件的特性名稱

    > for...in 主要是為了遍歷物件而設計的，不適合用在陣列

    ex :

    ```javascript
    var es6 = {

      edition: 6,
      committee: 'TC39',
      standard: 'ECMA-262'
    };

    for (propertyName in es6) {

      console.log(propertyName);     // => 'edition' 'committee' 'standard'
    }
    ```

**[⬆ back to top](#table-of-contents)**

<br />
<br />

<a name="generators"></a>
Generators

  * Generator 函式是 ECMAScript 6 提供的一種非同步程式設計的解決方案

  * 形式上是一個普通函式，呼叫它會回傳一個遍歷器物件 (_Iterator Object_)，且有兩種特徵

    - `function` 與函式名稱之間存在一個 `*`

    - 函式內部存在 `yield`

    ex :

    ```javascript
    function* helloWorldGenerator() {

      yield 'hello';

      yield 'world';

      return 'ending';
    }

    var hw = helloWorldGenerator();
    ```

  * Generator 函式的呼叫方式和一般函式一樣，但是呼叫後不會馬上執行，回傳的也不是函式執行結果，而是一個內部狀態的指針物件，也就是遍歷器物件 (_Iterator Ojbect_)，必須用 next() 才能將指針指向下一個狀態，而內部指針會在函式遇到 `yield` 或 `return` 時停下來

    ex :

    > 呈如上一個範例，透過 next() 調整指針

    ```javascript
    console.log(hw.next());     // => { done: false, value: 'hello' }
    console.log(hw.next());     // => { done: false, value: 'world' }
    console.log(hw.next());     // => { done: true, value: 'ending' }
    console.log(hw.next());     // => { done: true, value: undefined }
    ```

  * `yield` 和 `return` 的差別在於，指針改變後 `yield` 會調整自己的位置指向到下一個狀態，但是 `return` 不會，一個函式只能擁有一個 `return`

  * for...of 可以自動遍歷 Generator 函式產生的 Iterator 物件

    ex :

    ```javascript
    function *oneToFive() {

      yield 1;
      yield 2;
      yield 3;
      yield 4;
      yield 5;
      return 6;
    }

    for (let value of oneToSix()) {

      console.log(value);     // => 1 2 3 4 5
    }
    ```

**[⬆ back to top](#table-of-contents)**

<br />
<br />

<a name="promises"></a>
Promises

  * Promise 也是一個非同步程式設計的解決方案，他本身是一個建構式，用來產生 Promise 物件

    ex :

    ```javascript
    function loadImageAsync(url) {

      return new Promise(function(resolve, reject) {

        var image = new Image();

        image.onload = function() {

          var str1 = 'Google Logo';

          resolve(str1);
        };

        image.onerror = function() {

          reject(new Error('Could not load image at ' + url).toString());
        };

        image.src = url;
      });
    }


    var logoUrl = 'https://www.google.com.tw/images/branding/googlelogo/1x/googlelogo_color_272x92dp.png';

    loadImageAsync(logoUrl).then(

      function(arg1) {

        console.log(`${arg1} 圖片： 載入成功！`);

        // 我是 resolve() 的 Callback 區塊

    }, function(arg1) {

        console.log(arg1);     // => Error: Could not load image at...

        // 我是 reject() 的 Callback 區塊
    });
    ```

  * Promise.prototype.then()：為 Promise 實例添加狀態改變時的 Callback 函式，可採用鏈式寫法

    - 第一個參數：Resolved 狀態的 Callback

    - 第二個參數：Rejected 狀態的 Callback (可選的)

  * Promise.prototype.catch()：為 .then(null, rejection) 的別名，用於錯誤發生時的 Callback

    ex :

    ```javascript
    function loadImageAsync(url) {

      return new Promise(function(resolve, reject) {

        var image = new Image();

        image.onload = function() {

          var str1 = 'Google Logo';

          resolve(str1);
        };

        image.onerror = function() {

          reject(new Error('Could not load image at ' + url).toString());
        };

        image.src = url;
      });
    }

    var logoUrl = 'ttps://www.google.com.tw/images/branding/googlelogo/1x/googlelogo_color_272x92dp.png';

    loadImageAsync(logoUrl).then(

      function(arg1) {

        console.log(`${arg1} 圖片： 載入成功！`);

    }).catch(function(error) {

      console.log(error);     // => Error: Could not load image at...
    });
    ```

  * Promise.prototype.then() 的 Callback 中，拋出的錯誤，也會被 Promise.prototype.catch() 捕捉

  * Promise.all()：將多個 Promise 實例包裝成一個新的 Promise 實例，用來判斷參數中的 Promise 是否都成功

    - Resolved 狀態判定：參數中所有的 Promise 狀態都是 fulfilled 時，新實例的狀態才會是 fulfilled，而新實例的 Resolved Callback 才會被呼叫

    - Rejected 狀態判定：參數中有一個 Promise 狀態是 rejected 時，新實例的狀態就會是 rejected，而第一個 rejected 的實例的回傳值會傳給 Rejected Callback

  * Promise.race()：也是包裝成一個新 Promise 實例，並判斷多個 Promise 實例，何者的狀態先改變，將最先改變狀態的實例的值傳給 Callback

  * Promise.resolve()：將一個物件轉換成 Promise 物件

**[⬆ back to top](#table-of-contents)**

<br />
<br />

<a name="classes"></a>
Classes

  * ECMAScript 6 實作了 `class`，讓 JavaScript 的物件導向寫法和其他語言差異沒這麼大 (看起來)

    > class 大部分的功能，ECMAScript 5 都做得到，下列兩者相同

    ex :

    ```javascript
    // ECMAScript 5 寫法

    function Point(x, y) {

      this.x = x;
      this.y = y;
    }

    Point.prototype.toString = function () {

      return '(' + this.x + ', ' + this.y + ')';
    };

    var pointInstance = new Point(1, 2);

    // ECMAScript 6 寫法

    class Point {

      constructor(x, y) {

        this.x = x;
        this.y = y;
      }

      toString() {
        return '(' + this.x + ', ' + this.y + ')';
      }
    }
    ```

  * 定義 class 的方法時，不需要在函式前面加上 `function`

  * 實例化 class，也是直接使用 `new` 就可以，就像使用建構式一樣

  * class 上的方法，事實上都定義在 class 的 prototype 特性上

    ex :

    ```javascript
    class Point {

      constructor() {

      }

      toString() {

      }

      toValue() {

      }
    }

    Point.prototype = {

      toString() {

      },
      toValue() {

      }
    };

    console.log(Point.prototype.toString() === new Point().toString());     // => true
    ```

  * prototype 的 constuctor 特性直接指向 class 本身

    > 呈如上一個範例

    ex :

    ```javascript
    console.log(Point.prototype.constructor === Point);     // => true
    ```

  * class 的內部定義的方法，全都是不可列舉的 (_non-enumerable_)

  * constructor() 是 class 的預設方法，當 class 被使用 `new` 實例化時，會自動呼叫它

  * 一個 class 一定要有 constructor()，如果沒有定義的話，預設還是會自動添加一個空的 constructor()

    ex :

    ```javascript
    constructor() {

    }
    ```

  * class 的特性除非透過 `this` 直接定義在實例上，否則都是定義在 prototype 上

  * class 不存在變數拉升 (_hoisting_)

    ex :

    ```javascript
    new Point();     // => ReferenceError: Point is not defined

    class Point {

    }
    ```

  * class 和 函式一樣，都可以使用運算式的方式定義

    ex :

    ```javascript
    const MyClass = class Me {

      getClassName() {

        console.log(Me.name);
      }
    };

    let instance = new MyClass();

    instance.getClassName();     // => 'Me'
    ```

    > MyClass 是這個 class 的名稱，而不是 Me，Me 只在 class 內部可以使用，指的是目前這個 class

  * class 運算式可以寫出立即執行的 class，如同 IIFE

    ex :

    ```javascript
    let person = new class {

      constructor(name) {

        this.name = name;
      }

      sayName() {

        console.log(this.name);
      }
    }('張三');

    person.sayName();     // => '張三'
    ```

  * class 的 name 特性會回傳 class 後面定義的名稱

  * class 可以透過 `extends` 繼承 parent class

  * subclass 必須在 constructor() 呼叫 super()，去取得 parent class 的 `this`，因為它本身並沒有

    ex :

    ```javascript
    class ColorPoint extends Point {

      constructor(x, y, color) {

        super(x, y);
        this.color = color;
      }

      toString() {

        return this.color + ' ' + super.toString();
      }
    }
    ```

  * 如果 subclass 沒有定義 constructor()，預設的 constructor() 會自動呼叫 super()

  * `super` 可以當作函式使用，也可以當作物件使用

    - 當作函式：表示 parent class 的 constructor()，只能用在 subclass 的 constructor()

    - 當作物件：指向 parent class 的原型物件

  * class 的方法前面加上 `*`，就是一個 Generator 函式

  * class 靜態方法：在方法前面加上 `static`，表示該方法為靜態方法，且不會被實例繼承，需要直接透過 class 呼叫

    ex :

    ```javascript
    class sayHi {

      static classMethod() {

        console.log('Hello World');
      }
    }

    sayHi.classMethod();     // => 'Hello World'
    ```

  * new.target：回傳目前的 class，subclass 繼承時，會回傳 subclass，而不是 parent class

    ex :

    ```javascript
    class Rectangle {

      constructor(length, width) {
        console.log(new.target === Rectangle);     // => false，改成 Square 就會是 true

      }
    }

    class Square extends Rectangle {

      constructor(length) {

        super(length, length);
      }
    }

    var obj = new Square(3);
    ```

**[⬆ back to top](#table-of-contents)**

<br />
<br />

<a name="module"></a>
Module

  * ECMAScript 6 的模組自動採用`嚴格模式`，不管你有沒有在模組開頭加上 `"use strict";`

  * 模組功能主要由兩個命令構成: `export` 和 `import`

    - `export` 命令用於規定模組的對外接口，可輸出變數、函數 和 類別

    - `import` 命令用於輸入其他模組提供的功能

  * 一個模組就是一個獨立的文件，該文件內部的所有變數，外部無法獲取，如果你希望外部能夠讀取模組內部的某個變數，就必須使用 `export` 關鍵字輸出該變數

  * `export` 基本用法

    ex :

    ```javascript
    // 輸出變數

    // 寫法 1

    export var firstName = 'Michael';
    export var lastName = 'Jackson';
    export var year = 1958;

    // 寫法 2

    var firstName = 'Michael';
    var lastName = 'Jackson';
    var year = 1958;

    export { firstName, lastName, year };
    ```

    ```javascript
    // 輸出函數

    // 寫法 1
    export function multiply(x, y) {
      return x * y;
    };

    // 寫法 2
    function v1() { ... }
    function v2() { ... }

    export {
      v1 as streamV1,
      v2 as streamV2,
      v2 as streamLatestVersion   // v2 可以用不同的名字輸出兩次
    };
    ```
    > 可以使用 `as` 關鍵字，重新命名輸出函數的對外接口

<br>

  * `export` 輸出的接口，與其對應的值是動態綁定關係，即通過該接口，可以取到模組內部即時的值

    ex :

    ```javascript
    export var foo = 'bar';

    setTimeout(() => foo = 'baz', 500);
    ```

  * `export` 可以出現在模組的任何位置，只要處於模組頂層即可

  * `export default`：使 `import` 時，不需要知道變數或函數名稱，即可載入的一種輸出方式

    ex :

    ```javascript
    // export-default.js

    export default function () {        // 寫法 1 (匿名函數)
      console.log('foo');
    }

    export default function foo() {     // 寫法 2
      console.log('foo');
    }

    function foo() {                    // 寫法 3
      console.log('foo');
    }

    export default foo;
    ```

    ```javascript
    // import-default.js

    import customName from './export-default';

    customName();                       // 'foo'
    ```

  * `import` 基本用法

    ex :

    ```javascript
    import { firstName, lastName, year } from './profile.js';
    ```

    ```javascript
    import { lastName as surname } from './profile.js';
    ```
    > 使用 `as` 關鍵字，將輸入的變數重新命名

<br>

  * `import` 具有拉升 (Hoisting) 效果，會拉升到整個模組的開頭，首先執行

    ex :

    ```javascript
    foo();

    import { foo } from 'my_module';
    ```

  * 整體載入: 用星號 (*) 指定一個物件，所有輸出值都載入到這個物件上面

    ex :

    ```javascript
    import * as circle from './circle';

    console.log('圓面積: ' + circle.area(4));
    console.log('圓周長: ' + circle.circumference(14));
    ```

**[⬆ back to top](#table-of-contents)**

<br />
<br />


## Reference Information

JavaScript ECMAScript 6 Primer (Author：阮一峰)

Exploring ES6：Upgrade to the next version of JavaScript (Author：Dr. Axel Rauschmayer)

<br />
