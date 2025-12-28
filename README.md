# bk コマンド

Linux運用で使うバックアップ用スクリプトです。

## これ使うと何がうれしい？

- サクッとファイル・フォルダ単位でバックアップできる
- 「cp -a xxx xxx.`date +%Y%m%d_%H%M`」とか打たなくてすむ。「bk xxx」でいい
- フォルダ指定で再帰的に取得も可能。ファイルごとに「.`date +%Y%m%d_%H%M`」相当の文字列が付く
- バックアップは「/root/backup.d」の下に本番パスを再現して格納
- 例: `bk /etc/ssh/sshd_config` → `/root/backup.d/etc/ssh/sshd_config.20251228_1149`
- 設定ディレクトリとかを汚さない。編集前にbk！を癖付けると後で追いやすい
- ログも残るよ！

## 特徴

- root権限で実行（`sudo bk`でもOK）
- 指定したファイル/ディレクトリ配下のファイルを`/root/backup.d`に退避
- 退避先に元の絶対パスを再現し、末尾に`.YYYYmmdd_HHMM`を付与
- ログを`/root/backup.d/backuplog.txt`に記録
- `/root/backup.d`配下はバックアップ対象として拒否

## 使い方

```bash
sudo bk [options] [target1] [target2] ...
# rootユーザーの場合はsudo不要
bk [options] [target1] [target2] ...
```

注意: 本スクリプトはroot権限での実行が必要です。

オプション:

- `-h` ヘルプ表示
- `-l` ログ表示

例:

- `sudo bk /etc/hosts /etc/passwd`
- `bk /etc/wireguard` (rootユーザーの場合)

## インストール

```bash
sudo install -m 0755 bk /usr/local/bin/bk
```

Gitで取得した場合は、Linux側で`chmod +x bk`を実行してください。

## アンインストール

```bash
sudo rm -f /usr/local/bin/bk
```

## 仕様まとめ

- 実行形式: `bk [options] [target1] [target2] ...`（1つ以上の対象が必須）
- 出力先の構成:
  - 退避先は`/root/backup.d`配下に元の絶対パスを再現
  - ファイル名末尾に`.YYYYmmdd_HHMM`を付与
- 動作:
  - ファイル指定: `cp -a`で1件コピーし、成功時にログへ記録
  - ディレクトリ指定: 配下の全ファイルを再帰的にコピーして件数を表示
  - 存在しない対象/不明な種別はエラー扱いで継続

## ログ

- ログファイル: `/root/backup.d/backuplog.txt`
- `bk -l`でログを表示

## ライセンス

MIT License
