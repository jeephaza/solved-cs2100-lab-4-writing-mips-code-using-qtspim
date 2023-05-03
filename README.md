Download Link: https://assignmentchef.com/product/solved-cs2100-lab-4-writing-mips-code-using-qtspim
<br>
In this lab, you will use <strong>QtSpim </strong>to understand how typical programs are written. This document and its associated files (<strong>messages.asm</strong> and <strong>arrayCount.asm</strong>) is provided in the ZIP file and can also be downloaded from the CS2100 module website.

<h2>Reading and Writing Message to Console Window: messages.asm</h2>

Recall that in Lab #3 <strong>sample2.asm</strong>, you made use of the system call (<strong><sub>syscall</sub></strong>) to print some text. SPIM provides a small set of operating-system-like services through the system call

(<strong><sub>syscall</sub></strong>) instruction (see Appendix A, pages A-43 to A-45).




To request a service, a program loads the system call code into register <strong>$v0</strong> and arguments into registers <strong>$a0</strong> – <strong>$a3</strong> (see Figure A.9.1 below).  System calls that return values put their results in register <strong>$v0</strong>. For this lab, we are interested in only the following system calls: <strong>print_string</strong>, <strong>print_int</strong>, <strong>read_int</strong> and <strong>exit</strong>.







For example, the following code in <strong>messages</strong><strong><sub>.asm</sub></strong> prints “<strong>the answer = 5</strong>”.

<table width="520">

 <tbody>

  <tr>

   <td width="520"><strong># messages.asm </strong><strong>  .data  </strong><strong>str: .asciiz “the answer = ” </strong><strong>  .text  main:      li   $v0, 4    </strong><strong># system call code for print_string</strong><strong>     la   $a0, str  </strong><strong># address of string to print </strong><strong>    syscall        </strong><strong># print the string</strong><strong> </strong><strong>    li   $v0, 1    </strong><strong># system call code for print_int</strong><strong>     li   $a0, 5    </strong><strong># integer to print</strong><strong>     syscall        </strong><strong># print the integer</strong><strong> </strong><strong>    li   $v0, 10   </strong><strong># system call code for exit</strong><strong>     syscall        </strong><strong># terminate program</strong></td>

  </tr>

 </tbody>

</table>




The <strong>print_string</strong> system call (system call code 4) is passed a pointer (memory address) to a null-terminated string, which it writes to the console. The <strong>print_int</strong> system call (system call code 1) is passed an integer and it prints the integer on the console. The <strong>exit</strong> system call (system call code 10) indicates the end of the program.

The <strong>li</strong> (load immediate) and <strong>la</strong> (load address) are pseudo-instructions (refer to Lab #3).

Run the above program to verify your understanding.




<h2>Task 1: Modify messages.asm</h2>

Modify <strong>messages.asm</strong> and call the new program <strong>task1.asm</strong>. The modified program should read the value to be printed from the console before printing the value. The system call <strong>read_int</strong> reads an entire line of input up to and including the newline. Characters following the number are ignored. Note that <strong>read_int</strong> modifies the register <strong>$v0</strong> (where you put the code for system call) as it returns the integer value in register <strong>$v0</strong>.




The following screen capture shows a run of the program. The first line is your input, and the second line is the output of your program.




Demonstrate your new program <strong>task1.asm</strong> to your lab TA.

<h2>Task 2: Getting Real (arrayCount.asm)</h2>

When we discuss MIPS code in the lecture, it is common to see the “variable mappings” list. The list indicates how certain program variables are “mapped” to their respective registers. In this task, we are going to actually perform these mappings.

First, let us learn about allocating memory space for variables in a program. The assembler directive “<strong>.data</strong>” allows us to reserve memory space in the data segment. These reserved locations are used to store the values of various program variables during program execution.

<strong>Key idea: </strong>Values of program variables are stored in the memory. We load them into registers (perform a mapping) only when we want to manipulate or access them during execution.

<strong> </strong>

This is because register is a fast storage <strong>in the processor</strong>, while memory is a much slower storage <strong>outside the processor</strong>. As the access speed is not simulated in the QtSpim, the separation and mapping between memory and register may seem strange to you. In real processor, the difference in access speed of register versus memory can be much more than 10 times!

Let us modify the “count zero element” example from Lecture #10 (Section: Array and Loop) for this task. For simplicity, let us reduce the array size to <strong>8</strong>. The problem statement now reads:

<em>Count the number of <strong>multiples of X</strong> in a given array of <strong>8 </strong>non-negative numbers</em>, where <strong><em>X</em></strong> is a user chosen power-of-two value, e.g. 1, 2, 4, 8, ….




Use <strong>arrayCount.asm</strong> in the zip file or download it from the module website. The initial content of the file is:




<table width="573">

 <tbody>

  <tr>

   <td width="573"><strong># arrayCount.asm </strong><strong>        .data  </strong><strong>arrayA: .word 1, 0, 2, 0, 3   </strong><strong># arrayA has 5 values</strong> <strong>count:  .word 999       </strong><strong># dummy value</strong><strong> </strong><strong>        .text main: </strong><strong>     </strong><strong># code to setup the variable mappings</strong><strong>     add $zero, $zero, $zero </strong><strong># dummy instructions, can be removed</strong><strong>     ………… </strong><strong>     </strong><strong># code for reading in the user value X</strong><strong>     </strong><strong># code for counting multiples of X in arrayA </strong><strong> </strong><strong>     </strong><strong># code for printing result</strong><strong>     </strong><strong># code for terminating the program </strong><strong>     li  $v0, 10      syscall </strong></td>

  </tr>

 </tbody>

</table>




The main routine contains several dummy instructions (instructions with no real effect) so that you can step through the program to observe the content in the data segment.







Where is the array <strong>arrayA</strong> located in the data segment? Give the base address (starting address) of the array:




<strong>arrayA</strong> is at 0x<strong>______________________</strong>




Where is the program variable <strong>count</strong> in the data segment?




<h2>count is at 0x_______________________</h2>




(<em>Hint:</em> Don’t forget that 999 is in decimal…)




The given code only allocates 5 elements for <strong>arrayA</strong>. Enlarge the array to size 8. You can place any valid integer values for the new locations. Fill in the assembler directive below:




<strong>arrayA:</strong>  ____________________________________







Now, let us perform the following mappings:




Base address of <strong>arrayA</strong> ➔ <strong>$t0</strong> (similar to notation used in lectures)  <strong>count</strong>   ➔ <strong>$t8 </strong>

<strong> </strong>

You may use the “<strong>la</strong>” (load address) instruction here to help. Give the instruction sequence (which may consist of 1 or 2 instructions) below:




To map <strong>arrayA</strong>:        _____________________

_____________________




To map <strong>count</strong> :  _____________________

_____________________




We are almost ready to tackle the task. One last obstacle is to figure out how to check for “Multiples of X, where X is a power-of-two”. Recall that in <strong>Tutorial #2 Q3a</strong>, we learned that “<strong>andi</strong>” instruction can be used to find the remainder of division by 16. For the following questions, give the correct <strong>mask</strong> for the <strong>andi</strong> instruction to compute “<strong>$t4 = $t3 % X</strong>”.




If X is <strong>32</strong>,  <strong>andi $t4, $t3, 0x</strong>______




If X is <strong>8</strong>,    <strong>andi $t4, $t3,</strong> <strong>0x</strong>______




Observe that we can easily generate the mask from X. If X is stored in register <strong>$t8</strong>, complete the following instruction to generate the mask in register <strong>$t5</strong>. (<em>Hint:</em> look at the mask as a <strong>number</strong>).




______ <strong>$t5, $t8,</strong> ______ (fill in the operation and the last operand)




We are now ready to finish off the task. Write the necessary code to:

<ol>

 <li>Read user input value, <strong>X</strong>. You can assume <strong>X</strong> is always a power-of-two integer, i.e. there is no need to check for invalid user input.</li>

 <li>Count the number of multiples of <strong>X</strong> in <strong>arrayA</strong> <strong>and print the result on screen</strong>.</li>

</ol>




You should use loop wherever appropriate, or full credit will not be given. Sample code can be found in Lecture #8 MIPS Part 2 (slides 32-34). Here’s a sample output screenshot for a predefined array <strong>{11, 0, 31, 22, 9, 17, 6, 9} </strong>and user input of<strong> X = 2</strong>.  The output is 3 as there are three multiples of 2 in the array: 0, 22 and 6.




Try to use different values in your code to test. Also, please make sure the “<strong>count</strong>” value is properly recorded <strong>in the data segment at the end of execution</strong>.




<h3>Task 3: Making it “real-er” (inputArrayCount.asm)</h3>

This is a follow-up task on task 2. First, make a copy of your solution in task 2 and name it

“<strong>inputArrayCount.asm</strong>“.

Your task is very “simple” – add code to read <strong>8 values </strong>from the user and store them in the array <strong>arrayA</strong>. Then print the number of multiples of X found (where X is a power-of-two also entered by the user). By reusing your code in task 2, you only need to add a couple of new instructions. Below is a sample run:

Please note that we read the array values before the user enters the value X. Your labTA will test your program with some test cases.