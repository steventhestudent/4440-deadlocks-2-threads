# 4440-deadlocks-example-2-threads

Deadlock example where two threads are waiting on each other to release semaphores:
```
 import java.util.concurrent.Semaphore;

 public class DeadlockExample {
     static Semaphore semaphore1 = new Semaphore(1);
     static Semaphore semaphore2 = new Semaphore(1);

     public static void main(String[] args) {
         Thread p1 = new Thread(() -> {
             try {
                 semaphore1.acquire();
                 System.out.println("Thread 1 acquired semaphore 1");
                 Thread.sleep(100); // Simulate work
                 semaphore2.acquire();
                 System.out.println("Thread 1 acquired semaphore 2");
             } catch (InterruptedException e) {
                 e.printStackTrace();
             } finally {
                 semaphore1.release();
                 semaphore2.release();
             }
         });

         Thread p2 = new Thread(() -> {
             try {
                 semaphore2.acquire();
                 System.out.println("Thread 2 acquired semaphore 2");
                 Thread.sleep(100); // Simulate work
                 semaphore1.acquire();
                 System.out.println("Thread 2 acquired semaphore 1");
             } catch (InterruptedException e) {
                 e.printStackTrace();
             } finally {
                 semaphore1.release();
                 semaphore2.release();
             }
         });

         p1.start();
         p2.start();
     }
 }
```

In this example, t1 acquires semaphore1 and then tries to acquire semaphore2,
while t2 does the opposite, leading to a deadlock.
