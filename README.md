### Commit 1 Reflection Notes  
The `handle_connection` function processes incoming TCP connections by reading data from the `TcpStream`, which represents the communication channel between the client and the server. A `BufReader` is used to efficiently read the incoming stream line by line, which is suitable because HTTP requests are text-based. The `.lines()` method converts the stream into an iterator of lines, making it easier to process structured request data. The use of `.take_while(|line| !line.is_empty())` ensures that only the request headers are read, stopping at the empty line that separates headers from the body. This approach reflects how HTTP requests are structured and helps the program capture only the relevant metadata. Overall, this step demonstrates how low-level network communication can be translated into structured data for further processing.

### Commit 2 Screen Capture  
![Commit 2](assets/images/commit2.png) 

### Commit 2 Reflection Notes  
The updated `handle_connection` function constructs a valid HTTP response by combining a status line, headers, and HTML content into a single response string. The program reads the HTML file using `fs::read_to_string`, which allows the server to dynamically load content from disk. The `Content-Length` header is included to inform the browser about the size of the response body, ensuring proper rendering. Without this header, the browser may not correctly interpret the response, leading to unexpected behavior. This step highlights the importance of following HTTP protocol standards when building a web server. It also demonstrates how even a simple server must adhere to strict formatting rules for communication.
### Commit 3 Screen Capture  
![Commit 3](assets/images/commit3.png)  

### Commit 3 Reflection Notes  
In this step, the server begins to validate incoming requests by examining only the request line, which contains the HTTP method and requested path. By matching this line against specific patterns, the server can determine which resource to return. The use of a tuple to store both the status line and filename separates response configuration from content generation, making the code more organized. This approach introduces a basic form of routing, where different URLs result in different responses. Although the implementation is simple and relies on exact string matching, it effectively demonstrates the core concept of request validation. This lays the foundation for more advanced routing mechanisms in real-world web frameworks.

### Commit 4 Screen Capture
1. /sleep request (delayed)  
![Commit 4a](assets/images/commit4a.png)  

2. Normal request blocked  
![Commit 4b](assets/images/commit4b.png)  

### Commit 4 Reflection Notes  
By introducing a delay using `thread::sleep`, the server simulates a slow request to demonstrate how a single-threaded system behaves under blocking conditions. Since the server processes requests sequentially, a long-running request prevents other incoming requests from being handled. This causes noticeable delays for all users, even if their requests are simple and fast. The experiment highlights a critical limitation of single-threaded servers, especially in scenarios with multiple concurrent users. In real-world applications, this would lead to poor performance and degraded user experience. Therefore, this step emphasizes the need for concurrency to handle multiple requests efficiently.
### Commit 5 Screen Capture
![Commit 5](assets/images/commit5.png)

### Commit 5 Reflection Notes  
The implementation of a ThreadPool improves the server by allowing it to handle multiple requests concurrently while limiting the number of active threads. Instead of creating a new thread for each incoming connection, tasks are distributed to a fixed number of worker threads through a shared channel. This design reduces the overhead associated with frequent thread creation and destruction. It also prevents resource exhaustion that could occur if too many threads are spawned simultaneously. Each worker continuously listens for incoming jobs and executes them independently, enabling parallel request handling. This approach demonstrates a more scalable and efficient model for building high-performance servers.

## Bonus Reflection Notes
The introduction of the `build` function improves the ThreadPool design by replacing panic-based validation with explicit error handling using `Result`. Unlike the `new` function, which uses `assert!` and may terminate the program abruptly, `build` allows the caller to handle invalid input gracefully. This makes the implementation safer and more suitable for production environments. By returning a `Result`, the function clearly communicates potential failure cases to the caller. This change reflects Rust’s emphasis on explicit and reliable error handling. Overall, it demonstrates a more robust and idiomatic approach to constructing complex components.