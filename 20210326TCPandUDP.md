##复习

netstat -ano|findstr 8080 

找端口是不是被占用

InputStream()    还需要文件输出流

openStream()

URLConnection    openConnection()

URLConnection类 getInputStream()  getOutputStream()

实现TCP示例

1.客户端与服务器端的单次发送（注意：客户端/服务器端 不能都是先收后发）

2.服务器接收多次通讯

3.编程实例：聊天程序 ;

```java

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;

public class Server1 {
	public static void main(String[] args) throws IOException {
		
//		1、创建ServerSocket，监听特定端口
		ServerSocket server=new ServerSocket(9000);
		
//		2、等待客户端连接
		System.out.println("等待客户端连接");
        
        //方法中用ServerSocket.accept()方法来监听客户机
        
		Socket client=server.accept();//在这阻塞    accept源码在下方
		
        System.out.println("终于有客户端连接了");
		
//		3、传输数据
//		接收客户端数据
		
		
		InputStream is=client.getInputStream();
		byte[] b=new byte[1024];//接收到的数据需要有空间；
		is.read(b);//目前暂时理解为读取    网络编程Socket之TCP之read/write

		System.out.println("收到客户端的信息："+new String(b));

		OutputStream os=client.getOutputStream();
		os.write("我是Server".getBytes());
		
//		3、关闭连接
		is.close();
		os.close();
		client.close();
		server.close();				
		
	}

}

```







```java
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.InetAddress;
import java.net.Socket;
import java.net.UnknownHostException;

//例1:客户端与服务器端的单次发送
public class Client1 {

	public static void main(String[] args) throws UnknownHostException, IOException {
//		1、创建Socket对象，与服务器建立连接
		Socket client=new Socket("127.0.0.1",9000);
		
//		2、连接成功，通过流的方式传输数据
		
		
		InputStream is=client.getInputStream();
		byte[] b=new byte[1024];
		is.read(b);
		System.out.println("收到Server端的数据："+new String(b));
		
		String ip=InetAddress.getLocalHost().getHostAddress();
		OutputStream os=client.getOutputStream();
		os.write((ip+",sunnygao").getBytes());
        
		os.flush();
        
        /*
        flush()的理解，比如  信息比作水，水闸关了，水管中还有水，那就flush一下啊  强制性全倒出来，清理水管里留下水   close就是直接关闸门；
        
        面试题：close()和flush()的区别？
A:close()关闭流对象，但是先刷新一次缓冲区，关闭之后，流对象不可以继续再使用了。
B:flush()仅仅是刷新缓冲区(一般写字符时要用,因为字符是先进入的缓冲区)，流对象还可以继续使用
        */
		
	
//		3、关闭网络连接
		os.close();
		is.close();
		client.close();		
	}

}

```





1.客户端与服务器端的单次发送（注意：客户端/服务器端 不能都是先收后发）

2.服务器接收多次通讯

3.编程实例：聊天程序 ;



```java
//例一 客户端与服务器端单次
/*
1.创建SeverSocket
2.accept等待客户端连接
3.数据传输
4.关闭连接

*/
//Sever
SeverSocket sever = new SeverSocket(9999);

SocketServer socket = sever.accept();

//接收数据，使用输入流

//导包不对会报错
InputStream is = socket.getInputStream();
byte[] b = new byte[1024];
is.read(b);
System.out,println("客户端的信息" + new String(b));

//发送数据，输出流
//这里可能会有疑问啊？？？    这个client 是哪里的呢？

OutputStream os = client.getOutputStream();

os.write("我是sever".getBytes());
os.close();
is.close();
client.close();


sever.close();
```



```java
/*
1。创建Socket连接
2.数据传输
3.关闭连接

*/
//Client
Socket client = new Socket("127.0.0.1",9999);
//发送数据

String ip = InterAddress.getLocalHost().getHostAddress();//获取本地IP
String mes = "aaa";

OutputStream os = client.getOutputStream();

os.write(mse.getBytes());

os.flush();

//接收数据

InputStream is = client.getInputStream();
byte[] b = new byte[1024];
is.read(b);
System.out.printlnI(new String(b));

os.close;
is.close;
```



## 多次(点名操作)

监听功能

```java
/**
 * 
 1、创建ServerSocket 
 2、accept等待客户端连接 
 3、数据传输 
 4、关闭连接
 *
 */
public class Server2 {

	public static void main(String[] args) throws IOException {
		ServerSocket server = new ServerSocket(9999);
		System.out.println("等待客户端连接");
		int count=1;

		while (true) {
			Socket client = server.accept();

			// 接收数据，使用输入流
            
			InputStream is = client.getInputStream();
            
			byte[] b = new byte[1024];
			is.read(b);
			System.out.println("客户端的信息是：" + new String(b));

			//	发送数据，输出流
			OutputStream os = client.getOutputStream();
			os.write("我是server!我接收到了你的信息".getBytes());

			os.close();
			is.close();
			client.close();
			count++;
            
            //接收88次
			if(count==88) {
				break;
			}
		}
		server.close();

	}

}
```



```java
//实例2：实现服务器端接收客户端的多次通讯，点名

/**
 * 
 * 
 * 1、创建Socket,与服务器建立连接
 * 2、数据传输
 * 3、关闭连接
 *
 */
public class Client1 {

	public static void main(String[] args) throws UnknownHostException, IOException {
		Socket client=new Socket("127.0.0.1",9999);
		//发送数据，ip:姓名
		
		String  ip=InetAddress.getLocalHost().getHostAddress();//获取本地IP
		String msg=ip+":谦虚低调 高开高走";
		
		//发送数据
		OutputStream os=client.getOutputStream();
		os.write(msg.getBytes());
		os.flush();
	
		//接收数据
		InputStream is=client.getInputStream();
		byte[] b=new byte[1024];
		is.read(b);
		System.out.println(new String(b));
		
		is.close();
		os.close();
		client.close();
		
		

	}

}
```

和上面是一样的，就是为了证明  Sever可以接收两个人的消息。

```java

/**
 * 
 * 1、创建Socket,与服务器建立连接
 * 2、数据传输
 * 3、关闭连接
 *
 */
public class Client2 {

	public static void main(String[] args) throws UnknownHostException, IOException {
		Socket client=new Socket("127.0.0.1",9999);
//		发送数据，ip:姓名
		
		String  ip=InetAddress.getLocalHost().getHostAddress();//获取本地IP
		String msg=ip+"背叛懒惰！！";
		
		//发送数据
		OutputStream os=client.getOutputStream();
		os.write(msg.getBytes());
		os.flush();
	
		//接收数据
		InputStream is=client.getInputStream();
		byte[] b=new byte[1024];
		is.read(b);
		System.out.println(new String(b));
		
		is.close();
		os.close();
		client.close();
		
		

	}

}
```





```java
int count = 1;
while(ture){
    SocketServer socket = sever.accept();

//接收数据，使用输入流

//导包不对会报错
	InputStream is = socket.getInputStream();
	byte[] b = new byte[1024];
	is.read(b);
	System.out,println("客户端的信息" + new String(b));

//发送数据，输出流
    OutputStream os = client.getOutputStream();
	os.write("我是sever".getBytes());
	os.close();
	is.close();
	client.close();
    count++;
    if(count == 87){
        break;//接收87个人的信息就关闭；
    }
}
sever.close();
```

## 编程实例：聊天程序 ;

实现多线程，主线程和子线程；主线程创建SeverSocket  ;

```java
/*
主线程负责等待

*/
class SeverThread extends Thread{
    private Socket client;
    public SeverThread(Socket client){
        this.client = client
    }
    
    public void run(){
        InputStream is = socket.getInputStream();
		byte[] b = new byte[1024];
		is.read(b);
		System.out,println("客户端的信息" + new String(b));

//发送数据，输出流
		OutputStream os = client.getOutputStream();
		os.write("我是sever".getBytes());
		os.close();
		is.close();
		client.close();
    }
}
public static void main(String){
    
}
```



```java
int count = 1;
while(ture){
    SocketServer socket = sever.accept();

//接收数据，使用输入流

//导包不对会报错
	InputStream is = socket.getInputStream();
	byte[] b = new byte[1024];
	is.read(b);
	System.out,println("客户端的信息" + new String(b));

//发送数据，输出流
    OutputStream os = client.getOutputStream();
	os.write("我是sever".getBytes());
	os.close();
	is.close();
	client.close();
    count++;
    if(count == 87){
        break;//接收87个人的信息就关闭；
    }
}
sever.close();
```



```java
package com.lhz.tcpdemo4;

import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.OutputStreamWriter;
import java.net.InetAddress;
import java.net.Socket;
import java.net.UnknownHostException;
import java.util.Scanner;

//实例4：客户端键盘输入，使用字符流

/**
 * 
 * @author Administrator
 * 1、创建Socket,与服务器建立连接
 * 2、数据传输
 * 3、关闭连接
 *
 */
public class Client4 {

	public static void main(String[] args) throws UnknownHostException, IOException {
		Socket client=new Socket("127.0.0.1",9999);
//		发送数据，ip:姓名
		
//		String  ip=InetAddress.getLocalHost().getHostAddress();//获取本地IP
//		String msg=ip+":李焕贞1";
		Scanner scan=new Scanner(System.in);
		
		//用低级的字节流构造高级的字符流
//		BufferedWriter writer=new BufferedWriter
//				(new OutputStreamWriter
//						(client.getOutputStream()));
		
		OutputStreamWriter writer1=new OutputStreamWriter
						(client.getOutputStream());
		
		BufferedWriter writer=new BufferedWriter(writer1);
		
//		OutputStream os=client.getOutputStream();
//		os.write(msg.getBytes());
//		os.flush();
		
		System.out.println("请输入，exit，输入结束");
		while(true) {
			String msg=scan.nextLine();
			writer.write(msg+"\n");
			writer.flush();
			if(msg.equals("exit")) {
				break;
			}
			
		}
		
	
		//接收数据
//		InputStream is=client.getInputStream();
//		byte[] b=new byte[1024];
//		is.read(b);
//		System.out.println(new String(b));
		System.out.println("发送结束！");
		writer.close();
		client.close();
		
		

	}

}

```



```java
package com.lhz.tcpdemo4;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;

public class Server4 {

	public static void main(String[] args) throws IOException {
		ServerSocket server = new ServerSocket(9999);
		System.out.println("等待客户端连接");

		Socket client = server.accept();
		System.out.println("收到客户端连接");
		// 接收数据，使用输入流
		BufferedReader reader=new BufferedReader
				(new InputStreamReader
						(client.getInputStream()));
		
//		InputStream is = client.getInputStream();
		while(true) {
			String message=reader.readLine();
			System.out.println(message);
//			如果不再发送了，还接收，就是null
			if(message==null) {
				break;
			}
			
		}
	

//	发送数据，输出流
//		OutputStream os = client.getOutputStream();
//		os.write("我是server!我接收到了你的信息".getBytes());

		System.out.println("服务器端接收数据结束");
		reader.close();

		client.close();

		server.close();

	}

}

```



总结：

TCP编程，分为服务器端与客户端。

客户端：

1.创建Socket对象，构造方法含服务器的Ip和端口

2.通过Socket对象获得输入流、输出流，与服务器端进行交互

接收数据

发送数据

3.关闭流，关闭Socket对象，释放资源



服务器端

1.创建SeverSocket对象，含端口号

2.accept方法产生阻塞，直到有客户端连接，获取Socket对象，

3.通过Socket对象获得输入流、输出流，与客户端进行交互

4.关闭流，关闭Socket对象，释放资源。



# UDP

•建立网络连接时，有两种传输层协议（TCP传输协议和UDP传输协议）

–UDP传输协议：一种无连接的传输层协议，提供面向事务的简单不可靠信息传送服务

```java
sender
/*
1.DatagramSocket对象，如果不需要接收数据，不需要设端口号

2.准备数据，转换成字节数组；
3.创建报文对象DatagramPacket,需要指定目的ip和端口
4.send发送数据
5.关闭socket对象；
*/
package com.lhz.udpdemo1;

import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;
import java.net.SocketException;

//发送端
//考虑发送者怎么接收呢？接收方怎么发送数据？
public class Sender {

	public static void main(String[] args) throws IOException {
//		1、创建DatagramSocket对象，如果不需要接收数据，不需要设端口号
		DatagramSocket socket=new DatagramSocket();
//		2、准备数据，转换成字节数组
		byte[] msg="hello，客户端!".getBytes();
		
//		3、创建报文对象DatagramPacket，需要指定目的地ip和端口
		DatagramPacket packet=new DatagramPacket
				(msg,msg.length,InetAddress.getByName("127.0.0.1"),9000);
		
		System.out.println("准备发送");
//		4、send方法发送数据
		socket.send(packet);
		System.out.println("发送完成");
		
		
		
//		5、关闭socket对象
		socket.close();
		
		

	}

}

```



```java
reciver
/*
1.创建DatagramSocket对象，需要指定端口号；
2.创建DatagramPacket
3.阻塞式接收
4.分析数据
*/


import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.SocketException;

public class Recevier {

	public static void main(String[] args) throws IOException {
//		1、创建DatagramSocket对象，需要指定端口号
		DatagramSocket socket = new DatagramSocket(9000);

		byte[] buf = new byte[1024];
//		2、创建DatagramPacket
		DatagramPacket packet = new DatagramPacket(buf, buf.length);
		System.out.println("准备接收");
//	3、阻塞式接收	
		socket.receive(packet);
		System.out.println("接收完成");
//		4、分析数据
		byte[] cache = packet.getData();
		System.out.println(new String(cache));
//		5、关闭DatagramSocket
		socket.close();

	}

}

```



###UDP接收方收到信息之后，再发送信息

```java

import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;
import java.net.SocketException;
//接收方收到信息之后，再发送信息
public class Recevier2 {

	public static void main(String[] args) throws IOException {
//		1、创建DatagramSocket对象，需要指定端口号
		DatagramSocket socket = new DatagramSocket(9000);

		byte[] buf = new byte[1024];
//		2、创建DatagramPacket
		DatagramPacket packet = new DatagramPacket(buf, buf.length);
		System.out.println("准备接收");
//	3、阻塞式接收	
		socket.receive(packet);
		System.out.println("接收完成");
//		4、分析数据
		byte[] cache = packet.getData();
		System.out.println(new String(cache));
		
		DatagramPacket packet2 = new DatagramPacket
				("我收你的信息".getBytes(),"我收你的信息".getBytes().length,InetAddress.getByName("127.0.0.1"),9001);
		
		socket.send(packet2);
		
//		5、关闭DatagramSocket
		socket.close();

	}

}

```





```java


import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;
import java.net.SocketException;

//发送方，发送完信息之后，再接收信息
public class Sender2 {

	public static void main(String[] args) throws IOException {
//		1、创建DatagramSocket对象，如果不需要接收数据，不需要设端口号
		DatagramSocket socket=new DatagramSocket(9001);
//		2、准备数据，转换成字节数组
		byte[] msg="hello，我是发送方!".getBytes();
		
//		3、创建报文对象DatagramPacket，需要指定目的地ip和端口
		DatagramPacket packet=new DatagramPacket
				(msg,msg.length,InetAddress.getByName("127.0.0.1"),9000);
		
		System.out.println("准备发送");
//		4、send方法发送数据
		socket.send(packet);
		System.out.println("发送完成");
		
		
		byte[] buf=new byte[1024];
		DatagramPacket packet2 = new DatagramPacket(buf, buf.length);
		socket.receive(packet2);
		byte[] buffer=packet2.getData();
		System.out.println(new String(buffer));
		
		
		
		
		
//		5、关闭socket对象
		socket.close();
		
		

	}

}

```





```java
public class InetAddressDemo1 {

	public static void main(String[] args) throws UnknownHostException {
		
		//获取本机IP
		InetAddress addr1=InetAddress.getLocalHost();
		System.out.println(addr1.getHostAddress());
		System.out.println(addr1.getHostName());

		InetAddress addr2=InetAddress.getByName("www.baidu.com");
		System.out.println(addr2.getHostAddress());
		System.out.println(addr2.getHostName());
		
		
		InetAddress addr3=InetAddress.getByName("39.156.66.14");
		System.out.println(addr3.getHostAddress());
		System.out.println(addr3.getHostName());
		//返回的是ip，不是域名，因为DNS服务器不允许进行ip和域名的映射
	}

}

```



```java
import java.net.MalformedURLException;
import java.net.URL;

public class URLDemo1 {
	public static void main(String[] args) throws MalformedURLException {
		URL url=new URL("http://www.edu2act.cn/team/2019-ji-kai-yuan-ce-shi-kuang-jia/members/?status=joined");
		
		System.out.println(url.getProtocol());
		System.out.println(url.getHost());
		System.out.println(url.getPort());
		System.out.println(url.getFile());
		System.out.println(url.getPath());
		System.out.println(url.getQuery());		
		
	}

}

```



```java
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.MalformedURLException;
import java.net.URL;

/**
 * 方法一：下载网络上图片:网络输入流和本地输出流
 * URL类只能下载，不能发数据，因为没有获取输出流的方法
 * 如果向服务器发数据，需要借助URLConnection
 * 
 * @author Administrator
 *
 */
public class URLDemo2 {

	public static void main(String[] args) throws IOException {
		String baidu_logo = "https://www.baidu.com/img/flexible/logo/pc/result.png";
//		1、创建URL对象，对应网络资源
		URL img_url = new URL(baidu_logo);

//		2、获取输入流
		InputStream inStream = img_url.openStream();

//		3、输出流用来保存文件
		OutputStream outStream = new FileOutputStream("d:\\demo\\baidu202103191.png");

//		4、循环读写
		int n = -1;
//		while (true) {
//
//			n = inStream.read();
//			outStream.write(n);
//			if (n == -1) {
//				break;
//			}
//
//		}
		while ((n = inStream.read())!=-1) {
			outStream.write(n);
		
		}
		
		
//5、关闭流
		inStream.close();
		outStream.close();
		System.out.println("下载完成");

	}

}

```



```java
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.MalformedURLException;
import java.net.URL;
import java.net.URLConnection;

/**
 * 方法二：下载网络上图片:网络输入流和本地输出流
 * 获取URLConnection有两种方式
 * 1、URL的方法openConnection
 * 2、构造方法 URLConnection(URL url) 
 * 
 * @author Administrator
 *
 */
public class URLDemo3 {

	public static void main(String[] args) throws IOException {
		String baidu_logo = "https://www.baidu.com/img/flexible/logo/pc/result.png";
//		1、创建URL对象，对应网络资源
		URL img_url = new URL(baidu_logo);
//		2、创建URLConnection
		URLConnection conn=img_url.openConnection();
		
		InputStream is=conn.getInputStream();
		
		
		OutputStream os=new FileOutputStream("d://demo//baidu002.png");
		
		int n=-1;
		while((n=is.read())!=-1) {
			os.write(n);
		}
		is.close();
		os.close();
		
		
		


	}

}

```



```java
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.HttpURLConnection;
import java.net.MalformedURLException;
import java.net.URL;
import java.net.URLConnection;

/**

 * @author Administrator
 *
 */
public class URLDemo4 {

	public static void main(String[] args) throws IOException {
		String baidu_logo = "https://www.baidu.com/";
//		1、创建URL对象，对应网络资源
		URL img_url = new URL(baidu_logo);
//		2、创建URLConnection
		HttpURLConnection http_conn=(HttpURLConnection)img_url.openConnection();
		
	
//		http_conn.setRequestMethod("GET");
//		http_conn.set				
		
//		is.close();
	
	}

}

```

### UDP  type



```java
package udpdemo3_type;

import java.io.BufferedInputStream;
import java.io.ByteArrayInputStream;
import java.io.DataInputStream;
import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.SocketException;

public class Recevier {

	public static void main(String[] args) throws IOException {
//		1、创建DatagramSocket对象，需要指定端口号
		DatagramSocket socket = new DatagramSocket(9000);

		byte[] buf = new byte[1024];
//		2、创建DatagramPacket
		DatagramPacket packet = new DatagramPacket(buf, buf.length);
		System.out.println("准备接收");
//	3、阻塞式接收	
		socket.receive(packet);
		System.out.println("接收完成");
//		4、分析数据
		byte[] cache = packet.getData();
//		System.out.println(new String(cache));
		
		BufferedInputStream bis=
				new 		BufferedInputStream
				(new ByteArrayInputStream(cache));
		DataInputStream dis=new DataInputStream(bis);
		System.out.println(dis.readDouble());
		System.out.println(dis.readBoolean());
		System.out.println(dis.readUTF());
		
		
		
//		5、关闭DatagramSocket
		socket.close();

	}

}

```



```java
package udpdemo3_type;

import java.io.BufferedOutputStream;
import java.io.ByteArrayOutputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.io.ObjectOutputStream;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;
import java.net.SocketException;

//发送内容是基本数据类型
public class Sender {

	public static void main(String[] args) throws IOException {
//		1、创建DatagramSocket对象，如果不需要接收数据，不需要设端口号
		DatagramSocket socket=new DatagramSocket();
//		2、准备数据，转换成字节数组
//		byte[] msg="hello，客户端!".getBytes();
		
		
//		ByteArrayOutputStream bos=new ByteArrayOutputStream();
//		DataOutputStream dos=new DataOutputStream(new BufferedOutputStream
//				(bos));
//		dos.writeDouble(1.1);
//		dos.writeBoolean(true);
//		dos.writeUTF("测试2019");
		
		Student s1=new Student("tom");
		
		ByteArrayOutputStream bos=new ByteArrayOutputStream();
		ObjectOutputStream dos=new ObjectOutputStream(new BufferedOutputStream
				(bos));
		dos.writeObject(s1);
		dos.flush();
		
		byte[] msg=bos.toByteArray();
		dos.close();
		
		
		
		
//		3、创建报文对象DatagramPacket，需要指定目的地ip和端口
		DatagramPacket packet=new DatagramPacket
				(msg,msg.length,InetAddress.getByName("127.0.0.1"),9000);
		
		System.out.println("准备发送");
//		4、send方法发送数据
		socket.send(packet);
		System.out.println("发送完成");
		
		
		
//		5、关闭socket对象
		socket.close();
		
		

	}

}

```



```java
package udpdemo3_type;

import java.io.Serializable;

public class Student implements Serializable{
	private String name;
	public Student(String name){
		this.name=name;
	}
	

}

```

