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
`auth()` next applies an algorithm using both inputs.

Our goal will be to have our second input `nb` equal to the return of the encryption algorithm.

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
  
  printf("algo_ret[%d] == second_input[%d]\n", ret, nb);
}
```

To crack it, we're just going to paste the algorithm in a `.c` file and brute force it.

```bash
./a.out 99999999 6233452
algo_ret[6233452] == second_input[6233452]
```

After multiple tests, we find out that passing `99999999` and `6233452` solve the problem 

## Step 2: Exploiting the Binary

```bash
level06@OverRide:~$ ./level06 
***********************************
*                 level06         *
***********************************
-> Enter Login: 99999999
***********************************
***** NEW ACCOUNT DETECTED ********
***********************************
-> Enter Serial: 6233452
Authenticated!
% cat /home/users/level07/.pass
GbcPDRgsFK77LNnnuh7QyFYA2942Gp8yKj9KrWD8
```