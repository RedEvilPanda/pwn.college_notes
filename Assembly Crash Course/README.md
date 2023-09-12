#### All assembly code is launched using the special script `start_assembly.sh` presented below:
```
#!/bin/bash
nasm -felf64 $1 -o asm.o && objcopy -O binary --only-section=.text ./asm.o ./asm.bin && cat ./asm.bin | /challenge/run
```
#### The script takes the name of the solution file as an argument.
```Shell
~$ chmod +x start_assembly.sh
~$ ./start_assembly.sh <file_name>
```
