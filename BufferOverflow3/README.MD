![alt text](bof3.png)

# Gathering Information
the challenge provide a binary,it's source code and a netcat service.
- the binary is ELF 64bit mode.
- when reading the source code, the binary reads 0x100 bytes and the size of our buffer is 60, so we have a buffer overflow vulnerability.
- we have a win function that calls system("/bin/bash")
