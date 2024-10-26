Lecture_11
========================
<h2> How C and C++ languages are actually similar after compilation

```C
void foo()
{
    int x;
    int y;
    
    x = 11;
    y = 17;
    swap(&x, &y);
         
}
```
- Assembly version of function foo() 
```C
SP = SP - 8;
M[SP+4] = 11;
M[SP] = 17;
R1 = SP;        // &x
R2 = SP + 4     // &y

SP = SP - 8;
M[SP] = R2;
M[SP+4] = R1;
CALL <swap>
SP = SP + 8;

SP = SP + 8;
RET;

```

```C
void swap(int *ap, int *bp)
{
    int temp = *ap;
    *ap = *bp;
    *bp = temp;
    
}
```

```C
SP = SP - 4;
R1 = M[SP + 8];            // get the address
R2 = M[R1];                // get the value at the address
// We cannot do M[M[SP+8] because it is not supported with any architecture to do double cascade with sing assembly instruction
M[SP] = R2;
R1 = M[SP+12];
R2=[R1]
R3 =M[SP+8];
M[R3] = M[R2];

R1 = M[SP];
R2 = M[SP +12];
M[R2] = R1;

SP = SP + 4
RET;
```
Lets imagine a c code with C++ reference feature
```C
void swap(int& a, int& b)
{
    int temp = a;
    a = b;
    b = temp;
}
void foo()
{
    int x;
    int y;
    x = 11;
    y = 17;
    swap(x,y);
}
```
- The **a** and **b** are looking like normal integers from the code, but they are in reality, automatically dereferenced pointers.
- When we are calling this swap function, even if are not passing & symbol with x and y, it doesnt mean the compiler is blocked from passing their addresses.
- It does exactly the same thing when we were working directly with pointers. The C++ can tell from the prototype that the variables are passed by reference.  The assembly generated for this function is exactly similar to the former **swap()** function.
```C
    int &z = y;
    int *z = &y;
    // these two instructions are similar in assembly
    // the only difference is that you cannot alot a new l_value to the reference but pointer can be re directed to any memory location.
```
