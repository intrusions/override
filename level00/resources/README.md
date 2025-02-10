## Step 1: Static & Dynamic Analysis

```c
bool main(void)
{
  int buff [4];
  
  puts("***********************************");
  puts("* \t     -Level00 -\t\t  *");
  puts("***********************************");
  printf("Password:");
  __isoc99_scanf(&DAT_08048636,buff);
  if (buff[0] != 5276) {
    puts("\nInvalid Password!");
  }
  else {
    puts("\nAuthenticated!");
    system("/bin/sh");
  }
  return buff[0] != 5276;
}
```

### Explanation

This program uses `scanf()` to ask for an input of 4 digits maximum.
It'll open a bash terminal with the rights of the user of the next level if the input is 5276.

## Step 2: Exploiting the Binary

```bash
level00@OverRide:~$ ./level00
***********************************
*            -Level00-            *
***********************************
Password:5276

Authenticated!
$ cat /home/users/level01/.pass
uSq2ehEGT6c9S24zbshexZQBXUGrncxn5sD5QfGL
```