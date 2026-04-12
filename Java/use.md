
## 🧠 JVM Architecture Overview

When you run a Java program, the **Java Virtual Machine (JVM)** performs three main functions:

1. **Loads** the `.class` files (bytecode)
2. **Verifies and executes** them
3. **Manages memory and system resources**


---

### 🏗 High-Level JVM Architecture


---

## 🧩 1. **Class Loader Subsystem**

Responsible for **loading class files into JVM memory**.

It goes through **three phases**:

### (a) **Loading**

- Loads `.class` files from disk, network, or JAR. to heap memory

- Converts bytecode into `Class` objects in memory.

- Uses **ClassLoader hierarchy**:

    1. **Bootstrap ClassLoader** – loads core Java classes (from `rt.jar` / `java.base`).

    2. **Extension (Platform) ClassLoader** – loads extension libraries (`lib/ext`).

    3. **Application (System) ClassLoader** – loads classes from your classpath.


📌 **Delegation Model:**  
Each class loader first delegates the loading request to its parent before trying itself — this prevents core classes from being overridden.

---

### (b) **Linking**

Ensures the class is **ready to use**.

1. **Verification:** Checks bytecode for security and format errors.  
   (e.g., illegal access, stack underflow/overflow)

2. **Preparation:** Allocates memory for static variables and sets default values.

3. **Resolution:** Replaces symbolic references (like class names) with direct references (like memory addresses).


---

### (c) **Initialization**

- Assigns **actual values** to static variables.

- Executes static blocks (`static { ... }`).


---


------------------------------------------------------------------------------------------



![](https://notebook.zohopublic.in/api/v1/public/notecards/74tdo52a4834de5554f09bc9ec3f11572cd11/resources/74tdo561baa112285413191d81fb4a43b7582)



   

![](https://notebook.zohopublic.in/api/v1/public/notecards/74tdo52a4834de5554f09bc9ec3f11572cd11/resources/74tdoc83163e8f8da41bea7ffd0ebfe1aee7f)



   

![](https://notebook.zohopublic.in/api/v1/public/notecards/74tdo52a4834de5554f09bc9ec3f11572cd11/resources/74tdo3451830d1d904ac4b603b7e9e0399880)



   

![](https://notebook.zohopublic.in/api/v1/public/notecards/74tdo52a4834de5554f09bc9ec3f11572cd11/resources/74tdoca254c2012c2443fa309219b34887080)



   

![](https://notebook.zohopublic.in/api/v1/public/notecards/74tdo52a4834de5554f09bc9ec3f11572cd11/resources/74tdodb8cf0e5cffb45b18c9c6a0899e8ee17)




