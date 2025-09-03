## Concurrency-vs parallelism

Concarency - is parallel execution of tasks, each core executes a separate task.

Paralelism - execution of one task by many threads simultaneously. 

## Java virtual threads

Virtual threads are different from original platform threads, they are much more lightweight in terms of RAM, virtual threads are running on platform threads.

![java-virtual-threads-1.png](java-virtual-threads-1.png)

Virtual threads are pinned to the platform thread until the virtual thread makes a blocking network call, in which case the virtual thread is unmounted from the platform thread.

If virtual thread makes a blocking file system call it is not unmounted from the platform thread, virtual thread remain pinned to the platform thread.

There are other situations that can pin virtual thread to platform thread, For instance, entering synchronized block.

### Creating java virtual threads

```
Thread vThread = Thread.ofVirtual().start(runnable); 
```
or
```
Thread vThreadUnstarted = Thread.ofVirtual().unstarted(runnable);
vThreadUnstarted.start();
```

You can join virtual threads just like platform thread.

## Race conditions and critical sections

Critical section - section od code that is executed by multiple threads and the sequence of execution for the threads makes the difference in the result.

Race condition - concurrency problem that may occur in a critical section 

1. Raed-modify-write race condition - two threads read value (to CPU registers) then modify the value (in the CPU registers) and then write the values back.
2. Check-then-act race condition  - two threads read check given value of map, see that value is present and then both threads try to remove the value.

Atomic instruction - once single thread is execution instruction no other thread can execute it.

Race condition can be avoided by using synchronized block of code, locks and atomic variables.

## Thread safety and shared resources

Code that is save to call by multiple threads simultaneously is called thread safe.

1. Local variables - are stored in each thread own stack, they are never shared between threads.
2. Local object references - reference itself is not shared but the object referenced is not stored in each threads local stack, it5 is stored int shared heap.
```
public void someMethod(){

LocalObject localObject = new LocalObject();

localObject.callMethod();
method2(localObject);
}

public void method2(LocalObject localObject){
localObject.setValue("value");
}
```

The localObject reference i this example is not returned from method and is not passed to other objects that are accessible outside of someMethod() 

3. Object member values (fields) - object fields are stored on the heap along with the object. If two threads call method on the same object instance and this method updates object member variables, the method is not thread safe.
    Example :
```
   public class NotThreadSafe{
    StringBuilder builder = new StringBuilder();

    public add(String text){
        this.builder.append(text);
    }
```

### The thread control escape rule 

If a resource is created, used and disposed within
the control of the same thread,
and never escapes the control of this thread,
the use of that resource is thread safe.

Race conditions occure when at list one of threads writes to the resource, if all thread read race condition newer occurs.

### Thread safety and immutability

We can make sure that objects that are shared between threads are thread safe by making them immutable.

## Synchronized keyword 

Only one thread can execute synchronized block of code at a time.

Synchronized keyword can be used on

1. Instance methods 
2. Static methods
3. Code block inside instance method
4. Code block inside static method
5. Inside Lambda expression



