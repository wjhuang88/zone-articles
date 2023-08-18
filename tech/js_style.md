*用合理的方式编写JavaScript*
**[原文链接](https://github.com/airbnb/javascript/tree/master/es5)**

## 类型

  - **基本类型**: 对于基本类型的操作是对值的操作。

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    var foo = 1;
    var bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - **复杂类型**: 对于复杂类型的操作是对引用的操作。

    + `object`
    + `array`
    + `function`

    ```javascript
    var foo = [1, 2];
    var bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

## 对象

  - 尽量使用字面量创建对象。

    ```javascript
    // 不推荐
    var item = new Object();

    // 推荐
    var item = {};
    ```

  - 不要使用[保留字](http://es5.github.io/#x7.6.1)作为属性键值(在IE8中不被支持)，查看[更多信息](https://github.com/airbnb/javascript/issues/61)。

    ```javascript
    // 不推荐
    var superman = {
      default: { clark: 'kent' },
      private: true
    };

    // 推荐
    var superman = {
      defaults: { clark: 'kent' },
      hidden: true
    };
    ```

  - 使用可读性强的同义词来代替保留字。

    ```javascript
    // 不推荐
    var superman = {
      class: 'alien'
    };

    // 不推荐
    var superman = {
      klass: 'alien'
    };

    // 推荐
    var superman = {
      type: 'alien'
    };
    ```

## 数组

  - 尽量使用字面量创建数组。

    ```javascript
    // 不推荐
    var items = new Array();

    // 推荐
    var items = [];
    ```

  - 使用Array#push方法而不要直接指定数组下标来添加元素。

    ```javascript
    var someStack = [];

    // 不推荐
    someStack[someStack.length] = 'abracadabra';

    // 推荐
    someStack.push('abracadabra');
    ```

  - 当你需要复制一个数组的时候，使用Array#slice。 [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    var len = items.length;
    var itemsCopy = [];
    var i;

    // 不推荐
    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // 推荐
    itemsCopy = items.slice();
    ```

  - 使用Array#slice还可以将一个类数组对象(例如:arguments对象)转化为真正的数组。

    ```javascript
    function trigger() {
      var args = Array.prototype.slice.call(arguments);
      ...
    }
    ```


## 字符串

  - 使用单引号`''`定义字符串.

    ```javascript
    // 不推荐
    var name = "Bob Parr";

    // 推荐
    var name = 'Bob Parr';

    // 不推荐
    var fullName = "Bob " + this.lastName;

    // 推荐
    var fullName = 'Bob ' + this.lastName;
    ```

  - 超过100个字符的字符串应该写成多行形式并用字符串连接符`+`连接。
  - 注意: 如果过度使用字符串连接符可能会影响性能。[jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40).

    ```javascript
    // 不推荐
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // 不推荐
    var errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // 推荐
    var errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';
    ```

  - 当你需要在代码中构建字符串时，使用Array#join方法而不是字符串连接符`+`。特别是IE: [jsPerf](http://jsperf.com/string-vs-array-concat/2).

    ```javascript
    var items;
    var messages;
    var length;
    var i;

    messages = [{
      state: 'success',
      message: 'This one worked.'
    }, {
      state: 'success',
      message: 'This one worked as well.'
    }, {
      state: 'error',
      message: 'This one did not work.'
    }];

    length = messages.length;

    // 不推荐
    function inbox(messages) {
      items = '<ul>';

      for (i = 0; i < length; i++) {
        items += '<li>' + messages[i].message + '</li>';
      }

      return items + '</ul>';
    }

    // 推荐
    function inbox(messages) {
      items = [];

      for (i = 0; i < length; i++) {
        items[i] = '<li>' + messages[i].message + '</li>';
      }

      return '<ul>' + items.join('') + '</ul>';
    }
    ```


## 函数

  - 函数表达式：

    ```javascript
    // 匿名函数表达式
    var anonymous = function () {
      return true;
    };

    // 有命名的函数表达式
    var named = function named() {
      return true;
    };

    // 立即执行函数表达式(IIFE)
    (function () {
      console.log('Welcome to the Internet. Please follow me.');
    }());
    ```

  - 永远不要在非函数代码块(例如if, while块)中声明函数，但可以将函数赋值给一个变量。当然所有浏览器都允许你在非函数代码块中写函数声明，但不同的浏览器的执行方式不完全一致，这会带来不可预知的问题。
  - **注意:** ECMA-262标准中将一个代码块`block`定义为一个表达式列表，但函数声明并没有被当做一个表达式。[参考ECMA-262中的相关条目](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97)。

    ```javascript
    // 不推荐
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // 推荐
    var test;
    if (currentUser) {
      test = function test() {
        console.log('Yup.');
      };
    }
    ```

  - 永远不要将一个参数命名为`arguments`，这么做会覆盖每个函数默认的`arguments`对象。

    ```javascript
    // 不推荐
    function nope(name, options, arguments) {
      // ...stuff...
    }

    // 推荐
    function yup(name, options, args) {
      // ...stuff...
    }
    ```



## 属性

  - 使用点号`.`来获取对象的属性。

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    // 不推荐
    var isJedi = luke['jedi'];

    // 推荐
    var isJedi = luke.jedi;
    ```

  - 当需要获取的属性键值是一个变量来给出，可以使用`[]`来获取。

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    function getProp(prop) {
      return luke[prop];
    }

    var isJedi = getProp('jedi');
    ```


## 变量

  - 声明变量的时候永远要写上`var`，不这么做的话将会得到一个全局变量，并因此污染全局变量命名空间。

    ```javascript
    // 不推荐
    superPower = new SuperPower();

    // 推荐
    var superPower = new SuperPower();
    ```

  - 每个变量声明使用一个单独的`var`。
    这么做更便于添加新的变量声明，而不用关心`;`和`,`修改的问题。

    ```javascript
    // 不推荐
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // 不推荐
    // (对比上面的代码，你看出错误的地方了吗)
    var items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // 推荐
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';
    ```

  - 将未赋值变量的声明放在最后，这有利于后面在给变量赋值的时候需要依赖另一个已赋值变量的情况。

    ```javascript
    // 不推荐
    var i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // 不推荐
    var i;
    var items = getItems();
    var dragonball;
    var goSportsTeam = true;
    var len;

    // 推荐
    var items = getItems();
    var goSportsTeam = true;
    var dragonball;
    var length;
    var i;
    ```

  - 将变量声明在其作用域的最顶部，这样可以避免变量提升所带来的问题。

    ```javascript
    // 不推荐
    function () {
      test();
      console.log('doing stuff..');

      //..other stuff..

      var name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // 推荐
    function () {
      var name = getName();

      test();
      console.log('doing stuff..');

      //..other stuff..

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // 不推荐 - 会产生不必要的函数调用
    function () {
      var name = getName();

      if (!arguments.length) {
        return false;
      }

      this.setFirstName(name);

      return true;
    }

    // 推荐
    function () {
      var name;

      if (!arguments.length) {
        return false;
      }

      name = getName();
      this.setFirstName(name);

      return true;
    }
    ```


## 变量提升

  - 变量的声明语句会被解释器自动提升到其作用域的最顶部，但其赋值的语句不会被提升。

    ```javascript
    // 我们知道这是不会运行成功的 (假设没有
    // 定义notDefined这个全局变量)
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // 由于有变量提升机制，在你声明一个变量之前
    // 就使用这个变量是不会抛出错误的。
    // 注意：这里给变量赋的值`true`没有被提升。
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // 解释器把变量声明提升到了作用域顶部，这意味着
    // 我们上面的例子可以被重写为：
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }
    ```

  - 匿名函数表达式的变量声明会被提升，但函数体不会被提升。

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function () {
        console.log('anonymous function expression');
      };
    }
    ```

  - 有命名的函数表达式的变量声明会被提升，但函数名和函数体都不会被提升。

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // 当变量名与函数名相同的时候得到的结果没有变化。
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }
    ```

  - 函数声明时解释器会将函数名和函数体都提升到作用域顶部。

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - 获取更多信息参考：[JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) by [Ben Cherry](http://www.adequatelygood.com/).



## 比较运算符

  - 使用`===`和`!==`比使用`==`和`!=`更理想.
  - 条件语句(例如`if`语句)在判断表达式值的时候，会使用`ToBoolean`抽象方法将结果强制转化为布尔型，转化规则如下：

    + **Objects**转化为**true**
    + **Undefined**转化为**false**
    + **Null**转化为**false**
    + **Booleans**不变
    + **Numbers**如果是**+0, -0或NaN**转化为**false**, 否则转化为**true**
    + **Strings**如果是空字符串`''`转化为**false**, 否则转化为**true**

    ```javascript
    if ([0]) {
      // true
      // 数组是一个对象, 对象会被转化为true
    }
    ```

  - 使用简短形式。

    ```javascript
    // 不推荐
    if (name !== '') {
      // ...stuff...
    }

    // 推荐
    if (name) {
      // ...stuff...
    }

    // 不推荐
    if (collection.length > 0) {
      // ...stuff...
    }

    // 推荐
    if (collection.length) {
      // ...stuff...
    }
    ```

  - 更多信息参考[Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.


## 代码块

  - 多行的代码块使用大括号包围。

    ```javascript
    // 不推荐
    if (test)
      return false;

    // 推荐
    if (test) return false;

    // 推荐
    if (test) {
      return false;
    }

    // 不推荐
    function () { return false; }

    // 推荐
    function () {
      return false;
    }
    ```

  - 如果使用多行形式的`if`，`else`代码块，将`else`放在`if`块关闭大括号的同一行。

    ```javascript
    // 不推荐
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // 推荐
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```


## 注释

  - 多行注释使用`/** ... */`，包括描述，参数类型和值，返回值等信息。

    ```javascript
    // 不推荐
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // 推荐
    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     * @param {String} tag
     * @return {Element} element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  - 单行注释使用`//`，单行注释放在被注释内容的前一行，并在注释前留一个空行。

    ```javascript
    // 不推荐
    var active = true;  // is current tab

    // 推荐
    // is current tab
    var active = true;

    // 不推荐
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }

    // 推荐
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }
    ```

  - 如果你想指出一个需要重新审查的问题或是想提出一个未解决问题的实现思路，用`FIXME`或`TODO`在注释中做出标注，这样可以帮助其他开发者快速理解你的意图，这样的注释和普通注释不一样，因为他们是需要被处理的。处理方式为：`FIXME -- 需要解决问题`或者`TODO -- 需要被实现`。

  - 使用`// FIXME:`标注问题.

    ```javascript
    function Calculator() {

      // FIXME: 不应该使用全局变量
      total = 0;

      return this;
    }
    ```

  - 使用`// TODO:`指出实现方式。

    ```javascript
    function Calculator() {

      // TODO: total应该被配置到一个options参数中
      this.total = 0;

      return this;
    }
    ```


## 空白符

  - 使用soft tabs并设置成2个空格。

    ```javascript
    // 不推荐
    function () {
    ∙∙∙∙var name;
    }

    // 不推荐
    function () {
    ∙var name;
    }

    // 推荐
    function () {
    ∙∙var name;
    }
    ```

  - 左大括号之前留一个空格。

    ```javascript
    // 不推荐
    function test(){
      console.log('test');
    }

    // 推荐
    function test() {
      console.log('test');
    }

    // 不推荐
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });

    // 推荐
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });
    ```

  - 控制语句(如`if`, `while`)的左小括号前留一个空格，函数的参数列表前不要留空格。

    ```javascript
    // 不推荐
    if(isJedi) {
      fight ();
    }

    // 推荐
    if (isJedi) {
      fight();
    }

    // 不推荐
    function fight () {
      console.log ('Swooosh!');
    }

    // 推荐
    function fight() {
      console.log('Swooosh!');
    }
    ```

  - 将运算符用空格隔开。

    ```javascript
    // 不推荐
    var x=y+5;

    // 推荐
    var x = y + 5;
    ```

  - 文件最后以一个单独的换行符结尾。

    ```javascript
    // 不推荐
    (function (global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // 不推荐
    (function (global) {
      // ...stuff...
    })(this);↵
    ↵
    ```

    ```javascript
    // 推荐
    (function (global) {
      // ...stuff...
    })(this);↵
    ```

  - 遇到很长的方法调用链，使用换行缩进的方式(参照下面的代码)，并以点号`.`开头，强调此行是一个方法而非一个新的语句。

    ```javascript
    // 不推荐
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // 不推荐
    $('#items').
      find('.selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // 推荐
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // 不推荐
    var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
        .attr('width', (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // 推荐
    var leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width', (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

  - 在代码块结束和新语句之间留一个空行。

    ```javascript
    // 不推荐
    if (foo) {
      return bar;
    }
    return baz;

    // 推荐
    if (foo) {
      return bar;
    }

    return baz;

    // 不推荐
    var obj = {
      foo: function () {
      },
      bar: function () {
      }
    };
    return obj;

    // 推荐
    var obj = {
      foo: function () {
      },

      bar: function () {
      }
    };

    return obj;
    ```

## 逗号

  - 放在行首的逗号: **不要使用！**

    ```javascript
    // 不推荐
    var story = [
        once
      , upon
      , aTime
    ];

    // 推荐
    var story = [
      once,
      upon,
      aTime
    ];

    // 不推荐
    var hero = {
        firstName: 'Bob'
      , lastName: 'Parr'
      , heroName: 'Mr. Incredible'
      , superPower: 'strength'
    };

    // 推荐
    var hero = {
      firstName: 'Bob',
      lastName: 'Parr',
      heroName: 'Mr. Incredible',
      superPower: 'strength'
    };
    ```

  - 对象或数组末尾额外的逗号： **不要使用！** 这在IE6/7或IE9的quirks模式会导致一些问题。在一些ES3的实现中额外的逗号还会影响数组的长度计算，  这个问题在ES5 ([source](http://es5.github.io/#D))中已经被明确了：

  > Edition 5 clarifies the fact that a trailing comma at the end of an ArrayInitialiser does not add to the length of the array. This is not a semantic change from Edition 3 but some implementations may have previously misinterpreted this.

    ```javascript
    // 不推荐
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn',
    };

    var heroes = [
      'Batman',
      'Superman',
    ];

    // 推荐
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn'
    };

    var heroes = [
      'Batman',
      'Superman'
    ];
    ```


## 分号

  - **请使用！**

    ```javascript
    // 不推荐
    (function () {
      var name = 'Skywalker'
      return name
    })()

    // 推荐
    (function () {
      var name = 'Skywalker';
      return name;
    })();

    // 推荐 (当两个定义了立即执行函数的文件被连接使用的时候可以防止意外的错误)
    ;(function () {
      var name = 'Skywalker';
      return name;
    })();
    ```

    [了解更多](http://stackoverflow.com/a/7365214/1712802)


## 类型转化和强制转化

  - 在语句开始的时候执行类型强制转换。
  - 字符串:

    ```javascript
    //  => this.reviewScore = 9;

    // 不推荐
    var totalScore = this.reviewScore + '';

    // 推荐
    var totalScore = '' + this.reviewScore;

    // 不推荐
    var totalScore = '' + this.reviewScore + ' total score';

    // 推荐
    var totalScore = this.reviewScore + ' total score';
    ```

  - 数字类型转换时使用`parseInt`，并且总是带上基数。
    ```javascript
    var inputValue = '4';

    // 不推荐
    var val = new Number(inputValue);

    // 不推荐
    var val = +inputValue;

    // 不推荐
    var val = inputValue >> 0;

    // 不推荐
    var val = parseInt(inputValue);

    // 推荐
    var val = Number(inputValue);

    // 推荐
    var val = parseInt(inputValue, 10);
    ```

  - 如果你遇到一些极端情况，`parseInt`成为了瓶颈，并且因为[性能因素](http://jsperf.com/coercion-vs-casting/3)需要使用移位操作，留下注释来解释你这么做的原因。

    ```javascript
    // 推荐
    /**
     * parseInt是我代码缓慢的元凶，
     * 使用移位操作来将字符串转换成数字可解此忧
     */
    var val = inputValue >> 0;
    ```

  - **注意:** 使用移位操作的时候请注意，数字(Number)类型是一个[64位值](http://es5.github.io/#x4.3.19)，但是移位操作总是会返回一个32位整数([source](http://es5.github.io/#x11.7))。在处理大于32位整数的值的时候，移位操作会导致一些无法预测的现象。[讨论](https://github.com/airbnb/javascript/issues/109)。32位有符号整型数的最大值为2,147,483,647:

    ```javascript
    2147483647 >> 0 //=> 2147483647
    2147483648 >> 0 //=> -2147483648
    2147483649 >> 0 //=> -2147483647
    ```

  - 布尔型:

    ```javascript
    var age = 0;

    // 不推荐
    var hasAge = new Boolean(age);

    // 推荐
    var hasAge = Boolean(age);

    // 推荐
    var hasAge = !!age;
    ```


## 命名规范

  - 避免单字母的命名，尽量使用有意义的命名方式。

    ```javascript
    // 不推荐
    function q() {
      // ...stuff...
    }

    // 推荐
    function query() {
      // ..stuff..
    }
    ```

  - 对象、函数、实例命名时，使用骆驼命名(小驼峰命名)。

    ```javascript
    // 不推荐
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    var o = {};
    function c() {}

    // 推荐
    var thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

  - 构造方法或类的命名使用帕斯卡命名(大驼峰命名)。

    ```javascript
    // 不推荐
    function user(options) {
      this.name = options.name;
    }

    var bad = new user({
      name: 'nope'
    });

    // 推荐
    function User(options) {
      this.name = options.name;
    }

    var good = new User({
      name: 'yup'
    });
    ```

  - 命名私有属性的时候，以下划线`_`开头。

    ```javascript
    // 不推荐
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // 推荐
    this._firstName = 'Panda';
    ```

  - 要保存一个到`this`的引用，请使用`_this`来命名变量。

    ```javascript
    // 不推荐
    function () {
      var self = this;
      return function () {
        console.log(self);
      };
    }

    // 不推荐
    function () {
      var that = this;
      return function () {
        console.log(that);
      };
    }

    // 推荐
    function () {
      var _this = this;
      return function () {
        console.log(_this);
      };
    }
    ```

  - 给你的函数命名，这将有利于调试时代码栈的追踪。

    ```javascript
    // 不推荐
    var log = function (msg) {
      console.log(msg);
    };

    // 推荐
    var log = function log(msg) {
      console.log(msg);
    };
    ```

  - **注意：** 命名的函数表达式在IE8或以下版本会有一些怪异的表现。参考[http://kangax.github.io/nfe/](http://kangax.github.io/nfe/)。

  - 如果你的源码文件中只导出(exports)了一个类，请保持你的文件名与类名一致。
    ```javascript
    // 文件内容
    class CheckBox {
      // ...
    }
    module.exports = CheckBox;

    // 在其他文件中
    // 不推荐
    var CheckBox = require('./checkBox');

    // 不推荐
    var CheckBox = require('./check_box');

    // 推荐
    var CheckBox = require('./CheckBox');
    ```


## 属性存取器

  - 属性存取器方法不是必需的。
  - 如果你要定义属性存取器，按照getVal()和setVal('hello')的模式来命名。

    ```javascript
    // 不推荐
    dragon.age();

    // 推荐
    dragon.getAge();

    // 不推荐
    dragon.age(25);

    // 推荐
    dragon.setAge(25);
    ```

  - 如果属性是布尔型，使用isVal()或hasVal()的形式。

    ```javascript
    // 不推荐
    if (!dragon.age()) {
      return false;
    }

    // 推荐
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  - 创建get()和set()函数也是可以的，但要保持其行为的一致性。

    ```javascript
    function Jedi(options) {
      options || (options = {});
      var lightsaber = options.lightsaber || 'blue';
      this.set('lightsaber', lightsaber);
    }

    Jedi.prototype.set = function set(key, val) {
      this[key] = val;
    };

    Jedi.prototype.get = function get(key) {
      return this[key];
    };
    ```


## 构造器

  - 将新增方法赋值给对象的原型(prototype)，而不要直接用新的对象覆盖对象的原型。 如果每次都覆盖对象的原型，就不能实现继承了，因为每次你都会直接覆盖掉基类的所有方法。

    ```javascript
    function Jedi() {
      console.log('new jedi');
    }

    // 不推荐
    Jedi.prototype = {
      fight: function fight() {
        console.log('fighting');
      },

      block: function block() {
        console.log('blocking');
      }
    };

    // 推荐
    Jedi.prototype.fight = function fight() {
      console.log('fighting');
    };

    Jedi.prototype.block = function block() {
      console.log('blocking');
    };
    ```

  - 方法可以返回`this`来帮助构建方法链式调用。

    ```javascript
    // 不推荐
    Jedi.prototype.jump = function jump() {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function setHeight(height) {
      this.height = height;
    };

    var luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // 推荐
    Jedi.prototype.jump = function jump() {
      this.jumping = true;
      return this;
    };

    Jedi.prototype.setHeight = function setHeight(height) {
      this.height = height;
      return this;
    };

    var luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```


  - 创建一个自定义的toString()也是可以的，但要确保其正确性，并注意它对代码的其他地方会不会产生影响。

    ```javascript
    function Jedi(options) {
      options || (options = {});
      this.name = options.name || 'no name';
    }

    Jedi.prototype.getName = function getName() {
      return this.name;
    };

    Jedi.prototype.toString = function toString() {
      return 'Jedi - ' + this.getName();
    };
    ```


## 事件

  - 当你需要将数据绑定到一个事件(无论是DOM事件还是其他的事件)的时候，不要直接传入数据对象本身，将其包装到一个键值对象中再传入，因为你不能确保后续操作中不需要传入其他更多的数据到本事件中。例如, 下面这样就是不好的：

    ```js
    // 不推荐
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function (e, listingId) {
      // do something with listingId
    });
    ```

    更好的做法是：

    ```js
    // 推荐
    $(this).trigger('listingUpdated', { listingId : listing.id });

    ...

    $(this).on('listingUpdated', function (e, data) {
      // do something with data.listingId
    });
    ```


## 模块

  - 模块应该以一个`!`开头，确保如果本模块被合并到另一个第三方模块的末尾，而这个第三方模块忘记了结尾的分号的时候，不会报错。[更多解释](https://github.com/airbnb/javascript/issues/44#issuecomment-13063933)
  - 文件应该按骆驼命名(小驼峰命名)规则命名。如果文件夹中只有一个文件，文件夹名和文件名保持一致。
  - 添加一个名叫`noConflict()`的方法将到处模块设置为前一个版本并返回这个模块。
  - 在模块顶部总是要声明`'use strict';` 。

    ```javascript
    // fancyInput/fancyInput.js

    !function (global) {
      'use strict';

      var previousFancyInput = global.FancyInput;

      function FancyInput(options) {
        this.options = options || {};
      }

      FancyInput.noConflict = function noConflict() {
        global.FancyInput = previousFancyInput;
        return FancyInput;
      };

      global.FancyInput = FancyInput;
    }(this);
    ```


## jQuery

  -jQuery对象的变量命名以`$`开头。

    ```javascript
    // 不推荐
    var sidebar = $('.sidebar');

    // 推荐
    var $sidebar = $('.sidebar');
    ```

  - 缓存jQuery选择器结果。

    ```javascript
    // 不推荐
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // 推荐
    function setSidebar() {
      var $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - 使用级联形式`$('.sidebar ul')`或`$('.sidebar > ul')`来查找DOM元素。[jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - 使用`find`来查找某个范围内的jQuery对象。

    ```javascript
    // 不推荐
    $('ul', '.sidebar').hide();

    // 不推荐
    $('.sidebar').find('ul').hide();

    // 推荐
    $('.sidebar ul').hide();

    // 推荐
    $('.sidebar > ul').hide();

    // 推荐
    $sidebar.find('ul').hide();
    ```


## ECMAScript 5兼容性

  - 参考[Kangax](https://twitter.com/kangax/)的 ES5 [兼容性列表](http://kangax.github.com/es5-compat-table/)。


## 测试

  - **务必进行测试！**

    ```javascript
    function () {
      return true;
    }
    ```


## 性能

  - [On Layout & Web Performance](http://kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](http://jsperf.com/ya-string-concat)
  - Loading...


## 资源


**请阅读下面链接的内容**

  - [Annotated ECMAScript 5.1](http://es5.github.com/)

**工具**

  - 代码风格分析器
    + [JSHint](http://www.jshint.com/) - [Airbnb Style .jshintrc](https://github.com/airbnb/javascript/blob/master/linters/.jshintrc)
    + [JSCS](https://github.com/jscs-dev/node-jscs) - [Airbnb Style Preset](https://github.com/jscs-dev/node-jscs/blob/master/presets/airbnb.json)

**其他的代码风格指南**

  - [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
  - [jQuery Core Style Guidelines](http://docs.jquery.com/JQuery_Core_Style_Guidelines)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwldrn/idiomatic.js/)
  - [JavaScript Standard Style](https://github.com/feross/standard)

**其他的代码风格**

  - [Naming this in nested functions](https://gist.github.com/4135065) - Christian Johansen
  - [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52) - Ross Allen
  - [Popular JavaScript Coding Conventions on Github](http://sideeffect.kr/popularconvention/#javascript) - JeongHoon Byun
  - [Multiple var statements in JavaScript, not superfluous](http://benalman.com/news/2012/05/multiple-var-statements-javascript/) - Ben Alman

**深入阅读**

  - [Understanding JavaScript Closures](http://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll
  - [Basic JavaScript for the impatient programmer](http://www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer
  - [You Might Not Need jQuery](http://youmightnotneedjquery.com/) - Zack Bloom & Adam Schwartz
  - [ES6 Features](https://github.com/lukehoban/es6features) - Luke Hoban
  - [Frontend Guidelines](https://github.com/bendc/frontend-guidelines) - Benjamin De Cock

**参考书籍**

  - [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
  - [Secrets of the JavaScript Ninja](http://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig and Bear Bibeault
  - [Human JavaScript](http://humanjavascript.com/) - Henrik Joreteg
  - [Superhero.js](http://superherojs.com/) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
  - [JSBooks](http://jsbooks.revolunet.com/) - Julien Bouquillon
  - [Third Party JavaScript](http://manning.com/vinegar/) - Ben Vinegar and Anton Kovalyov
  - [Effective JavaScript: 68 Specific Ways to Harness the Power of JavaScript](http://amzn.com/0321812182) - David Herman
  - [Eloquent JavaScript](http://eloquentjavascript.net) - Marijn Haverbeke
  - [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS) - Kyle Simpson

**博客**

  - [DailyJS](http://dailyjs.com/)
  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](http://weblog.bocoup.com/)
  - [Adequately Good](http://www.adequatelygood.com/)
  - [NCZOnline](http://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [Dustin Diaz](http://dustindiaz.com/)
  - [nettuts](http://net.tutsplus.com/?s=javascript)

**播客**

  - [JavaScript Jabber](http://devchat.tv/js-jabber/)


## License

(The MIT License)

Copyright (c) 2014 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
