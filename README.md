
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
