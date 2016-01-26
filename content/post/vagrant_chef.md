+++
date = "2016-01-26T19:56:25+09:00"
description = "Vagrant + Chefで `source ~/.bashrc` 出来なかったお話"
keywords = ['vagrant', 'chef']
title = "Vagrant + Chefで `source ~/.bashrc` 出来なかったお話"

+++

<!-- more -->

VagrantとChef使っていろいろと環境構築の自動化をやろうとしてるんだけど  
その途中で`source ~/.bashrc`やろうとして出来なかった

やろうとした内容は下記の通り  

```
bash "source bashrc" do
  user  "vagrant"
  group "vagrant"
  code "source ~/.bashrc"
end
```

`code "source ~/.bashrc"`の部分を絶対パス指定にしたり、`bash`じゃなくて`file`使ってみたりしたんだけど  
どうあがいても成功しなかった(´・ω・)


そこで気づいた  
あれ？おれ別に`source ~/.bashrc`したいんじゃなくね？  

そう、`.bashrc`に書いた`export PATH〜`を実行して、環境変数を書き換えたいだけなんだ  


ってことで、環境変数を変更してコマンドを実行するときに  

```
bash "exec hugahuga" do
  user  "vagrant"
  group "vagrant"
  environment "PATH" => "/home/vagrant/hogehoge/bin:#{ENV["PATH"]}"
  code "hugahuga"
end
```

ってかいて環境変数を指定してからコマンドを実行するようにすれば上手くいきましたとさ  

