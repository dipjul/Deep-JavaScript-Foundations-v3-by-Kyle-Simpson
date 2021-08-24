

Deep JavaScript Foundations v3 by Kyle Simpson

****

## Contents

1. [Types](#1.-Types)
   1. [Primitive Types](#1.1-Primitive-Types)
   2. [Abstract Operations](#1.2-Abstract-Operations)
   3. [Coercion](#1.3-Coercion)
   4. [Equality](#1.4-Equality)
   5. [TypeScript, Flow, etc](#1.5-TypeScript,-Flow,-etc)

2. [Scope](#2-Scope)
   1. [Nested Scope](#2.1-Nested-Scope)
   2. [Hoisting](#2.2-Hoisting)
   3. [Closure](#2.3-Closure)
   4. [Modules](#2.4-Modules)

3. [Objects (Oriented)](#3-Objects-(Oriented))
   1. [this](#3.1-this)
   2. [class {}](#3.2-class{-})
   3. [Prototypes](#3.3-Prototypes)
   4. [OO vs OLOO](#3.3-OO-vs-OLOO)



<hr />

## 1. Types

> In JavaScript, everything is an object

​	It is a false statement, in fact 'false' is not an object

> **6.1 ECMAScript Language Types**
>
> An ECMAScript language type corresponds to values that are directly manipulated by an ECMAScript programmer using the ECMAScript language. The ECMAScript language types are Undefined, Null, Boolean, String, Symbol, Number, and Object. An *ECMAScript language* value is a value that is characterized by an ECMAScript language type.

​	Mentioned above types are Primitive Types

### 1.1 Primitive Types

- undefined

- string

- number

- boolean

- object

- symbol

- ~~undeclared~~ ? sort of but not

- *null* It's little bit corky, a historical bug

- ~~function~~ ? Subtype of object

- ~~array~~ ? Subtype of object

- ~~bigint~~ ? coming to JavaScript, still not here (it will be a primitive type)

  

  In JavaScript, variables don't have types, values do.

  ```javascript
  var v;
  typeof v;	// "undefined"
  v = "1";
  typeof v;	// "string"
  v = 2;
  typeof v;	// "number"
  v = true;
  typeof v;	// "boolean"
  v = {};
  typeof v;	// "object"
  v = Symbol();
  typeof v;	// "symbol"
  typeof doesntExist; // "undefined"
  var v = null;
  typeof v;	// "object"
  v = function(){};
  typeof v;	// "function" 
  v = [1,2,3];
  typeof v;	// "object"
  	//comming soon!
  	v = 42n;
  	// or: BigInt(42)
  	typeof v;	// "bigint"
  ```

  - `typeof` always return a string

  - `null` is mentioned in ECMAScript for resetting a object (a bug to the language, we might say so), while resetting other types is done by using `undefined`

  - `function` and `array` cases are also historical, we can't change it, learn it and make a way around it
  - 42n can grow infinitely large, up to the size of memory



- **Types of emptiness:** 

  - `undefined vs undeclared vs uninitialized`

- **Special Values:**

  - **NaN** 

    - NaN("~~not a number~~") it refers to an invalid number

    ```javascript
    var myAge = Number("0o46");		 // 38
    var myNextAge = Number("39");	 // 39
    var myCatsAge = Number("n/a");	 /* NaN "n/a" string is converted into not a
    							     number value*/
    myAge - "my son's age";			/* NaN '-' cna be used for numbers but there 
    								is string on other side, so JS converts
    								that string to a Not a Number value*/
    myCatsAge === myCatsAge;	     // false OOPSEE!
    isNaN(myAge);				    // false
    isNaN(myCatsAge);			    //  true
    isNaN("my son's age");			//  true OOPSEE!
    Number.isNaN(myCatsAge);		//  true
    Number.isNaN("my son's age");	 //  false
    ```

    
    - `NaN` operating with any mathematical operator will give you a `NaN`
    - `NaN` is the only value that is not equal to itself(IEEE)
    - `isNaN()` coerces the argument to a `Number`
    - `Number.isNaN()` doesn't coerce to a `Number`
    - `NaN`: Invalid Number 

  - **Negative Zero:**

    - 0 with the sign bit set

      ```javascript
      var trendRate = -0;
      trendRate === -0;                 // true
      
      trendRate.toString();             //  "0" OOPSEE!
      trendRate === 0;				//  true OOPSEE!
      trendRate < 0;					//  false
      trendRate > 0;					//  false
      
      Object.is(trendRate,-0);		 //  true
      Object.is(trendRate,0);			 //  false
      ```

    - Even the `===` lies, but `Object.is()` doesn't lie

      ```javascript
      Math.sign(-3);					//  -1
      Math.sign(3);					//  1
      Math.sign(-0);					//  -0 WTF?
      Math.sign(0);					//  0 WTF?
      
      // "fix" Math.sign(...)
      function sign(v) {
      return v !== 0 ? Math.sign(v) : Object.is(v,-0) ? -1 : 1;
      }
      
      sign(-3);						//  -1
      sign(3);						//  1
      sign(-0);						//  -1
      sign(0);						//  1
      ```

- **Fundamental Objects**

  - Use `new`:										Don't use `new`:

    - `Object()`									- `String()`

    - `Array()`                                      - `Number()`

    - `Function()`                                 - `Boolean()`

    - `Date()`

    - `RegExp()`

    - `Error()

      ```javascript
      var yesterday = new Date("March 6, 2020");
      yesterday.toUTCString();
      // "Wed, 06 Mar 2020 06:00:00 GMT"
      
      var myGPA = String(transcript.gpa);
      // "3.02"
      ```

      

### 1.2 Abstract Operations

> **7 Abstract Operations**
>
> These operations are not a part of the ECMAScript language; they are defined here to solely to aid the specification of the semantics of the ECMAScript language. Other, more specialized abstract operations are defined throughout this specification.
>
> **7.1 Type Conversion**
>
> The ECMAScript language implicitly performs automatic type conversion as needed. To clarify the semantics of certain constructs it is useful to defined a set of conversion abstract operations. The conversion abstract operations are polymorphic; they can accept a value of any ECMAScript language type. But no other specification types are used with these operations.

​	

- `ToPrimitive(hint)` 

  - hint: "number"			hint: "string"

    ​	`valueOf()`				`toString()`

    ​	`toString()`				`valueOf()`

- `ToString` 

  | Before      | After             |
  | :---------- | ----------------- |
  | `null`      | `"null"`          |
  | `undefined` | `"undefined"`     |
  | `true`      | `"true"`          |
  | `false`     | `"false"`         |
  | `3.14159`   | `"3.14159"`       |
  | `0`         | `"0"`             |
  | `-0`        | `"0"` **OOPSEE!** |

  - `ToString(object):` invokes `ToPrimitive(string)` 
    
  - aka: `toString()` then will call `valueOf()`
    
  - `ToString(Array)`:

    | Before             | After     |
    | ------------------ | --------- |
    | `[]`               | `""`      |
    | `[1,2,3]`          | `"1,2,3"` |
    | `[null,undefined]` | `","`     |
    | `[[[],[],[]],[]]`  | `",,,"`   |
    | `[,,,,]`           | `",,,"`   |

  

  - `ToString(Object)`:

    | Before                        | After               |
    | ----------------------------- | ------------------- |
    | `{}`                          | `"[object Object]"` |
    | `{a:2}`                       | `"[object Object]"` |
    | `{toString(){ return "X"; }}` | `"X"`               |

    

- `ToNumber` 

  | Before      | After         |
  | ----------- | ------------- |
  | `""`        | 0 **OOPSEE!** |
  | `"0"`       | 0             |
  | `"-0"`      | -0            |
  | `" 009 "`   | 9             |
  | `"3.14159"` | 3.14159       |
  | `"0."`      | 0             |
  | `".0"`      | 0             |
  | `"."`       | NaN           |
  | `"0xaf"`    | 175           |
  | `false`     | 0             |
  | `true`      | 1             |
  | `null`      | 0 **OOPSEE!** |
  | `undefined` | NaN           |

  - `ToNumber(object)`: invokes `ToPrimitive(number)`

    - aka: 1st it consult to `valueOf()` then it consult to `toString()`

    - for `[]` and `{}` by default:

      - `valueOf(){ return this; }` --> `toString()` 

      - **Coercion:** `ToNumber (Array)`:

        | Before        | After           |
        | ------------- | --------------- |
        | `[""]`        | `0` **OOPSEE!** |
        | `["0"]`       | `0`             |
        | `["-0"]`      | `-0`            |
        | `[null]`      | `0` **OOPSEE!** |
        | `[undefined]` | `0` **OOPSEE!** |
        | `[1,2,3]`     | `NaN`           |
        | `[[[[]]]]`    | `0` **OOPSEE!** |

      - **Coercion:** `ToNumber (Object)`:

        | Before                       | After |
        | ---------------------------- | ----- |
        | `{..}`                       | `NaN` |
        | `{ valueOf() { return 3; }}` | `3`   |

        

- `ToBoolean`:
  - It just do a lookup whether a value is in falsy list or not
  - **Falsy:**
    - `""`
    - 0, -0
    - `null`
    - `NaN`
    - `false`
    - `undefined`
  - **Truthy:** 
    - Everything else that is not in the **Falsy** list

### 1.3 Coercion

- People claim to avoid coercion because it's evil, but. . .

- **number to string** 

  ```javascript
  var numStudents = 16;
  console.log(
  	`There are ${numStudents} students.`
  );
  ```

  ​	Here, `numStudents` is coerced to a string type

  ```javascript
  var msg1 = "There are ";
  var numStudents = 16;
  var msg2 = " students.";
  console.log(msg1 + numStudents + msg2);
  ```

   	`+`operator is overloaded for doing different things in different scenarios. Whenever there is a string type of operand then the `+` operator does string concatenation. For doing so, it coercive other types to a string type.

  ​	<img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200607100949609.png" style="zoom:80%;" />

  ```javascript
  var numStudents = 16;
  console.log(
  	`There are ${numStudents.toString()} students.`
  );
  ```

  - **string to number** 

    ```javascript
    function addAStudent(numStudents) {
    	return numStudents + 1
    }
    
    addAStudent(studentsInputElem.value);
    // "161" OOPSEE!
    ```

    ```javascript
    function addAStudent(numStudents) {
    	return numStudents + 1
    }
    
    addAStudent(
        +studentsInputElem.value //unary '+'
    );
    // 17
    ```

    ```javascript
    function addAStudent(numStudents) {
    	return numStudents + 1
    }
    
    addAStudent(
        Number(studentsInputElem.value)
    );
    // 17
    ```

    ```javascript
    function kickStudentOut(numStudents) {
    	return numStudents - 1
    }
    
    kickStudentOut(
        studentsInputElem.value
    );
    // 15 '-' is not overloaded
    ```

    - **__ to boolean**

    ```javascript
    if (studentsInputElem.value) {
        numStudents = Number(studentsInputElem.value);
    }
    
    while (newStudents.length) {
        enrollStudent(newStudent.pop());
    }
    // conditions should be of boolean type but here we are not doing so,
    // instead javascript is implicitly doing for us.
    ```

    ```javascript
    if (!!studentsInputElem.value) {
        numStudents = Number(studentsInputElem.value);
    }
    
    while (newStudents.length > 0) {
        enrollStudent(newStudent.pop());
    }
    // it's better than the code above, here also same coercion is 
    // coming into picture
    ```

    ```javascript
    if (studentNameElem.value) {
        student.name = studentNameElem.value;
    }
    
    // ***************************************
    Boolean(""); //false
    Boolean("	\t\n"); // true OOPSEE!
    ```

    ```javascript
    var workshop = getRegistration(student);
    if (workshop) {
        console.log(
        	`Welcome ${student.name} to ${workshop.name}.`
        );
    }
    // *************************************************
    
    Boolean(undefined); // false
    Boolean(null);	   //  false
    Boolean({});	   //  trueHow we access
    ```

  - **Coercion: primitive to object:** 

  - **How can we use `.length()` etc on primitive values?**

    - That's boxing

    ```javascript
    if (studentNameElem.value.length > 50) {
        console.log("Student's name too long.");
    }
    ```

  - All programming languages have type conversions, because it's absolutely necessary.

  - You use coercion in JS whether you admit it or not, because you have to.

  - Every language has type conversion corner cases

    ```javascript
    Number( "" );						// 0	OOPSEE!
    Number( "	\t\n" );				// 0	OOPSEE!
    Number( null );						// 0	OOPSEE!
    Number( undefined );				// NaN	 
    Number( [] );						// 0	OOPSEE!
    Number( [1,2,3] );					// NaN	
    Number( [null] );					// 0	OOPSEE!
    Number( [undefined] );				 // 0	 OOPSEE!
    Number( {} );						// NaN
    
    String( -0 );						// "0"	 OOPSEE!
    String( null );						// "null"
    String( undefined );				// "undefined"
    String( [null] );					// ""	 OOPSEE!
    String( [undefined] );				// ""	  OOPSEE!
    Boolean( new Boolean(false) );		 // true	OOPSEE!
    ```

    <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200607224439513.png" alt="image-20200607224439513" style="zoom:80%;" />

    - You don't deal with these type conversion corner cases by avoiding coercions.

    - Instead, you have to adopt a coding style that makes value types plain and obvious.

    - A quality JS program embraces coercions, making sure the types involved in every operation are clear. Thus, corner cases are safely managed.

      - ~~Type Rigidity~~
      - ~~Static Types~~
      - ~~Type Soundness~~

    - JavaScript's dynamic typing is not a weakness, it's one of its strong qualities

      - Implicit != Magic
      - Implicit != Bad
      - Implicit: Abstracted

    - Hiding unnecessary details, re-focusing the reader and increasing clarity

    - Coercion: implicit can be good (sometimes)

      ```javascript
      var workshopEnrollment1 = 16;
      var workshopEnrollment2 = workshop2Elem.value;
      if (Number(workshopEnrollment1) < Number(workshoopEnrollment2)) {
          //
      }
      if (workshopEnrollment1 < workshoopEnrollment2) {
          // It appears better than the above code
          // if you are sure(you should be aware of the kind of input that 
          // the users must pass in) about your inputs than the explicit 
          // conversion is better. 
      }
      ```

      ```javascript
      var numStudents = 16;
      console.log(
      	`There are ${String(numStudents)} students.`
      );
      
      var numStudents = 16;
      console.log(
      	`There are ${numStudents} students.`
      );
      ```

    - Is showing the reader the extra type details helpful or distracting? **Both**

    - > ~~"If a feature is sometimes useful and sometimes dangerous and if there is a better option then always use the better option."~~
      >
      > ​                                                -- "The Good Parts", Crockford

    - Useful: when the reader is focused on what's important
    - Dangerous: when the reader can't tell what will happen
    - Better: when the reader understands the code
    - It is irresponsible to knowingly avoid usage of a feature that can *improve code readability*

  

  ### 1.4 Equality

  - **Equality**  == vs ===

  <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200608090050815.png" style="zoom:80%;" />

  ```javascript
  var studentName1 = "Frank";
  var studentName2 = `${studentName1}`;
  
  var workshopEnrollment1 = 16;
  var workshopEnrollment2 = workshopEnrollment1 + 0;
  
  studentName1 == studentName2;					// true
  studentName1 === studentName2;					// true
  
  workshopEnrollment1 == workEnrollment2;			 // true
  workshopEnrollment1 === workshopEnrollment2;	  // true
  ```

  <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200608090705903.png" style="zoom:80%;" />

  ```javascript
  var workshop1 = {
      name: "Deep JS Foundation"
  };
  var workshop2 = {
      name: "Deep JS Foundation"
  };
  
  if (workshop1 == workshop2) {
      // Nope
  }
  if (workshop1 === workshop2) {
      // Nope
  }
  ```

  - Here, Equality check does: identity equality check, not structure

  - **Coercive Equality vs. Non-Coercive Equality**

    - == allows coercion (types different)
    - === disallows coercion (types same)

  - **Coercive Equality: helpful?**

    - Like every other operation, is coercion helpful in an equality comparison or not?

  - **Coercive Equality: safe?**

    - Like every other operation, do we know the types or not?

  - **Coercive Equality: null == undefined**

    ```javascript
    var workshop1 = { topic: null };
    var workshop2 = {};
    if (
    	(workshop1.topic === null || workshop1.topic === undefined) &&
        (workshop2.topic === null || workshop2.topic === undefined) &&
    ) {
    	//    
    }
    if(
    	workshop1.topic === null && workshop1.topic === null
    ) {
        //
    }
    ```

  - **Coercive Equality: prefers numeric comparison**

    ```javascript
    var workshopEnrollment1 = 16;
    var workshopEnrollment2 = workshop2Elem.value;
    if (Number(workshopEnrollment1) === Number(workshoopEnrollment2)) {
        //
    }
    
    // Ask: what do we know about the types here?
    if (workshopEnrollment1 == workshoopEnrollment2) {
        // . 
    }
    ```

  - **Coercive Equality: only primitives**

    ```javascript
    var workshop1Count = 42;
    var workshop2Count = [42];
    
    // if (workshop1Count == workshop2Count) {
    // if (42 == "42") {
    // if (42 === 42) {
    if (workshop1Count == workshop2Count) {
        //
    }
    ```

  - **==** **Summary:** 

    - If the types are the same: ===
    - If null or undefined: equal 
    - If non-primitives: `ToPrimitive `
    - Prefer: `ToNumber`

  - **==** **Corner Cases**

    ```javascript
    [] == ![] // true
    var workshop1Students = [];
    var workshop2Students = [];
    
    if (workshop1Students == !workshop2Students) {
        
    }
    
    if (workshop1Students != workshop2Students) {
        
    }
    ```

    ```javascript
    var workshop1Students = [];
    var workshop2Students = [];
    
    // if (workshop1Students == !workshop2Students) {
    // if ([] == false) {
    // if ("" == false) {
    // if (0 == false) {
    // if (0 === 0) {
    if (true) {
        
    }
    
    // if (workshop1Students != workshop2Students) {
    // if (!(workshop1Students == workshop2Students)) {
    // if (!(false)) {
    if (true) {
    }
    ```

    ```javascript
    var workshopStudents = [];
    if (workshopStudents) {
    	// Yep
    }
    if (workshopStudents == true) {
        // Nope 
    }
    if (workshopStudents == false) {
        // Yep
    }
    ```

    ```javascript
    var workshopStudents = [];
    
    // if (workshopStudents) {
    // if (Boolean(workshopStudents)) {
    if (true) {
    	// Yep
    }
    // if (workshopStudents == true) {
    // if ("" == true) {
    // if (0 === 1 {
    if (false) {
        // Nope 
    }
    // if (workshopStudents == false) {
    // if ("" == false) {
    // if (0 == 0) {
    if (true) {
        // Yep
    }
    ```

  - **Avoid:**

    1. == with 0 or "" (or even " ") 
    2. == with non-primitives 
    3. == true or == false : allow 
    `ToBoolean` or use ===

  - **The case for preferring** **==**

    - Knowing types is always better than not knowing them
    - Static Types is not the only (or even necessarily best) way to know your types
    - == is not about comparisons with unknown types
    - == is about comparisons with known type(s), optionally where conversions are helpful

  - **If you know the type(s) in a** **comparison:**

    - If both types are the same, == is identical to ===
    - Using === would be unnecessary, so prefer the shorter ==
    - Since === is pointless when the types don't match, it's similarly unnecessary when they do match.
    - If the types are different, using one === would be broken
    - Prefer the more powerful == or don't compare at all
    - If the types are different, the equivalent of one == would be two (or more!) === (ie, "slower")
    - Prefer the "faster" single ==
    - If the types are different, two (or more!) === comparisons may distract the reader
    - Prefer the cleaner single ==
    - **Summary:** whether the types match or not, == is the more sensible choice

  - **If you don't know the type(s) in** **a comparison:**

    - Not knowing the types means not fully understanding that code
    - So, best to refactor so you can know the types
    - The uncertainty of not knowing types should be obvious to reader
    - The most obvious signal is ===
    - Not knowing the types is equivalent to assuming type conversion
    - Because of corner cases, the only safe choice is ===
    - **Summary:** if you can't or won't use known and obvious types, === is the only reasonable choice

  - Even if === would always be equivalent to == in your code, using it everywhere sends a wrong semantic signal: "Protecting myself since I don't know/trust the types"

  - **Summary:** making types known and obvious leads to better code. If types are known, == is best. Otherwise, fall back to ===

### 1.5 TypeScript, Flow, etc

- **Benefits:**
  1. Catch type-related mistakes 
  2. Communicate type intent 
  3. Provide IDE feedback
- **Caveats:**
  1. Inferencing is best-guess, not a 
  guarantee 
  2. Annotations are optional 
  3. Any part of the application that 
  isn't typed introduces uncertainty
- [Typescript vs Flowtype](https://github.com/niieani/typescript-vs-flowtype)
- **TypeScript & Flow:** **Pros**
  - They make types more obvious in code
  - Familiarity: they look like other language's type systems
  - Extremely popular these days
  - They're very sophisticated and good at what they do
- **TypeScript & Flow:** **Cons**
  - They use "non-JS-standard" syntax (or code comments)
  - They require* a build process, which raises the barrier to entry
  - Their sophistication can be intimidating to those without prior formal types experience
  - They focus more on "static types" (variables, parameters, returns, properties, etc) than value types
  - The only way to have confidence over the runtime behavior is to limit/eliminate dynamic typing
- **Alternative?**
  - [Typl](https://github.com/getify/Typl) 
  - Motivations:
    1. Only standard JS syntax 
    2. Compiler and Runtime (both optional) 
    3. Completely configurable (ie, ESLint) 
    4. Main focus: inferring or annotating values; Optional: "static typing" 
    5. With the grain of JS, not against it
- **Wrapping Up**
  - JavaScript has a (dynamic) type system, which uses various forms of coercion for value type conversion, including equality comparisons
  - However, the prevailing response seems to be: avoid as much of this system as possible, and use === to "protect" from needing to worry about types
  - Part of the problem with avoidance of whole swaths of JS, like pretending === saves you from needing to know types, is that it tends to systemically perpetuate bugs
  - You simply cannot write quality JS programs without knowing the types involved in your operations.
  - Alternately, many choose to adopt a different "static types" system layered on top
  - While certainly helpful in some respects, this is "avoidance" of a different sort
  - Apparently, JS's type system is inferior so it must be replaced, rather than learned and leveraged
  - Many claim that JS's type system is too difficult for newer devs to learn, and that static types are (somehow) more learnable
  - My claim: the better approach is to embrace and learn JS's type system, and to adopt a coding style which makes types as obvious as possible
  - By doing so, you will make your code more readable and more robust, for experienced and new developers alike
  - As an option to aid in that effort, I created Typl, which I believe embraces and unlocks the best parts of JS's types and coercion.

## 2 Scope

### 2.1 Nested Scope

- Scope: where to look for things
- JavaScript organizes scopes with functions and blocks
- JavaScript is 2-pass,
  - Compilation: Scopes and the variable's blueprint is created  (Compile time)
  - Execution: Values of the variables are assigned and execution occurs (Run time)
  - Different colour accounts for different scopes<img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200610094036612.png" alt="image-20200610094036612" style="zoom:80%;" />

​		     <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200610100250701.png" alt="image-20200610100250701" style="zoom:80%;" />

​     		<img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200610100604311.png" alt="image-20200610100604311" style="zoom:80%;" />

​	        <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200610101054317.png" alt="image-20200610101054317" style="zoom: 75%;" />

- JavaScript here used is in loose mode(non-strict mode). So, undeclared variable assignments will be auto-declared at global scope (done automatically)

- **strict mode**

  <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200610102033546.png" alt="image-20200610102033546" style="zoom:80%;" />

  <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200610102138309.png" alt="image-20200610102138309" style="zoom:80%;" />

- **undefined vs undeclared**

  - Undefined means variable exists but at the moment it has no value

  - Uncleared means variable is never formally declared in the scope we have access to

    <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200610103350509.png" alt="image-20200610103350509" style="zoom:80%;" />

    - 1. Line:1->Function declaration: `teacher` ->(global scope)
      2. Line:3->function expression: `myTeacher`->(global scope)->`anotherTeacher`->(local scope : of `myTeacher`)
      3. Line:9->tries to access `anotherTeacher` which is not in the scope->`ReferenceError`

  - **Named Function Expressions**

    ```javascript
    var clickHandler = function(){
    	//
    };
    
    var keyHandler = function keyHandler(){
    	//
    }
    ```

    - **Named Function Expressions: Benefits**

      1. Reliable function self-reference (recursion, etc) 
      2. More debuggable stack traces 
      3. More self-documenting code

    - **Named Function Expressions vs. Anonymous Arrow Functions**

      <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200610202554032.png" alt="image-20200610202554032" style="zoom:80%;" />

    - **Named (Arrow) Function Expressions? Still no...**

      <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200610202738078.png" alt="image-20200610202738078" style="zoom:80%;" />

    - **(Named) Function Declaration > Named Function Expression > Anonymous Function Expression**

- **Lexical Scope**

  <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200612084034038.png" alt="image-20200612084034038" style="zoom:80%;" />

  <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200612084120499.png" alt="image-20200612084120499" style="zoom:80%;" />

- **dynamic Scope**

  <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200612085144771.png" alt="image-20200612085144771" style="zoom:80%;" />

- **Function Scoping**

  - <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200612085257826.png" alt="image-20200612085257826" style="zoom:80%;" />
  - <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200612085432593.png" alt="image-20200612085432593" style="zoom:80%;" />
  - **IIFE - Immediately Invoked Function Expression:**
  - <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200612085505265.png" style="zoom:80%;" />
  - <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200612085804952.png" alt="image-20200612085804952" style="zoom:80%;" />
  - <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200612085846343.png" alt="image-20200612085846343" style="zoom:80%;" />

  - **Block Scoping:**
  - <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200612090042047.png" alt="image-20200612090042047" style="zoom:80%;" />
  - <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200612090123995.png" alt="image-20200612090123995" style="zoom:80%;" />
  - <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200612090209479.png" alt="image-20200612090209479" style="zoom:80%;" />
  - <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200612090246595.png" alt="image-20200612090246595" style="zoom:80%;" />
  - <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200612090321172.png" alt="image-20200612090321172" style="zoom:80%;" />	
  - <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200612090352886.png" alt="image-20200612090352886" style="zoom:80%;" />
  - <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200612090529303.png" alt="image-20200612090529303" style="zoom:80%;" />
  - <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200612090608553.png" alt="image-20200612090608553" style="zoom:80%;" />
  - <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200612090642499.png" alt="image-20200612090642499" style="zoom:80%;" />

  

  ### 2.2 Hoisting

  ​	

  ```javascript
  student;
  teacher;
  var student = "you";
  var teacher = "Kyle";
  ```

  - <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200612093936255.png" style="zoom:80%;" />

  - <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200612094017877.png" style="zoom:80%;" />

  - <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200612094122711.png" style="zoom:80%;" />

  - <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200612094217449.png" style="zoom:80%;" />

  - <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200612094311503.png" style="zoom:80%;" />

  - <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200612094359404.png" style="zoom:80%;" />

    

  ### 2.3 Closure

  > Closure is when a function "remembers" its lexical scope even when the function is executed outside that lexical scope

  ```javascript
  function ask(question) {
  setTimeout(function waitAsec(){
      console.log(question);
  	},100);
  }
  
  ask("What is closure?");
  // What is closure?
  ```

  ```javascript
  function ask(question) {
      return function holdYourQuestion() {
          console.log(question);
      };
  }
  var myQuestion = ask("What is closure?");
  
  myQuestion(); // What is closure?
  ```

  - Closure: NOT capturing a value
    - There is nothing like closing over values
    - Closing over is always over variable

  ```javascript
  var teacher = "Kyle";
  var myTeacher = function() {
      console.log(teacher);
  };
  teacher = "Suzy";
  myTeacher(); // Suzy
  ```

  - Closure: loops

  ```javascript
  for (var i = 1; i <= 3; i++) {
      setTimeout(function(){
          console.log(`i: ${i}`);
      },i*1000);
  }
  // i: 4
  // i: 4
  // i: 4
  ```

  - Solving the above issue:

  ```javascript
  for (var i = 1; i <= 3; i++) {
      let j = i;
      setTimeout(function(){
          console.log(`j: ${j}`);
      },j*1000);
  }
  // j: 4
  // j: 4
  // j: 4
  ```

  - Another way:

  ```javascript
  for (let i = 1; i <= 3; i++) {
      setTimeout(function(){
          console.log(`i: ${i}`);
      },i*1000);
  }
  // i: 1
  // i: 2
  // i: 3
  ```



### 2.4 Modules

- Namespace, NOT a module

  ```javascript
  var workshop = {
      teacher: "Kyle",
      ask(question) {
          console.log(this.teacher, question);
      },
  };
  
  workshop.ask("Is this a module?");
  // Kyle Is this a module?
  ```

  

- Modules encapsulate data and behavior (methods) together. The state (data) of a module is held by its methods via closure.

- Classic/Revealing module pattern

  ```javascript
  var workshop = (function Module(teacher){
      var publicAPI = {ask, };
      // *******************
      function ask(question) {
          console.log(teacher, question);
      }
  })("Kyle");
  workshop.ask("It's a module, right?");
  // Kyle It's a module, right?
  ```

- Module Factory

  ```javascript
  function WorkshopModule(teacher) {
      var publicAPI = {ask, };
      return publicAPI;
      // **************
      function ask(question) {
          console.log(teacher,question);
      }
  };
  var workshop = WorkshopModule("Kyle");
  workshop.ask("It's a module, right?");
  // Kyle It's a module, right?
  ```

- ES6 module pattern

  ```javascript
  //workshop.mjs:
  var teacher = "Kyle";
  
  export Default function ask(question) {
      console.log(teacher,question);
  };
  ```

  ```javascript
  import ask from "workshop.mjs";
  
  ask("It's a default import, right?");
  // Kyle It's a default import, right?
  
  import * as workshop from "workshop.mjs";
  
  workshop.ask("It's a namespace import, right?");
  // Kyle It's a namespace import, right?
  ```

****

## 3 Objects (Oriented)

### 3.1 this

- A function's this references the execution context for that call, determined entirely by how the function was called.

- A this-aware function can thus have a different context each time it's called, which makes it more flexible & reusable.

- **Recall: dynamic scope**

  ```javascript
  var teacher = "Kyle";
  function ask(question) {
      console.log(teacher,question);
  }
  function otherClass() {
      var teacher = "Suzy";
      ask("Why?");
  }
  otherClass(); // Suzy why?
  ```

- **Dynamic Context ~= JS's Dynamic Scope**

  ```javascript
  function ask(question) {
      console.log(this.teacher,question);
  }
  function otherClass() {
      var myContext = {
          teacher: "Suzy"
      };
      ask.call(myContext,"Why?"); // Suzy Why?
  }
  otherClass();
  ```

- **this: implicit binding**

  ```javascript
  var workshop = {
  	teacher: "Kyle",
  	ask(question) {
      	console.log(this.teacher,question);
      },
  };
  workshop.ask("What is implicit binding?");
  // Kyle What is implicit binding?
  ```

- **this: dynamic binding -> sharing**

  ```javascript
  function ask(question) {
      console.log(this.teacher,question);
  }
  var workshop1 = {
  	teacher: "Kyle",
      ask: ask,
  };
  var workshop2 = {
  	teacher: "Suzy",
      ask: ask,
  };
  workshop1.ask("How do I share a method?");
  // Kyle How do I share a method?
  workshop2.ask("How do I share a method?");
  // Suzy How do I share a method?
  ```

- **this: explicit binding**

  ```javascript
  function ask(question) {
      console.log(this.teacher,question);
  }
  var workshop1 = {
  	teacher: "Kyle",
      ask: ask,
  };
  var workshop2 = {
  	teacher: "Suzy",
      ask: ask,
  };
  ask.call(workshop1,"Can I explicitly set context?");
  // Kyle Can I explicitly set context?
  ask.call(workshop2,"Can I explicitly set context?");
  // Suzy Can I explicitly set context?
  ```

- **this: hard binding**

  ```javascript
  var workshop = {
  	teacher: "Kyle",
  	ask(question) {
      	console.log(this.teacher,question);
      },
  };
  setTimeout(workshop.ask,10,"Lost this?");
  // undefined Lost this?
  
  setTimeout(workshop.ask.bind(workshop),10,"Hard bound this>");
  // Kyle Hard bound this?
  ```

- **this: new binding, "constructor calls":**

  ```javascript
  function ask(question) {
      console.log(this.teacher,question);
  }
  var newEmptyObject = new ask("What is 'new' doing here?")
  // undefined What is 'new' doing here?
  ```

  - **new: steps**
    1. Create a brand new empty object 
    2. *Link that object to another object 
    3. Call function with `this` set to the new object 
    4. If function does not return an object, assume return of `this`

- **this: default binding**

  ```javascript
  var teacher = "Kyle";
  function ask(question) {
      console.log(this.teacher,question);
  }
  function askAgain(question) {
      "use strict";
      console.log(this.teacher,question);
  }
  ask("What's the non-strict-mode default?");
  // Kyle What's the non-strict-mode default?
  askAgain("What's the strict-mode default?");
  // TypeError
  ```

- **this: binding rule precedence?**<img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200613104558584.png" style="zoom:80%;" />

  - **this: determination**

    1. Is the function called by `new`? 

    2. Is the function called by `call()` or `apply()`? 

        Note: `bind()` effectively uses `apply()`

    3. Is the function called on a context object? 

    4. DEFAULT: global object (except strict mode)

- **this: arrow functions**

  <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200613104830689.png" style="zoom:80%;" />	

  - An arrow function is this-bound (aka `.bind()`) to its parent function.<img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200613104951210.png" style="zoom:80%;" />
  - ~~An arrow function is this-bound (aka .bind()) to its parent function.~~
  - An arrow function doesn't define a this, so it's like any normal variable, and resolves lexically (aka "lexical this").<img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200613105231580.png" style="zoom:80%;" />
  - Only use => arrow functions when you need lexical this.

### 3.2 class{ }

- ​	ES6 class:

  ​	

  ```javascript
  class Workshop {
      constructor(teacher) {
          this.teacher = teacher;
      }
      ask(question) {
          console.log(this.teacher,question);
      }
  }
  
  var deepJS = new Workshop("Kyle");
  var reactJS = new Workshop("Suzy");
  
  deepJS.ask("Is 'class' class?");
  // Kyle Is 'class' a class?
  
  reactJS.ask("Is this class OK?");
  // Suzy Is this class OK?
  ```

- ES6 class: extends (inheritance)

  ```javascript
  class Workshop {
      constructor(teacher) {
          this.teacher = teacher;
      }
      ask(question) {
          console.log(this.teacher,question);
      }
  }
  class AnotherWorkshop extends Workshop {
      speakUp(msg) {
          this.ask(msg);
      }
  }
  
  var JSRecentParts = new AnotherWorkshop("Kyle");
  
  JSRecentParts.speakUp("Are classes getting better?");
  // Kyle Are classes getting better?
  ```

- ES6 class: super (relative polymorphism)

  ```javascript
  class Workshop {
      constructor(teacher) {
          this.teacher = teacher;
      }
      ask(question) {
          console.log(this.teacher,question);
      }
  }
  class AnotherWorkshop extends Workshop {
      ask(msg){
         super.ask(msg.toUpperCas());
      }
  }
  
  var JSRecentParts = new AnotherWorkshop("Kyle");
  
  JSRecentParts.speakUp("Are classes super?");
  // Kyle ARE CLASSES SUPER?
  ```

- ES6 class: still dynamic this

  ```javascript
  class Workshop {
      constructor(teacher) {
          this.teacher = teacher;
      }
      ask(question) {
          console.log(this.teacher,question);
      }
  }
  var deepJS = new Workshop("Kyle");
  setTimeout(deepJS.ask,100,"Still losing 'this'?");
  // undefined still losing 'this'?
  ```

- ES6 class: "fixing" this? Bad thig to do

  ```javascript
  class Workshop {
      constructor(teacher) {
          this.teacher = teacher;
          this.ask = question => {
          console.log(this.teacher,question);
          };
      }
  }
  var deepJS = new Workshop("Kyle");
  setTimeout(deepJS.ask,100,"Is 'this' fixed?");
  // Kyle Is 'this' fixed?
  ```

- **ES6 class: hacktastrophy[Don't do this]** 

  <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200613142813272.png" alt="image-20200613142813272" style="zoom:80%;" />

- **ES6 class: inheritable hard this-bound methods**

  <img src="C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200613143106018.png" style="zoom:80%;" />

  

### 3.3 Prototypes

- Objects are built by "constructor calls" (via `new`)

- A "constructor call" makes an object "~~based on~~" its own prototype

- A "constructor call" makes an object linked to its own prototype

- **Prototypes: as "classes"**

  ```javascript
  function Workshop(teacher) {
      this.teacher = teacher;
  }
  Workshop.prototype.ask = function(question){
      console.log(this.teacher,question);
  };
  var deepJS = new Workshop("Kyle");
  var reactJS = new Workshop("Suzy");
  
  deepJS.ask("Is 'protype' a class?");
  // Kyle Is 'protype' a class?
  
  reactJS.ask("Isn't 'prototype' ugly?");
  // Suzy Isn't 'prototype' ugly?
  ```

  ![](C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200614080354691.png)

  ```javascript
  function Workshop(teacher) {
      this.teacher = teacher;
  }
  Workshop.prototype.ask = function(question){
      console.log(this.teacher,question);
  };
  var deepJS = new Workshop("Kyle");
  deepJS.constructor === Workshop;
  deepJS.__proto__ === Workshop.prototype; // true
  Object.getPrototypeOf(deepJS) === Workshop.prototype; // true
  ```

  ![](C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200614090950477.png)

  ![](C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200614091034620.png)

- **Prototypal Inheritance:**

  ![](C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200614091222691.png)

- **Clarifying Inheritance**

  ![](C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200614091341761.png)

  ![](C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200614091419095.png)

- **JavaScript ~~“Inheritance”~~ “Behavior Delegation”** 

### 3.3 OO vs OLOO

- **Let's Simplify!**

  - OLOO: Objects Linked to Other Objects

  ![](C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200614091616156.png)

  ![](C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200614091736244.png)

  ![](C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200614091812741.png)

- Pre ES5 days:

  ![](C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200614091904644.png)

- **Delegation: Design Pattern**

  ![](C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200614092145092.png)

  ![](C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200614092220009.png)

  ![](C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200614092250214.png)

  ![](C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200614092335776.png)

  ![](C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200614092405833.png)

  ![](C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200614092440995.png)

  ![](C:\Users\Dipjul\AppData\Roaming\Typora\typora-user-images\image-20200614092521765.png)

  