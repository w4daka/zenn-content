---
title: "neovimのluaスクリプトの設定memo"
emoji: "👌"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["neovim"]
published: false
---

## context

- 背景:neovimの設定をネットからのコピペと生成AIの力でほぼ全て設定してしまったのブラックボックス化しているので。そこでいくつかのブログシリーズに分けて解説を付そうと思います。

- やりたいこと: まず以下のluaのscriptを詳細な解説をつけてそのリンクをneovimの設定に付します

参考
私のお世辞にも綺麗とは言えない[dotfilesリポジトリ](https://github.com/w4daka/dotfiles)の[dot_config/nvim/lua/core/private_user_command.lua](https://github.com/w4daka/dotfiles/blob/main/dot_config/nvim/lua/core/private_user_command.lua)の2行目からのスクリプト

````lua
vim.api.nvim_create_user_command("InitLua", function()
  vim.cmd.edit("~/.config/nvim/init.lua")
end, { desc = "Open init.lua" })
````

## 解説

### 結論

このコマンドは`nvim_create_user_command`の3引数版です

### 必要な前提知識

- ユーザーコマンドは`nvim_create_user_command`で作ることができ、この関数は以下の3つの省略できない引数をとります
  - コマンドの名前の文字列(組み込みのコマンドと区別するため、大文字ではじめる必要があります。
  - コマンドを呼び出したときに実行するVimのコマンドの文字列ないしLuaの関数
  - command-attributesのテーブル

> ユーザコマンドを作る                                 *lua-guide-commands-create*

ユーザコマンドは |nvim_create_user_command()| で作れる。この関数は
3つの省略できない引数を取る:
• コマンドの名前の文字列 (組み込みのコマンドと区別するため、
  大文字で始める必要がある)
• コマンドを呼び出したときに実行するVimのコマンドの文字列ないしLuaの関数
• |command-attributes| のテーブル
  さらに以下も指定できる
  • `desc`: コマンドを説明する文字列
  • `force`: コマンドを上書きしないためには `false` にする。
  • `preview`: |:command-preview| に使われるLuaの関数

1. コマンドの名前の文字列(組み込みのコマンドと区別するために、大文字で始める必要があります。)上記のスクリプトの`"InitLua"`の部分ですね。

2. コマンドを実行したときに実行するVimのコマンドの文字列もしくはluaの関数

## 参考

nvim_create_user_commandのmanページ。コマンドラインモードで

```shell
:h nvim_create_user_command
```

とするとでてきます。webでみれるバージョンは[こちら](https://neovim.io/doc/user/api/#api)です。ページ内検索で`Command Function`を検索する出てきます。nvimのhelpを日本語化しても英語で出てくるので、英語が苦手な人は各自google翻訳などを使ってください。私はGoogle翻訳を使いました。

helpの`lua-guide-commands-create`。コマンドラインモードで

```shell
:h lua-guide-commands-create
```

とするとでてきます。webで見れるバージョンは[こちら](https://neovim.io/doc/user/lua-guide/#_creating-user-commands)
