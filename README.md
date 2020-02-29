# 595-C-Exam-Sheet

## VM math
- Addressable memory = address space * addressability
- Remember 1 byte = 8 bits, hex takes 4 bits
- 2<sup>10</sup> = kilo, 2<sup>20</sup> = mega, 2<sup>30</sup> = giga
- each process has its own page table that maps virtual page to physical frame
- VM/#pages = PM/#frames = page size = frame size
- Virtual address is split into log(#pages) bits and log(page_size/addressability) bits

## LL implementation

## Array reviews
- char msg[] = "Exams are fun!";
  - can change
  - msg[0] on stack
- char* format = "%s %d";
  - cannot change. segfault
  - string literal/constant
  - format points to symbol table
- int A[] = {7, 9, 22};
  - there is no var A. A not on the stack
  - A[0] therefore is just shorthand *(&(A[0]))
  - *A = *(&(A[0])) => A = &(A[0]) = an address
  - a pt'er s a var that holds addr. Since A isn't a var, it's not a pt'er
  - A = &x //no can do. A not a var therefore cannot assign like this
- TODO. Draw the diagram of program text, heap, stack

## Structs and Data Structures
```
typedef struct Witch witch;     //makes the shortcut
typedef struct Animal animal;
struct Witch {
  char* name;
  animal* familiar;
};
struct Animal {
  char species[100];
}
```
- You can use dot `.` operator on a proper struct or `->` on a pointer to a struct
  - `witch* w = malloc(sizeof(witch));`
  - `w->familiar = a;`//a defined, initialized
  - `(*w).familiar = a;`

## Intialization
- TODO: lecture #3 material

## Threads, Locking, Trylock
- `void* threadable_fun(void* p){...}`
- `pthread t`;
- `pthread_create(&t, <config>, &threadable_fun, &param);`
  - returns 0 on success
- can omit & on &threadable
- `void* r`;
- `pthread_join(t, <pter to t's entry pt/return value>);`
  - example: pthread_join(t, &r);
  - returns 0 on success
- now recast r for usage
  - `int r_int = \*(int*)r`;
  - must cast then deref b/c you can't deref a void*
- TODO locking and trylock

## fn pt'er syntax

## Gotchas
- Make sure you draw the stack going the right direction
- `if(x || b == NULL)` should be `if(a == NULL || b == NULL)`
- Using an uninitialized var is grabbing a random cup and drinking from it while hoping for a milkshake
- When dealing with hex, carefully consider RtoL or LtoR
- strcmp returns 0 if it's a match
- strcpy(to, from, max_num_non_null_characters);
