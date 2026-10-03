# lean-system-f-xi

| ディレクトリ | 内容 |
|---|---|
| `lean/` | Lean 4 (Lake) プロジェクト。ライブラリ `SystemFXi` |
| `tex/`  | LaTeX 文書（LuaLaTeX + luatexja） |

## 環境

VS Code で開き、"Dev Containers: Reopen in Container" を実行する。
コンテナには Lean 4（elan）と TeX Live が入っている（`.devcontainer/`）。

## ビルド

```sh
cd lean && lake build     # Lean
cd tex  && latexmk        # TeX → tex/build/main.pdf
```
