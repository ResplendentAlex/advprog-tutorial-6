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