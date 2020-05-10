# Virtualbox + Vagrant on Windows10でruby + psqlごった煮Rails開発環境を楽に構築し、Windows10側からVScodeでssh接続して開発する

h1が長くて自分でもよくわかっていないが、とりあえず残すのが先。

Ruby on Rails6実践ガイド(初版2019年12月21日) ではDockerに変わってたがWidows側からの使いやすさや、チーム内展開にはVagrantのほうがまだいい気がする。


また、Vagrantfileも自分で作らなくてもすでによいものが公開されている。
※使用するバージョンだけは注意

https://github.com/rails/rails-dev-box



Windows側のコマンド
```
host $ dir
```

Ubuntu側のコマンド
```
vagrant@rails-dev-box:~$ ls
```

Vagrantfileは以下のを使用します。

https://github.com/rails/rails-dev-box

# インストール手順

## Virtualboxをインストールする

https://www.virtualbox.org/wiki/Downloads


## Vagrantをインストールする

https://www.vagrantup.com/downloads.html

作業場所の作成
```
host $ cd c:\
host $ mkdir Vagrant & cd Vagrant
```

プラグインの追加
```
host $ vagrant plugin install vagrant-vbguest.
```

## rails-dev-boxを起動する

```
host $ git clone https://github.com/rails/rails-dev-box.git
host $ cd rails-dev-box
host $ vagrant up
```

sshでつながることを確認してログアウト
```
host $ vagrant ssh
```
```
vagrant@rails-dev-box:~$ exit
```

停止
```
vagrant halt
```

vagrantfileに同期変更の定義を追加する。
```
config.vm.synced_folder '.', '/vagrant', type: 'rsync'
```
※直接Windows10側からは触ることは稀だけど一応

起動
```
host $ vagrant up
host $ vagrant rsync-auto
host $ vagrant ssh
```

Windows10側のC:\Vagrant\rails-dev-boxと
Ubuntu側の/vagrantが同期されているか確認

```
host $ cd C:\Vagrant\rails-dev-box
host $ dir
```

```
vagrant@rails-dev-box:~$ cd /vagrant
vagrant@rails-dev-box:/vagrant$ ls
```

同期も問題ないならssh用のファイルを作成する
```
host $ vagrant ssh-config >> C:\Users\[ユーザー名]\.ssh\config
```


## VScodeインストール

https://code.visualstudio.com/download

Ctrl+Shift+xで拡張機能を表示し、`Remote Development`をインストールする。

VSCodeを立ち上げ、ウィンドウ左下の><ボタンを押下し、画面上部のコマンド候補から`Remote-SSH:Connect to host...`→`defualt`を選択する。

[メモ]他にもVagrantや他サーバーのSSH設定をしているときはAddになる。VagrantもIPアドレスポートの設定があるのでどこかでまとめる。

[メモ]Rails用の拡張機能もまとめる


## リポジトリ作成

railsのリポジトリがある場合はクローン。

```
vagrant@rails-dev-box:/vagrant$ git clone xxxxx
```

新規作成からの場合はRailsのインストール(rails newできないので入ってないんだと思う)
```
gem install rails
rails new app1 -d postgresql --skip-unit-test
```