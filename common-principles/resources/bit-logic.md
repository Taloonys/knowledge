* 1 - true
* 0 - false

# Upper bits
![Pasted image 20250227165917](image-storage/Pasted%20image%2020250227165917.png)

# AND (if both true)
> Basicly: only if both true -> true 
> 1 & 1 -> 1 else 0

# OR (if any is true)
> Basicly: if any is true -> true 
> 1 | 0 , 0 | 1, 1 | 1 -> 1 else 0

# NOT
> Basicly: invert
> !1 -> 0, !0 -> 1

# XOR (if only one is true)
> Basicly: OR, but if both true -> false
> Another way: only one operand is true -> true
> 1 ^ 1 -> 0
> 1 ^ 0, 0 ^ 1 -> 1

![Pasted image 20250227165113](image-storage/Pasted%20image%2020250227165113.png)

# Additional code
* 00000011-> 3
* 10000011 -> -3
![Pasted image 20250227165625](image-storage/Pasted%20image%2020250227165625.png)

# Decimal relation
* 10001011 -> pow(1, 0) + pow(1, 1) + pow(0, 2) + pow(1, 3) + pow(0, 4) + pow(0, 5) + pow(0, 6) + pow(1, 7) = 11

![Pasted image 20250227170028](image-storage/Pasted%20image%2020250227170028.png)

# Flags
> It's possible to store different states/flags inside 1 variable instead of any kind enumeration (enum)

## Check if flag is on (AND operator)
> `mask & flag != 0`
```
flag1 = 0x0110100        // random flag value
mask  = 0x0000010        // mask for checking if 0x0000010 flag is set

   0x0110100 
&  0x0000010
   0x0000000 // == 0, so.. the second (from the right) bit isn't set

if (mask & flag1 != 0)
	return SUCCESS;
else
	return ERROR;
```

## Modify flag
### Add flag (OR operator)
> `flag |= FLAG_MASK` or `flag = flag | FLAG_MASK`
```
flag1      = 0x0110100        // random flag value
FLAG_MASK  = 0x0000010        // mask for checking if 0x0000010 flag is set

flag1 |= FLAG_MASK;
   0x0110100 
|  0x0000010
   0x0110110 // changed third (from right) bit

if (FLAG_MASK & flag1 != 0)
	   0x0110110
	&  0x0000010
	   0x0000010 // != 0, flag was changed succesfully
	return SUCCESS;
```

### Drop flag (XOR operator)
```
flag1      = 0x0110110        // random flag value
FLAG_MASK  = 0x0000010        // mask for checking if 0x0000010 flag is set

flag1 ^= FLAG_MASK;
   0x0110110 
|  0x0000010
   0x0110100 // changed third (from right) bit

if (FLAG_MASK & flag1 != 0)
	return SUCCESS;
else 
	   0x0110100
	&  0x0000010
	   0x0000000 // 0= 0, flag was changed succesfully
	return ERROR;
```

# Shift
* binary `>>`, mostly it's equal to `multiply or divide by power of 2` + it would be rounded to the less value
```
    00001000 >> 1 = 00000100 // 8 >> 1 = 4
	11 >> 1 = 5              // to less value
```
* logical `>>>`
```
    01000001 <<< 1 = 10000010 //  65 <<< 1 = 130
    10000001 >>> 1 = 01000000 // 129 >>> 1 = 64
```
* cyclic   `>>>>`,  same as binary, but bits from *edged positions* are moved (aka in cycle) to the opposite *edge*, `unsigned only operation`
```
    00001001 >> 1 = 10000100 // 9 >> 1 = 132
```
