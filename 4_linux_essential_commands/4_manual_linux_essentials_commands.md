
---
`uptime` - command to check from how much time system is up and running, number of users logged in 

---

`man <command_name>` to know the details of command

---

`ps aux` 

    - a - for all process
    - u - user oriented seeing format
    - x - show process which are running in background as well

---
`sudo kill <processId>` - command to kill process

---
`top` - realtime process data
- similar to terminal task manager
- type k and the type process id to kill process
---
`sudo apt install htop`
`htop` - similar to top but more human friendly, 3rd party
- can filter, can sort , more human readable, modern, mouse support
- Ctrl + C to exit

---
`df` - disk free (hard disk storage info)
`df -h` human readabale 

/ main partision
tmpfs - ram memory , shared memory  (/dev/shm , /run)
/dev/xvda16 (/boot , /boot/efi) - system partision  for os boot 
tmpfs (/run/user/1000) - running user temp storage 

---
`free -h` - ram details human readable
command to check ram usage
mem - memory 
swap - swap memory 

-----
`history` - get history of all commands executed
- it will give history of commands with number you can executed the command with speicied number by using `!<number>`

`history | grep <command>`
`usr\home\.bash_history` - file contains that user command history
---

we should hit curl with api key as someone may use history command to get the API key and using that API key someone may do something wrong but it will be tracked to your API key 

istead use below command

`read -s -p "Enter you API Key" API_KEY` - using this command user will be propmpt to enter the API key and that key will be stored in the variable not in history 
`$API_KEY` - variable can be used this way

---
if you want any command to not shown in history just add a space ` ` before the command
 ` curl ......` etc it will not be displayed in history
some distribution may still save it 

`echo $HISTCONTROL` - if this contain ignorespace then it will ignore space to save to histroy  
`HISTCONTROL=ignoredups` - ignore dublicates
`HISTCONTROL=ignorespace` - to exclude command with leading space
`HISTCONTROL=erasedups` - eleminate all previous dublicates
`HISTCONTROL=ignoreboth` is same as `HISTCONTROL=ignorespace:ignoredups`

---
`which <command>` it will give path of the binary of that command (will not work for inbuild commands)
example `which python3`

can be used to know which version is getting used, which directory is the cammand present

---
`whereis python3` - it will tell where is python3, in detail

---
`echo $PATH` - to check folder are in path or not , seprated by `:`

---
`alias` - create custom command
`alias staff='sudo cat /etc/passwd'` - example alias to see /etc/passed file

by default alias is temprary you have to add this comand in ./bashrc , add command at the end of the terminal

now restart it using source ./bashrc

---
sometime we need to add a dynamic input to command so we cant directly use alias for that 

below are the bash functions
add below code in ./bashrc
```bash
gac() {
    git add . && git commit -m "$1"
}
```
`$1` us the first parameter

call function by using `gac "inital commit"`

---
`tail -n 10 -f `
- n - number of lines
- f - live logs

`tail -f -n 20 /var/log/apache2/acces.log` - last 20 lines logs with live logs

---
## bash
bourne again shell

path - `/bin/bash`
`echo $SHELL` - command to get user shell

---

`.sh` is the file name exitionsion

`#!` is called shiband
`#!/usr/bin/env bash` - add this line on the top of shell script to specify which shell to use
`#` is used for comment

```bash
    #!/usr/bin/env bash
    #aboce line spcifies which shell to use

    #variables
    name="rakesh"
    balance=100
    # make sure to not give space in name = as it will cause problem
    echo "I am $name, and i have $balance left"

    #arrays
    languages=(javascript golanf java python)

    # access singlt item
    echo ${languages[2]}

    # list all items
    echo ${languages[@]} # preferred way
    echo ${languages[*]}

    #length of array
    echo ${#languages[@]}

    #loops
    # for loops
    for ((i=0;i<${#languages[@]};i++)); do 
        echo "index $i = ${languages[$i]}"
    done


    # for in loop
    for language in "#languages[@]"; do
        echo "$language"
    done

    #maps (dictionary) 
    # will work when bash verison is > 4
    # declare -A prices
    # prices[Pizza]=400
    # prices[salad]=100
    # prices[Rolles]=300

    # echo prices[Pizza]


    # if else
    points=20
    if [ $points -gt 40 ]; then # make sure to add space in [] before and after the condition 
        echo "You are pass"
    else
        echo "You are fail"
    fi

    # create file if does not exists
    if [ -f hello.txt ]; then
        echo "file exists"
    else
        echo "file does not exists"
        touch hello.txt
    fi


    # create file if does not exists
    if [ -d scr ]; then
        echo "folder exists"
    else
        echo "folder does not exists"
        mkdir scr
    fi

    # list all txt file for processing
    for file in *.txt; do 
        cat "$file"
    done

    # write in file with data
    echo "this is my data" > hello.txt

    # append data to hello.txt
    echo "this is new data" >> hello.txt
    

    # Multi line file data addition will save data till EOF is recived

    # cat <<EOF > about.txt
    # this is my first line
    # this is my second line
    # this is my third line
    # EOF
    # EOF should be at the start of the file without tab space



    # functions
    greet(){
        echo "Hello , $1"
    }


```

---

`$1` is the first argument 
`$0` is the script name 




















