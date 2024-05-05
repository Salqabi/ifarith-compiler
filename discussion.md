# Discussion and Reflection


The bulk of this project consists of a collection of five
questions. You are to answer these questions after spending some
amount of time looking over the code together to gather answers for
your questions. Try to seriously dig into the project together--it is
of course understood that you may not grasp every detail, but put
forth serious effort to spend several hours reading and discussing the
code, along with anything you have taken from it.

Questions will largely be graded on completion and maturity, but the
instructors do reserve the right to take off for technical
inaccuracies (i.e., if you say something factually wrong).

Each of these (six, five main followed by last) questions should take
roughly at least a paragraph or two. Try to aim for between 1-500
words per question. You may divide up the work, but each of you should
collectively read and agree to each other's answers.

[ Question 1 ] 

For this task, you will three new .irv programs. These are
`ir-virtual?` programs in a pseudo-assembly format. Try to compile the
program. Here, you should briefly explain the purpose of ir-virtual,
especially how it is different than x86: what are the pros and cons of
using ir-virtual as a representation? You can get the compiler to to
compile ir-virtual files like so: 

racket compiler.rkt -v test-programs/sum1.irv 

(Also pass in -m for Mac)

pro: Pseudo assembly is more concise and simpler to work with. The representation is also more user friendly when it comes to display.
Pro: Another pro could be that the IR-virtual might be good for optimization levels which could lead to an efficient code.
Con: A con would be trying to map and point to what each thing does as it takes time and it could be wrong. 
Con: Translating ir-virtual to machine code introduces an additional step in the compilation process, which may incur overhead.

[ Question 2 ] 

For this task, you will write three new .ifa programs. Your programs
must be correct, in the sense that they are valid. There are a set of
starter programs in the test-programs directory now. Your job is to
create three new `.ifa` programs and compile and run each of them. It
is very much possible that the compiler will be broken: part of your
exercise is identifying if you can find any possible bugs in the
compiler.

For each of your exercises, write here what the input and output was
from the compiler. Read through each of the phases, by passing in the
`-v` flag to the compiler. For at least one of the programs, explain
carefully the relevance of each of the intermediate representations.

For this question, please add your `.ifa` programs either (a) here or
(b) to the repo and write where they are in this file.

test0.ifa
input: 
(let* ([x 4]      
    [y 2]       
    [z (* x (+ x y))])  
(print z))

output: 24

test1.ifa
input: (print (* (- 3 1) 5))
Output: 10

test2.ifa
input: 
(let* ([x 5]       
    [y 10]       
    [z (+ x y)])  
    (print z))

Output: 15
Input source tree in IfArith:
This is the original source program written in IfArith. It's a high-level representation of the program's structure, including variable bindings and expressions.
ifarith-tiny:
This is a simplified version of the source tree. It introduces let expressions and reduces nesting by bringing all variable bindings to the outermost level.It's still in a high-level language format at this stage.
anf:
This representation simplifies the program further, preparing it for translation into a lower-level representation.
ir-virtual:
This representation is closer to the machine level. It's a sequence of instructions for a virtual machine.It's designed to be easily translated into actual machine code for execution on a specific platform.
x86:
This is the final stage of compilation, where the program is translated into assembly language specific to the x86 architecture.The assembly code is then assembled into an object file and linked to produce an executable.The resulting executable can be run to execute the original program.

[ Question 3 ] 

Describe each of the passes of the compiler in a slight degree of
detail, using specific examples to discuss what each pass does. The
compiler is designed in series of layers, with each higher-level IR
desugaring to a lower-level IR until ultimately arriving at x86-64
assembler. Do you think there are any redundant passes? Do you think
there could be more?

In answering this question, you must use specific examples that you
got from running the compiler and generating an output.

Pass 1: From IfArith to IfArith-Tiny
This pass simplifies the high-level IfArith language by desugaring complex syntactic constructs into simpler ones. It aims to reduce the complexity of later stages by dealing with high-level language constructs early.

Pass 2: From IfArith-Tiny to Administrative Normal Form (ANF)
The transformation to ANF simplifies complex expressions by ensuring that every intermediate result is stored in a variable. This form is closer to how machine code operates and simplifies the code generation process.

Pass 3: From ANF to IR-Virtual
This pass translates the ANF, which still operates at a high level, into a form where operations are expressed in terms of virtual instructions that resemble real assembly instructions but use an unlimited number of virtual registers.

Pass 4: From IR-Virtual to x86 Assembly
The final transformation where the virtual instructions are mapped to actual x86 instructions. This involves resolving virtual registers to physical ones and managing the machine-specific details such as calling conventions and stack management.

In terms of redundancy, each pass in this pipeline serves a distinct purpose, progressively lowering the level of abstraction of the code. No single pass appears redundant because each simplifies a different aspect of the transition from high-level language to machine code. However, potential additional passes could include an optimization pass between any of the described transformations, optimization passes could improve performance or reduce the generated code's size.


[ Question 4 ] 

This is a larger project, compared to our previous projects. This
project uses a large combination of idioms: tail recursion, folds,
etc.. Discuss a few programming idioms that you can identify in the
project that we discussed in class this semester. There is no specific
definition of what an idiom is: think carefully about whether you see
any pattern in this code that resonates with you from earlier in the
semester.

- The match cases, where we have been heavily using match and quasiquotes since project 3 and moving onto this project.
- This project also entails a lot of recursion, more specifically tail recursion.
- A combination of folds and recursion is used as well..
- Error handling in the match cases. 
- These are some of the idioms that we were able to point out.


[ Question 5 ] 

In this question, you will play the role of bug finder. I would like
you to be creative, adversarial, and exploratory. Spend an hour or two
looking throughout the code and try to break it. Try to see if you can
identify a buggy program: a program that should work, but does
not. This could either be that the compiler crashes, or it could be
that it produces code which will not assemble. Last, even if the code
assembles and links, its behavior could be incorrect.

To answer this question, I want you to summarize your discussion,
experiences, and findings by adversarily breaking the compiler. If
there is something you think should work (but does not), feel free to
ask me.

Your team will receive a small bonus for being the first team to
report a unique bug (unique determined by me).

A potential error(s):
1- The imul function being commented out, the task of this function is to move the mul into the rax. However, that is not happening due to it being commented out. 
;; mul needs to go in rax
      #;
      [`(imul ,dst ,src)
       `((mov "rdi" ,(format "[rbp~a]" (hash-ref reg->stackpos src)))
         (mov "rax" ,(format "[rbp~a]" (hash-ref reg->stackpos dst)))
         (imul "rdi")
         (mov "rax" ,(format "[rbp~a]" (hash-ref reg->stackpos dst))))]

2 - hash-ref: no value found for key
  key: '>

[ High Level Reflection ] 

In roughly 100-500 words, write a summary of your findings in working
on this project: what did you learn, what did you find interesting,
what did you find challenging? As you progress in your career, it will
be increasingly important to have technical conversations about the
nuts and bolts of code, try to use this experience as a way to think
about how you would approach doing group code critique. What would you
do differently next time, what did you learn?

The team met on Saturday and Sunday at approximately 6:00 P.M and started debriefing on how to approach the project. First, we forked and cloned the repository and started reading the ReadMe to understand the objective that we needed to accomplish.
Step 1: The team started implementing the first version of the let function where it would account for 0 bindings, it didn’t take much time to finish implementing this part.
Step 2: After implementing the first case, accounting for the 0 bindings, the team proceeded with the implementation of 1+/- binding.  After encountering some struggles with the implementation and trying to match certain cases, the team was able to finally complete the implementation after reviewing some of the Professor’s code and succeeded with the test cases. 
After analyzing  the code, some interesting findings were how the code slowly progresses through the different stages of compilation starting from high level and then goes to low-level representation. Ultimately, we find it interesting that the code is able to transform high level code into executable computer instructions.
The challenging part of this project was: getting the team to meet at a certain time all together, figuring out the format of the answers and how we would like to have them worded, and setting up in general. On the technical side, some of the challenging things were figuring out how to test the code and how to know if the tests were successful. We ultimately figured out the solutions on how to tackle these challenges and completed the project.