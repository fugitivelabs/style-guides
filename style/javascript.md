## JavaScript Style Guide

----
* [Naming Conventions](#naming-conventions)
  * [Naming private methods and properties](#naming-private-methods-and-properties)
  * [File names](#file-names)
* [Syntax](#syntax)
  * [Indentation](#indentation)
  * [Braces](#braces)
  * [Spaces](#spaces)
  * [Line length](#line-length)
  * [require() lines.](#require-lines)
* [Comments and documentation](#comments-and-documentation)
  * [Inline Comments](#inline-comments)
  * [Top level file and class comments](#top-level-file-and-class-comments)
  * [Methods and properties comments](#methods-and-properties-comments)
* [Core language rules](#core-language-rules)
  * [Equality](#equality)
  * [Array and Object literals](#array-and-object-literals)
  * [Use a new var statement for each declaration](#use-a-new-var-statement-for-each-declaration)
  * [Avoid href="#" for JavaScript triggers](#avoid-href-for-javascript-triggers)
  * [Use modules, not global variables](#use-modules-not-global-variables)
* [ES6/7 rules](#es67-rules)
  * [Use =&gt; instead of bind(this)](#use--instead-of-bindthis)
  * [Use backticks for string interpolation](#use-backticks-for-string-interpolation)
  * [Use ES6 classes for React classes](#use-es6-classes-for-react-classes)
  * [Do not use async/await or generators](#do-not-use-asyncawait-or-generators)
  * [Do not use Set or Map](#do-not-use-set-or-map)
  * [Use let and const for new files; do not use var](#use-let-and-const-for-new-files-do-not-use-var)


----

This guide is adapted from the Khan Academy and AirBng style guides.

----------

## Naming Conventions

#### Files

```
genericFileNamesLikeThis.js
ModelNamesLikeThis.js
JsxFileNamesLikeThis.js.jsx // show the extension
```
Filenames should be verbose enough to determine what you're looking at by glancing at the top of your editor.

  > Why? When working in SublimeText or Atom or most any other editor, it's a terrible experience to have mulitple files open all named `controller.js`

```js
// bad
resources/
  |_ products/
      |_ controller.js
      |_ model.js
      |_ api.js

// better, but still bad
resources/
  |_ products/
      |_ products.js
      |_ Product.js
      |_ api.js

// best
resources/
  |_ products/
      |_ productsController.js
      |_ ProductModel.js
      |_ productApi.js


```


#### Namespaceing

Avoid single letter names. Be descriptive with your naming. eslint: [`id-length`](http://eslint.org/docs/rules/id-length)

```javascript
  // bad
  function q() {
    // ...
  }

  // good
  function query() {
    // ...
  }
```

Use camelCase when naming objects, functions, and instances. eslint: [`camelcase`](http://eslint.org/docs/rules/camelcase.html) jscs: [`requireCamelCaseOrUpperCaseIdentifiers`](http://jscs.info/rule/requireCamelCaseOrUpperCaseIdentifiers)

```javascript
  // bad
  const OBJEcttsssss = {};
  const this_is_my_object = {};
  function c() {}

  // good
  const thisIsMyObject = {};
  function thisIsMyFunction() {}
```

Use PascalCase only when naming constructors or classes. eslint: [`new-cap`](http://eslint.org/docs/rules/new-cap.html) jscs: [`requireCapitalizedConstructors`](http://jscs.info/rule/requireCapitalizedConstructors)

```javascript
  // bad
  function user(options) {
    this.name = options.name;
  }

  const bad = new user({
    name: 'nope',
  });

  // good
  class User {
    constructor(options) {
      this.name = options.name;
    }
  }

  const good = new User({
    name: 'yup',
  });
```




Private methods and properties (in files, classes, and namespaces)
should be named with a leading underscore.

While we do not currently use any compilers to enforce this, clients
of an API or class are expected to respect these conventions.

```js
function _PrivateClass() {
    // should not be instantiated outside of this file
}

function PublicClass(param) {
    this.publicMember = param;
    this._privateMember = new _PrivateClass();
}

var x = new _PrivateClass();  // OK - we’re in the same file.
var y = new PublicClass();    // OK
var z = y._privateMember;     // NOT OK!
```

Rationale: leading underscores for private methods and properties is
consistent with the styles used in numerous JavaScript libraries, many
of which we include in our code base.


Don't save references to `this`. Use arrow functions or [Function#bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind). jscs: [`disallowNodeTypes`](http://jscs.info/rule/disallowNodeTypes)

```javascript
  // bad
  function foo() {
    const self = this;
    return function () {
      console.log(self);
    };
  }

  // bad
  function foo() {
    const that = this;
    return function () {
      console.log(that);
    };
  }

  // good
  function foo() {
    return () => {
      console.log(this);
    };
  }
```

A base filename should exactly match the name of its default export.

```javascript
  // file 1 contents
  class CheckBox {
    // ...
  }
  export default CheckBox;

  // file 2 contents
  export default function fortyTwo() { return 42; }

  // file 3 contents
  export default function insideDirectory() {}

  // in some other file
  // bad
  import CheckBox from './checkBox'; // PascalCase import/export, camelCase filename
  import FortyTwo from './FortyTwo'; // PascalCase import/filename, camelCase export
  import InsideDirectory from './InsideDirectory'; // PascalCase import/filename, camelCase export

  // bad
  import CheckBox from './check_box'; // PascalCase import/export, snake_case filename
  import forty_two from './forty_two'; // snake_case import/filename, camelCase export
  import inside_directory from './inside_directory'; // snake_case import, camelCase export
  import index from './inside_directory/index'; // requiring the index file explicitly
  import insideDirectory from './insideDirectory/index'; // requiring the index file explicitly

  // good
  import CheckBox from './CheckBox'; // PascalCase export/import/filename
  import fortyTwo from './fortyTwo'; // camelCase export/import/filename
  import insideDirectory from './insideDirectory'; // camelCase export/import/directory name/implicit "index"
  // ^ supports both insideDirectory.js and insideDirectory/index.js
```

Use camelCase when you export-default a function. Your filename should be identical to your function's name.

```javascript
  function makeStyleGuide() {
    // ...
  }

  export default makeStyleGuide;
```

Use PascalCase when you export a constructor / class / singleton / function library / bare object.

```javascript
  const FugitiveLabsStyleGuide = {
    es6: {
    },
  };

  export default FugitiveLabsStyleGuide;
```

Acronyms and initialisms should always be all capitalized, or all lowercased.

  > Why? Names are for readability, not to appease a computer algorithm.

```javascript
  // bad
  import SmsContainer from './containers/SmsContainer';

  // bad
  const HttpRequests = [
    // ...
  ];

  // good
  import SMSContainer from './containers/SMSContainer';

  // good
  const HTTPRequests = [
    // ...
  ];

  // best
  import TextMessageContainer from './containers/TextMessageContainer';

  // best
  const Requests = [
    // ...
  ];
```

## Syntax

#### Whitespace

**Always** use soft tabs (space character) set to 2 spaces. eslint: [`indent`](http://eslint.org/docs/rules/indent.html) jscs: [`validateIndentation`](http://jscs.info/rule/validateIndentation)

```javascript
  // bad
  function foo() {
  ∙∙∙∙let name;
  }

  // bad
  function bar() {
  ∙let name;
  }

  // good
  function baz() {
  ∙∙let name;
  }
```

Place 1 space before the leading brace. eslint: [`space-before-blocks`](http://eslint.org/docs/rules/space-before-blocks.html) jscs: [`requireSpaceBeforeBlockStatements`](http://jscs.info/rule/requireSpaceBeforeBlockStatements)

```javascript
  // bad
  function test(){
    console.log('test');
  }

  // good
  function test() {
    console.log('test');
  }

  // bad
  dog.set('attr',{
    age: '1 year',
    breed: 'Bernese Mountain Dog',
  });

  // good
  dog.set('attr', {
    age: '1 year',
    breed: 'Bernese Mountain Dog',
  });
```

Place 1 space before the opening parenthesis in control statements (`if`, `while` etc.). Place no space between the argument list and the function name in function calls and declarations. eslint: [`keyword-spacing`](http://eslint.org/docs/rules/keyword-spacing.html) jscs: [`requireSpaceAfterKeywords`](http://jscs.info/rule/requireSpaceAfterKeywords)

```javascript
  // bad
  if(isJedi) {
    fight ();
  }

  // good
  if (isJedi) {
    fight();
  }

  // bad
  function fight () {
    console.log ('Swooosh!');
  }

  // good
  function fight() {
    console.log('Swooosh!');
  }
```

Set off operators with spaces. eslint: [`space-infix-ops`](http://eslint.org/docs/rules/space-infix-ops.html) jscs: [`requireSpaceBeforeBinaryOperators`](http://jscs.info/rule/requireSpaceBeforeBinaryOperators), [`requireSpaceAfterBinaryOperators`](http://jscs.info/rule/requireSpaceAfterBinaryOperators)

```javascript
  // bad
  const x=y+5;

  // good
  const x = y + 5;
```

End files with a single newline character. eslint: [`eol-last`](https://github.com/eslint/eslint/blob/master/docs/rules/eol-last.md)

```javascript
  // bad
  import { es6 } from './AirbnbStyleGuide';
    // ...
  export default es6;
  ```

  ```javascript
  // bad
  import { es6 } from './AirbnbStyleGuide';
    // ...
  export default es6;↵
  ↵
  ```

  ```javascript
  // good
  import { es6 } from './AirbnbStyleGuide';
    // ...
  export default es6;↵
```

Use indentation when making long method chains (more than 2 method chains). Use a leading dot, which
    emphasizes that the line is a method call, not a new statement. eslint: [`newline-per-chained-call`](http://eslint.org/docs/rules/newline-per-chained-call) [`no-whitespace-before-property`](http://eslint.org/docs/rules/no-whitespace-before-property)

```javascript
  // bad
  $('#items').find('.selected').highlight().end().find('.open').updateCount();

  // bad
  $('#items').
    find('.selected').
      highlight().
      end().
    find('.open').
      updateCount();

  // good
  $('#items')
    .find('.selected')
      .highlight()
      .end()
    .find('.open')
      .updateCount();

  // bad
  const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
      .attr('width', (radius + margin) * 2).append('svg:g')
      .attr('transform', `translate(${radius + margin},${radius + margin})`)
      .call(tron.led);

  // good
  const leds = stage.selectAll('.led')
      .data(data)
    .enter().append('svg:svg')
      .classed('led', true)
      .attr('width', (radius + margin) * 2)
    .append('svg:g')
      .attr('transform', `translate(${radius + margin},${radius + margin})`)
      .call(tron.led);

  // good
  const leds = stage.selectAll('.led').data(data);
```

Leave a blank line after blocks and before the next statement. jscs: [`requirePaddingNewLinesAfterBlocks`](http://jscs.info/rule/requirePaddingNewLinesAfterBlocks)

```javascript
  // bad
  if (foo) {
    return bar;
  }
  return baz;

  // good
  if (foo) {
    return bar;
  }

  return baz;

  // bad
  const obj = {
    foo() {
    },
    bar() {
    },
  };
  return obj;

  // good
  const obj = {
    foo() {
    },

    bar() {
    },
  };

  return obj;

  // bad
  const arr = [
    function foo() {
    },
    function bar() {
    },
  ];
  return arr;

  // good
  const arr = [
    function foo() {
    },

    function bar() {
    },
  ];

  return arr;
```

Do not pad your blocks with blank lines. eslint: [`padded-blocks`](http://eslint.org/docs/rules/padded-blocks.html) jscs:  [`disallowPaddingNewlinesInBlocks`](http://jscs.info/rule/disallowPaddingNewlinesInBlocks)

```javascript
  // bad
  function bar() {

    console.log(foo);

  }

  // also bad
  if (baz) {

    console.log(qux);
  } else {
    console.log(foo);

  }

  // good
  function bar() {
    console.log(foo);
  }

  // good
  if (baz) {
    console.log(qux);
  } else {
    console.log(foo);
  }
```

Do not add spaces inside parentheses. eslint: [`space-in-parens`](http://eslint.org/docs/rules/space-in-parens.html) jscs: [`disallowSpacesInsideParentheses`](http://jscs.info/rule/disallowSpacesInsideParentheses)

```javascript
  // bad
  function bar( foo ) {
    return foo;
  }

  // good
  function bar(foo) {
    return foo;
  }

  // bad
  if ( foo ) {
    console.log(foo);
  }

  // good
  if (foo) {
    console.log(foo);
  }
```

Do not add spaces inside brackets. eslint: [`array-bracket-spacing`](http://eslint.org/docs/rules/array-bracket-spacing.html) jscs: [`disallowSpacesInsideArrayBrackets`](http://jscs.info/rule/disallowSpacesInsideArrayBrackets)

```javascript
  // bad
  const foo = [ 1, 2, 3 ];
  console.log(foo[ 0 ]);

  // good
  const foo = [1, 2, 3];
  console.log(foo[0]);
```

Add spaces inside curly braces. eslint: [`object-curly-spacing`](http://eslint.org/docs/rules/object-curly-spacing.html) jscs: [`requireSpacesInsideObjectBrackets`](http://jscs.info/rule/requireSpacesInsideObjectBrackets)

```javascript
  // bad
  const foo = {clark: 'kent'};

  // good
  const foo = { clark: 'kent' };
```

Avoid having lines of code that are longer than 100 characters (including whitespace). Note: per [above](#strings--line-length), long strings are exempt from this rule, and should not be broken up. eslint: [`max-len`](http://eslint.org/docs/rules/max-len.html) jscs: [`maximumLineLength`](http://jscs.info/rule/maximumLineLength)

  > Why? This ensures readability and maintainability.

```javascript
  // bad
  const foo = jsonData && jsonData.foo && jsonData.foo.bar && jsonData.foo.bar.baz && jsonData.foo.bar.baz.quux && jsonData.foo.bar.baz.quux.xyzzy;

  // bad
  $.ajax({ method: 'POST', url: 'https://airbnb.com/', data: { name: 'John' } }).done(() => console.log('Congratulations!')).fail(() => console.log('You have failed this city.'));

  // good
  const foo = jsonData
    && jsonData.foo
    && jsonData.foo.bar
    && jsonData.foo.bar.baz
    && jsonData.foo.bar.baz.quux
    && jsonData.foo.bar.baz.quux.xyzzy;

  // good
  $.ajax({
    method: 'POST',
    url: 'https://airbnb.com/',
    data: { name: 'John' },
  })
    .done(() => console.log('Congratulations!'))
    .fail(() => console.log('You have failed this city.'));
```

#### Commas

Slightly controversially, we prefer leading commas for readability, ease of commenting, and eliminating the trailing comma problem.

```javascript
  // bad
  const story = [
    once,
    upon,
    aTime,
  ];

  // good
  const story = [
    once
    , upon
    , aTime
  ];

  // bad
  const hero = {
    firstName: 'Ada',
    lastName: 'Lovelace',
    birthYear: 1815,
    superPower: 'computers',
  };

  // good
  const hero = {
      firstName: 'Ada'
    , lastName: 'Lovelace'
    , birthYear: 1815
    , superPower: 'computers'
  };

```




#### Semicolons

**Yup.** eslint: [`semi`](http://eslint.org/docs/rules/semi.html) jscs: [`requireSemicolons`](http://jscs.info/rule/requireSemicolons)

```javascript
  // bad
  (function () {
    const name = 'Skywalker'
    return name
  })()

  // good
  (function () {
    const name = 'Skywalker';
    return name;
  }());

  // good, but legacy (guards against the function becoming an argument when two files with IIFEs are concatenated)
  ;((() => {
    const name = 'Skywalker';
    return name;
  })());
```

[Read more](https://stackoverflow.com/questions/7365172/semicolon-before-self-invoking-function/7365214%237365214).



#### Braces

Braces should always be used on blocks.

`if/else/for/while/try` should always have braces and always go on
multiple lines, with the opening brace on the same line.

Yes:
```js
if (true) {
    blah();
}
```

`else/else if/catch` should go on the same line as the brace:

```js
if (blah) {
    baz();
} else {
    baz2();
}
```

No:
```js
if (true)
    blah();
```

#### `require()` & `import` lines.

Import or `require()` base library code first.

Separate first party and third party `require()` lines, and sort
`require()` lines.

"First party" code is anything we wrote whose primary source lives in
the repository its being used in.  Lodash is third party because
we didn't write it.  

Imports should be sorted lexicographically (as per unix `sort`).

Yes:
```js
// import base library
var React = require("react");

var $ = require("jquery");
var _ = require("lodash");

var APIActionResults = require("../shared-package/api-action-results.js");
var Cookies = require("../shared-package/cookies.js");
var DashboardActions = require('./datastores/dashboard-actions.js');
var HappySurvey = require("../missions-package/happy-survey.jsx");
var UserMission = require("../missions-package/user-mission.js");
var cookieStoreRenderer = require("../shared-package/cookie-store.handlebars");
```

No:
```js
var $ = require("jquery");
var APIActionResults = require("../shared-package/api-action-results.js");
var Cookies = require("../shared-package/cookies.js");
var cookieStoreRenderer = require("../shared-package/cookie-store.handlebars");
var _ = require("lodash");
var HappySurvey = require("../missions-package/happy-survey.jsx");
var DashboardActions = require('./datastores/dashboard-actions.js');
var React = require("react");
var UserMission = require("../missions-package/user-mission.js");
var Kicksend = require("../../third_party/javascript-khansrc/mailcheck/mailcheck.js");
```

Object destructuring should go after all require lines.

Write requires on a single line, even if they extend past 80 chars, so they are easier to sort.

Yes:
```js
// import base library
import React from 'react';

// import third-party libraries
import _ from 'lodash';

// import
import ProductList from './ProductList.js.jsx';
import ProductListItem from './ProductListItem.js.jsx';

// import
import { TextInput, NumberInput } from '../global/components/forms';

```

No:
```js
import _ from 'lodash';
import { TextInput, NumberInput } from '../global/components/forms';
import React from 'react';
import ProductList from './ProductList.js.jsx';

```


------------------------------
### Comments and documentation

Use `/** ... */` for multi-line comments.

```javascript
  // bad
  // make() returns a new element
  // based on the passed in tag name
  //
  // @param {String} tag
  // @return {Element} element
  function make(tag) {

    // ...

    return element;
  }

  // good
  /**
   * make() returns a new element
   * based on the passed-in tag name
   */
  function make(tag) {

    // ...

    return element;
  }
```

Use `//` for single line comments. Place single line comments on a newline above the subject of the comment. Put an empty line before the comment unless it's on the first line of a block.

```javascript
  // bad
  const active = true;  // is current tab

  // good
  // is current tab
  const active = true;

  // bad
  function getType() {
    console.log('fetching type...');
    // set the default type to 'no type'
    const type = this.type || 'no type';

    return type;
  }

  // good
  function getType() {
    console.log('fetching type...');

    // set the default type to 'no type'
    const type = this.type || 'no type';

    return type;
  }

  // also good
  function getType() {
    // set the default type to 'no type'
    const type = this.type || 'no type';

    return type;
  }
```

Start all comments with a space to make it easier to read. eslint: [`spaced-comment`](http://eslint.org/docs/rules/spaced-comment)

```javascript
  // bad
  //is current tab
  const active = true;

  // good
  // is current tab
  const active = true;

  // bad
  /**
   *make() returns a new element
   *based on the passed-in tag name
   */
  function make(tag) {

    // ...

    return element;
  }

  // good
  /**
   * make() returns a new element
   * based on the passed-in tag name
   */
  function make(tag) {

    // ...

    return element;
  }
```

Prefixing your comments with `FIXME` or `TODO` helps other developers quickly understand if you're pointing out a problem that needs to be revisited, or if you're suggesting a solution to the problem that needs to be implemented. These are different than regular comments because they are actionable. The actions are `FIXME: -- need to figure this out` or `TODO: -- need to implement`.

Use `// FIXME:` to annotate problems.

```javascript
  class Calculator extends Abacus {
    constructor() {
      super();

      // FIXME: shouldn't use a global here
      total = 0;
    }
  }
```

Use `// TODO:` to annotate solutions to problems.

```javascript
  class Calculator extends Abacus {
    constructor() {
      super();

      // TODO: total should be configurable by an options param
      this.total = 0;
    }
  }
```



#### Methods and properties comments

All non-trivial methods and properties should also have JSDoc comments.

Type annotations are strongly encouraged; if there is even a slight
chance that the type will be ambiguous to future readers, put in a
type annotation.

Type annotations are based on the ES4/JS2 type system, and are
documented in the [Google JavaScript style
guide](https://google.github.io/styleguide/javascriptguide.xml).

`@param` and `@return` type annotations that have comments that do not
fit on one line wrap to the next line and indent 2 spaces.

Example:

```js
/**
 * A UI component allows users to select from a list of exercises within
 * a table.
 * Expects an array of exercise IDs as a prop.
 */

class ExerciseList extends Base {
  constructor(props) {
    super(props);
    /**
     * Check if the exercise list is available at render,
     * otherwise, set the state to an empty array
     */
    this.state = {
      exercises: props.exercises || []
    }
    this._bind(
      '_showExercise'
    )
  }

  /**
   * Determines which exercise to display
   * @param {string=} id the database ID of the exercise
   */
  _showExercise(id) {
    ...
  }
  ...
}
```

#### Console logs

Only use `console.log(...)` during active development. If you feel they're important enough to keep in the file for future testing/debugging, please comment them out prior to pushing to git. **NEVER** deploy console logs in production deployment.

  > Why? A few reasons.  
    1. This causes memory leaks in the client (particularly in ReactNative).
    2. They also naturally tend to build up exponentially over time making it **MORE** difficult to debug.  
    3. It's ugly

Prefer verbose documentation in with comments.

-----------------------

## Core language rules

#### References
Use `const` for all of your references; avoid using `var`. eslint: [`prefer-const`](http://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](http://eslint.org/docs/rules/no-const-assign.html)

> Why? This ensures that you can't reassign your references, which can lead to bugs and difficult to comprehend code.

```javascript
  // bad
  var a = 1;
  var b = 2;

  // good
  const a = 1;
  const b = 2;
```

If you must reassign references, use `let` instead of `var`. eslint: [`no-var`](http://eslint.org/docs/rules/no-var.html) jscs: [`disallowVar`](http://jscs.info/rule/disallowVar)

> Why? `let` is block-scoped rather than function-scoped like `var`.

```javascript
  // bad
  var count = 1;
  if (true) {
    count += 1;
  }

  // good, use the let.
  let count = 1;
  if (true) {
    count += 1;
  }
```

Note that both `let` and `const` are block-scoped.

```javascript
// const and let only exist in the blocks they are defined in.
{
  let a = 1;
  const b = 1;
}
console.log(a); // ReferenceError
console.log(b); // ReferenceError
```

If you must use `var` create new statement for each declaration.
Yes:
```js
var a = "foo";
var b = a + "bar";
var c = fn(a, b);
```

No:
```js
var a = "foo",
b = a + "bar",
c = fn(a, b);
```

A single var statement is bad because:

* If you forget a comma, you just made a global
* It originated when people wanted to save bytes, but we have a minifier
* It makes line-based diffs/editing messier
* It encourages C89-style declarations at the top of scope, preventing
you from only declaring vars before first use, the latter preferable
as it conveys intended scope to the reader


#### Strings


Use single quotes `''` for strings. eslint: [`quotes`](http://eslint.org/docs/rules/quotes.html) jscs: [`validateQuoteMarks`](http://jscs.info/rule/validateQuoteMarks)

```javascript
// bad
const name = "Capt. Janeway";

// bad - template literals should contain interpolation or newlines
const name = `Capt. Janeway`;

// good
const name = 'Capt. Janeway';
```

Strings that cause the line to go over 100 characters should not be written across multiple lines using string concatenation.

    > Why? Broken strings are painful to work with and make code less searchable.

```javascript
   // bad
   const errorMessage = 'This is a super long error that was thrown because \
   of Batman. When you stop to think about how Batman had anything to do \
   with this, you would get nowhere \
   fast.';

   // bad
   const errorMessage = 'This is a super long error that was thrown because ' +
     'of Batman. When you stop to think about how Batman had anything to do ' +
     'with this, you would get nowhere fast.';

   // good
   const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
   ```

When programmatically building up strings, use template strings instead of concatenation. eslint: [`prefer-template`](http://eslint.org/docs/rules/prefer-template.html) [`template-curly-spacing`](http://eslint.org/docs/rules/template-curly-spacing) jscs: [`requireTemplateStrings`](http://jscs.info/rule/requireTemplateStrings)

   > Why? Template strings give you a readable, concise syntax with proper newlines and string interpolation features.

```javascript
  // bad
  function sayHi(name) {
    return ['How are you, ', name, '?'].join();
  }

  // bad
  function sayHi(name) {
    return `How are you, ${ name }?`;
  }

  // good
  function sayHi(name) {
    return `How are you, ${name}?`;
  }
```

Never use `eval()` on a string, it opens too many vulnerabilities.

Do not unnecessarily escape characters in strings. eslint: [`no-useless-escape`](http://eslint.org/docs/rules/no-useless-escape)

> Why? Backslashes harm readability, thus they should only be present when necessary.

```javascript
// bad
const foo = '\'this\' \i\s \"quoted\"';

// good
const foo = '\'this\' is "quoted"';
const foo = `my name is '${name}'`;
```



#### Comparison Operators & Equality

Prefer `===` (strict equality) to `==` due to the [numerous oddities
related to JavaScript's type coercion](https://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/).

The only valid use of `==` is for comparing against null and undefined
at the same time:

```js
// Check null and undefined, but distinguish between other falsey values
if (someVariable == null) {
  ...
}
```

Though you will often want to just check against falsey values, and
can just say `if (!someVariable) {...}`.


Conditional statements such as the `if` statement evaluate their expression using coercion with the `ToBoolean` abstract method and always follow these simple rules:

  + **Objects** evaluate to **true**
  + **Undefined** evaluates to **false**
  + **Null** evaluates to **false**
  + **Booleans** evaluate to **the value of the boolean**
  + **Numbers** evaluate to **false** if **+0, -0, or NaN**, otherwise **true**
  + **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

```javascript
  if ([0] && []) {
    // true
    // an array (even an empty one) is an object, objects will evaluate to true
  }
```

Use shortcuts for booleans, but explicit comparisons for strings and numbers.

```javascript
  // bad
  if (isValid === true) {
    // ...
  }

  // good
  if (isValid) {
    // ...
  }

  // bad
  if (name) {
    // ...
  }

  // good
  if (name !== '') {
    // ...
  }

  // bad
  if (collection.length) {
    // ...
  }

  // good
  if (collection.length > 0) {
    // ...
  }
```

For more information see [Truth Equality and JavaScript](https://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.

Use braces to create blocks in `case` and `default` clauses that contain lexical declarations (e.g. `let`, `const`, `function`, and `class`).

  > Why? Lexical declarations are visible in the entire `switch` block but only get initialized when assigned, which only happens when its `case` is reached. This causes problems when multiple `case` clauses attempt to define the same thing.

  eslint rules: [`no-case-declarations`](http://eslint.org/docs/rules/no-case-declarations.html).

```javascript
  // bad
  switch (foo) {
    case 1:
      let x = 1;
      break;
    case 2:
      const y = 2;
      break;
    case 3:
      function f() {
        // ...
      }
      break;
    default:
      class C {}
  }

  // good
  switch (foo) {
    case 1: {
      let x = 1;
      break;
    }
    case 2: {
      const y = 2;
      break;
    }
    case 3: {
      function f() {
        // ...
      }
      break;
    }
    case 4:
      bar();
      break;
    default: {
      class C {}
    }
  }
```

Ternaries should not be nested and generally be single line expressions. eslint rules: [`no-nested-ternary`](http://eslint.org/docs/rules/no-nested-ternary.html).

```javascript
  // bad
  const foo = maybe1 > maybe2
    ? "bar"
    : value1 > value2 ? "baz" : null;

  // better
  const maybeNull = value1 > value2 ? 'baz' : null;

  const foo = maybe1 > maybe2
    ? 'bar'
    : maybeNull;

  // best
  const maybeNull = value1 > value2 ? 'baz' : null;

  const foo = maybe1 > maybe2 ? 'bar' : maybeNull;
```

Avoid unneeded ternary statements. eslint rules: [`no-unneeded-ternary`](http://eslint.org/docs/rules/no-unneeded-ternary.html).

```javascript
  // bad
  const foo = a ? a : b;
  const bar = c ? true : false;
  const baz = c ? false : true;

  // good
  const foo = a || b;
  const bar = !!c;
  const baz = !c;
```

#### Array and Object literals

Always use `[]` and `{}` style literals to initialize arrays and
objects, not the `Array` and `Object` constructors.

Array constructors are error-prone due to their arguments: `new
Array(3)` yields `[undefined, undefined, undefined]`, not `[3]`.

To avoid these kinds of weird cases, always use the more readable
array literal.

Object constructors don't have the same problems, but follow the same
rule for consistency with arrays.  Plus, `{}` is more readable.



#### Avoid `href="#"` for JavaScript triggers

When you want a link-like thing rather than a button to trigger a
JavaScript operation, rather than going to a new address.

Here's a discussion on Stack Overflow about options:
http://stackoverflow.com/questions/134845/href-tag-for-javascript-links-or-javascriptvoid0


Yes:
```js
<a href="javascript:void 0">Flag</a>
```

No:
```js
<a href="#">Flag</a>
```

---------------
### ES6/7 rules

Several of our supported browsers support only ES5 natively.  We use
polyfills to emulate [some -- but not all -- ES6 and ES7 language
features](https://docs.google.com/spreadsheets/d/12mF99oCpERzLKS07wPPV3GiISUa8bPkKveuHsESDYHU/edit#gid=0)
so they run on ES5-capable browsers.

In some cases, we do not yet allow a new language feature, if it's
expensive to polyfill.  In others, we require using the newer language
feature and avoiding the old:

| Construct | Use...                                | ...instead of |
| --------- | ------------------------------------- | ---------------------- |
| backticks | `` `http://${host}/${path}` `` | `"http://" + host + "/" + path` |
| destructuring | `var {x, y} = a;` | `var x = a.x; var y = a.y;` |
| fat arrow | `foo(() => {...})` | `foo(function() {...}.bind(this))` |
| let/const | `let a = 1; const b = "4EVAH"; a++;` | `var a = 1; var b = "4EVAH"; a++;` |
| includes | `array.includes(item)` | `array.indexOf(item) !== -1` |
| for/of | `for (const [key, value] of Object.entries(obj)) {...}` | `_.each(obj, function(value, key) {...})` |
| spread | `{...a, ...b, c: d}` | `_.extend({}, a, b, {c: d})` |
| rest params | `function(bar, ...args) {foo(...args);}` | `function(bar) {var args = Array.prototype.slice.call(arguments, 1); foo.apply(null, args);}` |

#### Use `=>` instead of `bind(this)`

Arrow functions are easier to read (and with Babel, more efficient)
than calling `bind` manually.

#### Use rest params instead of `arguments`

The magic `arguments` variable has some odd quirks. It's simpler to
use rest params like `(...args) => foo(args)`.

#### Use backticks for string interpolation

`+` is not forbidden, but backticks are encouraged!

#### Use ES6 classes for React classes

See [React Use ES6 classes](react.md#use-es6-classes) for details.

For classes outside of React -- which should actually be pretty rare -- you
should also use ES6 classes.  Some things to keep in mind when using ES6
classes:

- Use `static` properties instead of adding properties to the class object
  after defining the class.
- Use `extend` syntax for inheritance.
