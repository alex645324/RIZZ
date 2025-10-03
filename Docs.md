# Foundation-Only Syntax 

* Keywords 
    1. global
        - Declares a variable at the top level (outside of any { }).
        - Always written with global in front.
        Example: 
            global x = 10; 

    2. Variables inside a block of code (no keyword)
        - Inside { } you can just write a variable name to define it.
        Example: 
            {
                x = 10;
            }
    
    3. Functions 
        - Define a function by name with parameters 
        - Syntax: Function name(placeholders) {....}
        - And the placeholder are just spots you can use to plug in what you want. its like a free varaiable that doesnt have a value until you give it one 
        inside that specific function. 
        Example: 
            Function greet(name) {
                say("Hi" + name); 
            }

    4. if and else 
        - standard conditional control flow. 
        Example: 
            if(x > 5 ){ 
                say("big"); 
            } else { 
                say("small")
            }

    5. run as long as 
        - loop that repeats while a condtion is true (same as while loop). 
        Example: 
            run as long as (x < 10) { 
                x = x + 1; 
            }
    
    6. poop
        - return a value from a function (my version of return)
        Example: 
            Function add(a, b) { 
                poop a + b; 
            }

            global x = add(2, 6); 
                say(x); 
    
    7. say 
        - this is my version of the print in python 
        Example: 
            say("hello world")
    
    8. true and false
        - works as normal boolean values 
    
    9. N/A
        - this i my lanauge version of null 

Give me a summary:
* **Variables**

    * `global` → top-level variables.
    * Inside `{ }`, just write `name = value;` (no keyword).

* **Functions**

    * Declared with `Function name(params) { ... }`.
    * Use `poop` to return a value.

* **Control flow**

    * `if (...) { ... } else { ... }` → branching.
    * `run as long as (condition) { ... }` → looping.

* **Built-in**

    * `say(expr)` → prints to terminal with a newline.

    * **Literals**

    * `true`, `false` → booleans.
    * `N/A` → null.



# Lexical Rules (Tokenizer Spec)

## 1) Case & encoding

* **ASCII letters/digits** are enough for v1.0.
* **Case-sensitive.** `Function` (capital F) is a keyword; `function` is just an identifier.
    

## 2) Whitespace

* Spaces, tabs, newlines are **separators only**.
* They can appear anywhere **except inside multi-word keywords** (see §6).

## 3) Comments

* **Line comment:** starts with `//`, goes to end of line.
* **Block comment:** `/* ... */` (no nesting in v1.0).
* Comments are ignored by the lexer.

## 4) Identifiers (names)

* Pattern: start with a **letter or `_`**, then letters/digits/`_`.
* Examples: `x`, `_tmp`, `sum2`.
* Identifiers that match a keyword (exact case) become that **keyword token**.

## 5) Literals (values)

* **Booleans:** `true`, `false`.
* **Null:** `N/A` (exactly capital N, slash, capital A).
* **Numbers:**

  * Integers: `0`, `7`, `12345`.
  * Decimals: `3.14`, `0.5`, `.5` **not allowed** (must have leading digit).
  * No exponent form in v1.0 (`1e9` not allowed).
* **Strings:**

  * Double quotes only: `"hello"`
  * No multi-line strings.
  * Minimal escapes: `\"` and `\\` only. Anything else is a lexical error.

## 6) Keywords (exact spellings)

* Single-word: `global`, `if`, `else`, `poop`, `true`, `false`, `Function`
* Multi-word: **`run as long as`**

  * Must appear exactly as `run␠as␠long␠as` (single spaces, same line).
  * If spacing differs or line breaks appear, tokenize the words separately (not a keyword).

## 7) Operators & punctuation (single tokens)

* **Assignment:** `=`
* **Arithmetic:** `+  -  *  /  %`
* **Comparisons:** `<  <=  >  >=  ==  !=`
* **Logical:** `&&  ||  !`
* **Grouping & separators:** `(` `)` `{` `}` `,` `;`

> Longest-match rule: when two tokens share a prefix (e.g., `=` vs `==`, `<` vs `<=`), always take the **longer** one if possible.

## 8) Statement terminators

* A **semicolon `;`** ends simple statements (assignments, calls like `say(...)`, `poop` lines, `global` lines).
* Blocks `{ ... }` are not followed by a semicolon.

## 9) Error shapes (lexer level)

* Unknown character → `LexError(line:col): unexpected character '…'`
* Unterminated string → `LexError(line:col): unterminated string`
* Bad escape → `LexError(line:col): invalid escape '\x'`

---

If this matches your intent, say **“lock lexical rules”** and I’ll move to the **Grammar (parsing)** next. If you want any tweaks (e.g., allow `1e9`, allow `\n` escape, or relax the multi-word keyword spacing), tell me now.
