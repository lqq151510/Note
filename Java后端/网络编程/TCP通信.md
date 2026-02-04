在Java中实现TCP通信，确实需要客户端和服务器端协同工作。下面这张图清晰地展示了整个通信流程的核心步骤和双向数据交换。

```
flowchart TD
    A[客户端Client] --> B[创建Socket<br>指定服务端IP与端口]
    B --> C[连接至服务器]
    C --> D[获取输出流OutputStream<br>发送数据]
    D --> E[获取输入流InputStream<br>接收响应]
    E --> F[关闭连接]
    
    G[服务器端Server] --> H[创建ServerSocket<br>绑定监听端口]
    H --> I[调用accept方法<br>等待客户端连接]
    I --> J[接受连接<br>获取与客户端通信的Socket]
    J --> K[获取输入流InputStream<br>接收请求]
    K --> L[获取输出流OutputStream<br>发送响应]
    L --> M[关闭连接]
    
    D --> K
    L --> E
```

下面我们来看具体的代码实现。

### 🔧 客户端实现

客户端负责发起连接，并向服务器发送请求和接收响应。

```
import java.io.*;
import java.net.Socket;

public class TCPClient {
    public static void main(String[] args) {
        // 服务器地址和端口
        String serverAddress = "localhost"; // 或 "127.0.0.1"
        int serverPort = 12345;

        // 使用 try-with-resources 自动关闭资源
        try (Socket socket = new Socket(serverAddress, serverPort);
             // 获取输出流，用于向服务器发送数据
             OutputStream outputStream = socket.getOutputStream();
             PrintWriter writer = new PrintWriter(outputStream, true);
             
             // 获取输入流，用于接收服务器响应
             InputStream inputStream = socket.getInputStream();
             BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream));
             
             // 从控制台读取用户输入
             BufferedReader userInput = new BufferedReader(new InputStreamReader(System.in))) {

            System.out.println("已连接到服务器。请输入消息：");
            String messageToSend = userInput.readLine();

            // 发送消息给服务器
            writer.println(messageToSend);
            System.out.println("发送至服务器: " + messageToSend);

            // 接收服务器的响应
            String serverResponse = reader.readLine();
            System.out.println("收到服务器响应: " + serverResponse);

        } catch (IOException e) {
            System.err.println("客户端错误: " + e.getMessage());
        }
    }
}
```

### 🖥️ 服务器端实现

服务器端负责监听特定端口，接受客户端连接，并处理请求和发送响应。

```
import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class TCPServer {
    public static void main(String[] args) {
        int port = 12345; // 监听的端口

        // 使用 try-with-resources 自动关闭ServerSocket
        try (ServerSocket serverSocket = new ServerSocket(port)) {
            System.out.println("服务器启动，在端口 " + port + " 监听...");

            while (true) { // 持续运行，接受多个客户端连接
                // 等待客户端连接（阻塞方法，直到有客户端连接）
                Socket clientSocket = serverSocket.accept();
                System.out.println("客户端已连接: " + clientSocket.getInetAddress());

                // 为每个客户端连接创建一个新的线程或处理逻辑
                // 这里为了简单，在主线程中处理。实际应用应考虑多线程或线程池。
                handleClient(clientSocket);
            }
        } catch (IOException e) {
            System.err.println("服务器错误: " + e.getMessage());
        }
    }

    private static void handleClient(Socket clientSocket) {
        // 处理每个客户端连接的具体逻辑
        try (clientSocket; // 使用 try-with-resources 确保clientSocket被关闭
             InputStream inputStream = clientSocket.getInputStream();
             BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream));
             
             OutputStream outputStream = clientSocket.getOutputStream();
             PrintWriter writer = new PrintWriter(outputStream, true)) {

            // 读取客户端发送的消息
            String clientMessage = reader.readLine();
            System.out.println("收到客户端消息: " + clientMessage);

            // 处理消息（这里简单做个回声处理）
            String response = "服务器已收到您的消息: 【" + clientMessage + "】";
            
            // 发送响应给客户端
            writer.println(response);
            System.out.println("已向客户端发送响应。");

        } catch (IOException e) {
            System.err.println("处理客户端连接时出错: " + e.getMessage());
        }
    }
}
```

### 💡 代码要点说明

1. **建立连接**：客户端通过 `new Socket(serverAddress, serverPort)`主动连接服务器。服务器通过 `ServerSocket.accept()`方法**阻塞**等待客户端的连接 。
    
2. **数据收发**：连接建立后，双方通过Socket获取**输入流（InputStream）**​ 和**输出流（OutputStream）**​ 进行通信。示例中使用了`PrintWriter`和`BufferedReader`来方便地处理文本数据 。
    
3. **资源管理**：使用 **try-with-resources**​ 语句可以自动关闭Socket和流，这是避免资源泄漏的最佳实践 。
    
4. **简单处理逻辑**：`handleClient`方法中实现了简单的“回声”服务，服务器将收到的消息原样返回并附加确认文本。
    

### 🚀 运行示例

1. **启动服务器**：首先运行 `TCPServer`类的 `main`方法。服务器将启动并等待客户端连接。
    
2. **启动客户端**：然后运行 `TCPClient`类的 `main`方法。客户端会连接服务器，你可以在控制台输入消息并查看服务器的回声响应。
    

### ⚠️ 重要注意事项

- **异常处理**：网络操作可能因各种原因（如网络不可达、端口被占用等）失败，必须进行异常处理 。
    
- **多客户端处理**：上面的服务器示例在同一时刻只能处理一个客户端。要同时处理多个客户端，需要在 `accept()`连接后，为每个 `Socket`创建一个**新线程**（或使用线程池）来处理 。例如：
    
    ```
    // 在serverSocket.accept()之后
    new Thread(() -> handleClient(clientSocket)).start();
    ```
    
- **流的正确使用**：
    
    - 使用 `PrintWriter`时，注意**刷新缓冲区**。示例中 `PrintWriter`的构造函数的第二个参数设为 `true`，意味着每次调用 `println`后会自动刷新缓冲区，确保数据及时发送 。
        
    - 如果传输的是二进制数据（如图片、文件），应直接使用 `InputStream`和 `OutputStream`进行字节流的读写 。
        
    
- **连接关闭**：通信完毕后，应关闭Socket及其关联的流，以释放系统资源 。
    

希望这些详细的解释和示例能帮助你顺利实现Java中的TCP数据收发。如果你在特定步骤上遇到问题，或者想了解如何处理多客户端并发连接等更深入的内容，我们可以继续探讨。