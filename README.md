### 2
```console
a6@a6-Ex:~$ mkdir -p ~/11-606/Shakirov
a6@a6-Ex:~$ cd ~/11-606/Shakirov/
a6@a6-Ex:~/11-606/Shakirov$ 
```
### 3
file `1.c` :
```c
#include <stdio.h>
void main()
{
    printf("HELLO Ubuntu\n");
}
```
```console
a6@a6-Ex:~/11-606/Shakirov$ gcc -o 1.exe 1.c
a6@a6-Ex:~/11-606/Shakirov$ ./1.exe
HELLO Ubuntu
```
### 4
file `t4.sh` :
```bash
#!/bin/bash
echo $@
echo $@ > 4.txt
```
```console
a6@a6-Ex:~/11-606/Shakirov$ ./t4.sh 123 456
123 456
```
### 5
file `t5.sh` :
```bash
#!/bin/bash
printf "" > $1
for f in $2*; do
  filename=$(basename "$f")
  extension=${filename##*.}
  if [[ $extension == $3 ]]; then
    echo $filename >> $1
    continue;
  fi
done
```
```console
a6@a6-Ex:~/11-606/Shakirov$ ./t5.sh t5.txt ./ sh
```
file `t5.txt` :
```
t4.sh
t5.sh
t6.sh
t7_5.sh
```
### 6
file `t6.sh` :
```bash
#!/bin/bash
gcc $1 -o $2
if [[ $? -eq 0 ]]; then
  ./$2
else
  echo "error!"
fi
```
file `6_ok.c` :
```c
#include <stdio.h>
void main()
{
    printf("HELLO Ubuntu\n");
}
```
file `6_err.c` :
```c
#include <stdio.h>
void main()
{
    printf("HELLO Ubuntu\n")
}
```
```console
a6@a6-Ex:~/11-606/Shakirov$ ./t6.sh ./6_ok.c 6_1.exe
HELLO Ubuntu
```
```console
a6@a6-Ex:~/11-606/Shakirov$ ./t6.sh ./6_err.c 6_1.exe
./6_err.c: In function ‘main’:
./6_err.c:5:1: error: expected ‘;’before ‘}’ token
 }
 ^
error!
```

### 7_5

file `t7_5.sh` :
```bash
#!/bin/bash
printf "" > $3
let count=0
while read f; do
  owner=$(stat -c %U "$f")
  if [[ $owner == $1 ]]; then
    let count++
    filename=$(basename "$f")
    fullpath=$(realpath "$f")
    size=$(stat --printf="%s" "$f")
    printf "%s,%s,%s\n" "$fullpath" "$filename" "$size">> $3
  fi
done <<< "$(find $2 -type f)"
printf "count = $count\n"
```

```console
a6@a6-Ex:~/11-606/Shakirov$ mkdir -p ./dir1
a6@a6-Ex:~/11-606/Shakirov$ touch ./dir1/file\ 1
a6@a6-Ex:~/11-606/Shakirov$ touch ./dir1/root_file
a6@a6-Ex:~/11-606/Shakirov$ sudo chown root:root ./dir1/root_file
```

```console
a6@a6-Ex:~/11-606/Shakirov$ tree
.
├── 1.c
├── 1.exe
├── 4.txt
├── 6_1.exe
├── 6_err.c
├── 6_ok.c
├── dir1
│   ├── file 1
│   └── root_file
├── res.md
├── t4.sh
├── t5.sh
├── t5.txt
├── t6.sh
├── t7_5.sh
└── t7_5.txt

1 directory, 15 files
```

```console
a6@a6-Ex:~/11-606/Shakirov$ ./t7_5.sh a6 /home/a6/11-606/Shakirov ./t7_5.txt
count = 15
```

file `t7_5.txt` :
```
/home/a6/11-606/Shakirov/t7_5.sh,t7_5.sh,866
/home/a6/11-606/Shakirov/6_err.c,6_err.c,64
/home/a6/11-606/Shakirov/6_1.exe,6_1.exe,8296
/home/a6/11-606/Shakirov/6_ok.c,6_ok.c,65
/home/a6/11-606/Shakirov/4.txt,4.txt,8
/home/a6/11-606/Shakirov/t5.sh,t5.sh,216
/home/a6/11-606/Shakirov/1.c,1.c,65
/home/a6/11-606/Shakirov/t4.sh,t4.sh,35
/home/a6/11-606/Shakirov/dir1/file 1,file 1,0
/home/a6/11-606/Shakirov/t5.txt,t5.txt,26
/home/a6/11-606/Shakirov/1.exe,1.exe,8296
/home/a6/11-606/Shakirov/res.md,res.md,2212
/home/a6/11-606/Shakirov/t7_5.txt,t7_5.txt,507
/home/a6/11-606/Shakirov/t6.sh,t6.sh,155
```


