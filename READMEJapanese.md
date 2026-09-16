# ✨ N6LTranspiler-WEBLLVM  
JavaScript をベース言語として、ユーザー定義言語（N6LScript）を JS 内に安全に埋め込むためのトランスパイラです。  
N6LScript は、**JS のコードブロック内に独自記法を混在させ、最終的に純粋な JavaScript に変換する**ための軽量 DSL（Domain Specific Language）です。

👉 [DemoPage](https://nas6mixfoolv.github.io/N6LTranspiler-WEBLLVM-/)

👉 [README English ver.](https://github.com/NAS6mixfoolv/N6LTranspiler-WEBLLVM-/blob/main/README.md)

---

## 🚀 目的  
N6LTranspiler-WEBLLVM の目的は次の通りです：

- **JavaScript の中に「別言語」を安全に埋め込む仕組みを作ること**
- **ユーザーが自由に構文・記法を定義できる柔軟なトランスパイル環境を提供すること**
- **JS の曖昧さを排除し、数学的・構造的に厳密な記法を導入すること**
- **ブロック構造により変換範囲を限定し、誤変換を防ぐこと**

実装を単純に表現すれば置換テーブルを作っただけですが、一風変わった趣になります 。 
  
---

## 🧩 特徴

### ✔ **1. ブロック構造による安全な埋め込み**
N6LScript は次のようなブロックで囲むことで、  
**変換範囲を完全に限定できます**。

```
\N---[
   // N6LScript block
]---\N
```

この中だけが N6LScript として扱われ、  
外側は通常の JavaScript としてそのまま実行されます。

---

### ✔ **2. ユーザー定義の置換テーブル**
設定ファイル（JSON）で、  
**任意の記法 → JS の記法** を定義できます。

例：

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

これにより、次のような N6LScript 記法が：

```
\Ncout( a \N+ b \M* c \M) )
```

最終的に JS の

```
console.log(a.add(b).mul(c));
```

へ変換されます。

## langBlockSyntaxについて

**config.langBlockSyntax.start**:  
これは独自スクリプトブロックの開始を示し、  
**config.langBlockSyntax.end**:  
これは独自スクリプトブロックの終了を示します。  
**config.langBlockSyntax.esstart**:  
これは独自スクリプトブロック内でスペースとタブを削除するブロックの開始を示し、  
**config.langBlockSyntax.esend**:  
これは独自スクリプトブロック内でスペースとタブを削除するブロックの終了を示します。  
  
---

### ✔ **3. JS → N6L の危険性を回避する設計**
N6LScript は **JS に存在しない記号体系**で構成されているため、  
変換後の JS が再び N6L と誤認識されることがありません。

これにより、  
**N6L → JS の一方向変換は極めて安全**になります。

---

### ✔ **4. 数学的厳密さを補助する記法**
例えば `\L(` のような補助記号を N6LScript 側だけで使い、  
JS に変換するときは削除することができます。

```
\L( \L( a \N+ b \M)*c \M);
```

→ JS では単に

```
a.add(b).mul(c);
```

となる。

---

## 📘 使用例

### 入力コード（N6LScript）
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

### 出力コード（JavaScript）
```
let a = new FourArithmeticOperations(5);
let b = new FourArithmeticOperations(4);
let c = new FourArithmeticOperations(2);

console.log(a.add(b).mul(c).div(c).sub(b).val);
```

---

## 🛠 実装構造（概要）

- **N6LTranspilerBlock**  
  ブロックの種類（JS / N6LScript）と内容を保持するクラス。

- **N6LTranspiler**  
  N6LScriptあるいは独自記法からJSへ変換するためのクラス。

- **BlockParser**  
  ソースコードをブロック単位に分解する。

- **ReplaceEngine**  
  設定ファイルに基づき、N6LScript 記法を JS 記法へ変換する。

- **Executor**  
  トランスパイル後の JS を安全に実行する。

---

## 🌐 デモページ  
ブラウザ上で N6LScript を入力し、  
リアルタイムで JS へ変換して実行できます。

👉 [DemoPage](https://nas6mixfoolv.github.io/N6LTranspiler-WEBLLVM-/)

---

## 📄 ライセンス

GPL-v3.0


