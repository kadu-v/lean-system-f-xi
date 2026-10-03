FROM mcr.microsoft.com/devcontainers/base:ubuntu

RUN apt-get update \
    && apt-get install -y --no-install-recommends curl git ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# TeX Live（日本語・LuaLaTeX・数式/証明図用パッケージ、整形用の latexindent）
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        texlive-latex-recommended \
        texlive-latex-extra \
        texlive-lang-japanese \
        texlive-luatex \
        texlive-science \
        texlive-fonts-recommended \
        texlive-fonts-extra \
        texlive-extra-utils \
        libyaml-tiny-perl \
        libfile-homedir-perl \
        latexmk \
    && rm -rf /var/lib/apt/lists/*

USER vscode

RUN curl -sSfL https://raw.githubusercontent.com/leanprover/elan/master/elan-init.sh \
    | sh -s -- -y --default-toolchain leanprover/lean4:stable

ENV PATH="/home/vscode/.elan/bin:${PATH}"

# elan は toolchain を遅延取得するため、ビルド時に明示的にダウンロードしておく
RUN lean --version

# luatexja のフォントキャッシュを事前に作っておく
RUN luaotfload-tool -u
