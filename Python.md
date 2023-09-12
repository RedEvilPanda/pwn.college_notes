
## Creating a subprocess passing arguments and environment variables

```Python
import subprocess
subprocess.run(['./program', '--argv', '...'], env={'HOST':'Linux'})
```

or

```Python
from pwn import *
p = process(['./target', '--arg1', 'some data'], env={'env1': 'some data'})
p.interactive()
```

## Passing a stream from a file to a process and vice versa

```Python
# STDIN (FILE -> PROCESS) 
import os
import subprocess

with open("/tmp/file", 'r') as f:
	file_handler = os.open("/tmp/file", O_RDONLY)
	subprocess.run(['./proc'], stdin=file_handler)
```


```Python
# STDOUT (FILE <- PROCESS)
import subprocess
with open("/tmp/file", 'w') as file:
	subprocess.run(['./proc'], stdout=file)
```

## Passing output via PIPE to another process

```Python
import subprocess
p1 = subprocess.Popen("/challenge/embryoio_level50", stdout=subprocess.PIPE)
p2 = subprocess.Popen(['sed',"-e '/^$/d'"], stdin=p1.stdout)
```

