# Pickle Rick | TryHackMe
> A Rick and Morty CTF. Help turn Rick back into a human!
level -> Easy

使ったツール･コマンド
1. nmap
2. gobuster

まずはIPを設定しておく
`
export IP=machine_ip_adress
`

#### Task1, What is the first ingredient that Rick needs?
ピクルスを人間に戻すための素材を見つけるらしい(?

どのポートが空いているのか確認
`
nmap -sV -sC $IP
`
結果をみると80と22が空いているらしいので80を見に行く

