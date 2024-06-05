# Binary Bomb Lab Write-up .

---

# Phase 1 .

---

- Let's get started.. The first thing is to write command "**bp phase_1**" to go to the first challenge and see the disassembly of the code by entering any input, so we assume that it will be "***input***".

![Screenshot 2024-04-29 191235.png](Binary%20Bomb%20Lab%20Write-up%205b45ea3675d64bf5bd0118d5d793f179/Screenshot_2024-04-29_191235.png)

```nasm
6cfd2010 48894c2408       mov     qword ptr [rsp+8], rcx
6cfd2015 55               push    rbp
6cfd2016 57               push    rdi
6cfd2017 4881ece8000000   sub     rsp, 0E8h
6cfd201e 488d6c2420       lea     rbp, [rsp+20h]
6cfd2023 488bfc           mov     rdi, rsp
6cfd2026 b93a000000       mov     ecx, 3Ah
6cfd202b b8cccccccc       mov     eax, 0CCCCCCCCh
6cfd2030 f3ab             rep stos dword ptr [rdi]
6cfd2032 488b8c2408010000 mov     rcx, qword ptr [rsp+108h]
6cfd203a 488d0dc93f0100   lea     rcx, [bomb!NULL_IMPORT_DESCRIPTOR+0x1bc6 (7ff66cfe600a)]
6cfd2041 e8a7f3ffff       call    bomb!@ILT+1000(CheckForDebuggerJustMyCode) (7ff66cfd13ed)
6cfd2046 488d15aba10000   lea     rdx, [bomb!`string' (7ff66cfdc1f8)]
6cfd204d 488b8de0000000   mov     rcx, qword ptr [rbp+0E0h]
6cfd2054 e8d6f2ffff       call    bomb!@ILT+810(strings_not_equal) (7ff66cfd132f)
6cfd2059 85c0             test    eax, eax
6cfd205b 7405             je      bomb!phase_1+0x52 (7ff66cfd2062)
6cfd205d e854f3ffff       call    bomb!@ILT+945(explode_bomb) (7ff66cfd13b6)
6cfd2062 488da5c8000000   lea     rsp, [rbp+0C8h]
6cfd2069 5f               pop     rdi
6cfd206a 5d               pop     rbp
6cfd206b c3               ret
```

- Well, we found a function called **(strings_not_equal)**. Let's go to it and find out what this function is for.

```nasm
6cfd2054 e8d6f2ffff       call    bomb!@ILT+810(strings_not_equal) (7ff66cfd132f)
```

- Well, after accessing the function, we have seen a change in the **rcx, rdx**, let's find out what it is.

![WinDbg 4_29_2024 7_38_03 PM.png](Binary%20Bomb%20Lab%20Write-up%205b45ea3675d64bf5bd0118d5d793f179/WinDbg_4_29_2024_7_38_03_PM.png)

> *Well, let's use the command "da + number rdx" to see what this register has in store for us.*
> 

![WinDbg 4_29_2024 7_42_21 PM.png](Binary%20Bomb%20Lab%20Write-up%205b45ea3675d64bf5bd0118d5d793f179/WinDbg_4_29_2024_7_42_21_PM.png)

- **Ok let's try this input to see what happens 😱 .**

![Screenshot 2024-04-29 200620.png](Binary%20Bomb%20Lab%20Write-up%205b45ea3675d64bf5bd0118d5d793f179/Screenshot_2024-04-29_200620.png)

- ***Boom, we did it. This Phase was solved successfully 🥳 . The input was already correct .***

# Phase 2 .

---

- After we successfully solved **Phase 1()** , we will now start in **Phase 2()** . The first thing we will use is ***bp Phase 2*** and we will enter “***input***” so that we can create a dissembly for Phase 2.

![Screenshot 2024-04-28 214321.png](Binary%20Bomb%20Lab%20Write-up%205b45ea3675d64bf5bd0118d5d793f179/Screenshot_2024-04-28_214321.png)

```nasm
63d52090 48894c2408       mov     qword ptr [rsp+8], rcx
63d52095 55               push    rbp
63d52096 57               push    rdi
63d52097 4881ec38010000   sub     rsp, 138h
63d5209e 488d6c2420       lea     rbp, [rsp+20h]
63d520a3 488bfc           mov     rdi, rsp
63d520a6 b94e000000       mov     ecx, 4Eh
63d520ab b8cccccccc       mov     eax, 0CCCCCCCCh
63d520b0 f3ab             rep stos dword ptr [rdi]
63d520b2 488b8c2458010000 mov     rcx, qword ptr [rsp+15
63d520ba 488b0597d60000   mov     rax, qword ptr [bomb!__security_cookie (7ff763d5f758)]
63d520c1 4833c5           xor     rax, rbp
63d520c4 48898508010000   mov     qword ptr [rbp+108h], rax
63d520cb 488d0d383f0100   lea     rcx, [bomb!__NULL_IMPORT_DESCRIPTOR+0x1bc6 (7ff763d6600a)]
63d520d2 e816f3ffff       call    bomb!@ILT+1000(__CheckForDebuggerJustMyCode) (7ff763d513ed)
63d520d7 488d5528         lea     rdx, [rbp+28h]
63d520db 488b8d30010000   mov     rcx, qword ptr [rbp+130h]
63d520e2 e8ebefffff       call    bomb!@ILT+205(read_six_numbers) (7ff763d510d2)
63d520e7 b804000000       mov     eax, 4
63d520ec 486bc000         imul    rax, rax, 0
63d520f0 837c052801       cmp     dword ptr [rbp+rax+28h], 1
63d520f5 7405             je      bomb!phase_2+0x6c (7ff763d520fc)
63d520f7 e8baf2ffff       call    bomb!@ILT+945(explode_bomb) (7ff763d513b6)
63d520fc c7450401000000   mov     dword ptr [rbp+4], 1
63d52103 eb08             jmp     bomb!phase_2+0x7d (7ff763d5210d)
63d52105 8b4504           mov     eax, dword ptr [rbp+4]
63d52108 ffc0             inc     eax
63d5210a 894504           mov     dword ptr [rbp+4], eax
63d5210d 837d0406         cmp     dword ptr [rbp+4], 6
63d52111 7d1f             jge     bomb!phase_2+0xa2 (7ff763d52132)
63d52113 48634504         movsxd  rax, dword ptr [rbp+4]
63d52117 8b4d04           mov     ecx, dword ptr [rbp+4]
63d5211a ffc9             dec     ecx
63d5211c 4863c9           movsxd  rcx, ecx
63d5211f 8b4c8d28         mov     ecx, dword ptr [rbp+rcx*4+28h]
63d52123 d1e1             shl     ecx, 1
63d52125 394c8528         cmp     dword ptr [rbp+rax*4+28h], ecx
63d52129 7405             je      bomb!phase_2+0xa0 (7ff763d52130)
63d5212b e886f2ffff       call    bomb!@ILT+945(explode_bomb) (7ff763d513b6)
63d52130 ebd3             jmp     bomb!phase_2+0x75 (7ff763d52105)
63d52132 488d4de0         lea     rcx, [rbp-20h]
63d52136 488d15539d0000   lea     rdx, [bomb!string'+0xf8 (7ff763d5be90)]
63d5213d e838f2ffff       call    bomb!@ILT+885(_RTC_CheckStackVars) (7ff763d5137a)
63d52142 488b8d08010000   mov     rcx, qword ptr [rbp+108h]
63d52149 4833cd           xor     rcx, rbp
63d5214c e8a8f0ffff       call    bomb!@ILT+500(__security_check_cookie) (7ff763d511f9)
63d52151 488da518010000   lea     rsp, [rbp+118h]
63d52158 5f               pop     rdi
63d52159 5d               pop     rbp
```

- After reading the code, I found a ***function*** called **(read_six_numbers)**, which indicates that the input will be 6 numbers. Let us discover what the numbers are by completing the code review.

```nasm
63d520e7 b804000000       mov     eax, 4
63d520ec 486bc000         imul    rax, rax, 0
63d520f0 837c052801       cmp     dword ptr [rbp+rax+28h], 1
63d520f5 7405             je      bomb!phase_2+0x6c (7ff763d520fc)
63d520f7 e8baf2ffff       call    bomb!@ILT+945(explode_bomb) (7ff763d513b6)
63d520fc c7450401000000   mov     dword ptr [rbp+4], 1
```

- Through the previous code, you noticed a matrix using the form r/mX. So it goes to this entry and checks that the address points to the first element where the input we have is stored on the stack. It compares the first number we entered with the integer 1, ***so this is how we get the first of the 6 numbers, which is “1”.***
- So if we write '1' as first and **jump to it (Avoid calling Bomb!phase_2+0x6c (00007ff6`037720fc) (Avoid calling bomb_bomb()).** Validation is done first.

```nasm
63d520fc c7450401000000   mov     dword ptr [rbp+4], 1
63d52103 eb08             jmp     bomb!phase_2+0x7d (7ff763d5210d)
63d52105 8b4504           mov     eax, dword ptr [rbp+4]
63d52108 ffc0             inc     eax
63d5210a 894504           mov     dword ptr [rbp+4], eax
63d5210d 837d0406         cmp     dword ptr [rbp+4], 6
63d52111 7d1f             jge     bomb!phase_2+0xa2 (7ff763d52132)
63d52113 48634504         movsxd  rax, dword ptr [rbp+4]
63d52117 8b4d04           mov     ecx, dword ptr [rbp+4]
63d5211a ffc9             dec     ecx
63d5211c 4863c9           movsxd  rcx, ecx
63d5211f 8b4c8d28         mov     ecx, dword ptr [rbp+rcx*4+28h]
63d52123 d1e1             shl     ecx, 1
63d52125 394c8528         cmp     dword ptr [rbp+rax*4+28h], ecx
63d52129 7405             je      bomb!phase_2+0xa0 (7ff763d52130)
63d5212b e886f2ffff       call    bomb!@ILT+945(explode_bomb) (7ff763d513b6)
63d52130 ebd3             jmp     bomb!phase_2+0x75 (7ff763d52105)
```

> **By reading and reviewing the code, we found that there is a loop, and it will be explained step by step.**
> 

```nasm
63d520fc c7450401000000   mov     dword ptr [rbp+4], 1
63d52103 eb08             jmp     bomb!phase_2+0x7d (7ff763d5210d)
```

- Through these two lines, we assume that **i = 0 ,** and then it **jumps to (7ff763d5210d).**

```nasm
63d5210d 837d0406         cmp     dword ptr [rbp+4], 6
63d52111 7d1f             jge     bomb!phase_2+0xa2 (7ff763d52132)
```

- After going to the address (7ff763d5210d) , we find in these two lines that it compares **“i < 6 “**,  and this is the second part of the loop.
- We all work on the third part of the loop, which is ***i++ .***

```nasm
63d52113 48634504         movsxd  rax, dword ptr [rbp+4]
63d52117 8b4d04           mov     ecx, dword ptr [rbp+4]
63d5211a ffc9             dec     ecx
63d5211c 4863c9           movsxd  rcx, ecx
63d5211f 8b4c8d28         mov     ecx, dword ptr [rbp+rcx*4+28h]
63d52123 d1e1             shl     ecx, 1
63d52125 394c8528         cmp     dword ptr [rbp+rax*4+28h], ecx
63d52129 7405             je      bomb!phase_2+0xa0 (7ff763d52130)
63d5212b e886f2ffff       call    bomb!@ILT+945(explode_bomb) (7ff763d513b6)
63d52130 ebd3             jmp     bomb!phase_2+0x75 (7ff763d52105)
```

- Through these lines we will find the **body** of the loop 😱 .

• **`shl`** instruction is generated by Microsoft VS compiler when performing multiplication of usually an **`unsigned integer`** by a **power of 2** (otherwise imul instruction is used). Here `shl ecx,1` essentially multiplies ecx by 2.

> ***So I will now write the code again, but in C based on my understanding of the code.***
> 

```c
  int nums[6]; 
  
  if ( nums[0] != "1" )
  {
     explode_bomb(); 
  }  
    read_six_numbers (ips, a);
    for (int i = 1; i < 6; ++i)
     {
         if ((num[i-1] * 2) != num[i])
             explode_bomb();
      }
```

- After translating the code into C language, we find that the number before it must be multiplied by 2, and we will start the first number with 1 as we mentioned previously, and thus the 6 numbers will be as follows: “**1 2 4 8 16 32”.**

![Screenshot 2024-04-28 234854.png](Binary%20Bomb%20Lab%20Write-up%205b45ea3675d64bf5bd0118d5d793f179/Screenshot_2024-04-28_234854.png)

<aside>
🔑 Boom 🤯 , **this Phase has been solved as well** 👌 , as we did before 😉 .

</aside>

# Phase 3 .

---

- Well, as we did before, in order to see the disassembly of the code, we write the command **“bp phase_3”** and then enter any input until we reach the assembly code for this phase.

![Screenshot 2024-04-29 203124.png](Binary%20Bomb%20Lab%20Write-up%205b45ea3675d64bf5bd0118d5d793f179/Screenshot_2024-04-29_203124.png)

```nasm
00007ff6`6cfd21ed 488d152ca00000   lea     rdx, [bomb!`string' (7ff66cfdc220)]
00007ff6`6cfd21f4 488b8d60010000   mov     rcx, qword ptr [rbp+160h]
00007ff6`6cfd21fb e8c6f0ffff       call    bomb!@ILT+705(sscanf) (7ff66cfd12c6)
00007ff6`6cfd2200 894564           mov     dword ptr [rbp+64h], eax
00007ff6`6cfd2203 837d6402         cmp     dword ptr [rbp+64h], 2
00007ff6`6cfd2207 7d05             jge     bomb!phase_3+0x7e (7ff66cfd220e)
00007ff6`6cfd2209 e8a8f1ffff       call    bomb!@ILT+945(explode_bomb) (7ff66cfd13b6)
00007ff6`6cfd220e 8b4504           mov     eax, dword ptr [rbp+4]
00007ff6`6cfd2211 898534010000     mov     dword ptr [rbp+134h], eax
00007ff6`6cfd2217 83bd3401000007   cmp     dword ptr [rbp+134h], 7
00007ff6`6cfd221e 776a             ja      bomb!phase_3+0xfa (7ff66cfd228a)
00007ff6`6cfd2220 48638534010000   movsxd  rax, dword ptr [rbp+134h]
00007ff6`6cfd2227 488d0dd2ddfeff   lea     rcx, [bomb!__enc$textbss$begin+0xfffffffffffff000 (7ff66cfc0000)]
00007ff6`6cfd222e 8b8481cc220100   mov     eax, dword ptr [rcx+rax*4+122CCh]
00007ff6`6cfd2235 4803c1           add     rax, rcx
00007ff6`6cfd2238 ffe0             jmp     rax
00007ff6`6cfd223a 8b4544           mov     eax, dword ptr [rbp+44h]
00007ff6`6cfd223d 0574020000       add     eax, 274h
00007ff6`6cfd2242 894544           mov     dword ptr [rbp+44h], eax
00007ff6`6cfd2245 8b4544           mov     eax, dword ptr [rbp+44h]
00007ff6`6cfd2248 2d4c020000       sub     eax, 24Ch
00007ff6`6cfd2248 2d4c020000       sub     eax, 24Ch
00007ff6`6cfd224d 894544           mov     dword ptr [rbp+44h], eax
00007ff6`6cfd2250 8b4544           mov     eax, dword ptr [rbp+44h]
00007ff6`6cfd2253 05b0020000       add     eax, 2B0h
00007ff6`6cfd2258 894544           mov     dword ptr [rbp+44h], eax
00007ff6`6cfd225b 8b4544           mov     eax, dword ptr [rbp+44h]
00007ff6`6cfd225e 83e87e           sub     eax, 7Eh
00007ff6`6cfd2261 894544           mov     dword ptr [rbp+44h], eax
00007ff6`6cfd2264 8b4544           mov     eax, dword ptr [rbp+44h]
00007ff6`6cfd2267 83c07e           add     eax, 7Eh
00007ff6`6cfd226a 894544           mov     dword ptr [rbp+44h], eax
00007ff6`6cfd226d 8b4544           mov     eax, dword ptr [rbp+44h]
00007ff6`6cfd2270 83e87e           sub     eax, 7Eh
00007ff6`6cfd2273 894544           mov     dword ptr [rbp+44h], eax
00007ff6`6cfd2276 8b4544           mov     eax, dword ptr [rbp+44h]
00007ff6`6cfd2279 83c07e           add     eax, 7Eh
00007ff6`6cfd227c 894544           mov     dword ptr [rbp+44h], eax
00007ff6`6cfd227f 8b4544           mov     eax, dword ptr [rbp+44h]
00007ff6`6cfd2282 83e87e           sub     eax, 7Eh
00007ff6`6cfd2285 894544           mov     dword ptr [rbp+44h], eax
00007ff6`6cfd2288 eb05             jmp     bomb!phase_3+0xff (7ff66cfd228f)
00007ff6`6cfd228a e827f1ffff       call    bomb!@ILT+945(explode_bomb) (7ff66cfd13b6)
00007ff6`6cfd228f 837d0405         cmp     dword ptr [rbp+4], 5
00007ff6`6cfd2293 7f08             jg      bomb!phase_3+0x10d (7ff66cfd229d)
00007ff6`6cfd2295 8b4524           mov     eax, dword ptr [rbp+24h]
00007ff6`6cfd2298 394544           cmp     dword ptr [rbp+44h], eax
00007ff6`6cfd229b 7405             je      bomb!phase_3+0x112 (7ff66cfd22a2)
00007ff6`6cfd229d e814f1ffff       call    bomb!@ILT+945(explode_bomb) (7ff66cfd13b6)
00007ff6`6cfd22a2 488d4de0         lea     rcx, [rbp-20h]
00007ff6`6cfd22a6 488d15a39c0000   lea     rdx, [bomb!`string'+0x1b8 (7ff66cfdbf50)]
00007ff6`6cfd22ad e8c8f0ffff       call    bomb!@ILT+885(_RTC_CheckStackVars) (7ff66cfd137a)
00007ff6`6cfd22b2 488b8d38010000   mov     rcx, qword ptr [rbp+138h]
00007ff6`6cfd22b9 4833cd           xor     rcx, rbp
00007ff6`6cfd22bc e838efffff       call    bomb!@ILT+500(__security_check_cookie) (7ff66cfd11f9)
00007ff6`6cfd22c1 488da548010000   lea     rsp, [rbp+148h]
00007ff6`6cfd22c8 5f               pop     rdi
00007ff6`6cfd22c9 5d               pop     rbp
00007ff6`6cfd22ca c3               ret
```

- This is the important part ***to check*** ( يلا بيناا ).

```nasm
6cfd21ed 488d152ca00000   lea     rdx, [bomb!`string' (7ff66cfdc220)]
6cfd21f4 488b8d60010000   mov     rcx, qword ptr [rbp+160h]
6cfd21fb e8c6f0ffff       call    bomb!@ILT+705(sscanf) (7ff66cfd12c6)
```

> Well, here we notice a call to a function called **(sscanf)** . I think this function receives a number of inputs, but what are they ? Let's find out.
> 

![WinDbg 4_29_2024 8_51_15 PM.png](Binary%20Bomb%20Lab%20Write-up%205b45ea3675d64bf5bd0118d5d793f179/WinDbg_4_29_2024_8_51_15_PM.png)

- **Well, through these outputs, we discover that the input contains two numbers, an integer. Let us check the rest of the code and find out what the two numbers are.**

```nasm
00007ff6`6cfd220e 8b4504           mov     eax, dword ptr [rbp+4]
00007ff6`6cfd2211 898534010000     mov     dword ptr [rbp+134h], eax
00007ff6`6cfd2217 83bd3401000007   cmp     dword ptr [rbp+134h], 7
00007ff6`6cfd221e 776a             ja      bomb!phase_3+0xfa (7ff66cfd228a)
```

> From these instructions, we conclude that the first number must be **less than 7**, otherwise the ***bomb will explode***.
> 

```nasm
00007ff6`6cfd228f 837d0405         cmp     dword ptr [rbp+4], 5
00007ff6`6cfd2293 7f08             jg      bomb!phase_3+0x10d (7ff66cfd229d)
00007ff6`6cfd2295 8b4524           mov     eax, dword ptr [rbp+24h]
00007ff6`6cfd2298 394544           cmp     dword ptr [rbp+44h], eax
00007ff6`6cfd229b 7405             je      bomb!phase_3+0x112 (7ff66cfd22a2)
00007ff6`6cfd229d e814f1ffff       call    bomb!@ILT+945(explode_bomb) (7ff66cfd13b6)
00007ff6`6cfd22a2 488d4de0         lea     rcx, [rbp-20h]
```

> We also note here that the first number ***must be less than 5*** so that the bomb does not explode.
> 

```nasm
00007ff6`6cfd2298 394544           cmp     dword ptr [rbp+44h], eax
```

- **The instruction `cmp dword ptr [rbp+44h],eax` then compares the second integer (inside eax) with value at `[rbp+44h]` . Let’s display a DWORD (`dd`) value at `[rbp+44h]`.**

![WinDbg 4_29_2024 10_16_59 PM.png](Binary%20Bomb%20Lab%20Write-up%205b45ea3675d64bf5bd0118d5d793f179/WinDbg_4_29_2024_10_16_59_PM.png)

- Since, **`0xffffffe6`** is **`-26`** in signed decimal . Therefore, the input becomes `(num < 5) -26`

![Screenshot 2024-04-29 222040.png](Binary%20Bomb%20Lab%20Write-up%205b45ea3675d64bf5bd0118d5d793f179/Screenshot_2024-04-29_222040.png)

- ***Boom, we did it again. This phase has been resolved as well. Congratulations***

# Phase 4 .

---

```nasm
00007ff6`6cfd2350 48894c2408       mov     qword ptr [rsp+8], rcx
00007ff6`6cfd2355 55               push    rbp
00007ff6`6cfd2356 57               push    rdi
00007ff6`6cfd2357 4881ec88010000   sub     rsp, 188h
00007ff6`6cfd235e 488d6c2420       lea     rbp, [rsp+20h]
00007ff6`6cfd2363 488bfc           mov     rdi, rsp
00007ff6`6cfd2366 b962000000       mov     ecx, 62h
00007ff6`6cfd236b b8cccccccc       mov     eax, 0CCCCCCCCh
00007ff6`6cfd2370 f3ab             rep stos dword ptr [rdi]
00007ff6`6cfd2372 488b8c24a8010000 mov     rcx, qword ptr [rsp+1A8h]
00007ff6`6cfd237a 488b05d7d30000   mov     rax, qword ptr [bomb!__security_cookie (7ff66cfdf758)]
00007ff6`6cfd2381 4833c5           xor     rax, rbp
00007ff6`6cfd2384 48898558010000   mov     qword ptr [rbp+158h], rax
00007ff6`6cfd238b 488d0d783c0100   lea     rcx, [bomb!__NULL_IMPORT_DESCRIPTOR+0x1bc6 (7ff66cfe600a)]
00007ff6`6cfd2392 e856f0ffff       call    bomb!@ILT+1000(__CheckForDebuggerJustMyCode) (7ff66cfd13ed)
00007ff6`6cfd2397 4c8d4d24         lea     r9, [rbp+24h]
00007ff6`6cfd239b 4c8d4504         lea     r8, [rbp+4]
00007ff6`6cfd239f 488d157a9e0000   lea     rdx, [bomb!`string' (7ff66cfdc220)]
00007ff6`6cfd23a6 488b8d80010000   mov     rcx, qword ptr [rbp+180h]
00007ff6`6cfd23ad e814efffff       call    bomb!@ILT+705(sscanf) (7ff66cfd12c6)
00007ff6`6cfd23b2 898584000000     mov     dword ptr [rbp+84h], eax
00007ff6`6cfd23b8 83bd8400000002   cmp     dword ptr [rbp+84h], 2
00007ff6`6cfd23bf 750c             jne     bomb!phase_4+0x7d (7ff66cfd23cd)
00007ff6`6cfd23c1 837d0400         cmp     dword ptr [rbp+4], 0
00007ff6`6cfd23c5 7c06             jl      bomb!phase_4+0x7d (7ff66cfd23cd)
00007ff6`6cfd23c7 837d040e         cmp     dword ptr [rbp+4], 0Eh
00007ff6`6cfd23cb 7e05             jle     bomb!phase_4+0x82 (7ff66cfd23d2)
00007ff6`6cfd23cd e8e4efffff       call    bomb!@ILT+945(explode_bomb) (7ff66cfd13b6)
00007ff6`6cfd23d2 c745640a000000   mov     dword ptr [rbp+64h], 0Ah
00007ff6`6cfd23d9 41b80e000000     mov     r8d, 0Eh
00007ff6`6cfd23df 33d2             xor     edx, edx
00007ff6`6cfd23e1 8b4d04           mov     ecx, dword ptr [rbp+4]
00007ff6`6cfd23e4 e8f1eeffff       call    bomb!@ILT+725(func4) (7ff66cfd12da)
00007ff6`6cfd23e9 894544           mov     dword ptr [rbp+44h], eax
00007ff6`6cfd23ec 8b4564           mov     eax, dword ptr [rbp+64h]
00007ff6`6cfd23ef 394544           cmp     dword ptr [rbp+44h], eax
00007ff6`6cfd23f2 7508             jne     bomb!phase_4+0xac (7ff66cfd23fc)
00007ff6`6cfd23f4 8b4564           mov     eax, dword ptr [rbp+64h]
00007ff6`6cfd23f7 394524           cmp     dword ptr [rbp+24h], eax
00007ff6`6cfd23fa 7405             je      bomb!phase_4+0xb1 (7ff66cfd2401)
00007ff6`6cfd23fa 7405             je      bomb!phase_4+0xb1 (7ff66cfd2401)
00007ff6`6cfd23fc e8b5efffff       call    bomb!@ILT+945(explode_bomb) (7ff66cfd13b6)
00007ff6`6cfd2401 488d4de0         lea     rcx, [rbp-20h]
00007ff6`6cfd2405 488d15149c0000   lea     rdx, [bomb!`string'+0x288 (7ff66cfdc020)]
00007ff6`6cfd240c e869efffff       call    bomb!@ILT+885(_RTC_CheckStackVars) (7ff66cfd137a)
00007ff6`6cfd2411 488b8d58010000   mov     rcx, qword ptr [rbp+158h]
00007ff6`6cfd2418 4833cd           xor     rcx, rbp
00007ff6`6cfd241b e8d9edffff       call    bomb!@ILT+500(__security_check_cookie) (7ff66cfd11f9)
00007ff6`6cfd2420 488da568010000   lea     rsp, [rbp+168h]
00007ff6`6cfd2427 5f               pop     rdi
00007ff6`6cfd2428 5d               pop     rbp
00007ff6`6cfd2429 c3               ret     
```

- Well, the first thing we noticed is the presence of the **(sscanf)** function.

```nasm
00007ff6`6cfd239f 488d157a9e0000   lea     rdx, [bomb!`string' (7ff66cfdc220)]
00007ff6`6cfd23a6 488b8d80010000   mov     rcx, qword ptr [rbp+180h]
00007ff6`6cfd23ad e814efffff       call    bomb!@ILT+705(sscanf) (7ff66cfd12c6)
```

![WinDbg 4_29_2024 10_40_49 PM.png](Binary%20Bomb%20Lab%20Write-up%205b45ea3675d64bf5bd0118d5d793f179/WinDbg_4_29_2024_10_40_49_PM.png)

- We note that the ***input has two integer numbers***, like the previous phase.

```nasm
00007ff6`037723c1  cmp  dword ptr [rbp+4],0
00007ff6`037723c5  jl   bomb!phase_4+0x7d (00007ff6`037723cd)  
00007ff6`037723c7  cmp   dword ptr [rbp+4],0Eh
00007ff6`037723cb  jle   bomb!phase_4+0x82 (00007ff6`037723d2)  
00007ff6`037723cd  call  bomb!ILT+945(explode_bomb) 
```

> From this comparison, we note that **the first number must be less than 14 so that the bomb does not explode.**
> 
- Next, we see a call to `func4()` whose return value decides whether bomb explodes or not.
- `func4()` :
    
    ![WinDbg 4_29_2024 11_01_22 PM.png](Binary%20Bomb%20Lab%20Write-up%205b45ea3675d64bf5bd0118d5d793f179/WinDbg_4_29_2024_11_01_22_PM.png)
    

```nasm
  119 00007ff6`6cfd1f4d 8b8508010000    mov     eax,dword ptr [rbp+108h]
  119 00007ff6`6cfd1f53 8b8d10010000    mov     ecx,dword ptr [rbp+110h]
  119 00007ff6`6cfd1f59 2bc8            sub     ecx,eax
  119 00007ff6`6cfd1f5b 8bc1            mov     eax,ecx
  119 00007ff6`6cfd1f5d 99              cdq
  119 00007ff6`6cfd1f5e 2bc2            sub     eax,edx
  119 00007ff6`6cfd1f60 d1f8            sar     eax,1
  119 00007ff6`6cfd1f62 8b8d08010000    mov     ecx,dword ptr [rbp+108h]
  119 00007ff6`6cfd1f68 03c8            add     ecx,eax
  119 00007ff6`6cfd1f6a 8bc1            mov     eax,ecx
  119 00007ff6`6cfd1f6c 894504          mov     dword ptr [rbp+4],eax
  121 00007ff6`6cfd1f6f 8b8500010000    mov     eax,dword ptr [rbp+100h]
  121 00007ff6`6cfd1f75 394504          cmp     dword ptr [rbp+4],eax
  121 00007ff6`6cfd1f78 7e20            jle     bomb!func4+0x8a (00007ff6`6cfd1f9a)  Branch

```

- Therefore, `[rbp+100]` is **first** parameter,whereas `[rbp+108]` and `[rbp+110]` are **second** and **third** parameter.
- **eax = (((b - a)) - a)/2;
*(rbp+4) = ecx = a + eax**

```nasm
121 00007ff6`03771f75   cmp    dword ptr [rbp+4],eax
121 00007ff6`03771f78   jle    bomb!func4+0x8a (00007ff6`03771f9a)

...
123 00007ff6`03771f9a   mov     eax,dword ptr [rbp+100h]
123 00007ff6`03771fa0   cmp     dword ptr [rbp+4],eax
123 00007ff6`03771fa3   jge     bomb!func4+0xb5 (00007ff6`03771fc5)
else
124 00007ff6`03771fa5   mov     eax,dword ptr [rbp+4]
124 00007ff6`03771fa8   inc     eax
124 00007ff6`03771faa   mov     r8d,dword ptr [rbp+110h]
124 00007ff6`03771fb1   mov     edx,eax
124 00007ff6`03771fb3   mov     ecx,dword ptr [rbp+100h]
124 00007ff6`03771fb9   call    bomb!ILT+725(func4) (00007ff6`037712da)
124 00007ff6`03771fbe   add     eax,dword ptr [rbp+4]
124 00007ff6`03771fc1   jmp     bomb!func4+0xb8 (00007ff6`03771fc8)
...
126 00007ff6`03771fc5  mov     eax,dword ptr [rbp+4]    // BASE CASE
127 00007ff6`03771fc8  lea     rsp,[rbp+0E8h]
127 00007ff6`03771fcf  pop     rdi
127 00007ff6`03771fd0  pop     rbp
127 00007ff6`03771fd1  ret
```

> ***I will simplify it now in C so that you understand what this function is about.***
> 

```c
if ( *(rbp+4) == int ) 
{
return *(rbp+4)
}
if ( *(rbp+4) < int ) 
{
eax = func4 (int ,*(rbp+4) + 1 , b )
}
```

```nasm
122 00007ff6`03771f7a  mov     eax,dword ptr [rbp+4]
122 00007ff6`03771f7d  dec     eax
122 00007ff6`03771f7f  mov     r8d,eax
122 00007ff6`03771f82  mov     edx,dword ptr [rbp+108h]
122 00007ff6`03771f88  mov     ecx,dword ptr [rbp+100h]
122 00007ff6`03771f8e  call    bomb!ILT+725(func4) (00007ff6`037712da)
122 00007ff6`03771f93  add     eax,dword ptr [rbp+4]
122 00007ff6`03771f96  jmp     bomb!func4+0xb8 (00007ff6`03771fc8)
...
127 00007ff6`03771fc8  lea     rsp,[rbp+0E8h]
127 00007ff6`03771fcf  pop     rdi
127 00007ff6`03771fd0  pop     rbp
127 00007ff6`03771fd1  ret
```

> ***I will simplify it now in C so that you understand what this function is about.***
> 

```c
else 
{
eax = func4 (int , b , *(rbp+4) - 1 )
}
```

- `func4()` **by C** :
    
    ```c
    int func4 ( int , a , b ) 
    {
    **eax = (((b - a)) - a)/2;
    *(rbp+4) = ecx = a + eax**
    if ( *(rbp+4) == int ) 
    {
    return *(rbp+4)
    }
    if ( *(rbp+4) < int ) 
    {
    eax = func4 (int ,*(rbp+4) + 1 , b )
    }
    ****else 
    {
    eax = func4 (int , b , *(rbp+4) - 1 )
    }
    }
    ```
    

> ***func4 (x, 0, 14)[expr = 7 ((14-0-0)/2 + 0)]***
> 

> -> if(x > 7): call func4(x , **7+1** , **14**): [**expr = 7** ( (14-16)/2+8)]
> 

> -> if(x < 7): call func4(x,  **0**, **7-1**):  [**expr = 3**   ((6-0)/2+0)]
> 
- **The second level (when x < 7) returns us a value of 3, if we provide an integer of 3 as input it will fall into the base case (i.e. `x == expr`). The return value of `3` when added to initial evaluation of expression, i.e. `7` returns a value of `10`( `eax += *(rbp+4); return eax;` in the end of our func4() pseudocode) as needed by phase_4(). Let’s try a value of `3` as our input.**

![Screenshot 2024-04-29 232203.png](Binary%20Bomb%20Lab%20Write-up%205b45ea3675d64bf5bd0118d5d793f179/Screenshot_2024-04-29_232203.png)

- ***Boom, we did it again and successfully solved phase 4 🤩🥳 .***