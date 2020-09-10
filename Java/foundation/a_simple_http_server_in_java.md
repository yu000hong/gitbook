# A Simple HTTP Server in Java

> In this article, we discuss how to create a simple HTTP server in Java that can handle GET and POST requests with Java SDK's HttpServer class.

### HTTPServer class

Java SDK provides an in-built server called `HttpServer`. This class belongs to `com.sun.net` package.

We can instantiate the server like this:

```java
HttpServer server = HttpServer.create(new InetSocketAddress("localhost", 8001), 0);
```

The above line creates an `HttpServer` instance on localhost with port number 8001. But, there is one more argument with value 0. This value is used for **back logging**.

### Back Logging

When a server accepts a client request, this request first will be queued by the operating system. Later, it will be given to the server to process the request. All of these simultaneous requests will be queued by the operating system. However, the operating system will decide how many of these requests can be queued at any given point in time. This value represents back logging. In our example, this value is 0, **which means that we do not queue any requests**.

### Server Code

We are going to develop the following HTTP server code:

```java
server.createContext("/test", new  MyHttpHandler());
server.setExecutor(threadPoolExecutor);
server.start();
logger.info(" Server started on port 8001");
```

We created a context called, `test`. This is nothing but the context root of the application. The second parameter is a handler instance, which will handle the HTTP requests. We will look into this class shortly.

We can use a thread pool executor, along with this server instance. In our case, we created a thread pool with 10.

```java
ThreadPoolExecutor threadPoolExecutor = (ThreadPoolExecutor)Executors.newFixedThreadPool(10);
```

Next, we start the server:

```java
server.start();
```

With just three to four lines of code, we created an HTTP server with a context root that listens on a port!

### HTTPHandler Class

This is an interface with a method called `handle(..)`. Let us take a look at our implementation of this interface.

```java
private class MyHttpHandler implements HttpHandler {
    @Override
    public void handle(HttpExchange httpExchange) throws IOException {
        String requestParamValue=null;
        if("GET".equals(httpExchange.getRequestMethod())) {
            requestParamValue = handleGetRequest(httpExchange);
        }else if("POST".equals(httpExchange)) {
            requestParamValue = handlePostRequest(httpExchange);
        }

        handleResponse(httpExchange,requestParamValue);
    }

    private String handleGetRequest(HttpExchange httpExchange) {
        return httpExchange
                .getRequestURI()
                .toString()
                .split("\\?")[1]
                .split("=")[1];
    }

    private void handleResponse(HttpExchange httpExchange, String requestParamValue) throws IOException {
        OutputStream outputStream = httpExchange.getResponseBody();
        StringBuilder htmlBuilder = new StringBuilder();
        htmlBuilder
            .append("<html>")
            .append("<body>")
            .append("<h1>")
            .append("Hello ")
            .append(requestParamValue)
            .append("</h1>")
            .append("</body>")
            .append("</html>");

        // encode HTML content
        String htmlResponse = StringEscapeUtils.escapeHtml4(htmlBuilder.toString());
        // this line is a must
        httpExchange.sendResponseHeaders(200, htmlResponse.length());
        outputStream.write(htmlResponse.getBytes());
        outputStream.flush();
        outputStream.close();
    }
}
```

This is the code that handles the request and sends the response back to the client. The request and the response is handled by the `HttpExchange` class.

### 参考资料

[A Simple HTTP Server in Java](https://dzone.com/articles/simple-http-server-in-java)
