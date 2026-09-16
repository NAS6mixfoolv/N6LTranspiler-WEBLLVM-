# ✨ N6LTranspiler-WEBLLVM-
> N6LTranspiler-WEBLLVM is a transpiler designed to safely embed Domain-Specific Languages ??(DSLs) within JavaScript.
> It converts custom syntax found in specific blocks into pure JavaScript based on a substitution table.
> Conversion and execution can be performed in real-time within the browser.

👉 [DemoPage](https://nas6mixfoolv.github.io/N6LTranspiler-WEBLLVM-/)  

👉 [README 日本語版](https://github.com/NAS6mixfoolv/N6LTranspiler-WEBLLVM-/blob/main/READMEJapanese.md)

---

## 🚀 Purpose
The objectives of N6LTranspiler-WEBLLVM are as follows:

- **To create a mechanism for safely embedding "other languages" within JavaScript**
- **To provide a flexible transpilation environment where users can freely define syntax and notation**
- **To eliminate JavaScript's ambiguities and introduce mathematically and structurally rigorous notation**
- **To limit the scope of conversion using block structures, thereby preventing erroneous conversions**

In simple terms, the implementation merely involves creating a substitution table, yet it results in a rather unique touch.

---

## 🧩 Features

### ✔ **1. Safe embedding via block structures**
By enclosing code in blocks like the one below,
**the scope of conversion can be strictly limited**.

```
\N---[
// N6LScript block
]---\N
```

Only the content within these blocks is treated as N6LScript,
while the surrounding code executes as standard JavaScript.

---

### ✔ **2. User-defined replacement tables**
You can define mappings from
**arbitrary notation to JS notation** in a configuration file (JSON).

Example:

```json
{
  "langName": "N6LScript",

  "langBlockSyntax": {
    "start": "\\N---[",
    "end":   "]---\\N",
    "esstart": "\\N---<",
    "esend":   ">---\\N"
  },

  "replace": [
    [ "", "\\L(" ],
    [ ")", "\\M)" ],
    [ ").add(", "\\M+" ],
    [ ").sub(", "\\M-" ],
    [ ").mul(", "\\M*" ],
    [ ").div(", "\\M/" ],
    [ ".add(", "\\N+" ],
    [ ".sub(", "\\N-" ],
    [ ".mul(", "\\N*" ],
    [ ".div(", "\\N/" ],
    [ "console.log(", "\\Ncout(" ],
    [ "FourArithmeticOperations", "\\Num" ]
  ]

}
```

This allows N6LScript notation like this:

```
\Ncout( a \N+ b \M* c \M) )
```

to be ultimately converted into the following JS code:

```
console.log(a.add(b).mul(c));
```

### About langBlockSyntax
  
**config.langBlockSyntax.start**:  
This indicates the start of a custom script block,  
**config.langBlockSyntax.end**:  
This indicates the end of a custom script block.  
**config.langBlockSyntax.esstart**:  
This indicates the start of a block within a custom script block where spaces and tabs are removed,  
**config.langBlockSyntax.esend**:  
This indicates the end of a block within a custom script block where spaces and tabs are removed.  
  
```
JS code
\N---[
   N6LScript code
   \N---<
      N6LScript code(no spaces)
   >---\N
   N6LScript code
]---\N
JS code
```
  
### Regarding the Substitution Table Format  
The substitution table is structured as `[JS side, Custom syntax side]`.  
This is because the initial design focused primarily on how to map JavaScript syntax to the custom syntax.  
In the actual transpilation process, the custom syntax side ([1]) is replaced with the JS side ([0]).  
  
---

### ✔ **3. Design to Avoid JS → N6L Ambiguity**
Since N6LScript is composed of a **symbol system that does not exist in JS**,
the converted JS code will never be mistakenly recognized as N6L code again.

This ensures that
**one-way conversion from N6L to JS is extremely safe**.

---

### ✔ **4. Notation Supporting Mathematical Rigor**
For instance, auxiliary symbols like `\L(` can be used exclusively within N6LScript
and removed during conversion to JS.

Before
```
\N---<
\L( \L( a \N+ b \M)* c \M);
>---\N
```

→ In JS, this simply becomes

After
```
a.add(b).mul(c);
```

---

## 📘 Usage Example

### Input Code (N6LScript)
```
\N---[
let a = new \Num(5);
let b = new \Num(4);
let c = new \Num(2);

\N---<
\Ncout( a \N+ b \M* c \M/ c \M- b \M) .val );
>---\N
]---\N
```

### Output Code (JavaScript)
```
let a = new FourArithmeticOperations(5);
let b = new FourArithmeticOperations(4);
let c = new FourArithmeticOperations(2);

console.log(a.add(b).mul(c).div(c).sub(b).val);
```

---

## 🛠 Implementation Structure (Overview)

- **N6LTranspilerBlock**
A class that holds the block type (JS / N6LScript) and its content.

- **N6LTranspiler**
A class for converting N6LScript or custom syntax into JS.

- **BlockParser**
Decomposes source code into individual blocks.

- **ReplaceEngine**
Converts N6LScript syntax to JS syntax based on configuration files.

- **Executor**
Safely executes the transpiled JS code.

---

## 🌐 Demo Page
You can enter N6LScript in your browser,
convert it to JS in real-time, and execute it.

👉 [DemoPage](https://nas6mixfoolv.github.io/N6LTranspiler-WEBLLVM-/)

---
## 📄 License

License: GPL-v3.0
