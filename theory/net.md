# 1. TCP

客户端

1. 连接服务器 Socket

2. 发送消息 

   `os = socket.getOutputStream();`
   `os.write("hello".getBytes());`

```java
package com.isaiah;

import java.io.IOException;
import java.io.OutputStream;
import java.net.InetAddress;
import java.net.Socket;
import java.net.UnknownHostException;

// 服务端
public class TCPClientDemo {
    public static void main(String[] args) {

        Socket socket = null;
        OutputStream os = null;
        try {
            // 1. 服务器地址
            InetAddress serverIP = InetAddress.getByName("127.0.0.1");
            // 2. 端口号
            int port = 9999;
            // 3. 创建一个socket连接 127.0.0.1:9999
            socket = new Socket(serverIP, port);
            // 4. 发送消息 IO流
            os = socket.getOutputStream();
            os.write("hello".getBytes());
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(os!=null){
                try {
                    os.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

            if(socket!=null){
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

```



服务端

1. 建立服务的端口 ServerSocket

   `new ServerSocket(9999);`

2. 等待用户的连接 accpet

   `Socket socket = serverSocket.accept();`

3. 接受用户的消息

   `is = socket.getInputStream();`

   `is.read(buffer);`

```java
package com.isaiah;

import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.net.ServerSocket;
import java.net.Socket;

// 客户端
public class TCPServerDemo {
    public static void main(String[] args) {

        ServerSocket serverSocket = null;
        Socket socket = null;
        InputStream is = null;
        ByteArrayOutputStream byteArrayOutputStream = null;

        try {
            // 1.地址
            serverSocket = new ServerSocket(9999);
            while(true){
                // 2.等待客户端连接过来
                socket = serverSocket.accept();
                // 3.读取客户端的消息
                is = socket.getInputStream();

                // 第一种
    //            byte[] buffer = new byte[1024];
    //            int len;
    //            while ((len=is.read(buffer))!=-1){
    //                String msg = new String(buffer, 0, len);
    //                System.out.println(msg);
    //            }
                // 第二种
                byteArrayOutputStream = new ByteArrayOutputStream();
                byte[] buffer = new byte[1024];
                int len;
                while((len=is.read(buffer))!=-1){
                    byteArrayOutputStream.write(buffer,0,len);
                }
                System.out.println(byteArrayOutputStream.toString());
            }

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            // 关闭资源
            if(byteArrayOutputStream!=null){
                try {
                    byteArrayOutputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

            if(is != null){
                try {
                    is.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

            if(socket!=null){
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

            if(serverSocket!=null){
                try {
                    serverSocket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

        }
    }

}

```



## 文件上传

服务器端

```java
package com.isaiah.TCP_2;

import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class TCPServerDemo {
    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket(9000);
        Socket socket = serverSocket.accept(); // 阻塞监听
        InputStream inputStream = socket.getInputStream();

        FileOutputStream fileOutputStream = new FileOutputStream(new File("receive.jpg"));
        byte[] buffer = new byte[1024];
        int len = 0;
        while ((len=inputStream.read(buffer))!=-1){
            fileOutputStream.write(buffer, 0, len);
        }

        // 通知客户端接受完毕
        OutputStream outputStream = socket.getOutputStream();
        outputStream.write("接收完毕,可以断开".getBytes());

        fileOutputStream.close();
        inputStream.close();
        socket.close();
        serverSocket.close();
    }
}

```

客户端

```java
package com.isaiah.TCP_2;

import java.io.*;
import java.net.InetAddress;
import java.net.Socket;

public class TCPClientDemo {
    public static void main(String[] args) throws IOException {
        Socket socket = new Socket(InetAddress.getByName("127.0.0.1"), 9000);
        OutputStream outputStream = socket.getOutputStream();
        FileInputStream fileInputStream = new FileInputStream(new File("D:/PICTURE/19luca.jpg"));

        byte[] buffer = new byte[1024];
        int len = 0;
        while ((len=fileInputStream.read(buffer))!=-1){
            outputStream.write(buffer, 0, len);
        }

        // 通知服务器已经结束
        socket.shutdownOutput();

        // 确定服务器接收完毕
        InputStream inputStream = socket.getInputStream();
        ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream();
        while ((len=inputStream.read(buffer))!=-1){
            byteArrayOutputStream.write(buffer, 0, len);
        }
        System.out.println(byteArrayOutputStream.toString());

        byteArrayOutputStream.close();
        inputStream.close();
        fileInputStream.close();
        outputStream.close();
        socket.close();
    }
}

```

# 2. UDP

发送端

```java
package com.isaiah.UDP_1;

import java.io.IOException;
import java.net.*;

public class UDPClientDemo {
    public static void main(String[] args) throws IOException {
        // 建立一个socket
        DatagramSocket socket = new DatagramSocket();

        // 建立数据包
        String msg = "你好, 我的服务器!";
        InetAddress ip = InetAddress.getByName("127.0.0.1");
        int port = 9090;
        DatagramPacket packet = new DatagramPacket(msg.getBytes(), 0, msg.getBytes().length, ip, port);

        // 发送包
        socket.send(packet);

        // 关闭连接
        socket.close();
    }
}

```

接收端

```java
package com.isaiah.UDP_1;

import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;

public class UDPServerDemo {
    public static void main(String[] args) throws IOException {
        // 开放端口
        DatagramSocket socket = new DatagramSocket(9090);

        // 接收数据包
        byte[] buffer = new byte[1024];
        DatagramPacket packet = new DatagramPacket(buffer, 0, buffer.length);

        socket.receive(packet); // 阻塞接收
        System.out.println(new String(packet.getData(), 0, packet.getLength()));

        socket.close();
    }
}

```



## 聊天

接收数据报

 ```java
 package com.isaiah.UDP_2;
 
 import java.io.IOException;
 import java.net.DatagramPacket;
 import java.net.DatagramSocket;
 
 public class UDPReceiveDemo {
     public static void main(String[] args) throws IOException {
         DatagramSocket socket = new DatagramSocket(2222);
 
         while (true) {
             byte[] buffer = new byte[1024];
             DatagramPacket datagramPacket = new DatagramPacket(buffer, 0, buffer.length);
             socket.receive(datagramPacket);
 
             byte[] data = datagramPacket.getData();
             String dataString = new String(data, 0, data.length);
             System.out.println(dataString);
 
             if("bye".equals(dataString)){
                 break;
             }
         }
 
         socket.close();
     }
 }
 
 ```

发送数据报

```java
package com.isaiah.UDP_2;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;
import java.net.SocketException;

public class UDPSenderDemo {
    public static void main(String[] args) throws IOException {
        // 开放端口
        DatagramSocket socket = new DatagramSocket(1111);

        while (true) {
            // 准备数据
            BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
            String data = bufferedReader.readLine();

            DatagramPacket datagramPacket = new DatagramPacket(data.getBytes(),
                    0,
                    data.getBytes().length,
                    InetAddress.getByName("127.0.0.1"),
                    2222);

            socket.send(datagramPacket);
            if ("bye".equals(data)) {
                break;
            }
        }


        socket.close();
    }
}

```



## 多线程在线咨询

```java
// 发送方换皮，实现Runnable接口，完成run方法
package com.isaiah.UDP_3;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;
import java.net.SocketException;

public class TalkSend implements Runnable {
    private DatagramSocket socket = null;
    private BufferedReader bufferedReader = null;

    private int fromPort;
    private String toIP;
    private int toPort;

    public TalkSend() {

    }

    public TalkSend(int fromPort, String toIP, int toPort) {
        this.fromPort = fromPort;
        this.toIP = toIP;
        this.toPort = toPort;
        try {
            socket = new DatagramSocket(fromPort);
        } catch (SocketException e) {
            e.printStackTrace();
        }
    }

    @Override
    public void run() {
        while (true) {
            // 准备数据
            bufferedReader = new BufferedReader(new InputStreamReader(System.in));
            String data = null;
            try {
                data = bufferedReader.readLine();
                DatagramPacket datagramPacket = new DatagramPacket(data.getBytes(),
                        0,
                        data.getBytes().length,
                        InetAddress.getByName(toIP),
                        toPort);

                socket.send(datagramPacket);
                if ("bye".equals(data)) {
                    break;
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

        socket.close();
    }
}

```

```java
// 接收方换皮，实现Runnable接口，完成run方法
package com.isaiah.UDP_3;

import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.SocketException;

public class TalkReceive implements Runnable {
    DatagramSocket socket = null;
    private int ReceivePort;
    private String msgFrom;

    public TalkReceive(int receivePort, String msgFrom) {
        ReceivePort = receivePort;
        this.msgFrom = msgFrom;
        try {
            socket = new DatagramSocket(ReceivePort);
        } catch (SocketException e) {
            e.printStackTrace();
        }
    }

    @Override
    public void run() {
        try {
            while (true) {
                byte[] buffer = new byte[1024];
                DatagramPacket datagramPacket = new DatagramPacket(buffer, 0, buffer.length);
                socket.receive(datagramPacket);

                byte[] data = datagramPacket.getData();
                String dataString = new String(data, 0, data.length);
                System.out.println(msgFrom+" : "+dataString);

                if("bye".equals(dataString)){
                    break;
                }
            }
            socket.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

```java
// 学生端开启两个线程：发送线程和接收线程
package com.isaiah.UDP_3;

public class TalkStudent {
    public static void main(String[] args) {
        new Thread(new TalkSend(7777,"127.0.0.1", 8888)).start();
        new Thread(new TalkReceive(9999, "老师")).start();
    }
}

```

```java
// 教师端开启两个线程：发送线程和接收线程
package com.isaiah.UDP_3;

public class TalkTeacher {
    public static void main(String[] args) {
        new Thread(new TalkSend(1111, "127.0.0.1", 9999)).start();
        new Thread(new TalkReceive(8888, "学生")).start();
    }
}

```



##  URL下载网络资源

```java
// 可以
package com.isaiah.UrlDownLoad;

import javax.net.ssl.HttpsURLConnection;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.net.URL;

public class UrlDown {
    public static void main(String[] args) throws IOException {
        URL url = new URL("https://vdownload-50.sb-cd.com/1/2/12315889-480p.mp4?secure=8czuMxQbPxJJjOoOHJQN8Q,1665861685&m=50&d=1&_tid=12315889");

        HttpsURLConnection httpURLConnection = (HttpsURLConnection) url.openConnection();
        InputStream inputStream = httpURLConnection.getInputStream();
        FileOutputStream fileOutputStream = new FileOutputStream("1.mp4");

        byte[] buffer = new byte[1024];
        int len = 0;
        while((len = inputStream.read(buffer))!=-1){
            fileOutputStream.write(buffer, 0, len);
        }

        fileOutputStream.close();
        inputStream.close();
        httpURLConnection.disconnect();
    }
}

```

