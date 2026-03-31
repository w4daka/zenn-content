---
title: "ArchLinuxでDockerを導入するときに詰まったところのメモ"
emoji: "🐥"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["archlinux", "Docker", "linux", "arch"]
published: true
published_at: 2025-03-31

---
## Context（背景・問題）

- 背景: ArchLinuxでAtCoderをRustでやるにあたって[cargo-compete](https://github.com/qryxip/cargo-compete)を導入しようと思ったが自分の力ではflake.nix+direnvで導入できなかった。よってDockerを導入しようと考えた。しかし、うまくいかないところが導入部分で直面した。その解決までの記録
- 何が困っているか:デーモン起動（systemd）を起動しようとして

```shell
sudo systemctl enable --now docker
```

というコマンドを打ったところ

```shell
❯ sudo systemctl enable --now docker

Job for docker.service failed because start of the service was attempted too often.
See "systemctl status docker.service" and "journalctl -xeu docker.service" for details.
To force a start use "systemctl reset-failed docker.service"
followed by "systemctl start docker.service" again.
```

というエラーが出て起動できなかった

## Goal（結論・ゴール）

- 結論:linux-zenを再インストールし、rebootすることでできた
- 何が得られるか:dockerのデーモン起動

## Solution（解決方法）

- 手順:私の環境ではmodulesディレクトリがなくなっていた。下記のコマンドで診断可能

```shell
ls -la /usr/lib/modules/$(uname -r)
ls -la /lib/modules/$(uname -r)
```

そこで、linux-zenを再インストールし、modulesディレクトリを復元した。下記コマンドで可能

```shell
sudo pacman -Syu
sudo pacman -S linux-zen linux-zen-headers --needed
sudo pacman -S linux-zen --overwrite '*'
```

- ポイント:再インストール後再起動することが重要

## Constraints（注意点）

- この手法はLinux-zenカーネルを使っている場合のみ可能なため、それをまず確認すべき。下記コマンドで確認可能

```shell
uname -r
```

## 最後に

chatGPTと共に解決したためただの記録ですが、ここまで読んでいただきありがとうございました。今後も不定期に更新していく予定ですので他の記事も見ていただけるとありがたいです。[私のX](https://x.com/w4daka_tter)のフォローもよろしくお願いします。フルリモートの場合は受託開発やバイトのお誘いもお待ちしています。7:00〜13:00までなら最低賃金で働きます。コメント欄か私のXのリプライか私のGithubの任意のリポジトリにIssueを立ててで連絡してください。
