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

