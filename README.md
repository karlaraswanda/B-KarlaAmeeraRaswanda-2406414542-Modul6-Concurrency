### Commit 1 Reflection Notes  
The `handle_connection` function processes incoming TCP connections by reading data from the `TcpStream`. A `BufReader` is used to efficiently read the stream line by line, since HTTP requests are text-based. The `.take_while(|line| !line.is_empty())` stops reading at the empty line that separates headers from the body, resulting in a vector of request lines containing the HTTP request structure.  

### Commit 2 Screen Capture  
![Commit 2](assets/images/commit2.png) 

### Commit 2 Reflection Notes  
The updated `handle_connection` function constructs a valid HTTP response by combining a status line, headers, and HTML content. The server reads the HTML file using `fs::read_to_string`, calculates its length, and formats the response accordingly.  

### Commit 3 Screen Capture  
![Commit 3](assets/images/commit3.png)  

### Commit 3 Reflection Notes  
The implementation now distinguishes between different HTTP requests by analyzing the request line and mapping it to specific responses. Using a tuple for status and filename helps decouple response configuration from content generation.