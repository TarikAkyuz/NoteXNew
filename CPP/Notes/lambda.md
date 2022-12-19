There are two kinds of concurrent programming model:
1. Multiprocessing:
    - Each process has only one thread running and all the processes communicate to each other through regular inter process communication channel such as `Files, pipes, message queues...`
    - Pros: Fast to start, Low overhead
    - Cons: Diffucult to implement, Cant run on distributed system    
2. Multithreading:
    - One process contains two or more threads and all the threads communicate to each other through shared memory.
    - Pros: Fast to start, Low overhead, Better performance
    - Cons: Diffucult to implement, Cant run on distributed system

> Multi-threading: the threads should not communicate through shared memory they should use some kind of channels that is similar to inter process communication channels that way your program can be easily converted to a multi processing.


```c
#include <iostream>
#include <thread>

void function_1() {
        std::cout << "Hello world" << std::endl;
}

int main() {
        std::thread t1(function_1);
        //t1.join();    //Main thread waits for t1 to finish
        t1.detach();    //t1 will freely on its own -- daemon process
        //You can join or detach not both or
        if ( t1.joinable() ) t1.join();
        return 0;
}
```
#
#


> A thread object can be constructed not only with aregular function but also with any callable function such as functor or lambda function


> Instead of writing `join` in the `except`, make it a class and write it in the `destructor`.



```c
#include <iostream>
#include <thread>
#include <string>


class Fctor {
        public:
                void operator()(std::string& msg) {
                        std::cout << "from fctor:" << msg << std::endl;
                }
};

int main() {
        //Fctor fct;
        //std::thread t1(fct);
        std::string s = "Message from main";
        //If you dont use std::ref, everytime will be pass it over to the thread by value.
        //Memory sharing creates data waste problem.
        //std::move efficient and safe
        std::thread t1((Fctor()), std::move(s));

        std::cout << "from main" << std::endl;

        //see the thread id
        std::cout << "main thread id: " << std::this_thread::get_id() << std::endl;
        std::cout << "t1 thread id: "  << t1.get_id() << std::endl;
        t1.join();
        return 0;
}

```

#
#





