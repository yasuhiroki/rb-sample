# rb コマンドのサンプル

[rb](https://github.com/thisredone/rb) のサンプル

# Install

rb のインストールは次コマンドを実行

```bash
curl https://raw.githubusercontent.com/thisredone/rb/master/rb -o /usr/local/bin/rb && chmod +x ${USER_BIN_PATH}/bin/rb
```

## .rbrc を配置

```bash
cp .rbrc ~/
```

# 仕組み

STDINを配列として受け取り、 rb コマンドの引数を配列に続くメソッドチェーンとして実行する。

例えば `seq 5` の標準出力を受け取ると、

```ruby
[ '1\n', '2\n', '3\n', '4\n', '5\n' ]
```

の配列として受け取る。

その後 `rb drop 1` などとすると、

```ruby
[ '1\n', '2\n', '3\n', '4\n', '5\n' ].drop(1)
```

をしたことになる。

# `-l` オプション

`-l` オプションを付与すると一行ごとに `rb` コマンドの引数を実行する。  
感覚的には `sed` コマンドに近い。

ex)

```bash
$ ls -l | rb -l 'upcase'
TOTAL 32
-RW-R--R--  1 YASUHIROKI  STAFF  1067 11 27 02:21 LICENSE
-RW-R--R--  1 YASUHIROKI  STAFF  2303 11 27 02:41 README.MD
```

# `self` を活用する

`*` のような演算子や `[]` といった記号は `self` を使えば呼び出せる。

```bash
$ seq 5 | rb -l 'self*10'
1111111111
2222222222
3333333333
4444444444
5555555555
```

もちろん `send` でもいい。

```bash
$ seq 5 | rb -l 'send(:*, 10)'
1111111111
2222222222
3333333333
4444444444
5555555555
```

# alias に登録してオレオレCLIツールに

## 奇数/偶数

```bash
alias odd="rb 'each_slice(2).map(&:first)'"
alias even="rb 'each_slice(2).map(&:pop)'"
```

ex)

```bash
$ seq 10 | odd
1
3
5
7
9
$ seq 10 | even
2
4
6
8
10
```

## 最大値/最小値を一緒に出す

```bash
alias minmax="rb 'minmax'"
```

ex)

```bash
$ seq 10 |  minmax
1
9
```

## 数値の合計

```bash
alias numsum="rb 'map(&:to_i).inject(:+)'"
```

ex)

```
$ seq 10 | numsum
55
```

## 文字連結

```bash
alias strsum="rb 'map(&:chomp).inject(:+)'"
```

ex)

```bash
$ seq 10 | strsum
12345678910
```

## 列を複製

```bash
alias dup="rb -l '[self, self].join(\"\t\")'"
```

ex)

```bash
$ seq 10 | dup
1       1
2       2
3       3
4       4
5       5
6       6
7       7
8       8
9       9
10      10
```

## CSV to TSV

```bash
alias csv2tsv="rb -l 'CSV.parse(self)[0].join(\"\t\")'"
```

ex)

```bash
$ cat s.csv
a,b,c,d
a b,c,d,e
"a,b",c,d,e
$ cat s.csv | csv2tsv
a       b       c       d
a b     c       d       e
a,b     c       d       e
```

