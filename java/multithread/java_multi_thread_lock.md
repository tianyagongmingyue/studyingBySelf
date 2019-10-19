





## **Lock**

 

   java.util.concurrent.locks.Lock   表示的是一个锁，可以通过其中的lock方法获取锁，然后通过unlock释放锁。   

​	**lock**方法获取锁的方式类似于synchronized，如果调用lock方法无法获取到锁，则线程进行等待。   	

​	**lockInterruptibly**方法获取锁类似于lock方法，允许当前线程在等待锁的过程中被中断。   

​	**tryLock**方法以非阻塞的方式获取锁，如果在调用tryLock方法时无法获取锁，则返回false，不会阻塞当前线程。   

 

**注意：**不要将获取锁的过程写在try块里面，因为如果在获取锁(自定义锁的实现)时发生异常，异常抛出的同时，则会导致锁无故释放。

```java
@Test
public void lock(){
	Lock lock = new ReentrantLock();
	// 获取锁
	lock.lock(); // 放在try块外面
	try {

	} finally {
		// 释放锁
		lock.unlock();
	}
}
```



 

 

### **ReentrantLock**

 

   **ReentrantLock**重入锁，就是支持**重进入**的锁，它表示该锁能够支持一个线程对资源的重复加锁。除此之外，该锁还支持获取锁的公平和非公平性选择。       

​	**重进入**是指任意线程在获取到锁后能够再次获取该锁而不会被锁所阻塞。   

​		1）**线程再次获取锁。**锁需要去识别获取锁的线程是否为当前占据锁的线程，如果是，则再次获取成功。   

​		2）**锁的最终释放。**线程重复N次获取了锁，随后在第N次释放该锁后，其他线程能够获取到该锁。锁最终释放要求：锁对于获取   进行计数自增，计数表示当前锁被重复获取的次数，而锁释放时，则计数自减，当计数等于0时，表示锁已经成功释放。       

​	**公平性**与否是针对获取锁而言的，如果一个锁是公平的，那么锁的获取顺序就应该符合请求时间的绝对时间顺序，即FIFO。   

​	实现则是**ReentrantLock(boolean fair)**，fair=true,则表示该锁是公平性的。   

​	**公平性锁**保证了锁的获取按照了FIFO原则，而代价就是进行了大量的线程切换。

​	**非公平性锁**虽然可能造成线程“饥饿”，但极少的线程切换，保证了其更大的吞吐量。   

 

 

 

### **ReadWriteLock**

 

   java.util.concurrent.locks.**ReadWriteLock**， 

​	**ReadWriteLock**读写锁接口实际上表示的是两个锁   一个是读取操作的**共享锁**，一个是写入操作使用的**排他锁**。其实现**ReentrantReadWriteLock**。   

​	可以通过ReadWriteLock接口的readLock方法和writeLock方法来获取表示对应的锁的Lock接口的实现对象。

​	在没有线程进行写入操作时，进行读取操作的多个线程都可以获取读取锁；而进行写入操作的线程只有获取到写入锁后才能进行写入操作。多个线程可以同时进行读取操作，但是同一时刻只允许一个线程进行写入操作。   

 
