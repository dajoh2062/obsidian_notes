
## C

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct {
    char name[32];
    int age;
} Person;

void inc(int *x) {
    if (x) (*x)++;
}

int main(void) {
    int x = 10;
    double d = 3.14;
    char c = 'A';

    printf("%d %.2f %c\n", x, d, c);

    int a[] = {1, 2, 3};
    size_t n = sizeof(a) / sizeof(a[0]);

    for (size_t i = 0; i < n; i++)
        printf("%d\n", a[i]);

    char s[32] = "hello";
    printf("%s %zu\n", s, strlen(s));

    if (strcmp(s, "hello") == 0)
        puts("equal");

    int *p = &x;
    *p = 20;
    inc(&x);

    Person user = {.name = "Bob", .age = 30};
    Person *up = &user;
    printf("%s %d\n", up->name, up->age);

    int *mem = malloc(5 * sizeof(*mem));
    if (!mem) return EXIT_FAILURE;

    mem[0] = 42;
    free(mem);
    mem = NULL;

    FILE *f = fopen("test.txt", "w");
    if (f) {
        fprintf(f, "hello\n");
        fclose(f);
    }

    /*
        &x      address
        *p      dereference
        p->x    struct via pointer
        NULL    no address
        malloc/free
        strcmp  compare strings
        '\0'    string terminator
    */

    return 0;
}
```
## Bash

```bash
#!/usr/bin/env bash

name="${1:-World}"
count=3

echo "$name"
echo "$(date)"

for arg in "$@"; do
    echo "$arg"
done

if [[ -f "file.txt" ]]; then
    echo "exists"
fi

if [[ "$name" == A* ]]; then
    echo "starts with A"
fi

if (( count > 0 )); then
    echo "positive"
fi

for x in 1 2 3; do
    echo "$x"
done

i=0
while (( i < 3 )); do
    echo "$i"
    ((i++))
done

greet() {
    local name="$1"
    echo "Hello $name"
}

greet "Bob"

result=$((5 + 3))
echo "$result"

echo "hello" > file.txt
echo "again" >> file.txt

cat file.txt | grep hello

while IFS= read -r line; do
    echo "$line"
done < file.txt

pwd
ls -la
cd dir
mkdir -p a/b
cp a b
mv a b
rm file
find . -name "*.c"
grep -R "text" .
tail -f app.log

cmd1 && cmd2
cmd1 || cmd2
command &

export DEBUG=1
export PATH="$HOME/bin:$PATH"

# "$var"   variable
# "$(cmd)" command output
# "$@"     all args
# "$?"     exit code
# $#       arg count
# > >>     overwrite / append
# |        pipe
# && ||    success / failure
```