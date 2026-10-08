# lean-system-f-xi

System Fξ の LaTeX 定義と Lean 4 による型安全性・効果安全性の形式化。

| ディレクトリ | 内容 |
|---|---|
| `lean/` | Lean 4 + Mathlib による構文、型付け、評価、証明 |
| `tex/` | LaTeX 文書（LuaLaTeX + luatexja） |

## macOS の環境

Lean のビルドには Docker を使わず、Apple Silicon のホスト上で Homebrew の elan と Lean を使う。

```sh
brew install elan-init
cd lean
elan toolchain install leanprover/lean4:v4.34.1
lake update
lake exe cache get
lake build
```

`lean/lean-toolchain` は Lean 4.34.1、`lean/lakefile.toml` は Mathlib 4.34.1 を指定する。`lake build` は効果多相版の `SystemFXi.EffectPolySafety` まで検査する。今回の環境では `arm64-apple-darwin` の Lean でビルドした。

## 証明の入口

- `SystemFXi.Poly.progress_effect_safety`：閉じた型付き式は値、簡約可能、または効果に含まれる操作を露出する。
- `SystemFXi.Poly.preservation`：一歩簡約は型と効果の上界を保存する。
- `SystemFXi.Poly.effect_safety`：任意の到達状態で上記の進行性を保つ。
- `SystemFXi.Poly.empty_effect_safety`：空効果の閉項は任意の到達状態で値か簡約可能な式である。
- `SystemFXi.Poly.effect_polymorphic_identity`：潜在効果が `μ` の関数を受け取る `Λμ::E` の型付け例。

効果式は重複のない操作ラベル集合と効果変数集合で正規化する。`∀μ::E` と `Λμ::E`、効果引数による具体化、型と効果の混合引数を持つ操作宣言を扱う。包含は効果変数への任意の有限集合割当てに対する意味論的包含と同値であることを証明している。Lean 内部の変数は de Bruijn 添字で表し、証明は整形式の大域シグネチャ `Signature.WF` を仮定する。旧形式の証明も `SystemFXi` 名前空間に残る。

## TeX

```sh
cd tex
latexmk
```

LuaLaTeX と luatexja が必要。
このリポジトリの TeX 環境が入ったイメージでは、次のコマンドでも `tex/build/main.pdf` を生成できる。

```sh
docker run --rm -v "$PWD":/workspace -w /workspace/tex lean-system-f-xi-dev:latest latexmk
```
