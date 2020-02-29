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

## Intialization
- TODO: lecture #3 material

## threads, locking, trylock

## fn pt'er syntax
