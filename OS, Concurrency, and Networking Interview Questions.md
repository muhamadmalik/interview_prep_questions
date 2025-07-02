### **Part 1: Core Operating System Concepts**

#### **1. What is a Process vs. a Thread?**
This is a fundamental OS question about units of execution.

| Feature           | **Process**                                                               | **Thread**                                                                                                                               |
| :---------------- | :------------------------------------------------------------------------ | :--------------------------------------------------------------------------------------------------------------------------------------- |
| **Definition**    | A program in execution. It is a heavyweight unit.                         | A single execution sequence within a process. It is a lightweight unit.                                                                  |
| **Memory**        | Each process has its own separate memory space (heap, stack, code, data). | Threads within the same process share the same memory space (heap, code, data). Each thread has its own stack.                           |
| **Analogy**       | An apartment building. It's self-contained and isolated.                  | The residents living in the apartment building. They share common areas (kitchen, living room) but have their own private rooms (stack). |
| **Creation**      | Slow and resource-intensive.                                              | Fast and less resource-intensive.                                                                                                        |
| **Communication** | Inter-Process Communication (IPC) is complex (e.g., pipes, sockets).      | Inter-Thread Communication is simple as they can directly access shared memory.                                                          |
| **Isolation**     | A crash in one process does not affect other processes.                   | A crash in one thread can crash the entire process.                                                                                      |

**Why use threads (Multithreading)?** To perform multiple operations concurrently within a single application, improving performance and responsiveness. For example, in a word processor, one thread can handle user input while another saves the file in the background.

#### **2. What is the Kernel and its functionality?**
The **Kernel** is the core component of an operating system. It acts as the primary interface between the system's hardware and its software. It has complete control over everything in the system.

**Main Functionalities:**
1.  **Process Management:** Creating, scheduling (deciding which process runs next), and terminating processes and threads.
2.  **Memory Management:** Allocating and deallocating memory space for processes (managing the heap and stack).
3.  **Device Management:** Acting as an intermediary between hardware devices (like a disk, keyboard, or network card) and processes via device drivers.
4.  **System Call Interface:** Providing a set of functions that applications can use to request services from the operating system (e.g., opening a file, creating a process).

#### **3. What is Context Switching?**
**Context Switching** is the process of storing the state (or context) of a process or thread so that it can be restored and resume execution at a later point. This allows multiple processes to share a single CPU.

**The process involves:**
1.  Saving the context of the currently running process (e.g., values of CPU registers, program counter, process state).
2.  Loading the saved context of the next process that is scheduled to run.
3.  Resuming execution of the new process.

Context switching is pure overhead, as the system does no useful work during the switch. Frequent context switching can degrade system performance.

#### **4. Zombie vs. Orphan Process?**
*   **Zombie Process:** A process that has completed its execution but still has an entry in the process table. This entry is needed so the parent process can read its child's exit status. Once the parent reads the status (using the `wait` system call), the zombie process is removed. If a parent doesn't `wait`, the zombies accumulate.
*   **Orphan Process:** A process whose parent process has terminated without waiting for its child. The orphan process is then "adopted" by the `init` process (process ID 1), which will periodically `wait` for it to complete and clean up its entry from the process table.

---

### **Part 2: Concurrency and Synchronization**

#### **5. What is a Race Condition, Critical Section, and Mutual Exclusion?**
These concepts are central to concurrent programming.

1.  **Critical Section:** A piece of code that accesses a shared resource (e.g., a shared variable, a file) that must not be concurrently accessed by more than one thread or process.
2.  **Race Condition:** An undesirable situation that occurs when multiple threads or processes access and manipulate a shared resource concurrently, and the final outcome depends on the unpredictable timing or "race" of their execution. This leads to corrupt or inconsistent data.
3.  **Mutual Exclusion (Mutex):** The mechanism used to prevent race conditions. It ensures that only one thread can be executing within a critical section at any given time. A **mutex** (or **lock**) is a common synchronization primitive used to enforce mutual exclusion.

**Real-world analogy:** A single-person restroom.
*   **Critical Section:** The restroom itself.
*   **Shared Resource:** The toilet.
*   **Race Condition:** Two people trying to enter at the same time.
*   **Mutex:** The lock on the door. Only one person can acquire the lock, enter, and use the restroom. They must release the lock when they leave, allowing the next person to enter.

**Python Example:**
```python
import threading

shared_counter = 0
lock = threading.Lock()

def increment_counter():
    global shared_counter
    for _ in range(100000):
        # Without a lock, this is a race condition
        # With a lock, we enforce mutual exclusion
        lock.acquire()
        shared_counter += 1
        lock.release()

threads = [threading.Thread(target=increment_counter) for _ in range(5)]
for t in threads: t.start()
for t in threads: t.join()

print(f"Final counter value: {shared_counter}") # Should be 500000
```

#### **6. What is a Semaphore? (vs. a Mutex)**
A **Semaphore** is a more general synchronization tool than a mutex. It is essentially a counter that controls access to a pool of shared resources.

*   **Binary Semaphore:** Can only have two values (0 or 1). It functions identically to a mutex.
*   **Counting Semaphore:** Can have any non-negative integer value. It is initialized to the number of available resources. A thread can access a resource by "decrementing" the semaphore (the `wait` or `P` operation). If the count is zero, the thread blocks. When a thread releases a resource, it "increments" the semaphore (the `signal` or `V` operation), potentially unblocking another thread.

**Mutex vs. Semaphore:**
*   A **mutex** is about **locking** and ownership. Only the thread that acquired the lock can release it. It's used for protecting a critical section.
*   A **semaphore** is about **signaling** and managing a number of resources. One thread can signal/increment the semaphore while another is waiting on it.

**Analogy:** A parking garage with N spots.
*   **Counting Semaphore:** The electronic sign at the entrance showing the number of available spots (initialized to N). A car can enter if the count > 0, decrementing the sign. When a car leaves, it increments the sign.
*   **Mutex:** A key to a single, private parking spot.

#### **7. What is a Deadlock? How to avoid it?**
A **Deadlock** is a state where two or more processes are blocked forever, each waiting for a resource that is held by another process in the set.

**The Four Necessary Conditions for Deadlock:**
1.  **Mutual Exclusion:** At least one resource must be held in a non-sharable mode.
2.  **Hold and Wait:** A process is holding at least one resource and is waiting to acquire additional resources held by other processes.
3.  **No Preemption:** A resource cannot be forcibly taken from a process; it must be released voluntarily.
4.  **Circular Wait:** A set of waiting processes {P0, P1, ..., Pn} must exist such that P0 is waiting for a resource held by P1, P1 is waiting for P2, ..., and Pn is waiting for a resource held by P0.

**Deadlock Avoidance:** The most famous strategy is the **Banker's Algorithm**. The OS will not grant a resource request if doing so could lead to a future deadlock state. It requires processes to declare their maximum resource needs in advance. In practice, most general-purpose operating systems focus on deadlock detection and recovery or simply ignore the problem, relying on programmers to write deadlock-free code.

---

### **Part 3: Networking Fundamentals**

#### **8. MAC Address vs. IP Address?**

| Feature           | **MAC (Media Access Control) Address**                         | **IP (Internet Protocol) Address**                               |
| :---------------- | :------------------------------------------------------------- | :--------------------------------------------------------------- |
| **Layer**         | **Layer 2** (Data Link Layer) of the OSI model.                | **Layer 3** (Network Layer) of the OSI model.                    |
| **Purpose**       | Provides a unique hardware identifier for a device on a local network (LAN). | Provides a logical address for a device on a larger network (like the internet). |
| **Analogy**       | A person's unique Social Security Number. It doesn't change.   | A person's mailing address. It can change if they move.          |
| **Format**        | 48-bit hexadecimal number (e.g., `00:1A:2B:3C:4D:5E`).          | 32-bit (IPv4, e.g., `192.168.1.1`) or 128-bit (IPv6).             |
| **Scope**         | Used for communication within the same local network (e.g., between your laptop and your Wi-Fi router). | Used for routing packets across different networks (e.g., from your router to Google's servers). |
| **Assignment**    | Burned into the Network Interface Card (NIC) by the manufacturer. | Assigned by a network administrator or DHCP server.              |

#### **9. TCP vs. UDP?**

| Feature          | **TCP (Transmission Control Protocol)**                                | **UDP (User Datagram Protocol)**                                 |
| :--------------- | :--------------------------------------------------------------------- | :--------------------------------------------------------------- |
| **Connection**   | **Connection-oriented.** Establishes a connection (three-way handshake) before sending data. | **Connectionless.** Sends data packets (datagrams) without establishing a connection. |
| **Reliability**  | **Reliable.** Guarantees that data will be delivered in order and without errors, using acknowledgements and retransmissions. | **Unreliable.** "Fire and forget." No guarantee of delivery, order, or error checking. |
| **Speed**        | Slower due to the overhead of ensuring reliability.                     | Faster due to its simplicity and lack of overhead.               |
| **Use Cases**    | Web browsing (HTTP/HTTPS), email (SMTP), file transfer (FTP). Places where data integrity is critical. | Live video/audio streaming, online gaming, DNS. Places where speed is more important than perfect reliability. |

#### **10. What is DNS?**
**DNS (Domain Name System)** is the phonebook of the internet. Humans access information online through domain names like `google.com`. Web browsers, however, interact through Internet Protocol (IP) addresses. DNS translates human-readable domain names into machine-readable IP addresses.

#### **11. If you type google.com, what happens behind the scenes?**
This is a classic question that ties together multiple networking concepts.

1.  **Browser Check:** The browser checks its own cache to see if it already knows the IP address for `google.com`.
2.  **OS Check:** If not in the browser cache, the browser asks the operating system, which checks its cache (and the `hosts` file).
3.  **DNS Resolution:** If the IP is still unknown, the OS's DNS resolver (typically your router or ISP's DNS server) starts the resolution process:
    *   It queries a **Root DNS Server**, which directs it to a **Top-Level Domain (TLD) DNS Server** for `.com`.
    *   The TLD server directs it to the **Authoritative Name Server** for `google.com`.
    *   The Authoritative Name Server returns the final IP address (e.g., `142.250.190.78`) for `google.com`.
4.  **TCP Connection:** The browser opens a TCP connection to the server at that IP address on port 443 (for HTTPS). This involves the **TCP Three-Way Handshake**:
    *   **Client -> Server:** SYN (Synchronize)
    *   **Server -> Client:** SYN-ACK (Synchronize-Acknowledge)
    *   **Client -> Server:** ACK (Acknowledge)
5.  **TLS Handshake:** For a secure HTTPS connection, a **TLS (Transport Layer Security)** handshake occurs to negotiate encryption algorithms and exchange security keys.
6.  **HTTP Request:** The browser sends an HTTP `GET` request to the server, asking for the content of the `/` page. The request includes headers (like browser type, cookies, etc.).
7.  **Server Processing:** Google's web server receives the request, processes it, and prepares an HTTP response.
8.  **HTTP Response:** The server sends back the response, which includes:
    *   A status code (e.g., `200 OK`).
    *   Response headers (e.g., `Content-Type: text/html`).
    *   The response body (the HTML content of the Google homepage).
9.  **Browser Rendering:** The browser receives the HTML, parses it to build the **DOM (Document Object Model)**, and starts rendering the page. As it encounters links to other resources (CSS files, JavaScript files, images), it repeats a similar process (DNS lookup, TCP connection, HTTP request) to fetch them.

---

### **Part 4: Memory Management**

#### **12. Heap vs. Stack Memory Difference? Where are objects stored?**

*   **Stack:**
    *   **Purpose:** Stores static memory allocations: local variables, function call frames, and primitive types.
    *   **Management:** Managed automatically by the CPU in a LIFO (Last-In, First-Out) manner. Memory is allocated when a function is called and deallocated when the function returns.
    *   **Size:** Relatively small and fixed.
    *   **Access Speed:** Very fast due to its simple, contiguous structure.

*   **Heap:**
    *   **Purpose:** Stores dynamic memory allocations: objects and data structures whose size is not known at compile time or that need to exist beyond the scope of a single function call.
    *   **Management:** Managed by the programmer (in languages like C++) or by a garbage collector (in languages like Java and Python). Memory must be explicitly allocated and deallocated.
    *   **Size:** Much larger than the stack.
    *   **Access Speed:** Slower than the stack due to more complex memory management and potential fragmentation.

**Where are objects stored?**
Objects created with the `new` keyword (in Java/C++) or simply by instantiation (e.g., `my_obj = MyClass()` in Python) are stored on the **Heap**. The variable that refers to the object (e.g., `my_obj`) is a pointer/reference that is stored on the **Stack**.

#### **13. What is a Segmentation Fault or Stack Overflow error?**
*   **Segmentation Fault:** An error that occurs when a program tries to access a memory location that it is not allowed to access (e.g., accessing a null pointer, writing to a read-only part of memory, or accessing an array out of bounds). It's a violation of memory access rules.
*   **Stack Overflow:** An error that occurs when the call stack becomes too full. This is almost always caused by excessively deep or infinite recursion, where a function calls itself so many times that it exhausts the limited space available on the stack.

#### **14. What is Garbage Collection?**
**Garbage Collection (GC)** is a form of automatic memory management. The garbage collector's job is to identify and reclaim memory that is no longer in use by the program, freeing the programmer from having to manually deallocate memory. Languages like Java, Python, C#, and Go have automatic garbage collection.

The most common strategy is to identify "unreachable" objects. An object is considered reachable if it is referenced by a variable on the stack or by another reachable object. Any object that is not reachable is considered "garbage" and can be collected.