# Capy Dispatch Queue is a multithreaded task dispatcher of queues

The lib is developed to optimize application support for systems with multi-core processors and other symmetric multiprocessing systems.
It is an implementation of task parallelism based on the thread pool pattern. The fundamental idea is to move the management 
of the thread pool out of the hands of the developer, and closer to the operating system. The developer injects "work tasks" into the pool oblivious of the pool's architecture. 
This model improves simplicity, portability and performance.

## Build 

    $ git clone https://github.com/aithea/capy-dispatchq
    $ cd capy-dispatchq; mkdir build; cd build; cmake ..; make -j4

---

## Code example

```c++
    auto q99 = capy::dispatchq::Queue(99);
    auto q10 = capy::dispatchq::Queue(10);
    
    int w = 10;
    for (int j = 0; j < w ; ++j) {
      q10.async([=,&q10]{
           //
           // handle task
           //
          std::this_thread::sleep_for(std::chrono::seconds(1));
      });
    }
    
    
    for (int j = 0; j < 100 ; ++j) {
      capy::dispatchq::main::async([=]{
    
        //
        // handle tasks async in default queue 
        //           
          
        std::this_thread::sleep_for(std::chrono::milliseconds(100));
    
        if (j>20) {
            //
            // Stop default loop and join to main thread
            //
            capy::dispatchq::main::loop::exit();
        }  
      });
    }
    
    //
    // Run default loop
    //      
    capy::dispatchq::main::loop::run();
    
    //
    // Wait tasks after default loop exiting
    //
    q99.wait(); 
 ```
