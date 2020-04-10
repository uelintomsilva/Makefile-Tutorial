# Makefile-Tutorial
Simple makefile tutorial for beginners.<br />

Build on:
- Ubuntu 16.04
- g++ 5.4.0
- c++ -std=11/-std=14/-std=17
- GNU make 4.1
  
(if ```make``` is not installed type in terminal: ```sudo apt-get install build-essential```)


## Understanding how a makefile works

 &emsp;A makefile is useful because (if properly defined) allows recompiling only what is needed when you make a change. In a large project rebuilding the program can take some serious time because there will be many files to be compiled and linked and there will be documentation, tests, examples etc.<br />
Rather than use ```g++ file file file file ... (20x) file -o output``` we can just run a ```make``` and compile our source code.<br />

Makefile text is constituted of three major tiles: 
```bash
<target> : <prerequisites> 
    <recipes>
```
**Target** normally is the name of object files or actions like make/clean.<br />
**Prerequisite** can exist or not. If exist is the name of input(s) file(s) to generate object file.<br />
**Recipes** is the command option for bash execution. Can be multiple commands using &&.

**Caracteristcs of symbols:**<br />
- Lines beginning with a hash (#) are comments and are ignored.
- Variables are in ALL_CAPS followed by a = sign. Its best practice to place all variables at the top of the file.
- To use variable later in the file use $(VARIABLE_NAME) or ${VARIABLE NAME}. The parens/brackets are mandatory.
- Starting the recipe with @ will not print the command on terminal.<br /><br />

**Let's build a simple c++ code with a basic Makefile**<br /><br />
First of all create main.cpp and Makefile (without extension) in same folder:<br /><br />
**main.cpp**
```c++
#include <iostream>

int main(){
  std::cout << "Hello Makefile" << std::endl;
  }
```
<br />
<br />

**Makefile**
```bash

SOURCE = main.cpp
HEADER =
OBJS = main.o
OUT = main
CC = g++
FLAGS = -g -c -Wall
LFLAGS = 

all: $(OBJS)
	$(CC) -g $(OBJS) -o $(OUT) $(LFLAGS)
  
main.o: main.cpp
	$(CC) $(FLAGS) main.cpp
  
clean:
	rm -f $(OBJS) $(OUT)
  
```

<br />Explaning MACROS in above code:<br />

**SOURCE**: .cpp files to compile<br />
**HEADER**: .h/.hpp files used in #include<br />
**OBJS**: .o Intermediate files on compiler process<br />
**OUT**: name of output file<br />
**CC**: compiler version in use<br />
**LFLAGS**: extra flags to lexical<br /><br /> 

Explain functional commands:<br />

**all**: when we run ```make``` this process will be call and execute what is in recipes<br />
**main.o**: is a process to compile main.cpp in main.o intermediate file<br />
**clean**: here we order to exclude intermediate files and output file. ```rm -f``` is a bash cmd to remove files.<br />

Now in your terminal type ```make```. In your folder the file ```main``` show up.

## Appendix

<dl>
  <dt> What is object files?
    <dd>
    </dd>
  </dt>
  <dt> Command MAKE workflow
    <dd><br />
      &emsp;When make recompiles the editor, each changed C source file must be recompiled. If a header file has changed, each C++ source file that includes the header file must be recompiled to be safe. Each compilation produces an object file corresponding to the source file. Finally, if any source file has been recompiled, all the object files, whether newly made or saved from previous compilations, must be linked together to produce the new executable editor.
    </dd>
  </dt>
  <dt> Compiler flags
    <dd>
    </dd>
  </dt>
</dl>
