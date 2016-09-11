# ASIS-CTF-Finals-2016-Diapers-Simulator-Writeup

There is a vulnerable printf call, but the string passed to printf can't be controled directly. On the stack just before the format string there is a 4 byte number and the diapers brand name which we can control. The change of the brand name involves calling strlen and then passing the resulting string lenght to read. Normaly the brand name is null terminated so we cannot overflow the format string passed to printf. By calling "Change Diapers" 257 times we can set the 4 byte number present on the stack to -1 which in binary representation is 0xffffffff. By doing so the brand name is not null terminated and we can control the buffer passed to printf.

The exploit involves overwriting the Global Offset Table entry of strlen to point to system. Using the vulnerable printf call we can dump adresses from GOT which enables us to identify the version of libc present on the remote system and bypass ASLR.
