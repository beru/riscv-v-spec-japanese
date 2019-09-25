## 20. ベクトル命令一覧

| Integer | Integer | FP   |      |      |        |      |      |      |        |      |      |      |
| :------ | :------ | :--- | ---- | ---- | ------ | ---- | ---- | ---- | ------ | ---- | ---- | ---- |
| funct3  |         |      |      |      | funct3 |      |      |      | funct3 |      |      |      |
| OPIVV   | V       |      |      |      | OPMVV  | V    |      |      | OPFVV  | V    |      |      |
| OPIVX   |         | X    |      |      | OPMVX  |      | X    |      | OPFVF  |      | F    |      |
| OPIVI   |         |      | I    |      |        |      |      |      |        |      |      |      |

| funct6 | funct6 | funct6 |      |            |        |      |      |             |        |      |      |           |
| :----- | :----- | :----- | ---- | ---------- | ------ | ---- | ---- | ----------- | ------ | ---- | ---- | --------- |
| 000000 | V      | X      | I    | vadd       | 000000 | V    |      | vredsum     | 000000 | V    | F    | vfadd     |
| 000001 |        |        |      |            | 000001 | V    |      | vredand     | 000001 | V    |      | vfredsum  |
| 000010 | V      | X      |      | vsub       | 000010 | V    |      | vredor      | 000010 | V    | F    | vfsub     |
| 000011 |        | X      | I    | vrsub      | 000011 | V    |      | vredxor     | 000011 | V    |      | vfredosum |
| 000100 | V      | X      |      | vminu      | 000100 | V    |      | vredminu    | 000100 | V    | F    | vfmin     |
| 000101 | V      | X      |      | vmin       | 000101 | V    |      | vredmin     | 000101 | V    |      | vfredmin  |
| 000110 | V      | X      |      | vmaxu      | 000110 | V    |      | vredmaxu    | 000110 | V    | F    | vfmax     |
| 000111 | V      | X      |      | vmax       | 000111 | V    |      | vredmax     | 000111 | V    |      | vfredmax  |
| 001000 |        |        |      |            | 001000 |      |      |             | 001000 | V    | F    | vfsgnj    |
| 001001 | V      | X      | I    | vand       | 001001 |      |      |             | 001001 | V    | F    | vfsgnjn   |
| 001010 | V      | X      | I    | vor        | 001010 |      |      |             | 001010 | V    | F    | vfsgnjx   |
| 001011 | V      | X      | I    | vxor       | 001011 |      |      |             | 001011 |      |      |           |
| 001100 | V      | X      | I    | vrgather   | 001100 |      |      |             | 001100 |      |      |           |
| 001101 |        |        |      |            | 001101 |      |      |             | 001101 |      |      |           |
| 001110 |        | X      | I    | vslideup   | 001110 |      | X    | vslide1up   | 001110 |      |      |           |
| 001111 |        | X      | I    | vslidedown | 001111 |      | X    | vslide1down | 001111 |      |      |           |

| funct6 | funct6 | funct6 |      |            |        |      |      |           |        |      |      |                 |
| :----- | :----- | :----- | ---- | ---------- | ------ | ---- | ---- | --------- | ------ | ---- | ---- | --------------- |
| 010000 | V      | X      | I    | vadc       | 010000 | V    |      | VWXUNARY0 | 010000 | V    |      | VWFUNARY0       |
|        |        |        |      |            | 010000 |      | X    | VRXUNARY0 | 010000 |      | F    | VRFUNARY0       |
| 010001 | V      | X      | I    | vmadc      | 010001 |      |      |           | 010001 |      |      |                 |
| 010010 | V      | X      |      | vsbc       | 010010 |      |      |           | 010010 |      |      |                 |
| 010011 | V      | X      |      | vmsbc      | 010011 |      |      |           | 010011 |      |      |                 |
| 010100 |        |        |      |            | 010100 | V    |      | VMUNARY0  | 010100 |      |      |                 |
| 010101 |        |        |      |            | 010101 |      |      |           | 010101 |      |      |                 |
| 010110 |        |        |      |            | 010110 |      |      |           | 010110 |      |      |                 |
| 010111 | V      | X      | I    | vmerge/vmv | 010111 | V    |      | vcompress | 010111 |      | F    | vfmerge.vf/vfmv |
| 011000 | V      | X      | I    | vmseq      | 011000 | V    |      | vmandnot  | 011000 | V    | F    | vmfeq           |
| 011001 | V      | X      | I    | vmsne      | 011001 | V    |      | vmand     | 011001 | V    | F    | vmfle           |
| 011010 | V      | X      |      | vmsltu     | 011010 | V    |      | vmor      | 011010 |      |      |                 |
| 011011 | V      | X      |      | vmslt      | 011011 | V    |      | vmxor     | 011011 | V    | F    | vmflt           |
| 011100 | V      | X      | I    | vmsleu     | 011100 | V    |      | vmornot   | 011100 | V    | F    | vmfne           |
| 011101 | V      | X      | I    | vmsle      | 011101 | V    |      | vmnand    | 011101 |      | F    | vmfgt           |
| 011110 |        | X      | I    | vmsgtu     | 011110 | V    |      | vmnor     | 011110 |      |      |                 |
| 011111 |        | X      | I    | vmsgt      | 011111 | V    |      | vmxnor    | 011111 |      | F    | vmfge           |

| funct6 | funct6 | funct6 |      |         |        |      |      |         |        |      |      |          |
| :----- | :----- | :----- | ---- | ------- | ------ | ---- | ---- | ------- | ------ | ---- | ---- | -------- |
| 100000 | V      | X      | I    | vsaddu  | 100000 | V    | X    | vdivu   | 100000 | V    | F    | vfdiv    |
| 100001 | V      | X      | I    | vsadd   | 100001 | V    | X    | vdiv    | 100001 |      | F    | vfrdiv   |
| 100010 | V      | X      |      | vssubu  | 100010 | V    | X    | vremu   | 100010 | V    |      | VFUNARY0 |
| 100011 | V      | X      |      | vssub   | 100011 | V    | X    | vrem    | 100011 | V    |      | VFUNARY1 |
| 100100 | V      | X      | I    | vaadd   | 100100 | V    | X    | vmulhu  | 100100 | V    | F    | vfmul    |
| 100101 | V      | X      | I    | vsll    | 100101 | V    | X    | vmul    | 100101 |      |      |          |
| 100110 | V      | X      |      | vasub   | 100110 | V    | X    | vmulhsu | 100110 |      |      |          |
| 100111 | V      | X      |      | vsmul   | 100111 | V    | X    | vmulh   | 100111 |      | F    | vfrsub   |
| 101000 | V      | X      | I    | vsrl    | 101000 |      |      |         | 101000 | V    | F    | vfmadd   |
| 101001 | V      | X      | I    | vsra    | 101001 | V    | X    | vmadd   | 101001 | V    | F    | vfnmadd  |
| 101010 | V      | X      | I    | vssrl   | 101010 |      |      |         | 101010 | V    | F    | vfmsub   |
| 101011 | V      | X      | I    | vssra   | 101011 | V    | X    | vnmsub  | 101011 | V    | F    | vfnmsub  |
| 101100 | V      | X      | I    | vnsrl   | 101100 |      |      |         | 101100 | V    | F    | vfmacc   |
| 101101 | V      | X      | I    | vnsra   | 101101 | V    | X    | vmacc   | 101101 | V    | F    | vfnmacc  |
| 101110 | V      | X      | I    | vnclipu | 101110 |      |      |         | 101110 | V    | F    | vfmsac   |
| 101111 | V      | X      | I    | vnclip  | 101111 | V    | X    | vnmsac  | 101111 | V    | F    | vfnmsac  |

| funct6 | funct6 | funct6 |      |           |        |      |      |          |        |      |      |            |
| :----- | :----- | :----- | ---- | --------- | ------ | ---- | ---- | -------- | ------ | ---- | ---- | ---------- |
| 110000 | V      |        |      | vwredsumu | 110000 | V    | X    | vwaddu   | 110000 | V    | F    | vfwadd     |
| 110001 | V      |        |      | vwredsum  | 110001 | V    | X    | vwadd    | 110001 | V    |      | vfwredsum  |
| 110010 |        |        |      |           | 110010 | V    | X    | vwsubu   | 110010 | V    | F    | vfwsub     |
| 110011 |        |        |      |           | 110011 | V    | X    | vwsub    | 110011 | V    |      | vfwredosum |
| 110100 |        |        |      |           | 110100 | V    | X    | vwaddu.w | 110100 | V    | F    | vfwadd.w   |
| 110101 |        |        |      |           | 110101 | V    | X    | vwadd.w  | 110101 |      |      |            |
| 110110 |        |        |      |           | 110110 | V    | X    | vwsubu.w | 110110 | V    | F    | vfwsub.w   |
| 110111 |        |        |      |           | 110111 | V    | X    | vwsub.w  | 110111 |      |      |            |
| 111000 | V      |        |      | vdotu     | 111000 | V    | X    | vwmulu   | 111000 | V    | F    | vfwmul     |
| 111001 | V      |        |      | vdot      | 111001 |      |      |          | 111001 | V    |      | vfdot      |
| 111010 |        |        |      |           | 111010 | V    | X    | vwmulsu  | 111010 |      |      |            |
| 111011 |        |        |      |           | 111011 | V    | X    | vwmul    | 111011 |      |      |            |
| 111100 | V      | X      |      | vwsmaccu  | 111100 | V    | X    | vwmaccu  | 111100 | V    | F    | vfwmacc    |
| 111101 | V      | X      |      | vwsmacc   | 111101 | V    | X    | vwmacc   | 111101 | V    | F    | vfwnmacc   |
| 111110 | V      | X      |      | vwsmaccsu | 111110 | V    | X    | vwmaccsu | 111110 | V    | F    | vfwmsac    |
| 111111 |        | X      |      | vwsmaccus | 111111 |      | X    | vwmaccus | 111111 | V    | F    | vfwnmsac   |



| vs2   |         |
| :---- | :------ |
| 00000 | vmv.s.x |

| vs1   |         |
| :---- | :------ |
| 00000 | vmv.x.s |
| 10000 | vpopc   |
| 10001 | vfirst  |

| vs2   |          |
| :---- | :------- |
| 00000 | vfmv.s.f |

| vs1   |          |
| :---- | :------- |
| 00000 | vfmv.f.s |

| vs1                   | name          |
| :-------------------- | :------------ |
| single-width converts |               |
| 00000                 | vfcvt.xu.f.v  |
| 00001                 | vfcvt.x.f.v   |
| 00010                 | vfcvt.f.xu.v  |
| 00011                 | vfcvt.f.x.v   |
|                       |               |
| widening converts     |               |
| 01000                 | vfwcvt.xu.f.v |
| 01001                 | vfwcvt.x.f.v  |
| 01010                 | vfwcvt.f.xu.v |
| 01011                 | vfwcvt.f.x.v  |
| 01100                 | vfwcvt.f.f.v  |
|                       |               |
| narrowing converts    |               |
| 10000                 | vfncvt.xu.f.v |
| 10001                 | vfncvt.x.f.v  |
| 10010                 | vfncvt.f.xu.v |
| 10011                 | vfncvt.f.x.v  |
| 10100                 | vfncvt.f.f.v  |

| vs1   | name      |
| :---- | :-------- |
| 00000 | vfsqrt.v  |
| 10000 | vfclass.v |

| vs1   |       |
| :---- | :---- |
| 00001 | vmsbf |
| 00010 | vmsof |
| 00011 | vmsif |
| 10000 | viota |
| 10001 | vid   |

