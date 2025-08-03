### Day-04- Shell Script for devops with hands on example

### 🧠 What is Bash Scripting? 
Bash scripting is a way to write a sequence of Linux/Unix commands in a file, and execute them all at once to automate repetitive tasks like deployments, monitoring, backups, etc.

### Get your shell
```sh
echo $0
cat /etc/shells      # get all shell list
cat /etc/passwd      # user is assigned by which shell
```


### ✍️ Write Your First Script
File: hello.sh
```sh
#!/bin/bash
echo "Hello, World!"
```
Make it executable:
```sh
chmod +x hello.sh
./hello.sh
```
🎙️ Narration:
<br>
This simple script prints 'Hello, World!'. The first line #!/bin/bash tells the system to use Bash to interpret the script."

### Run a shell file
```sh
./file-name.sh
path/file-name.sh
bash file-name.sh
```
### 📦 Variables in Bash 
File: variables.sh
```sh
#!/bin/bash
name="Rajiv"
echo "My name is $name"
```
🎙️ Narration:
<br>
"Variables store data. Use = without spaces. $name accesses the value.

### 👂 Read User Input
File: user_input.sh
```sh
#!/bin/bash
echo "Enter your name:"
read username
echo "Welcome, $username!"
```
🎙️ Narration:
<br>
"read takes input from the user and stores it in a variable."

### 🔀 If-Else Conditions 
File: if.sh
```sh
#!/bin/bash
echo "Enter your age:"
read age

if [ $age -ge 18 ]; then
  echo "You are eligible to vote."
else
  echo "You are not eligible to vote."
fi
```
🎙️ Narration:
<br>
"if, then, and else are used to control flow based on conditions. Use -ge for greater than or equal."

### 🔁 Loops in Bash
For loop:
```sh
#!/bin/bash
for i in 1 2 3 4 5
do
  echo "Number $i"
done
```
While loop:
```sh
#!/bin/bash
count=1
while [ $count -le 5 ]
do
  echo "Count is $count"
  ((count++))
done
```
🎙️ Narration:
<br>
"Loops help repeat tasks. for iterates over a list. while runs as long as the condition is true."

### 🧩 Functions in Bash
File: functions.sh
```sh
#!/bin/bash

greet() {
  echo "Hello $1, welcome to Bash scripting!"
}

greet "Rajiv"
```
🎙️ Narration:
<br>
"Functions are reusable blocks of code. $1, $2 are arguments passed to the function."

### Create a ping demo bash script
```sh
#!/bin/bash

ping -c1 192.168.1.1
        if [ $? -eq 0 ]
        then
        echo OK
        else
        echo NOT OK
        fi

Change the IP to 192.168.1.235


Don't show the output
ping -c1 192.168.1.1 &> /dev/null
        if [ $? -eq 0 ]
        then
        echo OK
        else
        echo NOT OK
        fi


Define variable
#!/bin/bash

hosts="192.168.1.1"
ping -c1 $hosts &> /dev/null
        if [ $? -eq 0 ]
        then
        echo $hosts OK
        else
        echo $hosts NOT OK
        fi

Change the IP to 192.168.1.235




Multiple IPs
#!/bin/bash

IPLIST="path_to_the_Ip_list_file"


for ip in $(cat $IPLIST)

do
   ping -c1 $ip &> /dev/null
   if [ $? -eq 0 ]
   then
   echo $ip ping passed
   else
   echo $ip ping failed
   fi
done
```
### linux command
```sh
history                         # see all used command
history | more                  # see long file
history | grep chmod            # search from history by chmod word
!223                            # run the command from histroy here i use 223
cat .bash_history               # histtory file location is home/user/.bash_history
cat /home/rajiv/.bash_history    # login as a root and see rajiv user history
```

### ✅ Wrap Up and Summary
Narration:
<br>
"Today you learned how to write basic Bash scripts with variables, inputs, conditions, loops, and functions. Bash scripting is a key tool for DevOps and system automation!"


## Intermediate level

### echo message with green color
```sh
 echo -e "\033[0;32mRajiv\033[0m"
```

### Now create a function which make the echo message green color
Create a funciton and run it in command line
```sh
function print_green(){
  GREEN="\033[0;32m"
  NC="\033[0m"

  echo -e "${GREEN} $1 ${NC}"
}
```
Now we can use this function to print anything in green color -  here we pass the messahe Dhaka
```sh
print_green "Dhaka"
```

### Now create another funciton -we can choose red or green color and the message we want 
```sh
function print_color(){
  case $1 in
    "green") COLOR="\033[0;32m";;
    "red") COLOR="\033[0;31m";;
    "*") COLOR="\033[0;32m";; 
  esac

  echo -e "${COLOR} $2 ${NC}"

}
```
Now we can use this function to print anything in green or red color -  here we pass the messahe Dhaka 
```sh
print_color "green" "dhaka"
print_color "red" "dhaka"
```









