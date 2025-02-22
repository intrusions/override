## Step 1: Static & Dynamic Analysis

```c
08048660    int32_t decrypt(char arg1)
08048660    {
08048660        int32_t ebp;
08048660        int32_t var_4 = ebp;
08048663        int32_t edi;
08048663        int32_t var_8 = edi;
08048664        int32_t esi;
08048664        int32_t var_c = esi;
08048668        void* gsbase;
08048668        int32_t eax = *(uint32_t*)((char*)gsbase + 0x14);
08048673        int32_t var_21;
08048673        __builtin_strncpy(&var_21, "Q}|u`sfg~sf{}|a3", 0x11);
08048693        int32_t var_50 = 0;
08048693        int32_t* esp_1 = &var_50;
0804869f        int32_t var_30 = 0xffffffff;
080486ad        int32_t i = 0xffffffff;
080486b0        int32_t* edi_1 = &var_21;
080486b0        
080486b2        while (i)
080486b2        {
080486b2            bool cond:0_1 = 0 != *(uint8_t*)edi_1;
080486b2            edi_1 += 1;
080486b2            i -= 1;
080486b2            
080486b2            if (!cond:0_1)
080486b2                break;
080486b2        }
080486be        int32_t var_2c = 0;
080486e8        bool c_1;
080486e8        bool z_1;
080486e8        
080486e8        while (true)
080486e8        {
080486e8            c_1 = var_2c < ~i - 1;
080486e8            z_1 = var_2c == ~i - 1;
080486e8            
080486eb            if (!c_1)
080486eb                break;
080486eb            
080486df            *(uint8_t*)(&var_21 + var_2c) ^= arg1;
080486e1            var_2c += 1;
080486e8        }
080486e8        
080486f7        int32_t i_1 = 0x11;
080486fc        int32_t* esi_1 = &var_21;
080486fe        void* const edi_2 = "Congratulations!";
08048700        while (i_1)
08048700        {
08048700            char temp1_1 = *(uint8_t*)esi_1;
08048700            char temp2_1 = *(uint8_t*)edi_2;
08048700            c_1 = temp1_1 < temp2_1;
08048700            z_1 = temp1_1 == temp2_1;
08048700            esi_1 += 1;
08048700            edi_2 += 1;
08048700            i_1 -= 1;
08048700            
08048700            if (!z_1)
08048700                break;
08048700        }
08048700        
08048713        int32_t result;
08048713        
08048713        if ((int32_t)((!z_1 && !c_1) - c_1))
08048713        {
08048723            esp_1[1] = "\nInvalid Password";
0804872a            result = puts();
08048713        }
08048713        else
08048713        {
08048715            esp_1[1] = "/bin/sh";
0804871c            result = system();
08048713        }
08048739        if (eax != *(uint32_t*)((char*)gsbase + 0x14))
08048739        {
08048743            __stack_chk_fail();
0804873b            /* no return */
08048739        }
08048739        
08048740        esp_1[0x11];
08048743        esp_1[0x12];
08048744        esp_1[0x13];
08048746        return result;
08048660    }

08048747    int32_t test(int32_t arg1, int32_t arg2)
08048747    {
08048747        int32_t ecx_1 = arg2 - arg1;
08048747        
08048773        switch (ecx_1)
08048773        {
0804877b            case 1:
0804877b            {
0804877b                return decrypt(ecx_1);
0804877b                break;
0804877b            }
0804878b            case 2:
0804878b            {
0804878b                return decrypt(ecx_1);
0804878b                break;
0804878b            }
0804879b            case 3:
0804879b            {
0804879b                return decrypt(ecx_1);
0804879b                break;
0804879b            }
080487ab            case 4:
080487ab            {
080487ab                return decrypt(ecx_1);
080487ab                break;
080487ab            }
080487bb            case 5:
080487bb            {
080487bb                return decrypt(ecx_1);
080487bb                break;
080487bb            }
080487cb            case 6:
080487cb            {
080487cb                return decrypt(ecx_1);
080487cb                break;
080487cb            }
080487db            case 7:
080487db            {
080487db                return decrypt(ecx_1);
080487db                break;
080487db            }
080487e8            case 8:
080487e8            {
080487e8                return decrypt(ecx_1);
080487e8                break;
080487e8            }
080487f5            case 9:
080487f5            {
080487f5                return decrypt(ecx_1);
080487f5                break;
080487f5            }
08048802            case 0x10:
08048802            {
08048802                return decrypt(ecx_1);
08048802                break;
08048802            }
0804880f            case 0x11:
0804880f            {
0804880f                return decrypt(ecx_1);
0804880f                break;
0804880f            }
0804881c            case 0x12:
0804881c            {
0804881c                return decrypt(ecx_1);
0804881c                break;
0804881c            }
08048829            case 0x13:
08048829            {
08048829                return decrypt(ecx_1);
08048829                break;
08048829            }
08048836            case 0x14:
08048836            {
08048836                return decrypt(ecx_1);
08048836                break;
08048836            }
08048843            case 0x15:
08048843            {
08048843                return decrypt(ecx_1);
08048843                break;
08048843            }
08048773        }
08048773        
08048852        return decrypt(rand());
08048747    }

0804885a    int32_t main(int32_t argc, char** argv, char** envp)
0804885a    {
0804885a        int32_t __saved_ebp_1;
0804885a        int32_t __saved_ebp = __saved_ebp_1;
08048863        int32_t eax;
08048863        int32_t var_34 = eax;
08048863        int32_t* esp_1 = &var_34;
0804886b        *(uint32_t*)esp_1;
0804886c        esp_1[1] = 0;
08048878        esp_1[1] = time();
0804887b        srand();
08048880        esp_1[1] = "********************************…";
08048887        puts();
0804888c        esp_1[1] = "*\t\tlevel03\t\t**";
08048893        puts();
08048898        esp_1[1] = "********************************…";
0804889f        puts();
080488a9        esp_1[1] = "Password:";
080488ac        printf();
080488ba        esp_1[2] = &esp_1[8];
080488be        esp_1[1] = &data_8048a85;
080488c1        __isoc99_scanf();
080488c6        int32_t eax_3 = esp_1[8];
080488ca        esp_1[2] = 0x1337d00d;
080488d2        esp_1[1] = eax_3;
080488d5        test();
080488e0        return 0;
0804885a    }
```

### Explanation

The `main()` function begins by calling `scanf()` to accept an integer as the `password`. It then passes this user input to the `test()` function with a value of `0x1337d00d` as the second parameter.

In the `test()` function, the second parameter (`0x1337d00d`) is subtracted from the user-provided `password`. The result of this subtraction is then evaluated to determine its range. If the result of the subtraction is between `1` and `9` or between `16` and `21`, the function call `decrypt()` with this result as its parameter, otherwise it call the function with a random value generated by `rand()`.

The `decrypt()` function starts by creating a buffer containing the following hexadecimal values:

```python
>>> bytes.fromhex('757c7d51').decode()[::-1]
'Q}|u'
>>> bytes.fromhex('67667360').decode()[::-1]
'`sfg'
>>> bytes.fromhex('7b66737e').decode()[::-1]
'~sf{'
>>> bytes.fromhex('33617c7d').decode()[::-1]
'}|a3'

>>> 'Q}|u' + '`sfg' + '~sf{' + '}|a3'
'Q}|u`sfg~sf{}|a3'
```

Next, the `decrypt()` function applies a `XOR` operation to each character of this buffer using the subtraction result passed as its parameter. The output of this operation must match, character for character, the string `Congratulations!`.

```python
>>> chr(ord('Q') ^ 18)
'C'
>>> chr(ord('}') ^ 18)
'o'
>>> chr(ord('|') ^ 18)
'n'
```

From this, we know that the result of the subtraction (`0x1337d00d - password`) must be between `1` and `9` or between `16` and `21` and be equal `18` to produce the correct XOR result.

```python
>>> 0x1337d00d - 18
322424827
```

## Step 2: Exploiting the Binary

```bash
level03@OverRide:~$ ./level03

***********************************
*               level03         **
***********************************
Password:322424827

% cat /home/users/level04/.pass
kgv3tkEb9h2mLkRsPkXRfc2mHbjMxQzvb2FrgKkf
```