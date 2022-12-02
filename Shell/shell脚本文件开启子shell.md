```bash
#!/bin/bash

gnome-terminal -- bash -c 'echo hello world 1 | less'
gnome-terminal -- bash -c 'echo hello world 2 | less'
```

less 用于把结果重定向给less,这样less执行完之前,是不会退出的。