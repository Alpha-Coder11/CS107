Lecture_9
========================

<h2> Stack Segment </h2>
 
 
```C
    int i;
    int j;
    i = 10;
    j = i+7;
    j++;
```


- **R1** stores the base adress of the activation record of the function. 

```C
    M[R1 + 4] =10; //store operation    
    // M is the base adress of the RAM 
    // its not actually a register sapce but it is RAM space
    
    R2 = M[R1+4];     // Load operation           // RAM space to register space
    
    R3 = R2+7;          // ALU operation          // single register and an integer or both registers
    
    M[R1] = R3;       // Store operation
    R2 = M[R1];
    R2 = R2 +1;
    M[R1] =R2;
```


> Hardware is designed to handle 4 bytes in optimized manner, it doesn't mean we cannot handle 2 bytes and 8 bytes, its just that Assembly will need more intructions to handle that

- Assembly is constructed in context insensitive manner

```C
    int i;
    short s1;
    short s2;
    i = 200;    --->        M[R1+4] = 200;
    
    s1 = i;     --->        M[R1+2] = M[R1+4];
    // This looks correct, but isn't. The first problem with this is you cannot use load and store type of statements in one go. It would require assembly instruction to encode in 4 bytes the source and destination memory adress.
```



```C
    R2 = M[R1+4];
    M[R1+2] = R2; 
    // This is syntactically correct, but it will yeild weird results because it is writing 4 bytes in the "short type" memory, so it will go over board and also modify adjacent memory space.
```

 
```C
    M[R1+2] = .2 R2;
    // It will just take 2 lower bytes and update the destination

    s2 = s1+1;
 
    R2 = .2 M[R1+2]; 

    // It will lay 2 bytes in lower byte space of the register. and then it sign extends so the rest of the register is padded with 2 bytes of **-**.

    R3 = R2+1;
    M[R1] = .2R3;
```


- If there is a loop in a language, you should know that there should be jump clauses that can jump the code back or when the conditional statement is there, the code will jump around based on the condition.
```C
    M[R1]0;
    R2 = M[R1];
    BGE R2, 4, PC + 40
    R3 = M[R1];
    R4 = R3 * 4;
    R5 = R1 + 4;
    R6 = R4 + R5;
    M[R6] = 0;
    R2= M[R1]
    R2 =R2+1;
    M[R1] = R2;
    JMP PC-40
    i--
```


- When you put a cast in c code, there is no assembly code instruction that gets generated as a result of cast operation, all the cast does is it allows the copiler to generate code that it wouldn't have been able to generate without it. Its just a permission to behave differently.
