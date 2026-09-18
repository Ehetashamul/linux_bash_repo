
```markdown
# 📘 Mastering Shell Scripting – Complete Notes

+---------+       +---------+       +---------+       +---------+
|  User   | --->  |  Shell  | --->  | Kernel  | --->  | Hardware|
+---------+       +---------+       +---------+       +---------+

---

## 1. Introduction to Shell Scripting
- **Definition**: Shell scripting automates tasks in Linux by writing sequential commands.
- **Uses**: System administration, automation, file manipulation, user interaction.
- **Popular Shells**:
  - `bash` (Bourne Again Shell – most common)
  - `sh` (Bourne Shell)
  - `ksh` (KornShell)
  - `zsh` (Z Shell)
  - `fish`, `tsh`

Check your shell:
```bash
echo $0
```

---

## 2. Creating & Running Scripts
1. Create file:
   ```bash
   vi my_script.sh
   ```
2. Add content:
   ```bash
   #!/bin/bash
   echo "Hello, World!"
   date
   ```
3. Save & exit: `:wq`
4. Make executable:
   ```bash
   chmod +x my_script.sh
   ```
5. Run:
   ```bash
   ./my_script.sh
   # or
   bash my_script.sh
   ```

---

## 3. Process Control Shortcuts
- `Ctrl+C` → terminate process
- `Ctrl+Z` → pause process, resume with `fg` or `bg`

---

## 4. Comments
- **Single-line**: `# This is a comment`
- **Multi-line (hack)**:
  ```bash
  : << 'COMMENT'
  This is a multi-line comment
  COMMENT
  ```

---

## 5. Variables & Constants
```bash
NAME="Linux"
echo "Welcome to $NAME"
readonly VERSION="1.0"
```

---

## 6. Arrays
```bash
myArray=(1 2 "Hello" "World")
echo "${myArray[1]}"   # 2
echo "${#myArray[@]}"  # length
myArray+=(5 6 8)       # update
```

**Associative Arrays**:
```bash
declare -A myMap
myMap=([name]=Paul [age]=20)
echo "${myMap[name]}"
```

---

## 7. String Operations
```bash
str="Shell Scripting"
echo ${#str}                     # length
echo ${str/Scripting/Programming} # replace
echo ${str:6:9}                  # substring
upper=${str^^}                   # uppercase
lower=${str,,}                   # lowercase
```

---

## 8. User Input
```bash
read var
echo "You entered: $var"

read -p "Your name: " NAME
echo "Hello, $NAME!"
```

---

## 9. Arithmetic
```bash
let a=5*10
((a++))
echo $((5*(3+2)))   # 25
```

---

## 10. Conditional Statements
```bash
if [ $a -gt $b ]; then
  echo "a > b"
elif [ $a -eq $b ]; then
  echo "a = b"
else
  echo "a < b"
fi
```

**Case Statement**:
```bash
case $choice in
  a) date ;;
  b) ls ;;
  *) echo "Invalid" ;;
esac
```

---

## 11. Operators
| Operator | Meaning |
|----------|----------|
| `-eq` / `==` | Equal |
| `-ne` / `!=` | Not Equal |
| `-gt` | Greater Than |
| `-lt` | Less Than |
| `-ge` | Greater or Equal |
| `-le` | Less or Equal |

**Logical**:
- `&&` → AND
- `||` → OR
- `!` → NOT

---

## 12. Loops
**For Loop**:
```bash
for i in 1 2 3; do
  echo "Number: $i"
done
```

**While Loop**:
```bash
count=1
while [ $count -le 3 ]; do
  echo "Count: $count"
  ((count++))
done
```

**Until Loop**:
```bash
count=1
until [ $count -gt 3 ]; do
  echo "Count: $count"
  ((count++))
done
```

**Infinite Loop**:
```bash
while :; do
  echo "Infinite..."
done
```

**Select Loop (Menu)**:
```bash
PS3="Choose a fruit: "
select fruit in Apple Banana Orange Exit; do
  case $fruit in
    Apple) echo "Apple";;
    Banana) echo "Banana";;
    Exit) break;;
  esac
done
```

---

## 13. Functions
```bash
greet() {
  echo "Hello, $1!"
}
greet "Adhyansh"
```

- **Return values** via `echo`
- **Recursion** possible
- **Default args**: `${1:-Guest}`
- **Arguments**:
  - `$1, $2` → positional
  - `$@` → all args (separate words)
  - `$*` → all args (single string)
  - `$#` → count

---

## 14. Shift Operator
```bash
shift_example() {
  echo "Original: $1 $2 $3"
  shift
  echo "After shift: $1 $2"
}
shift_example one two three
```

---

## 15. Loop Control
- `break` → exit loop
- `continue` → skip iteration
- `sleep 5` → pause
- `exit 1` → terminate script
- `$?` → exit status of last command

---

## 16. seq Command
```bash
seq 1 5
seq 1 2 10
seq -w 1 5
seq -s ", " 1 5
```

---

## 17. Brace Expansion
```bash
echo {a..e}         # abcde
echo {1..10..2}     # 1 3 5 7 9
echo file{1..3}.txt # file1.txt file2.txt file3.txt
```

---

## 18. getopts (Options Parsing)
```bash
while getopts "f:n:" opt; do
  case $opt in
    f) echo "File: $OPTARG" ;;
    n) echo "Name: $OPTARG" ;;
    ?) echo "Invalid"; exit 1 ;;
  esac
done
```

---

## 19. File Path Utilities
- `basename /path/file.txt` → `file.txt`
- `dirname /path/file.txt` → `/path`
- `realpath file.txt` → `/abs/path/file.txt`

**Existence Checks**:
```bash
[ -d folder ] && echo "Dir exists"
[ -f file ]   && echo "File exists"
```

---

## 20. Special Variables
- `$RANDOM` → random number (0–32767)
- `$UID` → current user ID
- `$0` → script name

---

## 21. Redirection
- `>` → overwrite
- `>>` → append
- `2>` → redirect errors
- `/dev/null` → discard output

---

## 22. Logging & Debugging
```bash
logger "Message"        # logs to /var/log/messages
set -x                  # enable debugging
set -e                  # exit on error
```

---

## 23. Background Execution
```bash
nohup ./script.sh &
```

---

## 24. Scheduling
- **at** → one-time jobs
  ```bash
  at 12:00
  ./myscript.sh
  Ctrl+D
  ```
- **cron** → recurring jobs
  ```bash
  crontab -e
  * * * * * /path/script.sh
  ```

---

## 25. Advanced Topics (Added)
- **Here Documents**:
  ```bash
  cat <<EOF
  Line1
  Line2
  EOF
  ```
- **Command Substitution**:
  ```bash
  files=$(ls)
  ```
- **Trap Signals**:
  ```bash
  trap "echo Interrupted; exit" SIGINT
  ```
- **Export Variables**:
  ```bash
  export VAR=value
  ```
- **Subshells**:
  ```bash
  (cd /tmp && ls)
  ```

---
