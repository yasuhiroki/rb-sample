# rb コマンドのサンプル

# 奇数/偶数

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

# 最大値/最小値を一緒に出す

```bash
alias minmax="rb 'minmax'"
```

ex)

```bash
$ seq 10 |  minmax
1
9
```

# 数値の合計

```bash
alias numsum="rb 'map(&:to_i).inject(:+)'"
```

ex)

```
$ seq 10 | numsum
55
```

# 文字連結

```bash
alias strsum="rb 'map(&:chomp).inject(:+)'"
```

ex)

```bash
$ seq 10 | strsum
12345678910
```

# 列を複製

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

# CSV to TSV

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

