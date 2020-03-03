# 595-C-Exam-Sheet

## VM math
- Addressable memory = address space * addressability
- Remember 1 byte = 8 bits, hex takes 4 bits
- 2<sup>10</sup> = kilo, 2<sup>20</sup> = mega, 2<sup>30</sup> = giga
- each process has its own page table that maps virtual page to physical frame
- VM/#pages = PM/#frames = page size = frame size
- Virtual address is split into log(#pages) bits and log(page_size/addressability) bits

full example: 
Consider a system with:

32 bit address space, byte addressable, 1GB physical memory, 2^18 entries in page table.

32 bit address space means 2^32 addresses
byte addressable means 2^32 theoretically possible bytes ( VM size)

Size in bytes of each page = 2^32 / 2^18 = 2^14

so 2^4 times 2^10 = 16 * 1KB

How many frames? Frame size == page size, so frames are also 2^14

1GB physical mem = 2^30

2^30 / 2^14 = 2^16 frames in physical memory

## LL implementation
``` c 

typedef struct Node node;
struct Node {
	int data;
	node* next;
};

//adding an element to the linked list at the head
//1. Create the new node
//2. Point the new node's next to the current head node
//3. Point the "head node pointer" to the new node

//function to add a node to the head follows
//have to pass in the pointer to a pointer, because we are needing to change head in a pass-by-reference fashion

int add_to_front(int value, node** head) {
	node* new = malloc(sizeof(node));
	if(new == NULL) return 1;
	new->data = value;
	new->next = *head;
	*head = new;
	return 0; //this means success, by convention
}
int fun() {
	node* h = NULL;
	add_to_front(8, &h); //the address of h is a node ptr ptr
}
```

<<<<<<< Updated upstream
=======
## Generic LL
``` c
typedef struct Node node;
struct Node {
  void* data; //void* is a variable that holds address of "something"
  node* next;
};

int contains(void* val, node* head, int (*compare)(void*, void*)){
  while(head != NULL){
    if(compare(val, head->data) == 0) return 1;
    head = head->next;
  }
  return 0;
}

int str_compare(void* a, void* b){
  char* ac = (char*) a;
  char* bc = (char*) b;
  return strcmp(ac, bc);
}

...

node* head = ...
add_to_front...
char* name = "dog";
if(contains(name, head, &str_compare)) printf("puppy!");
```

``` c
//function pointer syntax:
int (*funp) (int); 

//declares a pter funp to function that takes an int (second int) and returns an int (first int)

```

>>>>>>> Stashed changes
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
  - they key here is that the arrow notation is used when you are working with a ptr to a struct, the dot notation when you are using the struct var itself. so you can do something like `(*w).familiar = a ` because you are dereferencing the struct ptr first.

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
  - `int r_int = *(int*)r`;
  - must cast then deref b/c you can't deref a void*
``` c
LOCKS AND TRYLOCK
//global var x
int x = 0;
//also a global var. a struct of type mutex_t named lock. 
//defined in the <pthread.h> library. the struct is global.
//not yet initialized but we will need to
pthread_mutex_t lock, lock2;

//first entry point
void* fun5 (void* p) {
	//pass our lock to a function called lock
	//attempt to aquire the lock 
	//wait here if another thread holds it (goto sleep here)
	pthread_mutex_lock(&lock);
	//once lock is aquired, we can update the var x. 
	//this is our critical section (where race could occur)
	x += 8;
//once done with critical section, must unlock
	pthread_mutex_unlock(&lock);
}
//global var k
int k = 0;
void* fun6(void* p) {
	pthread_mutex_lock(&lock);
	x += 4;
	pthread_mutex_unlock(&lock);
}

void* fun7(void* p) {
	//we shouldn't use the same lock (lock) for this
 	//because no chance of race condition - two different global vars are being manipulated. 
// so use lock2
	pthread_mutex_lock(&lock2);
	k += 3;
	pthread_mutex_unlock(&lock2);
}
//we need to initialize the locks before starting the threads
main() {
	//pass lock to be initialized
	//pass NULL as the config param, we can use default
	pthread_mutex_init(&lock, NULL);
	pthread_mutex_init(&lock2, NULL);
}
//returns 0 and acquires lock if available.
//returns non-zero and proceeds if not available.
pthread_mutex_trylock(&lock);
```
## fn pt'er syntax
``` c
int (*funp)(int);
/**this is declaring a pointer which is named funp to a function that takes an int as a parameter and returns an int. 
```

## Scheduling
* *Non-Preemptive*: if a process is running, it continues to run until it completes or until it gives up the CPU. So it won’t be interrupted, though it may have to wait for some resource and thereby go from 
running -> blocked -> ready -> back to running. 
* *Preemptive*: the Scheduler may interrupt after a certain amount of time and/or if some other process becomes ready.
<<<<<<< Updated upstream
=======
- response_time = start - arrival
- turnaround = finish - arrival
- waiting time = turnaround - execution = how long in ready/blocked state
- a process is one or more threads
- a thread is a schedulable unit, also called lightweight process
- threads in a single process all share one process ID but have separate Program Counter, Frame Pointer, Stack Pointer, priority, NZP registers
- PCB process control block is a DS in memory for OS that tracks ready and waiting processes
- threads in the same process share the same heap
>>>>>>> Stashed changes

## Gotchas and Misc
- Make sure you draw the stack going the right direction
- `if(x || b == NULL)` should be `if(a == NULL || b == NULL)`
- Using an uninitialized var is grabbing a random cup and drinking from it while hoping for a milkshake
- When dealing with hex, carefully consider RtoL or LtoR
- strcmp returns 0 if it's a match
- strncpy(to, from, max_num_non_null_characters);
- deadlock can't happen if you're only using one lock (eddiemurphy_pointingtohead.gif)
- malloc syntax example: 
``` c 
 pthread_t *threads = malloc(sizeof(pthread_t)*N) 
  ```
  - remember if passing an array to a function to do stuff at run time, you have to also pass in the size of the array you wish to work with because you can't determine an array size in C just by looking at each element.
  - remember that when drawing symbol table, params start at FP+4 and are on the stack in increasing address, global vars aren't on the local symbol table
  - round robin scheduling = each process gets the same fixed amount of time on cpu

