# What is the Spark Kernel?
A model of an operating system which uses a multithreaded approach to handle processes. This kernel 
can schedule processes using 4 different types of algorithms:<br/><br/><b>
• First Come First Serve<br/>
• Round Robin<br/>
• Preemptive Priority<br/>
• Shortest Job First<br/></b>

The aim, of embedding various scheduling algorithms in this operating system kernel, was to analyze 
which scheduling algorithm works better (keeping the conditions in mind). Spark Kernel uses C++ 
with a twist of POSIX Threads to make processing and execution fast and efficient.<br/>

# How does Spark Kernel work?
Spark kernel has an easy-to-use command-line interface. This is how you can use Spark Kernel 
command line interface to apply different scheduling techniques:<br/><br/><b>
### To compile the program simply use the command 
```diff
g++ os-kernel.cpp -o os-kernel
```
• First Come First Serve: ./os-kernel <input_file> <#cpus> f <output_file><br/>
• Round Robin: ./os-kernel <input_file> <#cpus> r <time_slice> <output_file><br/>
• Preemptive Priority: ./os-kernel <input_file> <#cpus> p <output_file><br/>
• Shortest Job First: ./os-kernel <input_file> <#cpus> s <output_file><br/><br/></b>

> cpus refers to number of cores

> time_slice is also known as time quantum that is assigned in Round Robin

> input_file (processes1.txt) contains the different processes that can be changed as per your will

Spark Kernel parses these command line arguments and takes the appropriate route from there. The 
input file should have the following format:<br/><br/>

<b>Process | Name | Priority | Arrival Time | Process Type | CPU Time | I/O Times</b><br/>

If you have a valid input file and use the command-line interface as instructed, you will be able to see 
a live Gannt Chart of the whole operating system model. After complete execution of all processes, 
you will receive the total executed time, total time spent in ready state and number of context 
switches. This complete output will also be written to the text file you provide at the time of 
execution. Here is a sneak peek:<br/>

![fcfs](https://user-images.githubusercontent.com/88616338/182545060-639ce6d2-0282-43d6-8098-bda5b7afbef7.jpg)
<br/>

# Program Structure
### os-kernel.cpp
```diff
int main(int, char*[]) //driver code
```
### os-kernel.h
```diff
void init_proc(vector<entry>&, string) //initializes text file input to pcb entries
void sortVec(vector<entry>&) //sorts all entries according to arrival time
```
### process.h
```diff
struct entry //stores pcb-related data of a process 
struct compareEntry: bool operator()(entry*, entry*) //operator overload for priority queue
struct compareCpu: bool operator()(entry*, entry*) //operator overload for priority queue
struct sortEntry:inline bool operator()(const entry&, const entry&) //operator overload for sorting
```
### scheduler.h
```diff
struct fcfs{ //structure for first come first serve scheduling
  &emsp;void context_switch() //adds to number of context switches
  &emsp;void yield(entry*) //sends process to I/O
  &emsp;void terminate(entry*) //terminates process after execution
  &emsp;void wakeup(entry*) //sends process back to CPU from I/O
  &emsp;void cpu_thread(int) //CPU thread
  &emsp;void io_thread(int) // I/O thread
  &emsp;string idle(entry*) //decides if a process is idle or not
  &emsp;void schedule(char*) //schedules all processes
  &emsp;fcfs(vector<entry>*, int, char*) //start function to initialize scheduler
}
struct round_robin{ //structure for round robin scheduling
  &emsp;void context_switch() //adds to number of context switches
  &emsp;void yield(entry*) //sends process to I/O
  &emsp;void terminate(entry*) //terminates process after execution
  &emsp;void preempt(entry*) force-fully takes CPU for another process
  &emsp;void wakeup(entry*) //sends process back to CPU from I/O
  &emsp;void cpu_thread(int) //CPU thread
  &emsp;void io_thread(int) // I/O thread
  &emsp;string idle(entry*) //decides if a process is idle or not
  &emsp;void schedule(char*) //schedules all processes
  &emsp;round_robin(vector<entry>*, int, int, char*) //start function to initialize scheduler
}
struct priority{ //structure for preemptive priority-based scheduling
  &emsp;void context_switch() //adds to number of context switches
  &emsp;void preempt() //force-fully takes CPU for another process
  &emsp;void cpu_thread(int) //CPU thread
  &emsp;void io_thread(int) // I/O thread
  &emsp;string idle(entry*) //decides if a process is idle or not
  &emsp;void schedule(char*) //schedules all processes
  &emsp;priority(vector<entry>*, int, char*) //start function to initialize scheduler
}
struct sjf{
  &emsp;void context_switch() //adds to number of context switches
  &emsp;void preempt() //force-fully takes CPU for another process
  &emsp;void cpu_thread(int) //CPU thread
  &emsp;void io_thread(int) // I/O thread
  &emsp;string idle(entry*) //decides if a process is idle or not
  &emsp;void schedule(char*) //schedules all processes
  &emsp;sjf(vector<entry>*, int, char*) //start function to initialize scheduler
}
```
### Color.hpp
An open-source GitHub library to bring color to your terminal.<br/>
Repository: https://github.com/hugorplobo/colors.hpp<br/>
Colors used for data visualization:<br/><br/><b>
• Red: IDLE<br/>
• Yellow: I/O Bound Process<br/>
• Green: CPU Bound Process</b><br/><br/>
The code provided is well-commented. If you have any confusions after reading the above structure, 
the commented code can be used as reference. 


# Conclusion
This project has helped us strengthen all our core concepts of the Linux operating system including
multiprocessing, multithreading, scheduling and synchronization. It has made us confident about 
being able to work on any operating systems programming task/project in the future. As our field is 
cyber security, the Linux operating system is very important for us to learn and this project helped us 
to do exactly that.
