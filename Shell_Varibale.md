# 🔑 Special Shell Variables Cheat Sheet

These are built-in variables in Bash that provide information about the script, arguments, process IDs, and execution status.

---

## 📌 Positional Parameters
- `$0` → Name of the script itself
- `$1, $2, $3 ...` → First, second, third argument passed to the script
- `$#` → Number of arguments passed
- `$@` → All arguments (each as separate word)
- `$*` → All arguments (as a single string)

---

## 📌 Process & Execution
- `$$` → Process ID (PID) of the current shell
- `$!` → PID of the last background process
- `$?` → Exit status of the last command (0 = success, non-zero = error)
- `$-` → Current shell options (set by `set` command)
- `$PPID` → Process ID of the parent shell

---

## 📌 User & Environment
- `$UID` → User ID of the current user
- `$HOME` → Home directory of the current user
- `$PWD` → Current working directory
- `$OLDPWD` → Previous working directory
- `$PATH` → Search path for executables
- `$SHELL` → Default shell for the user
- `$USER` → Username of the current user
- `$HOSTNAME` → Name of the host machine

---

## 📌 Random & Special
- `$RANDOM` → Random integer between 0 and 32767
- `$LINENO` → Current line number in the script
- `$SECONDS` → Number of seconds since the script started
- `$FUNCNAME` → Name of the current function
- `$BASH_VERSION` → Version of Bash being used
- `$BASH_SOURCE` → Source filename of the current script
- `$BASH_SUBSHELL` → Subshell level (0 = main shell)

---

## 📌 Input/Output
- `$IFS` → Internal Field Separator (default: space, tab, newline)
- `$OPTARG` → Value of the current option argument (used with `getopts`)
- `$OPTIND` → Index of the next argument to be processed by `getopts`

---

## ✅ Quick Examples
```bash
echo "Script name: $0"
echo "First argument: $1"
echo "Number of args: $#"
echo "All args: $@"
echo "PID of script: $$"
echo "Last exit status: $?"
echo "Random number: $RANDOM"
