## C 

```c
/*
    c_quick_refresh.c

    Compile:
        gcc -Wall -Wextra -Wpedantic c_quick_refresh.c -o cquick
*/

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct {
    char name[32];
    int age;
} Person;

void increment(int *x)
{
    if (x != NULL) {
        (*x)++;
    }
}

int main(void)
{
    /* -------------------------
       BASICS
       ------------------------- */

    int x = 10;
    double price = 12.50;
    char grade = 'A';

    printf("%d %.2f %c\n", x, price, grade);


    /* -------------------------
       ARRAYS
       ------------------------- */

    int nums[] = {10, 20, 30, 40};

    size_t count = sizeof(nums) / sizeof(nums[0]);

    for (size_t i = 0; i < count; i++) {
        printf("%d\n", nums[i]);
    }

    /* C does NOT check array bounds. */


    /* -------------------------
       STRINGS

       C string = char array ending in '\0'
       ------------------------- */

    char name[] = "Alice";

    printf("%s\n", name);
    printf("length = %zu\n", strlen(name));

    char copy[32];

    snprintf(copy, sizeof(copy), "%s", name);

    /* Compare string CONTENTS with strcmp */
    if (strcmp(name, copy) == 0) {
        printf("same string\n");
    }

    /*
        Don't use:

            name == copy

        for string-content comparison.
    */


    /* -------------------------
       POINTERS
       ------------------------- */

    int value = 42;

    int *ptr = &value;

    /*
        value = normal value
        &value = address of value
        ptr = stored address
        *ptr = value at that address
    */

    printf("%d\n", *ptr);

    *ptr = 99;

    printf("%d\n", value);   /* 99 */


    /* -------------------------
       PASS BY POINTER
       ------------------------- */

    int n = 5;

    increment(&n);

    printf("%d\n", n);   /* 6 */

    /*
        C passes arguments by VALUE.

        To modify caller data:
            pass its address.
    */


    /* -------------------------
       STRUCTS
       ------------------------- */

    Person person = {
        .name = "Bob",
        .age = 30
    };

    printf("%s %d\n", person.name, person.age);

    Person *person_ptr = &person;

    /*
        pointer-to-struct field:

            person_ptr->age

        same as:

            (*person_ptr).age
    */

    printf("%d\n", person_ptr->age);


    /* -------------------------
       MALLOC
       ------------------------- */

    size_t length = 5;

    int *dynamic = malloc(length * sizeof(*dynamic));

    if (dynamic == NULL) {
        return EXIT_FAILURE;
    }

    for (size_t i = 0; i < length; i++) {
        dynamic[i] = (int)i * 10;
    }

    free(dynamic);
    dynamic = NULL;


    /* -------------------------
       FILES
       ------------------------- */

    FILE *file = fopen("test.txt", "w");

    if (file != NULL) {
        fprintf(file, "hello\n");
        fclose(file);
    }


    /* -------------------------
       INPUT
       ------------------------- */

    /*
    char input[100];

    if (fgets(input, sizeof(input), stdin) != NULL) {

        // Remove trailing newline
        input[strcspn(input, "\n")] = '\0';

        printf("%s\n", input);
    }
    */


    /* -------------------------
       QUICK REMINDERS

       &x       address of x
       *ptr     value pointed to
       ptr->x   struct field through pointer

       strlen() string length
       strcmp() compare strings
       snprintf() safer string formatting

       malloc() allocate
       free()   release

       NULL     pointer points nowhere

       Arrays don't know their length.
       C doesn't check bounds.
       Strings end with '\0'.

       Compile with warnings:

       gcc -Wall -Wextra -Wpedantic file.c -o app
       ------------------------- */

    return EXIT_SUCCESS;
}
```
## Bash

```bash
#!/usr/bin/env bash

# bash_zsh_quick_refresh.sh
# Mostly works in both Bash and Zsh.

# -------------------------
# VARIABLES
# -------------------------

name="Alice"
count=3

echo "$name"
echo "count = $count"

# Use quotes unless you specifically want word splitting/globbing.
file="my notes.txt"
echo "$file"


# -------------------------
# COMMAND SUBSTITUTION
# -------------------------

today="$(date)"
files="$(ls)"

echo "$today"


# -------------------------
# ARGUMENTS
# -------------------------

# $0  script name
# $1  first argument
# $2  second argument
# $#  number of arguments
# "$@" all arguments, safely preserved

echo "script: $0"
echo "first arg: ${1:-none}"
echo "arg count: $#"

for arg in "$@"; do
    echo "arg: $arg"
done


# -------------------------
# DEFAULT VALUES
# -------------------------

name="${1:-World}"

echo "Hello, $name"


# -------------------------
# EXIT STATUS
# -------------------------

some_command 2>/dev/null

if [ $? -eq 0 ]; then
    echo "success"
fi

# Better:
if some_command; then
    echo "success"
else
    echo "failed"
fi


# -------------------------
# TESTS
# -------------------------

file="example.txt"

if [ -f "$file" ]; then
    echo "regular file exists"
fi

if [ -d "/tmp" ]; then
    echo "directory exists"
fi

if [ -z "$name" ]; then
    echo "empty string"
fi

if [ -n "$name" ]; then
    echo "non-empty string"
fi

# Numeric:
if [ "$count" -gt 0 ]; then
    echo "positive"
fi

# Common numeric operators:
# -eq  equal
# -ne  not equal
# -gt  greater than
# -lt  less than
# -ge  greater/equal
# -le  less/equal


# -------------------------
# MODERN CONDITIONALS
# -------------------------

if [[ "$name" == A* ]]; then
    echo "starts with A"
fi

# [[ ... ]] is generally nicer in Bash/Zsh
# than [ ... ] for string/pattern tests.


# -------------------------
# LOOPS
# -------------------------

for item in one two three; do
    echo "$item"
done

for file in *.txt; do
    echo "$file"
done

i=0

while [ "$i" -lt 3 ]; do
    echo "$i"
    i=$((i + 1))
done


# -------------------------
# ARITHMETIC
# -------------------------

x=5
y=3

result=$((x + y))

echo "$result"

((x++))

if (( x > 5 )); then
    echo "x > 5"
fi


# -------------------------
# FUNCTIONS
# -------------------------

greet() {
    local person="$1"
    echo "Hello, $person"
}

greet "Bob"


# -------------------------
# PIPES
# -------------------------

# stdout from command 1 becomes stdin for command 2

printf '%s\n' apple banana avocado |
    grep '^a'


# -------------------------
# REDIRECTION
# -------------------------

echo "hello" > file.txt       # overwrite
echo "again" >> file.txt      # append

command > output.txt          # stdout
command 2> errors.txt         # stderr
command > all.txt 2>&1        # stdout + stderr

command >/dev/null 2>&1       # discard everything


# -------------------------
# READ FILE LINE BY LINE
# -------------------------

while IFS= read -r line; do
    echo "$line"
done < file.txt


# -------------------------
# READ USER INPUT
# -------------------------

read -r -p "Name: " user_name
echo "Hello, $user_name"


# -------------------------
# COMMON COMMANDS
# -------------------------

pwd                 # current directory
ls -la              # list files
cd dir              # change directory

cp source dest       # copy
mv source dest       # move / rename
rm file              # delete file
rm -r dir            # delete directory recursively

mkdir dir            # create directory
mkdir -p a/b/c       # create nested directories

touch file.txt       # create/update file

cat file.txt         # print file
less file.txt        # scroll file

head file.txt
tail file.txt
tail -f app.log      # follow log

grep "text" file
grep -R "text" .     # recursive

find . -name "*.c"

wc -l file.txt       # line count

sort file.txt
uniq file.txt

which gcc
command -v gcc       # usually preferable


# -------------------------
# CURL
# -------------------------

curl https://example.com

curl -O https://example.com/file.zip

curl -L URL          # follow redirects

curl -I URL          # headers only


# -------------------------
# PROCESSES
# -------------------------

ps aux

ps aux | grep program

kill PID
kill -9 PID          # force kill; use sparingly

jobs                # shell jobs
command &           # run in background
fg                  # bring job to foreground


# -------------------------
# PERMISSIONS
# -------------------------

chmod +x script.sh

chmod 755 script.sh

# owner/group:
chown user:group file


# -------------------------
# ENVIRONMENT VARIABLES
# -------------------------

export DEBUG=1

echo "$PATH"

export PATH="$HOME/bin:$PATH"


# -------------------------
# CHAINING COMMANDS
# -------------------------

cmd1 && cmd2
# cmd2 runs only if cmd1 succeeds

cmd1 || cmd2
# cmd2 runs only if cmd1 fails

cmd1 ; cmd2
# always run both


# -------------------------
# HERE DOCUMENT
# -------------------------

cat <<EOF
hello
multi-line
text
EOF


# -------------------------
# USEFUL SCRIPT HEADER
# -------------------------

# Bash:
#   #!/usr/bin/env bash
#
# Zsh:
#   #!/usr/bin/env zsh


# -------------------------
# SAFER BASH SCRIPTS
# -------------------------

# Common at top of Bash scripts:
#
# set -euo pipefail
#
# -e        stop on many command failures
# -u        error on unset variables
# pipefail  pipeline fails if any command fails
#
# Useful, but understand its behavior before blindly
# adding it to every script.


# -------------------------
# QUICK REMINDERS
# -------------------------

# "$var"      expand variable safely
# "${var}"    explicit variable boundary
# "$(cmd)"    command output
# "$@"        all script arguments
# "$?"        previous exit code
# "$#"        argument count
# "$$"        current shell/process ID
#
# >           overwrite
# >>          append
# |           pipe
# &&          run next on success
# ||          run next on failure
# &           background process
#
# Prefer:
#   "$variable"
#   "$(command)"
#   [[ condition ]]
#
# Be careful with:
#   rm -rf
#   unquoted variables
#   eval
#   sudo
```
