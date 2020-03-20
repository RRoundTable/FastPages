---
title: "Missing Semester 정리글"
branch: master
badges: true
comments: true
categories: ['shell', 'bash']
---

# Missing Semester 정리글



## 1. Shell

리눅스의 쉘은 명령어와 프로그램을 실행할 때 사용하는 인터페이스이다.

![]({{ site.baseurl }}/images/2020-03-05-missing-semeste-정리글/shell.png "Shell")



**echo**

리눅스 명령어 echo는 주어진 문자열을, 문자열 사이에 포함된 공백과 줄 마지막에 개행문자를 포함하여 표준출력으로 출력하는 명령어다.

```
echo "Hellow Wolrd"
>>> Hellow World

echo Hellow\ Wolrd
>>> Hellow World

echo $HOME
>>> /root

echo $PATH
>>> $PATH에 포함된 주소들을 출력

which echo
>>> /usr/bin/echo # 사용하고 있는 echo 위치 출력

```

**pwd**

```
pwd
>>> /home # present working directory
```

**cd**

```
cd /roundtable # change directory
```

**. (dot) / .. (dot dot)**

```
.. # parent directory
cd .. # /home/roundtable -> /home

. # current directory
cd ./roundtable # /home -> /home/roundtable
```

**ls**

```
ls
>>> print all files in the current directory
ls -help
>>> 다양한 옵션에 대한 정보를 받아볼 수 있음
ls -l
>>> 파일에 대한 추가적인 정보를 얻을 수 있음
```

**~**

```
cd ~ # home directory
cd ~/roundtable # cd /home/roundtable
```

**cd -**

```
cd - # back to parent directory
```

**mv**

```
mv <current_file> <new_file> # rename the file or move the file in dirrent directory
```

**cp**

```
cp <current_file> <new_file> # copy
cp -r <current folder> <new_folder>
```

**rm**

```
rm <current_file> # remove
rmdir <folder>
```

**mkdir**

```
mkdir <new directory name> # make directory
```

**man**

```
man ls # manual page for ls
# if you want to quik, press q
```

**ctrl + l**: clear the terminal

**Angle bracket signs**

input stream, output stream이 존재한다. 이를 적절히 조절할 수 있다.

```
# < file 
# > file

echo hello > hello.txt # hello(print)가 hello.txt의 입력으로 들어간다.
cat hello.txt
>>> hello

cat < hello.txt > hello2.txt
cat hello2.txt
>>> hello
```

**| (pipe)**

```
# file1 | file2 # make output of file1 input of file2
# tail # 마지막 line만 출력해준다.

ls -l / tail -n1
>>> drwxrwxr-x 11 ubuntu ubuntu 4096 Mar  4 12:55 dev # print last line

curl --head --silent google.com | grep -i content-length
```

**tail**

마지막 line만 출력해준다.

**curl**

```
curl --head --silent google.com | grep -i content-length
```

**sudo: root permission**

```
sudo find -L /sys/class/backlight -maxdepth 2 -name '*brightness*'
>>> /sys/class/backlight/thinkpad_screen/brightness

cd /sys/class/backlight/thinkpad_screen

sudo echo 3 > brightness

>>> An error occurred while redirecting file 'brightness'
open: Permission denied


echo 3 | sudo tee brightness
```

**cat**: concatenate

파일의 내용을 출력

```
cat file1
# file1의 내용 출력

cat file1 file2 file3
# file1, file2, file3 이어서 출력

cat > file1 # (내용을 입력하고 ctrl + d를 눌러 저장한다.) 기존 내용을 지우고
cat >> file1 # (내용을 입력하고 ctrl + d를 눌러 저장한다.) 기존의 내용에 이어서

cat file1 file2 > file3 # file1 + file2 = file3
```

**cd /sys**

```
cd /sys # to access various kernel parameters

total 0
drwxr-xr-x   2 root root  0 Mar  5 08:03 block
drwxr-xr-x  47 root root  0 Mar  3 06:53 bus
drwxr-xr-x  69 root root  0 Mar  3 06:51 class
drwxr-xr-x   4 root root  0 Mar  5 08:03 dev
drwxr-xr-x  71 root root  0 Mar  3 04:43 devices
drwxrwxrwt   2 root root 40 Mar  3 04:43 firmware
drwxr-xr-x  12 root root  0 Mar  3 04:43 fs
drwxr-xr-x   2 root root  0 Mar  5 08:03 hypervisor
drwxr-xr-x  14 root root  0 Mar  5 08:03 kernel
drwxr-xr-x 219 root root  0 Mar  5 08:03 module
drwxr-xr-x   2 root root  0 Mar  5 08:03 power
```



shell은 단순한 argument가 아니라 일종의 프로그래밍이라고 볼 수 있다. 예를 들어서 조건문이나 반복문같은 설정을 할 수 있다.

## 2. Shell Tools and Scripting

'' 하고 "" 는 유사해보이지만, 서로 다르다.

```
foo=bar
echo "$foo"
# prints bar
echo '$foo'
# prints $foo

echo "Value is $foo"
# prints 'Value is foo'

echo 'Value is $foo'
# prints 'Value is $foo'
```

bash는  if, case, while, for와 같은 구문을 제공한다.
```
mcd (){
	mkdir -p "$1"
	cd "$1"
}
```
```
/home: vim mcd.sh
/home: source mcd.sh
/home: mcd.sh test

/home/test:  # /home -> mkdir /home/test -> cd /home/test
```

- $0 - Name of the script
- $1 to $9 - Arguments to the script. $1 is the first argument and so on.
- $@ - All the arguments
- $# - Number of arguments
- $? - Return code of the previous command
- $$ - Process Identification number for the current script
- !! - Entire last command, including arguments. A common pattern is to execute a command only for it to fail due to missing permissions, then you can quickly execute it with sudo by doing sudo !!
- $_ - Last argument from the last command. If you are in an interactive shell, you can also quickly get this value by typing Esc followed by .

```
echo "Hello"
>>> Hello

echo $?
>>> 0 # No Error

grep foobar mcd.sh
echo $?
>>> 1 # Error
```

```
# || or
false || echo "Oops, fail" 
# Oops, fail


true || echo "Will not be printed"
#

# && and
true && echo "Things went well"
# Things went well

false && echo "Will not be printed"
#

false ; echo "This will always run"
# This will always run
```


```
#!/bin/bash

echo "Starting program at $(date)" # Date will be substituted

echo "Running program $0 with $# arguments with pid $$"

for file in $@; do
    grep foobar $file > /dev/null 2> /dev/null
    # When pattern is not found, grep has exit status 1
    # We redirect STDOUT and STDERR to a null register since we do not care about them
    if [[ $? -ne 0 ]]; then
        echo "File $file does not have any foobar, adding one"
        echo "# foobar" >> "$file"
    fi
done
```

**?** : one of character
***** : any amount of characters

```

convert image.{png,jpg}
# Will expand to
convert image.png image.jpg

cp /path/to/project/{foo,bar,baz}.sh /newpath
# Will expand to
cp /path/to/project/foo.sh /path/to/project/bar.sh /path/to/project/baz.sh /newpath

# Globbing techniques can also be combined
mv *{.py,.sh} folder
# Will move all *.py and *.sh files


mkdir foo bar
# This creates files foo/a, foo/b, ... foo/h, bar/a, bar/b, ... bar/h
touch {foo,bar}/{a..j}
touch foo/x bar/y
# Show differences between files in foo and bar
diff <(ls foo) <(ls bar)
# Outputs
# < x
# ---
# > y
```

**in python**

**Shebang**은 (사전에 검색해보면) 쉬뱅이라고 읽습니다. 쉬뱅은 `#!`로 시작하는 문자열이며 스크립트의 맨 첫번째 라인에 있습니다. 쉬뱅은 유닉스 계열 운영체제에서 스크립트가 실행될 때, 파이썬, 배쉬쉘 등 어떤 인터프리터에 의해서 동작이 되는지 알려줍니다.

```
#!/usr/local/bin/python
import sys
for arg in reversed(sys.argv[1:]):
    print(arg)
```

**shellcheck**

```
$ shellcheck test.sh

In test.sh line 2:
T0=`date +%s`
   ^-- SC2006: Use $(..) instead of legacy `..`.

In test.sh line 4:
T1=`date +%s`
   ^-- SC2006: Use $(..) instead of legacy `..`.

In test.sh line 5:
ELAPSED_TIME=$((T1-T0))
^-- SC2034: ELAPSED_TIME appears unused. Verify it or export it.

In test.sh line 7:
echo "START_TIME: " ${T0}
                    ^-- SC2086: Double quote to prevent globbing and word splitting.

In test.sh line 8:
echo "END_TIME: " ${T1}
                  ^-- SC2086: Double quote to prevent globbing and word splitting.

In test.sh line 9:
echo "ELAPSED_TIME: ${ELAPSES_TIME} sec"
                    ^-- SC2153: Possible misspelling: ELAPSES_TIME may not be assigned, but ELAPSED_TIME is.
```



**export**

환경변수를 저장하는 역할, 터미널이 꺼지면 사라진다.

```
vi ~/.bashrc # 해당 주소에서 작업을 하게되면 영구적으로 남는다.

export water="삼다수"
export TEMP_DIR=/tmp
export BASE_DIR=$TEMP_DIR/backup
```

```
# gpu idx를 지정할 때 사용할 수도 있다.

export CUDA_VISIBLE_DEVICES = 1
```



**Finding how to use commands**

```
ls -h
ls --help

man ls
```

- manpage
- [TLDR pages](https://tldr.sh/): 간단하게 찾아볼 수 있음

**Finding files**

```
# Find all directories named src
find . -name src -type d
# Find all python files that have a folder named test in their path
find . -path '**/test/**/*.py' -type f
# Find all files modified in the last day
find . -mtime -1
# Find all zip files with size in range 500k to 10M
find . -size +500k -size -10M -name '*.tar.gz'

# Delete all files with .tmp extension
find . -name '*.tmp' -exec rm {} \;
# Find all PNG files and convert them to JPG
find . -name '*.png' -exec convert {} {.}.jpg \;
```



**Finding code**

[**grep**](http://man7.org/linux/man-pages/man1/grep.1.html)



```
grep foobar mcd.sh
grep -R foobar . # source code 검색도 가능
```

[**ripgrep**](https://github.com/BurntSushi/ripgrep)



```
# Find all python files where I used the requests library
rg -t py 'import requests'
# Find all files (including hidden files) without a shebang line
rg -u --files-without-match "^#!"
# Find all matches of foo and print the following 5 lines
rg foo -A 5
# Print statistics of matches (# of matched lines and files )
rg --stats PATTERN
```





**Finding shell commands**

**history**

```
history

 1 cd .\OneDrive\sourceCode\CPPS\
 2 cd ..
 3 cd .\EEN-with-Keras\

```

**Ctrl + R** : history 추적, 유용

**zsh**: 유용한 bash 도구



**Directory Naviation**

- ls -R
- [tree](https://linux.die.net/man/1/tree)
- [broot](https://github.com/Canop/broot)
- nnn
- ranger

```
ls -R
tree 
```





- Reference: https://missing.csail.mit.edu/2020/?fbclid=IwAR2gQe5LToKuqVUwbfegqSOk6BnIqscbnqjK0e3js64EceMswNqW0KgeSEo