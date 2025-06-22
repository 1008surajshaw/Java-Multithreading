
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

