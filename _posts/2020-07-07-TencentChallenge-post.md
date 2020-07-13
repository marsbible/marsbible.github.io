---
layout: post
title: 腾讯安全平台部极客技术挑战赛第一轮题目解析
modified: 2020-07-07
tags: [面试]
categories: [技术]
---

### 题目
具体参见链接：

https://mp.weixin.qq.com/s/tZ9BmXfzGYpzrNm2Jl5Mrw

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
from Crypto.Cipher import AES
import base64
import time
import gzip
from hashlib import md5
import sys
import io
sys.stdout = io.TextIOWrapper(sys.stdout.detach(), encoding='utf-8', line_buffering=True)

def Decrypt(key:str, text:str) -> str:
  if len(key) < 32: key += ' ' * (32 - len(key))
  elif len(key) > 32: key = key[0:32]
  cipher = AES.new(bytes(key,encoding='utf-8'), AES.MODE_CBC, bytes(AES.block_size))
  return str(gzip.decompress(bytes.strip(cipher.decrypt(base64.b64decode(text)))), encoding='utf-8')


def Pass(id, priv_key):
  prefix = str(id) + str(int(time.time()))
  pub_key = prefix + md5(bytes(prefix + priv_key, 'utf8')).hexdigest()
  print('恭喜通过第%d关,通关公钥:%s' % (id, pub_key))

key=input('1+1=')
exec(Decrypt(key,'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'))
```

### 分析

* 为了方便调试，我们将Decrypt函数加上打印，方便观察：
  ```python
  def Decrypt(key: str, text: str) -> str:
    if len(key) < 32: key += ' ' * (32 - len(key))
    elif len(key) > 32: key = key[0:32]
    cipher = AES.new(bytes(key, encoding='utf-8'), AES.MODE_CBC, bytes(AES.block_size))
    src = str(gzip.decompress(bytes.strip(cipher.decrypt(base64.b64decode(text)))), encoding='utf-8')
    print(src)
    return src
  ```

### 题解

1. 题目1：1+1=?，直接输入2
2. 题目2：(x*18-27)/3-(x+7496)=0, x=?

   一元二次方程，套公式得x=1501
3. 题目3：41*x-31*x^2+74252906=0,(x^2表示x的2次方,下同),x的某个根=?
   
   同上题一样套公式得x=-1547
4. 题目4：(1234567^12345678901234567890)%999999997=?

   快速幂的计算，比较常见了，此题目算之前可以用欧拉定理简化一下：
   - 1234567和999999997互质，999999997=6257*71*2251, φ(999999997)=6256*70*2250=985320000
   - (1234567^12345678901234567890)%999999997=((1234567^(985320000))^12529613629*(1234567^308287890))%999999997
   - = (1234567^φ(999999997)%9999999)^12529613629 * (1234567^308287890)%999999997
   - = (1234567^308287890)%999999997 = (1234567 ^ 0x12601992‬) % 999999997
   - 这样只要循环29次模平方加上10次模乘即可得到计算结果，答案是42031180
5. 题目5：1_2_3_4_5_6_7_8_9=-497,每处_填入1个运算符+-*/,且4个运算符必须都用上,使得等式成立(答案保证唯一),表达式为?

   暴力循环即可，4^8次循环毛毛雨啦，答案是1/2*3*4-5+6-7*8*9
```python
ops = ['+', '-', '*', '/']
fs = '1%s2%s3%s4%s5%s6%s7%s8%s9'

for i in range(0, 65536):
  ll = []
  for j in range(0, 8):
      ll.append(ops[(i >> (j * 2)) & 0x3])

  x = fs % tuple(ll)
  if eval(x) == -497:
    print(x)
```
6. 题目6：x^5-2*x^4+3*x^3-4*x^2-5*x-6=0, x(精确到小数点后14位)=?

   连续函数，x=0时，y=-6，x=3时，y=105，采用二分查找在0-3的区间搜索零点，答案是2.19488134060852
   ```python
    def f(x):
        return x**5-2*(x**4)+3*(x**3)-4*(x**2)-5*x-6

    begin = 0.0
    end = 3.0
    mid = 0.0

    while True:
        mid = (begin+end)/2
        v = f(mid)
        if mid-begin < 1e-15:
            break
        if v < 0:
            begin = mid
        else:
            end = mid

    print(mid)
   ```
7. 题目7：请输入8位数字PIN码: ？

   此题目较为简略，看一下打印出来的源代码发现是求一个md5一千万次后的碰撞，这运算量夸张了，明显不是暴力能算出来的，既然是8位数字PIN码，那key的实际搜索空间也就一亿，穷举这些数字，如果解密解压成功的，就是结果了，不成功的解压时会抛出异常，答案是79547124。

   ```python
   def Hash(context):
       result = md5(bytes(context, 'utf8'))
       for i in range(0, 10000000):
           result = md5(result.digest())
       return result.hexdigest()

   Pass(6, 'azfH^1f;*nag91')
   key=input('请输入8位数字PIN码:')
   print("验证中……")
   if Hash(key) == '5f4654140971c47658de19d62ba472b6':
       exec(Decrypt(key,'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'))
   else:
       print("PIN码错误")
   ```

   ```python
   # 暴力穷举
   for i in range(100000000, 0, -1):
     try:
       key = "%08d" % i
       exec(Decrypt(key, 'rO0WHI...'))
       print(key)
     except:
       pass 
   ``` 
8. 题目8：计算第8关密钥中,时间可能非常长...此道题目难度较大，答案是3298258025528854553625821261494113，先看它的源代码：
   ```python
    Pass(7, 'TfpPfagH/?fa*gl01')
    print('计算第8关密钥中,时间可能非常长...')
    stack = []
    ins_len = [1] * 5 + [2] * 9 + [9, 1]
    reg = [0] * 16
    code = base64.b64decode('zyLpMs8CL9Oy/3QDdRlURZRGFHQHdRhURZFGIL/lv+MiNi+70AXRBtMD1wfYCNkJ5v3/iV14RWMB0n+/xgk=')
    while True:
        ins, r0 = code[reg[15]] >> 4, code[reg[15]] & 15
        length = ins_len[ins]
        if length > 1:
            arg = code[reg[15] + 1 : reg[15] + length]
            if length == 2: r1 = arg[0] >> 4; r2 = arg[0] & 15
        reg[15] += length
        if 0 == ins : break
        elif 1 == ins : stack.append(reg[r0])
        elif 2 == ins : reg[r0] = stack.pop()
        elif 3 == ins : 
            if not reg[r0] : reg[15] += ins_len[code[reg[15]] >> 4]
        elif 4 == ins : reg[r0] = 0 if reg[r0] else 1
        elif 5 == ins : reg[r0] = reg[r1] + reg[r2]
        elif 6 == ins : reg[r0] = reg[r1] - reg[r2]
        elif 7 == ins : reg[r0] = reg[r1] * reg[r2]
        elif 8 == ins : reg[r0] = reg[r1] / reg[r2]
        elif 9 == ins : reg[r0] = reg[r1] % reg[r2]
        elif 10 == ins : reg[r0] = 1 if reg[r1] < reg[r2] else 0
        elif 11 == ins : stack.append(reg[r0]); reg[r0] += int.from_bytes(arg, byteorder='little', signed=True)
        elif 12 == ins : reg[r0] += int.from_bytes(arg, byteorder='little', signed=True)
        elif ins in (13, 14) : reg[r0] = int.from_bytes(arg, byteorder='little', signed=True)

    key = str(reg[0])+str(reg[1])
    exec(Decrypt(key,'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'))
   ```

   这一看是一个小虚拟机执行指令，根据它的逻辑反汇编出来看看吧，后面基于汇编还原逻辑就看每个人的分析能力了
   ```python
   ins_names = [
    'RETURN', 'PUSH', 'POP', 'JZ', 'NOT', 'ADD', 'SUB', 'MUL', 'DIV', 'MOD', 'CMP', 'PUSHINC', 'INC', 'MOV', 'MOV8', 'NOP'
   ]
   while True:
     ins, r0 = code[reg[15]] >> 4, code[reg[15]] & 15
     length = ins_len[ins]
     if length > 1:
         arg = code[reg[15] + 1 : reg[15] + length]
         if length == 2: r1 = arg[0] >> 4; r2 = arg[0] & 15

     if ins_len[ins] == 1:
         print(reg[15], ":", ins_names[ins], "r"+str(r0))
     elif ins_len[ins] == 2:
         if ins < 11:
             print(reg[15], ":", ins_names[ins], "r"+str(r0), "r"+str(r1), "r"+str(r2))
         else:
             print(reg[15], ":", ins_names[ins], "r"+str(r0), int.from_bytes(arg, byteorder='little', signed=True))
     else:
        print(reg[15], ":", ins_names[ins], "r"+str(r0), int.from_bytes(arg, byteorder='little', signed=True)) 

     reg[15] += length
     if 0 == ins : break
     elif 1 == ins : stack.append(reg[r0])
     elif 2 == ins : reg[r0] = stack.pop()
     elif 3 == ins : 
         if not reg[r0] : reg[15] += ins_len[code[reg[15]] >> 4]
     elif 4 == ins : reg[r0] = 0 if reg[r0] else 1
     elif 5 == ins : reg[r0] = reg[r1] + reg[r2]
     elif 6 == ins : reg[r0] = reg[r1] - reg[r2]
     elif 7 == ins : reg[r0] = reg[r1] * reg[r2]
     elif 8 == ins : reg[r0] = reg[r1] / reg[r2]
     elif 9 == ins : reg[r0] = reg[r1] % reg[r2]
     elif 10 == ins : reg[r0] = 1 if reg[r1] < reg[r2] else 0
     elif 11 == ins : stack.append(reg[r0]); reg[r0] += int.from_bytes(arg, byteorder='little', signed=True)
     elif 12 == ins : reg[r0] += int.from_bytes(arg, byteorder='little', signed=True)
     elif ins in (13, 14) : reg[r0] = int.from_bytes(arg, byteorder='little', signed=True)
    ```
   能力所限，通过分析执行过程打印的汇编语句，发现是个递归的过程，f(n) = n + f(n-1) ... f(1) = 2^n - 1

   初始n=127，f里对r0和r1进行计算，r0, r1 = (r0 * 3 + r1 * 9) % 99999999999999997, (r0 * 7 + r1 * 8) % 99999999999999997，r0和r1初始值为5和6，总循环次数高的吓人，硬算确实不行。多变量线性迭代，可以想到使用矩阵表示后能够较好的优化，[r0', r1'] = [[3, 9], [7, 8]]^n * [r0, r1] % 99999999999999997，这里就要进行矩阵快速幂的计算了，和一般整数快速幂的算法类似。
   ```python
    mm = [[3, 9],
          [7, 8]]
    mod = 99999999999999997
    r0 = 5
    r1 = 6

    # 计算2^POW-1次矩阵幂
    POW = 127
    def mmul(m, n, module):
        r = [[0, 0], [0, 0]]
        r[0][0] = (m[0][0] * n[0][0] + m[0][1] * n[1][0]) % module
        r[0][1] = (m[0][0] * n[0][1] + m[0][1] * n[1][1]) % module
        r[1][0] = (m[1][0] * n[0][0] + m[1][1] * n[1][0]) % module
        r[1][1] = (m[1][0] * n[0][1] + m[1][1] * n[1][1]) % module
        return r

    mms = [None] * POW
    mms[0] = mm
    # 计算2^1到2^(POW-1)次矩阵幂
    for i in range(1, POW):
        mms[i] = mmul(mms[i-1], mms[i-1], mod)

    # 将2^0到2^(POW-1)次矩阵幂进行相乘，也就得到2^POW - 1次矩阵幂
    final = mms[0]
    for i in range(1, POW):
        final = mmul(final, mms[i], mod)
    
    # 乘以初始向量
    r0 = (5 * final[0][0] + 6 * final[0][1]) % mod
    r1 = (5 * final[1][0] + 6 * final[1][1]) % mod
    print(str(r0) + str(r1))
   ```