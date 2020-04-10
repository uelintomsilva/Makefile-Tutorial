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

Now in your terminal type ```make```. In your folder the file ```main and main.o``` show up.
```console
vitor@DESKTOP-2KPPLO8:~/test$ ls
main.cpp  Makefile
vitor@DESKTOP-2KPPLO8:~/test$ make
g++ -g -c -Wall main.cpp
g++ -g main.o -o main
vitor@DESKTOP-2KPPLO8:~/test$ ls
main  main.cpp  main.o  Makefile
```
You can run your code
```console
vitor@DESKTOP-2KPPLO8:~/test$ ./main 
Hello Makefile
```
Using ```make clean``` cmd we remove additional files in folder
```console
vitor@DESKTOP-2KPPLO8:~/test$ make clean
rm -f main.o main
vitor@DESKTOP-2KPPLO8:~/test$ ls
main.cpp  Makefile
```

## Building a more "complex" makefile

&emsp;This time we will need create the files ```main.cpp``` ```Employee.cpp``` ```Person.cpp``` ```Student.cpp``` source codes and ```Employee.h``` ```Person.h``` ```Student.h``` header files.<br />

**main.cpp**
```c++
#include <iostream>
#include <string>

#include "Person.h"
#include "Student.h"
#include "Employee.h"


int main(){

    Person monica("monica");
    monica.print();

    Student demi("Demi", "MIT");
    demi.print();

    Employee charles("Charles", "Microsoft");
    charles.print();

    
}
```

**Person.cpp**
```c++
#include <string>
#include <iostream>
#include "Person.h"

Person::Person(std::string name) : m_name(name){}

void Person::print(){
    std::cout << "Person " << m_name << std::endl;
}
```

**Student.cpp**
```c++
#include <string>
#include <iostream>

#include "Person.h"
#include "Student.h"

Student::Student(std::string name, std::string university) : Person(name), m_university(university){}

void Student::print(){
    Person::print();
    std::cout << "University " << m_university << std::endl;
}
```
**Employee.cpp**
```c++
#include <string>
#include <iostream>

#include "Person.h"
#include "Employee.h"

Employee::Employee(std::string name, std::string company) : Person(name), m_company(company){}

void Employee::print(){
    Person::print();
    std::cout << "Company " << m_company << std::endl;
}
```
**Student.h**
```c++
class Student : public Person{

    public:
        Student(std::string name, std::string univerty);
        void print();

    private:
        std::string m_university;
};
```
**Person.h**
```c++
class Person{
    public:
        Person(std::string name);
        virtual void print();

    private:
        std::string m_name;
};
```
**Employee.h**
```c++
class Employee : public Person{
    public:
        Employee(std::string name, std::string company);
        void print();

    private:
        std::string m_company;
};
```
**Makefile**
```bash
OBJS	= main.o Employee.o Person.o Student.o
SOURCE	= main.cpp Employee.cpp Person.cpp Student.cpp
HEADER	= Employee.h Person.h Student.h
OUT	= person
CC	 = g++
FLAGS	 = -g -c -Wall
LFLAGS	 = 

all: $(OBJS)
	$(CC) -g $(OBJS) -o $(OUT) $(LFLAGS) && rm -f $(OBJS) && ./$(OUT)

main.o: main.cpp
	$(CC) $(FLAGS) main.cpp

Employee.o: Employee.cpp
	$(CC) $(FLAGS) Employee.cpp

Student.o: Student.cpp
	$(CC) $(FLAGS) Student.cpp

Person.o: Person.cpp
	$(CC) $(FLAGS) Person.cpp


clean:
	rm -f $(OBJS) $(OUT)
```

Running Makefile
```console
vitor@DESKTOP-2KPPLO8:~/cplusplus$ ls
car.cpp  Employee.cpp  main.cpp  Person.cpp  Student.cpp
car.h    Employee.h    Makefile  Person.h    Student.h
vitor@DESKTOP-2KPPLO8:~/cplusplus$ make
g++ -g -c -Wall main.cpp
g++ -g -c -Wall Employee.cpp
g++ -g -c -Wall Person.cpp
g++ -g -c -Wall Student.cpp
g++ -g main.o Employee.o Person.o Student.o -o person  && rm -f main.o Employee.o Person.o Student.o && ./person
Person monica
Person Demi
University MIT
Person Charles
Company Microsoft
vitor@DESKTOP-2KPPLO8:~/cplusplus$ ls
car.cpp  Employee.cpp  main.cpp  person      Person.h     Student.h
car.h    Employee.h    Makefile  Person.cpp  Student.cpp
vitor@DESKTOP-2KPPLO8:~/cplusplus$ make clean
rm -f main.o Employee.o Person.o Student.o person
vitor@DESKTOP-2KPPLO8:~/cplusplus$ ls
car.cpp  Employee.cpp  main.cpp  Person.cpp  Student.cpp
car.h    Employee.h    Makefile  Person.h    Student.h
vitor@DESKTOP-2KPPLO8:~/cplusplus$
```

## Appendix

<dl>
  <dt> What is object files?
    <dd>&emsp;A C++ object file is an intermediate file produced by a C++ compiler from a C++ implementation file and the C++ header files that the implementation file includes. The C++ linker produces the output executable or library of your project from your C++ object files.
    </dd>
  </dt>
  <dt> Command MAKE workflow
    <dd><br />
      &emsp;When make recompiles the editor, each changed C source file must be recompiled. If a header file has changed, each C++ source file that includes the header file must be recompiled to be safe. Each compilation produces an object file corresponding to the source file. Finally, if any source file has been recompiled, all the object files, whether newly made or saved from previous compilations, must be linked together to produce the new executable editor.
    </dd>
  </dt>
  <dt> Compiler flags
    <dd><br />
	&emsp;g++ [-c|-S|-E] [-std=standard] [-g] [-pg] [-Olevel] [-Wwarn...] [-pedantic] [-Idir...] [-Ldir...] [-Dmacro[=defn]...] [-Umacro] [-foption...] [-mmachine-option...] [-o outfile] [@file] infile... 
    </dd>
  </dt>
</dl>
