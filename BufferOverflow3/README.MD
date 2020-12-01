![alt text](bof3.png)

# Gathering Information
the challenge provides a binary,it's source code and a netcat service.
- the binary is ELF 64bit mode.
- when reading the source code, the binary reads 0x100 bytes and the size of our buffer is 60, so we have a buffer overflow vulnerability.
- we have a print_flag function that calls system("/bin/bash") only if the first argument = 0xdeadbeef and the second argument = 0xc0febabe, so we have to use gadgets to
fill the arguments with the required values.
# Exploitation
- find the size of the buffer to overwrite the saved return pointer RIP
- find the address of print_flag function
- find gadget addresses : pop rdi and pop rsi
- chain our payload


