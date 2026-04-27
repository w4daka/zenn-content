---
title: "nvim_create_user_commandについて実例を混じえつつ解説してみようという試み"
emoji: "😽"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["neovim"]
published: false
---

## nvim_create_user_commandとは？

- neovimで便利コマンドを作れる機能のこと(まちがってたらごめんなさい)

## 実例集とその解説

### 1. `:SayHello`→`Hello!`とだけ表示するコマンド(単純な文字列実行)

#### コマンド

```lua
vim.api.nvim_create_user_command('SayHello', 'echo "Hello!"', {})
```

#### 解説

- そもそもuser_commandを作るには、

```lua
vim.api.nvim_create_user_command({name}, {command}, {opts})
```

という形が必要。

- `{name}`,`{command}`,`{opts}`はそれぞれ必須
- `{name}`にはcommandの名前の文字列
- `{command}`にはneovimにVimのcommandとして直接実行させるための文字列か、luaの関数を書く。

- `{opt}`には

| キー | 型 | 内容 |
|------|-----|------|
| `desc` | string | コマンドの説明 |
| `force` | boolean | 既存定義を上書きするか（デフォルトtrue） |
| `complete` | string/function | 補完の種類または関数 |
| `preview` | function | `inccommand`用プレビューハンドラ |
| `bang`等 | boolean | `:command-bang`などのboolean属性をtrueに設定 |

を渡せる
