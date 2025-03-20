# advprog-tutorial-6
**Nama:** Alexander William Lim  
**NPM:** 2306207505  
**Kelas:** A

---
### Commit 1 Reflection Notes
Inside the `handle_connection()` method, there is code that: 
1. Creates a buffered reader(BufReader) from the TCP stream that's passed in as a parameter.
2. It also read lines from the HTTP request until it encounter an empty line (this marks the end of the HTTP headers).
3. It then unwraps each line result and collects thme into a vector.
4. FInally, it prints the entire HTTP request to the console in a debug format.

### Commit 2 Reflection Notes
The new `handle_connection()` method does additional tasks as follows:
1. Creates a buffered reader from the TCP stream.
2. It then read line from the HTTP request until it reaches and empty line.
3. THen it constructs an HTTP response with:
- A "200 OK" status line
- Read the contents of a file named "hello.html" from the filesystem.
- Add a Content-length header with the size of the file.
- Then includes the fiel contents in the response body.
4. Finally, it writes the complete HTTP response back to the client through the TCP stream.
![Commit 2 screen capture](/assets/commit2.jpg)

### Commit 3 Reflection Notes
**Why Split Responses**
1. Different HTTP requests need different responses.
2. The server must correctly respond with appropriate status codes (200 OK, 404 Not Found) based on the request validity.
3. Different resources should be served based on what the client requests.

**Why Refactoring is Needed**
Previously, there were a lot of code repetition in the codebase, causing code duplication to happen. When we need to change that code logic, then we would need to change a lot whole other part of the codebase, causing the chances of error occuring to become higher.

![Commit 3 screen capture](/assets/commit3.jpg)

### Commit 4 Reflection Notes
The page loads slower because of this part of the `handle_connection` method:
```
"GET /sleep HTTP/1.1" => {
    thread::sleep(Duration::from_secs(10));
    ("HTTP/1.1 200 OK", "hello.html")
}
```
What's happening is that when a client makes a rquest to the `/sleep` endpoint, the server intentionally pauses the execution for a total time of 10 seconds. This simulates a slow response or long-running operation on the server. Only after this 10-second delay does the server return the HTTp response with the contents of hello.html. This also happens to other pages because our program is working on a single thread, causing it to only run once the delay is finished.

### Commit 5 Reflection Notes
There are three core components in the ThreadPool implementation:
1. ThreadPool: The main structure that contains:
- workers: A collection of worker threads
- sender: A channel to send jobs to the workers.
2. Worker: A warraper around an OS thread that:
- Has an ID
- Contains a thread that continuously listens for jobs
3. Job: A boxed closure that can be executed by a worker

**Execution Flow:**
1. Initialization (`ThreadPool::new`):
- Creates a channel for communication
- Wraps the receiver in an `Arc<Mutex>` for thread-safe sharing 
- Arc allows multiple thread to the same dat by using atomic operations to track references, ensuring memory safety accross threads.
- Mutex wrapped inside ensures that only one thread can access the protected data at any given time by requiring threads to acquire a lock before accessing the value.
- Spawn a specified number of worker threads
- Each worker get a clone of the receiver
2. Submission (`ThreadPool::execute`):
- Client code submits a task as a closure
- The closure is boxed and becomes a "Job"
- The job is sent through the channel to the workers
3. Execution (in `Worker`):
- Each worker thread runs in a loop
- Acquires the mutex lock to check for new jobs
- WHen a job arrives, the worker executes it
- After completion, it loops back to wait for more jobs