## 11. ベクトル算術演算命令フォーマット

The vector arithmetic instructions use a new major opcode (OP-V = 10101112) which neighbors OP-FP. The three-bit `funct3` field is used to define sub-categories of vector instructions.

ベクトル算術演算命令は新しいメジャーオペコード(OP-V = b1010111)を使用している。このオペコードはOP-FPと隣接しており、3ビットの`funct3`フィールドを使用してベクトル命令のサブカテゴリを定義している。

```
OP-Vメジャーオペコード下におけるベクトル算術命令のフォーマット

31       26  25   24      20 19      15 14   12 11      7 6     0
  funct6   | vm  |   vs2    |    vs1   | 0 0 0 |    vd   |1010111| OP-V (OPIVV)
  funct6   | vm  |   vs2    |    vs1   | 0 0 1 |  vd/rd  |1010111| OP-V (OPFVV)
  funct6   | vm  |   vs2    |    vs1   | 0 1 0 |  vd/rd  |1010111| OP-V (OPMVV)
  funct6   | vm  |   vs2    |   simm5  | 0 1 1 |    vd   |1010111| OP-V (OPIVI)
  funct6   | vm  |   vs2    |    rs1   | 1 0 0 |    vd   |1010111| OP-V (OPIVX)
  funct6   | vm  |   vs2    |    rs1   | 1 0 1 |    vd   |1010111| OP-V (OPFVF)
  funct6   | vm  |   vs2    |    rs1   | 1 1 0 |  vd/rd  |1010111| OP-V (OPMVX)
     6        1        5          5        3        5        7
```

### 11.1. ベクトル算術演算命令のエンコーディング

`funct3`フィールドがオペランドの型とソースオペランドの場所をエンコードしている。

| funct3[2:0] |      |      |       | Operands            | スカラオペランドのソース     |
| :---------- | :--- | :--- | :---- | ------------------- | ---------------------------- |
| 0           | 0    | 0    | OPIVV | ベクトル - ベクトル | -                            |
| 0           | 0    | 1    | OPFVV | ベクトル - ベクトル | -                            |
| 0           | 1    | 0    | OPMVV | ベクトル - ベクトル | -                            |
| 0           | 1    | 1    | OPIVI | ベクトル - 即値     | imm[4:0]                     |
| 1           | 0    | 0    | OPIVX | ベクトル - スカラ   | GPR x register rs1           |
| 1           | 0    | 1    | OPFVF | ベクトル - スカラ   | FP f register rs1            |
| 1           | 1    | 0    | OPMVX | ベクトル - スカラ   | GPR x register rs1           |
| 1           | 1    | 1    | OPCFG | スカラ - 即値       | GPR x register rs1 & rs2/imm |

整数算術演算は符号なし整数化、2の歩数表現での符号付整数を使用して実行される。どちらを使用するかはオペコードに依存する。

すべての標準的な浮動小数点算術演算はIEEE-754/2008標準に準拠する。すべてのベクトル浮動小数点演算は`frm`レジスタに基づいて動的な丸めモードを使用する。

ベクトル - ベクトル算術演算はベクトルグループから`vs2`と`vs1`で指定される2つのベクトルグループを取り、算術演算を実行する。

ベクトル - スカラ演算は3つの形式をとることができるが、すべてのケースにおいて`vs2`ベクトルレジスタグループにより1つのベクトルオペランドを取り、2番目のスカラソースオペランドは以下の3つのうちどれかの景色を選択する。

1. 整数演算では、スカラ値は`rs1`フィールドからエンコードされる5ビットの即値を使用する。値は符号拡張されるか、符号なし拡張される。
2. 整数演算では、スカラ値は`rs1`から指定される整数レジスタ`x`から取得される。もし`XLEN>SEW`であれば、`x`レジスタのうち界のビットが使用される。XLEN<SEWであれば、`x`レジスタの値はSEWビットまで符号拡張される。
3. 浮動小数点演算では、スカラ値は浮動小数点スカラレジスタ`f`から取得される。FLEN>SEWであれば、`f`レジスタはNaN-boxされた値かどうかをチェックし、`f`レジスタの会のビットが使用される。NaN-boxされた値でなければNaNが使用される。FLEN<SEWであれば、値はNaN-boxされSEWビット長まで拡張される。

> 現在提案されているZfinxバリアントは浮動小数点スカラの引数を`x`レジスタから取ることができる。

ベクトル算術演算命令は、vmフィールドの制御に基づいてマスクを適用することができる。

```
# ベクトルバイナリ算術演算命令のアセンブリ文法パタン

# Operations returning vector results, masked by vm (v0.t, <nothing>)
# ベクトル書き込みレジスタの指定により、算術演算結果はvmによりマスクされる(v0.tもしくは指定しない)
vop.vv  vd, vs2, vs1, vm  # 整数ベクトル - ベクトル演算 vd[i] = vs2[i] op vs1[i]
vop.vx  vd, vs2, rs1, vm  # 整数ベクトル - スカラ演算   vd[i] = vs2[i] op x[rs1]
vop.vi  vd, vs2, imm, vm  # 整数ベクトル - 即値演算     vd[i] = vs2[i] op imm

vfop.vv  vd, vs2, vs1, vm # 浮動小数点ベクトル - ベクトル演算 vd[i] = vs2[i] fop vs1[i]
vfop.vf  vd, vs2, rs1, vm # 浮動小数点ベクトル - スカラ演算   vd[i] = vs2[i] fop f[rs1]
```

エンコーディング中では、`vs2`は1番目のオペランドであり、`rs1/simm5`が2番目のオペランドとなっている。これは標準的なスカラ命令の順番とは逆である。この順番になっているのは現在のエンコーディングにおける1つのスカラレジスタを読み込む命令、`rs1`からレジスタを読み込む場合および5ビットの即値を取得するものと保持しているxxx。

```
# Assembly syntax pattern for vector ternary arithmetic instructions (multiply-add)
# 3つの読み込みオペランドを取るベクトル命令のアセンブリ文法パタン(乗算加算)

# 加算のオペランドを上書きする形式の整数演算命令
vop.vv vd, vs1, vs2, vm  # vd[i] = vs1[i] * vs2[i] + vd[i]
vop.vx vd, rs1, vs2, vm  # vd[i] = x[rs1] * vs2[i] + vd[i]

# 乗算のオペランドを上書きする形式の整数演算命令
vop.vv vd, vs1, vs2, vm  # vd[i] = vs1[i] * vd[i] + vs2[i]
vop.vx vd, rs1, vs2, vm  # vd[i] = x[rs1] * vd[i] + vs2[i]

# 加算のオペランドを上書きする形式の浮動小数点命令
vfop.vv vd, vs1, vs2, vm  # vd[i] = vs1[i] * vs2[i] + vd[i]
vfop.vf vd, rs1, vs2, vm  # vd[i] = f[rs1] * vs2[i] + vd[i]

# 乗算のオペランドを上書きする形式の浮動小数点命令
vfop.vv vd, vs1, vs2, vm  # vd[i] = vs1[i] * vd[i] + vs2[i]
vfop.vf vd, rs1, vs2, vm  # vd[i] = f[rs1] * vd[i] + vs2[i]
```

3オペランドを取る乗算加算命令では、アセンブリ命令の構文は、常に書き込みレジスタを先頭に配置し、`rs1`もしくは`vs1`を置き、最後に`vs2`を置く。この順序により乗算のオペランドが常に2番目に来るため、アセンブリ命令の算術演算の動作を自然に読むことができる。

### 11.2. ビット幅が広がるベクトル算術演算命令

A few vector arithmetic instructions are defined to be *widening* operations where the destination elements are 2*SEW wide and are stored in a vector register group with twice the number of vector registers.

いくつかのベクトル算術演算命令では**ビット幅が拡張される**演算命令として定義されており、書き込み先の要素は 2*SEW ビットまで拡張され、書き込み先のベクトルレジスタグループは2つ分使用される。

The first operand can be either single or double-width. These are generally written with a `vw*` prefix on the opcode or `vfw*` for vector floating-point operations.

1番目のオペランドでは、1倍幅もしくは2倍幅である。通常これらの命令はプレフィックスとして`vw*`がつけられ、ベクトル浮動小数点命令の場合は`vfw*`プレフィックスが付けられる。

```
ビット幅が拡張されるベクトル算術演算命令のアセンブリ文法パタン

# 演算結果は2倍幅、1つの1倍幅のソースオペランドを取る: 2*SEW = SEW op SEW
vwop.vv  vd, vs2, vs1, vm  # 整数ベクトル - ベクトル    vd[i] = vs2[i] op vs1[i]
vwop.vx  vd, vs2, rs1, vm  # 整数ベクトル - スカラ      vd[i] = vs2[i] op x[rs1]

# 最初のオペランドは2倍幅、2番目のオペランドは1倍幅で、演算結果は2倍幅: 2*SEW = 2*SEW op SEW
vwop.wv  vd, vs2, vs1, vm  # 整数ベクトル - ベクトル     vd[i] = vs2[i] op vs1[i]
vwop.wx  vd, vs2, rs1, vm  # 整数ベクトル - スカラ      vd[i] = vs2[i] op x[rs1]
```

> 最初は、`w`サフィックスをオペコードに使用していたが、ダブルワードの整数命令におけるワードサイズの命令を示すサフィックスである`w`と混同するため、`w`をプレフィックスに移動した。

> 浮動小数手のビット幅拡張命令は`vwf*`から`vfw*`に変更した。これはスカラの浮動小数点ビット幅拡張命令である`fw*`との一貫性を保つためである。

> 整数の乗算加算命令では、乗算加算命令のサイズを4SEWに増やすという選択肢があり得る(例えば 4SEW += SEWSEW)。これは`vw4`プレフィックスをオペコードに付与することで区別する。今回はこの命令は含まれていないが、仕様に追加される可能性がある。

書き込み先のベクトルレジスタグループはSEWとLMULが現在の設定よりも2倍になったかのように配置される(例えば、書き込み先の要素のサイズが2SEWであり、書き込み先のベクトルレジスタグループのLMULは2LMULとなっているように見える)。

すべてのビット幅を拡張する命令のために、書き込み先の要素の幅はサポートされているビット幅の範囲であり、LMULの値もサポートされているLMULの範囲に収まっている必要がある(例えば、現在のLMULが<=4であれば、<=8となる)。

書き込み先のベクトルレジスタグループはベクトルレジスタ番号で指定し、書き込み先のLMULの値において有効な値である必要がある。そうでなければ、不正命令例外が発生する。

The destination vector register group cannot overlap a source vector register group of a different element width (including the mask register if masked), otherwise an illegal instruction exception is raised.

書き込み先のベクトルレジスタグループは別の要素幅を持つソースベクトルレジスタグループとオーバラップしてはならない(マスクレジスタを使用していても、マスクを含めて被ってはならない)。そうでなければ、不正命令例外が発生する。

> 非ゼロの`vstart`をサポートする必要がある。

> `vw<op>.wv vd, vs2, vs1`フォーマットの命令では、`vd`と`vs2`が同一であっても問題ない。

### 11.3. ビット幅を縮小するベクトル算術演算命令

いくつかの命令では、2倍幅のソースベクトルオペランドを1倍幅の書き込みベクトルレジスタに書き込む命令が存在する。この命令では、ソースベクトルレジスタのサイズは現在のLMULおよびSEWの設定よりも2倍になっているように設定されており、通常のLMUL/SEWサイズのベクトルレジスタグループに縮小するための命令である。

(2*LMUL > 8)もしくは(2 * SEW) > ELENであれば、不正命令例外が発生する。

> 別の設計方針としては、LMULの設定をソースベクトルレジスタグループのサイズとして取り扱うというものである。この選択肢は、LMULの変更量を少なくできるという利点を持つと考えられている。

ソースベクトルレジスタグループと書き込みベクトルレジスタグループは、LMULの設定における有効なベクトルアドレスを指定しなければならない。そうでなければ、不正命令例外が発生する。

2番目路ベクトルレジスタ(`vs1`で指定される)は、書き込みベクトルレジスタと同一のサイズ(縮小されたサイズ)でなければならない。

書き込み先ベクトルレジスタグループは、1番目のベクトルレジスタグループ(`vs2`で指定される)とオーバラップしてはならない。書き込み先ベクトルレジスタグループは、マスクを使用する場合はLMUL=1でない限りマスクレジスタとオーバラップしてはならない。もしこの制約を違反する場合、不正命令例外が発生する。

> LMULと幅が同一で、要素のサイズが書き込みベクトルレジスタと同一のLMUL=1のときにマスクレジスタを上書きするために2番目のソースベクトルレジスタグループを上書きするのは安全であるxxx。

A `vn*` prefix on the opcode is used to distinguish these instructions in the assembler, or a `vfn*` prefix for narrowing floating-point opcodes.

アセンブラ中で命令を区別するために、`vn*`プレフィックスをオペコードに使用するか、浮動小数点命令の場合は`vfn*`プレフィックスを使用する。

> マスクレジスタを設定する比較演算命令も、暗黙的にビット幅を縮小する演算である。

