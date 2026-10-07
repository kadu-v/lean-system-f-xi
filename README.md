# lean-system-f-xi

System Fξ の LaTeX 定義と Lean 4 による型安全性・効果安全性の形式化。

| ディレクトリ | 内容 |
|---|---|
| `lean/` | Lean 4 + Mathlib による構文、型付け、評価、証明 |
| `tex/` | LaTeX 文書（LuaLaTeX + luatexja） |

## macOS の環境

Docker は使用しない。Apple Silicon のホスト上で Homebrew の elan と Lean を使う。

```sh
brew install elan-init
cd lean
elan toolchain install leanprover/lean4:v4.34.1
lake update
lake exe cache get
lake build
```

`lean/lean-toolchain` は Lean 4.34.1、`lean/lakefile.toml` は Mathlib 4.34.1 を指定する。`lake build` は `SystemFXi.Safety` までの全モジュールを検査する。今回の環境では `arm64-apple-darwin` の Lean でビルドした。

## 証明の入口

- `SystemFXi.progress_effect_safety`：閉じた型付き式は値、簡約可能、または効果集合に含まれる操作を露出する。
- `SystemFXi.preservation`：一歩簡約は型と効果の上界を保存する。
- `SystemFXi.effect_safety`：任意の到達状態で上記の進行性を保つ。
- `SystemFXi.empty_effect_safety`：空効果の閉項は任意の到達状態で値か簡約可能な式である。

効果集合は `Finset Op`、変数は de Bruijn 添字で表す。証明は整形式の大域シグネチャ `Signature.WF` を仮定する。操作節を一つ持つ deep handler と型引数付き操作呼出しを扱う。

## TeX

```sh
cd tex
latexmk
```

LuaLaTeX と luatexja が必要。
