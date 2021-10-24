# docker-on-vagrant

以下のサイトを参考に作った。

- https://qiita.com/necocoa/items/bd62ed3dba14b17552f2
- https://blog.mmmcorp.co.jp/blog/2020/05/25/vagrant-docker-mac

Linux OS に Ubuntu を選択したのは、今回の環境を作る上でサイトを見る限り多くの人が使っていたから。

## homebrew で必要な library を追加する

```sh
brew install mutagen-io/mutagen/mutagen
brew install vagrant --cask
brew install virtualbox --cask
```

## vagrant の setup

```sh
vagrant box add ubuntu/xenial64
vagrant plugin install vagrant-disksize vagrant-hostsupdater vagrant-mutagen vagrant-docker-compose vagrant-vbguest
```

## vagrant の起動

```sh
vagrant up --provision
vagrant ssh
```

`ubuntu` には docker はインストール済みのため、あとは、share フォルダにソースを `git clone` し、docker コマンドを実行すれば良い。



## 注意点

- mutagen-io が不安定なことがあるため、長時間作業を行わない場合はなるべく `vagrant halt` した方が良いです。
- vagrant でなく virtualbox から停止したり、強制的に落ちた場合などは mutagen のセッションが残った状態になります。
  - そうした場合、そのままでは vagrant を再起動できないため、以下のコマンドで mutagen のセッション状態を確認し、再起動してください。

```sh
# セッション確認方法
mutagen sync list

# 再起動方法
mutagen daemon stop
mutagen daemon start
```

- Rails の構成に合わせて、Mac <-> Linux で不要なフォルダを無視するように設定しています。
  - 調整したい場合は、`mutagen.yml` や `Vagrantfile`を修正してください。

