## Step 1: Static & Dynamic Analysis

```c
_BOOL4 auth(char *s, int nb)
{
  int i;
  int v4;
  int v5;

  s[strcspn(s, "\n")] = 0;
  v5 = strnlen(s, 32);
  if ( v5 <= 5 )
    return 1;
  if ( ptrace(PTRACE_TRACEME, 0, 1, 0) == -1 )
  {
    puts("\x1B[32m.---------------------------.");
    puts("\x1B[31m| !! TAMPERING DETECTED !!  |");
    puts("\x1B[32m'---------------------------'");
    return 1;
  }
  v4 = (s[3] ^ 0x1337) + 6221293;
  for ( i = 0; i < v5; ++i )
  {
    if ( s[i] <= 31 )
      return 1;
    v4 += (v4 ^ (unsigned int)s[i]) % 0x539;
  }
  if (nb == v4)
    return 0;
  return 1;
}

int main(int argc, const char **argv, const char **envp)
{
  int nb;
  char buff[28];
  unsigned int v6;

  v6 = __readgsdword(0x14u);
  puts("***********************************");
  puts("*\t\tlevel06\t\t  *");
  puts("***********************************");
  printf("-> Enter Login: ");
  fgets(buff, 32, stdin);
  puts("***********************************");
  puts("***** NEW ACCOUNT DETECTED ********");
  puts("***********************************");
  printf("-> Enter Serial: ");
  scanf(&unk_8048A60, &nb);
  if ( auth(buff, nb) )
    return 1;
  puts("Authenticated!");
  system("/bin/sh");
}
```

### Explanation

The program uses `fgets()` to store a login inside a buffer `buff` and uses `scanf()` to store an integer in `nb`.

Both inputs pass into a function `auth()` that uses `ptrace(PTRACE_TRACEME)` to deny the use of debuggers.

`auth()` next applies an algorithm on our inputs.

Our goal will be to have our second input `nb` equal to the return of the encryption algorithm.

To crack the algorithm, we're just going to paste it in a `.c` file and try differents inputs to solve the problem.

```c
#include "stdlib.h"
#include "stdio.h"
#include "string.h"

int main(int ac, char **av) {

  if (ac != 3)
    return 1;

  unsigned int nb = atoi(av[2]);
  int ret = (av[1][3] ^ 4919) + 6221293;
    
  for (int i = 0; i < strlen(av[1]); i++) {
    if (av[1][i] < ' ')
          return 1;
    ret += (ret ^ av[1][i]) % 1337;
  }
  
  printf("ret[%d] == nb[%d]\n", ret, nb);
}
```

## Step 2: Exploiting the Binary

When passing a random string "abc" as parameter the return value of the algorithm will always be **6229073** regardless of our second input.

```bash
./a.out abc 66229073
ret[6229073] == arg[66229073]
```

We just have to pass **6229073** as second input to make the shell terminal spawn.

```bash
level06@OverRide:~$ cat /home/users/level07/.pass
GbcPDRgsFK77LNnnuh7QyFYA2942Gp8yKj9KrWD8
```