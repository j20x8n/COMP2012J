java cOperating Systems
Assignment 02: Memory Management
COMP2012J
2024-25
1 Memory Management Simulator
Please ﬁnd the memory management source ﬁles from the moodle. This simulator illustrates page fault behaviour
in a paged virtual memory system. The program reads the initial state of the page table and a sequence of
virtual memory instructions and writes a trace log indicating the eﬀect of each instruction.
To make things easier for you, we have implemented the FIFO page replacement algorithm already. Please
go through the implementation carefully so that you can work out how to write your own page replacement
algorithms. Please go through the instructions carefully and complete the assignment.
2 Running the simulator
• Compile the java code using the following command.
1 $ javac *. java
• The program reads a command ﬁle, conﬁguration ﬁle, and writes a trace ﬁle.
• You can run the program by running the following command.
1 $ java MemoryManagement commands memory . conf
• ‘commands’ refers to the ﬁle where we state the command sequence we need to run on the system.
• ‘Memory.conf’ ﬁle has the initial conﬁguration of the system.(i.e: ’memory FIFO.conf’)
2.1 The command ﬁle
The command ﬁle for the simulator speciﬁes a sequence of memory instructions to be performed. Each instruction
 is either a memory READ or WRITE operation, and includes a virtual memory address to be read or
written. Depending on whether the virtual page for the address is present in physical memory, the operation
will succeed, or, if not, a page fault will occur.
2.1.1 Operations on Virtual Memory
There are two operations one can carry out on pages in memory: READ and WRITE. The format for each
command is,
1 operation address
Or
1 operation random
where the operation is READ or WRITE, and the address is the numeric virtual memory address, optionally
preceded by one of the radix keywords bin, oct, or hex. If no radix is supplied, the number is assumed to be
decimal.
The keyword random will generate a random virtual memory address (for those who want to experiment quickly)
rather than having to type an address.
For example, the sequence,
1 READ bin 01010101
2 WRITE bin 10101010
3 READ random
4 WRITE random
causes the virtual memory manager to:
 University College Dublin 1Operating Systems
Assignment 02: Memory Management
COMP2012J
2024-25
1. Read from virtual memory address 85
2. Write to virtual memory address 170
3. Read from some random virtual memory address
4. Write to some random virtual memory address
2.2 The Conﬁguration File
The conﬁguration ﬁle memory.conf is used to specify the initial content of the virtual memory map (which pages
of virtual memory are mapped to which pages in physical memory) and provide other conﬁguration information,
such as whether the operation should be logged to a ﬁle.
2.2.1 Setting Up the Virtual Memory Map
The ’memset’ command is used to initialize each entry in the virtual page map. ’memset’ is followed by six
integer values:
1. The virtual page number to initialize
2. The physical page number associated with this virtual page (-1 if no page assigned)
3. If the page has been read from (R) (0=no, 1=yes)
4. If the page has been modiﬁed (M) (0=no, 1=yes)
5. The amount of time the page has been in memory (in ns)
6. The last time the page has been modiﬁed (in ns)
The ﬁrst two parameters deﬁne the mapping between the virtual page and a physical page if any. The last four
parameters are values that might be used by a page replacement algorithm.
For example:
1 memset 34 23 0 0 0 0
speciﬁes that virtual page 34 maps to physical page 23, and that the page has not been read or modiﬁed.
Note:
• Each physical page should be mapped to exactly one virtual page.
• The default number of virtual pages is 64 (0..63).
• The number of physical pages cannot exceed 64 (0..63).
• If a virtual page is not speciﬁed by any ’memset’ command, it is assumed that the page is not mapped.
• ’memset’ commands must be de代 写COMP2012J、java
代做程序编程语言ﬁned at the end of the conﬁguration ﬁle.
2.2.2 Other Conﬁguration File Options
There are several other options which can be speciﬁed in the conﬁguration ﬁle. These are summarized in the
table below.
University College Dublin 2Operating Systems
Assignment 02: Memory Management
COMP2012J
2024-25
Keyword Values Description
enable logging
true
false
Whether logging of the operations should be enabled. If logging is
enabled, then the program writes a one-line message for each READ
or WRITE operation. By default, no logging is enabled. See also the
’log ﬁle’ option.
log ﬁle trace-ﬁle-name
The name of the ﬁle to which log messages should be written. If no
ﬁlename is given, then log messages are written to stdout. This option
has no eﬀect if ’enable logging’ is false or not speciﬁed.
pagesize
n
power p
The size of the page in bytes. This can be given as
a decimal number which is a power of two (1, 2, 4, 8, etc.) or as a
power of two using the power keyword. The maximum page size is
67108864 or power 26. The default page size is power 26.
addressradix n
The radix in which numerical values are displayed. The default radix is
2 (binary). You may prefer radix 8 (octal), 10 (decimal), or 16
(hexadecimal).
physicalMemSize n The size of the physical memory as a measurement of the number of pages.
replacementAlgorithm FIFO |LRU |Clock policy The page replacement algorithm to use in the simulator.
2.3 The Output File
The output ﬁle contains a log of the operations since the simulation started. It lists the command that was
attempted and what happened as a result. You can review this ﬁle after executing the simulation.
The output ﬁle contains one line per operation executed. The format of each line is:
1 command address ... status
Where:
• command is READ or WRITE.
• address is a number corresponding to a virtual memory address.
• status is okay or page fault.
Example:
1 READ 10000000 ... okay
2 READ 10000000 ... okay
3 WRITE c0001000 ... page fault
3 Assignment
3.1 Task 1
• Read and understand the simulator and the implementation of the FIFO algorithm.
• Run the program with the ’commands’ ﬁle and the ‘memory_FIFO.conf’ ﬁle.
• Identify how the FIFO algorithm works.
3.2 Task 2
• Implement Least recently used(LRU) page replacement algorithm in the ‘PageFault.java’ and call it
within the ‘replacePage()’ method.
• Use the ‘tracefile_LRU’ as a reference for what your output should look like when you run the program
with the ‘commands’ ﬁle and the ‘memory_LRU.conf’ ﬁle.
University College Dublin 3Operating Systems
Assignment 02: Memory Management
COMP2012J
2024-25
3.3 Task 3
• Implement the Clock-policy page replacement algorithm in the ‘PageFault.java’ and call it within the
‘replacePage()’ method.
• Use the ‘tracefile_clock’ as a reference for what your output should look like when you run the program
with the ‘commands’ ﬁle and the ‘memory_clock.conf’ ﬁle.
4 Submission
• You do not have to worry about the input type of the addresses while implementing page replacement
algorithms since all the addresses are converted and saved as decimal numbers by the kernel.
• Submit the ‘PageFault.java’ ﬁle to the submission link in the moodle before the deadline.
• Please keep the code clean and add comments. There will be marks for the code quality and comments.
Your submission will be tested against inputs that we have designed.
• Do NOT change any source ﬁle other than the ’PageFault.java’.
• Do NOT change the function interfaces of any functions in the ’PageFault.java’. Any change will result
in your code failing the tests.
• If you need more static variables for your implementation you can deﬁne them without changing other
data structures inside the ’PageFault.java’.
• Do NOT output anything other than what has been asked for. If you have added any outputs for your
convenience, you should remove/comment them before submission.
University College Dublin 4

         
加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com
