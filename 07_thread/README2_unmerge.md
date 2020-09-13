# Java多线程高并发讲解2

## 2 ReentrantLock
### 2.1 synchronzied 本身就是可重入锁
```java
public class Case104_ReeentrantLock1 {

    /**
     * 加锁之后代码段m，同一时间只能被一个线程使用
     */
    synchronized void m1(){
        for(int i = 0 ; i < 10; i++){
            try{
                TimeUnit.SECONDS.sleep(1);
            }catch(InterruptedException e){
                e.printStackTrace();
            }

            System.out.println(i);

            if(i % 2 == 0) m2();
        }
    }
    synchronized void m2(){
        System.out.println("m2....");
    }

    public static void main(String[] args){

        // 生命一个可重入锁
        Case104_ReeentrantLock1 r1 = new Case104_ReeentrantLock1();
        new Thread(r1::m1).start();
        try{
            TimeUnit.SECONDS.sleep(1);
        }catch(InterruptedException e){
            e.printStackTrace();
        }
        new Thread(r1::m2).start();
    }
}
```
### 2.2 ReentranLock
* 可以替代 synchronzied, 但是 lock是需要 lock 和 finally{ unlock}
> 参考  Case105_ReentrantLock2
* 使用 tryLock
* 使用 lockInterruptibly()试图唤醒原有sleep的线程
> 参考 Case106_ReentrantLock3
* ReentranLock是个公平锁，使用队列等待，而不是自旋锁的抢
* 
