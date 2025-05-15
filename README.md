# a

```asm
org 0x400
ans1: word 0x0000 ;мл
ans2: word 0xFFC0 ;ст

org 0x10
index: word 0x6DF
len: word 0x8
tmp_h: word ?
tmp_l: word ?
mask_plus: word 0x7F ;and
mask_minus: word 0xFF80 ;or

start: cla
  ld (index)+
  st $tmp_h
  and #0x40
  beq plus
  jump minus

plus: ld $tmp_h
  and $mask_plus
  st $tmp_h
  ld (index)+
  st $tmp_l
  jump check

minus: ld $tmp_h
  or $mask_minus
  st $tmp_h
  ld (index)+
  st $tmp_l
  jump check

check: ld $tmp_h
  cmp $ans2
  beq aboba
  bpl save
  bmi next
aboba: ld $tmp_l
  cmp $ans1
  bcc save
  bcs next

save: ld $tmp_h
  st $ans2
  ld $tmp_l
  st $ans1
  jump next

next: ld $index
  add #0x02
  st $index
  loop $len
  jump start
  hlt

```

-------------------
```asm
org 0x6C1
word 0xFFFF
word 0xFFFF
word 0xFFFF
word 0xFFFF
word 0xFFFF
word 0xFFFF
word 0xFFFF
word 0xFFFF
word 0xFFFF
word 0xFFFF
word 0xFFFF
word 0xFFFF
word 0xFFFF
word 0xFFFF
word 0xFFFF
word 0xFFFF
word 0xFFFF
word 0xFFFF
word 0xFFFF
word 0xFFFF
word 0xFFFF
word 0xFFFF
word 0xFFFF
word 0xFFFF
word 0xFFFF

org 0x10
compare: word 0x0800
mask_plus: word 0x0FFF
mask_minus: word 0xF000
index: word 0x6C1
len: word 0x19
ans_index: word 0x400
curr: word ?
tmp: word ?
st_num: word ?
const: word 0x4BA

org 0x30
start: cla
  ld (index)+
   st $curr
  and $compare
  beq plus
  jump minus

plus: ld $curr
  and $mask_plus
  st $curr
  ld #0x00
  st $st_num
  jump cnt_st

minus: ld $curr
  or $mask_minus
  st $curr
  ld #0xFF
  st $st_num
  jump cnt_st

cnt_st: call func
  ld $st_num
  st (ans_index)+
  ld $tmp
  st (ans_index)+
  loop $len
  jump start
  hlt

func: ld $curr
  asl
  asl
  asl
  st $tmp
  ld $curr
  asl
  asl
  add $tmp
  add $curr 
  add $const
  st $tmp
  cla
  adc $st_num
  st $st_num
  ret
```
