
## 🔹 Java Multithreading – Thread banane ke 2 tareeke:

### 1️⃣ **Thread class extend karke**

```java
// Thread ko extend karke thread banana
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Thread run ho raha hai - extend wale method se");
    }

    public static void main(String[] args) {
        MyThread t1 = new MyThread(); // thread object banao
        t1.start(); // thread start karo (run method call hoga)
    }
}
```

📝 **Note:** `Thread` class ko extend karte ho to `run()` method override karna padta hai. Fir `start()` method call karo thread chalane ke liye.

---

### 2️⃣ **Runnable interface implement karke**

```java
// Runnable ko implement karke thread banana
class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Thread run ho raha hai - runnable wale method se");
    }

    public static void main(String[] args) {
        MyRunnable obj = new MyRunnable();      // Runnable object banao
        Thread t1 = new Thread(obj);            // Thread class ka object, runnable pass karo
        t1.start();                             // thread start karo
    }
}
```

📝 **Note:** Ye method jyada flexible hai, kyunki aap multiple interface implement kar sakte ho (Java mein multiple inheritance allowed nahi hai via classes).

---




## 🔄 **Java Thread Life Cycle (Hinglish Notes)**

Java mein thread ka life cycle 5 main states mein divide hota hai:

1. **New** – Jab thread object create hota hai (`new Thread()`), lekin start nahi hua.
2. **Runnable** – Jab `start()` call karte ho, thread **ready to run** hota hai, CPU ka wait karta hai.
3. **Running** – Jab thread CPU le leta hai aur `run()` method execute karta hai.
4. **Blocked / Waiting** – Jab thread kisi resource ka wait karta hai ya manually `sleep()` / `wait()` / `join()` pe daal diya jata hai.
5. **Terminated (Dead)** – Jab `run()` method complete ho jata hai ya thread forcibly stop kar diya jaye.


### ☕ Code Example (Simple Explanation ke liye)

```java
class MyThread extends Thread {
    public void run() {
        try {
            System.out.println("Running state: Thread chal raha hai");
            Thread.sleep(1000); // Waiting state
            System.out.println("Thread phir se chalu ho gaya");
        } catch (InterruptedException e) {
            System.out.println("Thread interrupt hua");
        }
        System.out.println("Terminated state: Thread khatam");
    }

    public static void main(String[] args) {
        MyThread t = new MyThread(); // New state
        System.out.println("New state: Thread object create hua");

        t.start(); // Runnable state
        System.out.println("Runnable state: start() ke baad thread ready hai");

        // Optional: t.join() use karke Waiting state aur clear kar sakte ho
    }
}
```

---

### 📌 Summary Table (for quick notes):

| **State**         | **Description**                                      |
| ----------------- | ---------------------------------------------------- |
| `New`             | Thread object bana lekin `start()` call nahi hua     |
| `Runnable`        | `start()` ke baad, thread ready hai CPU ke liye      |
| `Running`         | CPU mil gaya, `run()` execute ho raha hai            |
| `Waiting/Blocked` | Thread ruk gaya hai - `sleep()`/`join()`/`wait()` se |
| `Terminated`      | Thread ka kaam complete, ya forcibly end ho gaya     |

---
## 🧵 **Kab Thread or Runnable use karein?**

**🧵Thread vs Runnable – Kab use karein?**

| 🔸**Use `Thread` class**                                           | 🔹**Use `Runnable` interface**                      |
| ------------------------------------------------------------------ | --------------------------------------------------- |
| Jab aap sirf thread logic likh rahe ho                             | Jab aap thread ke alawa aur kaam bhi kara rahe ho   |
| Already kisi aur class ko extend nahi kar rahe ho                  | Aapko multiple inheritance ki flexibility chahiye   |
| Simple, chhoti multithreading task hai                             | Code ko reusable aur scalable banana hai            |
| Thread class ko directly customize karna hai (override start etc.) | Business logic aur thread logic separate rakhna hai |

---

### ✅ Recommended: **Prefer `Runnable` interface**

**Kyu?**

* Java mein class inheritance ek hi baar hota hai, lekin interface multiple.
* Clean separation: logic vs thread handling.
* Better design for large/multi-threaded applications.

---

### ☕ Real-Life Analogy:

* `Thread` = Driver **and** Vehicle ek hi insaan hai.
* `Runnable` = Driver (logic) alag, Vehicle (thread) alag — zyada flexible ✅

---

### 📝 Short Advice:

> ✅ **Use `Runnable` when in doubt.**
> ❌ Avoid extending `Thread` unless really needed.

---

## 🧠 **Why Synchronization?**

Jab **multiple threads** ek hi shared resource ko access karte hain (like variable, object), tab **data inconsistency** ho sakti hai.
Synchronization ensure karta hai ki **ek time pe sirf ek thread** hi critical section access kare.

---

## 👇 Example Without Synchronization (❌ Wrong Output)

```java
class Counter {
    int count = 0;

    public void increment() {
        count++; // Not synchronized
    }
}

public class WithoutSync {
    public static void main(String[] args) throws InterruptedException {
        Counter counter = new Counter();

        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 10000; i++) counter.increment();
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 10000; i++) counter.increment();
        });

        t1.start();
        t2.start();

        t1.join();
        t2.join();

        System.out.println("Final Count (Without Sync): " + counter.count); // ❌ Incorrect Output (e.g., 18327)
    }
}
```

### 🔴 Issue:

> Expected output: `20000`
> But due to **race condition**, final count will be **less than 20000**.

---

## ✅ Example With Synchronization (✔ Correct Output)

```java
class Counter {
    int count = 0;

    public synchronized void increment() {
        count++; // Now synchronized
    }
}

public class WithSync {
    public static void main(String[] args) throws InterruptedException {
        Counter counter = new Counter();

        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 10000; i++) counter.increment();
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 10000; i++) counter.increment();
        });

        t1.start();
        t2.start();

        t1.join();
        t2.join();

        System.out.println("Final Count (With Sync): " + counter.count); // ✅ Correct Output: 20000
    }
}
```

---

## 📝 Summary Table:

| Concept        | Meaning                                                                 |
| -------------- | ----------------------------------------------------------------------- |
| `synchronized` | Lock lagata hai method/block pe; ek waqt pe sirf ek thread enter karega |
| Without sync   | Race condition create hoti hai; data corrupt ho sakta hai               |
| With sync      | Thread-safe, consistent output                                          |

---

### 📌 Real-world Analogy:

> Ek ATM machine – agar do log ek saath use karein bina queue ke, to balance galat ho sakta hai.
> Synchronization = ek waqt pe ek person hi ATM use kar sakta hai 🔐

---



## 🔐 1. `synchronized block` – Selective locking

### 🧠 Kya hai?

* Sirf **critical section** ko synchronize karte ho.
* Method ke bajay code block pe lock lagta hai.
* Better performance deta hai agar sirf chhota part critical hai.

### ☕ Example:

```java
class Counter {
    int count = 0;

    public void increment() {
        synchronized (this) {
            count++; // sirf ye block thread-safe hai
        }
    }
}
```

✅ Use karo jab:

* Puri method nahi, sirf kuch lines critical ho.

---

## 🔁 2. `ReentrantLock` – Advanced locking (java.util.concurrent.locks)

### 🧠 Kya hai?

* Explicit locking mechanism hai.
* More control deta hai than `synchronized`.
* Methods: `lock()`, `unlock()`, `tryLock()`, etc.

### ☕ Example:

```java
import java.util.concurrent.locks.ReentrantLock;

class Counter {
    int count = 0;
    ReentrantLock lock = new ReentrantLock();

    public void increment() {
        lock.lock(); // Lock lagaya
        try {
            count++;
        } finally {
            lock.unlock(); // Unlock karna zaroori
        }
    }
}
```

✅ Use karo jab:

* Aapko advanced control chahiye (timeout, fairness, nested locking)

---

## ⚙ 3. `AtomicInteger` – Lock-free thread-safe increment

### 🧠 Kya hai?

* `java.util.concurrent.atomic` package ka class.
* Internally atomic CPU instructions use karta hai.
* **No need of synchronized/locks**, still thread-safe.

### ☕ Example:

```java
import java.util.concurrent.atomic.AtomicInteger;

class Counter {
    AtomicInteger count = new AtomicInteger(0);

    public void increment() {
        count.incrementAndGet(); // atomic operation
    }
}
```

✅ Use karo jab:

* Sirf atomic operations (increment, decrement, etc.) chahiye without overhead.

---

## 📋 Summary Table for Notes:

| Feature      | `synchronized block`         | `ReentrantLock`                  | `AtomicInteger`                     |
| ------------ | ---------------------------- | -------------------------------- | ----------------------------------- |
| Locking Type | Implicit (monitor lock)      | Explicit                         | Lock-free                           |
| Scope        | Block-level                  | Flexible (lock/unlock manually)  | Predefined atomic ops only          |
| Flexibility  | Medium                       | High (tryLock, timeout, etc.)    | Limited (only atomic int ops)       |
| Performance  | Good                         | Overhead if misused              | Very fast for atomic counters       |
| Use When     | Small code block is critical | Fine control needed over locking | You need atomic counter/integer ops |

---

## 🎯 Recommendation:

| Situation                      | Preferred Tool       |
| ------------------------------ | -------------------- |
| Simple thread-safe counter     | `AtomicInteger`      |
| Critical section inside method | `synchronized block` |
| Advanced locking + control     | `ReentrantLock`      |

---


**simple aur clean Java program** diya gaya hai jismein  **`synchronized block`**, **`ReentrantLock`**, aur **`AtomicInteger`** ka comparison karte hain — **same structure**, same threads, alag implementations — easy to understand and compare ✅

---

## 🔀 Java Multithreading Comparison: `synchronized` vs `ReentrantLock` vs `AtomicInteger`

```java
import java.util.concurrent.atomic.AtomicInteger;
import java.util.concurrent.locks.ReentrantLock;

public class ThreadComparison {

    // 1️⃣ Using synchronized block
    static class SyncCounter {
        int count = 0;

        public void increment() {
            synchronized (this) {
                count++;
            }
        }
    }

    // 2️⃣ Using ReentrantLock
    static class LockCounter {
        int count = 0;
        ReentrantLock lock = new ReentrantLock();

        public void increment() {
            lock.lock();
            try {
                count++;
            } finally {
                lock.unlock();
            }
        }
    }

    // 3️⃣ Using AtomicInteger
    static class AtomicCounter {
        AtomicInteger count = new AtomicInteger(0);

        public void increment() {
            count.incrementAndGet();
        }
    }

    public static void main(String[] args) throws InterruptedException {
        // Objects
        SyncCounter syncCounter = new SyncCounter();
        LockCounter lockCounter = new LockCounter();
        AtomicCounter atomicCounter = new AtomicCounter();

        // 2 threads for each type
        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 10000; i++) {
                syncCounter.increment();
                lockCounter.increment();
                atomicCounter.increment();
            }
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 10000; i++) {
                syncCounter.increment();
                lockCounter.increment();
                atomicCounter.increment();
            }
        });

        // Start + wait
        t1.start();
        t2.start();
        t1.join();
        t2.join();

        // Print results
        System.out.println("SyncCounter: " + syncCounter.count);             // Expected: 20000
        System.out.println("LockCounter: " + lockCounter.count);             // Expected: 20000
        System.out.println("AtomicCounter: " + atomicCounter.count.get());   // Expected: 20000
    }
}
```

---

## 🔎 Output (Expected):

```
SyncCounter: 20000
LockCounter: 20000
AtomicCounter: 20000
```

---

## 📝 Notes:

* Har counter ko **2 threads** access kar rahe hain (each adds 10000).
* Har class ne apne tareeke se **thread-safety** ensure kiya:

  * `synchronized` via monitor lock
  * `ReentrantLock` via explicit lock
  * `AtomicInteger` via lock-free atomic ops

---

### ✅ Best Use Summary:

| Use Case                     | Tool            |
| ---------------------------- | --------------- |
| Simple thread-safe counter   | `AtomicInteger` |
| Part of method critical      | `synchronized`  |
| Need unlock control, timeout | `ReentrantLock` |

---



