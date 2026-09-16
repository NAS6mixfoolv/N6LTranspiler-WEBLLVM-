# ✨ N6LTranspiler-WEBLLVM-
This is a transpiler designed to safely embed a user-defined language (N6LScript)—based on JavaScript—directly into JavaScript code.
N6LScript is a lightweight Domain-Specific Language (DSL) that allows you to **mix custom syntax within JavaScript code blocks** and ultimately transpile it into pure JavaScript.

👉 [DemoPage](https://nas6mixfoolv.github.io/N6LTranspiler-WEBLLVM-/)  

---

## 🚀 Purpose
The objectives of N6LTranspiler-WEBLLVM are as follows:

- **To create a mechanism for safely embedding "other languages" within JavaScript**
- **To provide a flexible transpilation environment where users can freely define syntax and notation**
- **To eliminate JavaScript's ambiguities and introduce mathematically and structurally rigorous notation**
- **To limit the scope of conversion using block structures, thereby preventing erroneous conversions**

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

## About langBlockSyntax
  
**config.langBlockSyntax.start**:  
This indicates the start of a custom script block,  
**config.langBlockSyntax.end**:  
This indicates the end of a custom script block.  
**config.langBlockSyntax.esstart**:  
This indicates the start of a block within a custom script block where spaces and tabs are removed,  
**config.langBlockSyntax.esend**:  
This indicates the end of a block within a custom script block where spaces and tabs are removed.  
  
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

```
\L( \L( a \N+ b \M)*c \M);
```

→ In JS, this simply becomes

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
