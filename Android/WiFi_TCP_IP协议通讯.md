​		看了几天的Android WiFi通讯，总结一下，并附相关Demo(也是仿网上做的)，建议收藏

​		两台设备(手机与手机，手机与硬件都一样)在同一WiFi下通讯，用的是TCP/IP协议，那我们应如何使用？其实很简单：

​		服务端：用**ServerSocket**开启一个端口(port)，等待客户端连接，连接成功后可得到一个**Socket**，再用这个Socket就可以拿到**服务端**的**输入流**(InputStream)和**输出流**(OutputStream)；

​		客户端：通过服务端的**IP**(在局域网下的IP)和**Port**(ServerSocket开启的端口号)，用**Socket**进行连接，连接成功后当然就用这个Socket就可以拿到**客户端**的**输入流**(InputStream)和**输出流**(OutputStream)啦；

​		然后，服务端或客户端**向输出流里写**东西，另一端再从自己的输入流里读就完成了！~~

​		下面来看看具体实现

# WiFi通讯需要申请的权限

```xml
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
<uses-permission android:name="android.permission.CHANGE_WIFI_STATE"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
<uses-permission android:name="android.permission.CHANGE_NETWORK_STATE"/>
```

# 在同一局域网下通讯

## 1.服务端

### (1)启用一个服务端口

启用一个服务端口，并获得一个**ServerSocket**

```java
mServerSocket = new ServerSocket(5000);//port:5000
```

### (2)等待连接

定义一个用于**等待**客户端连接的线程

```java
/**
 * 连接线程
 * 得到Socket
 */
class SocketAcceptThread extends Thread{
    @Override
    public void run() {
        try {
            //等待客户端进行连接，此时accept会阻塞，直到建立连接
            mSocket = mServerSocket.accept();
        } catch (IOException e) {
            e.printStackTrace();
            return;//accept error
        }
        //连接成功后，启动消息接收线程
        startReader(mSocket);
    }
}

```

```java
//启动服务线程
SocketAcceptThread socketAcceptThread = new SocketAcceptThread();
socketAcceptThread.start();
```

### (3)接收消息

定义一个线程来接收客户端发来的消息

```java
private void startReader(final Socket socket) {
    new Thread() {
        @Override
        public void run() {
            DataInputStream reader;
            try {
                // 获取读取流
                reader = new DataInputStream(socket.getInputStream());
                while (true) {
                    // 读取数据
                    String msg = reader.readUTF();
                    Log.d(TAG, "客户端的信息:" + msg);

                    //告知客户端消息收到
                    DataOutputStream writer = new DataOutputStream(mSocket.getOutputStream());
                    writer.writeUTF("收到:" + msg); // 写一个UTF-8的信息

                    //向主线程发消息更新UI
                    Message message = new Message();
                    message.what = 1;
                    message.obj=msg;
                    handler.sendMessage(message);
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }.start();
}
```

### (4)关闭Socket

```java
	if (mServerSocket != null) {
            try {
                mServerSocket.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        if(mSocket!=null){
            try {
                mSocket.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
```

### (5)获取服务端ip地址

```java
public class NetWorkUtils {


    /**
     * 检查网络是否可用
     *
     * @param paramContext
     * @return
     */
    public static boolean checkEnable(Context paramContext) {
        boolean i = false;
        NetworkInfo localNetworkInfo = ((ConnectivityManager) paramContext
                .getSystemService("connectivity")).getActiveNetworkInfo();
        if ((localNetworkInfo != null) && (localNetworkInfo.isAvailable()))
            return true;
        return false;
    }

    /**
     * 将ip的整数形式转换成ip形式
     *
     * @param ipInt
     * @return
     */
    public static String int2ip(int ipInt) {
        StringBuilder sb = new StringBuilder();
        sb.append(ipInt & 0xFF).append(".");
        sb.append((ipInt >> 8) & 0xFF).append(".");
        sb.append((ipInt >> 16) & 0xFF).append(".");
        sb.append((ipInt >> 24) & 0xFF);
        return sb.toString();
    }

    /**
     * 获取当前ip地址
     *
     * @param context
     * @return
     */
    public static String getLocalIpAddress(Context context) {
        try {

            WifiManager wifiManager = (WifiManager) context
                    .getSystemService(Context.WIFI_SERVICE);
            WifiInfo wifiInfo = wifiManager.getConnectionInfo();
            int i = wifiInfo.getIpAddress();
            return int2ip(i);
        } catch (Exception ex) {
            return " 获取IP出错鸟!!!!请保证是WIFI,或者请重新打开网络!\n" + ex.getMessage();
        }
        // return null;
    }
}
```

## 2.客户端

### (1)进行连接

```java
	/**
     * 连接线程
     */
    class SocketConnectThread extends Thread{
        public void run(){
            try {
                //指定ip地址和端口号
                mSocket = new Socket(ipET.getText().toString(), 1989);
                //获取输出流、输入流
//                mOutStream = mSocket.getOutputStream();
//                mInStream = mSocket.getInputStream();
            } catch (Exception e) {
                e.printStackTrace();
                return;
            }
            startReader(mSocket);
        }

    }
```

```java
socketConnectThread = new SocketConnectThread();
socketConnectThread.start();
```

### (2)发送消息

```java
/**
 * 发送消息
 * @param msg
 */
public void sendMessage(final String msg) {
    if (msg.length() == 0){
        return;
    }
    new Thread() {
        @Override
        public void run() {
            try {
                DataOutputStream writer = new DataOutputStream(mSocket.getOutputStream());
                writer.writeUTF(msg); // 写一个UTF-8的信息
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }.start();
}
```

### (3)接收消息

```java
/**
 * 接收消息
 */
private void startReader(final Socket socket) {
    new Thread(){
        @Override
        public void run() {
            DataInputStream reader;
            try {
                // 获取读取流
                reader = new DataInputStream(socket.getInputStream());
                while (true) {
                    // 读取数据
                    String msg = reader.readUTF();
                    Message message = new Message();
                    message.what = 1;
                    message.obj=msg;
                    handler.sendMessage(message);
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }.start();
}
```

### (4)关闭Socket

```java
if(mSocket!=null){
    try {
        mSocket.close();
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

# 工具类WiFiModeUtil

​			***WiFiModeUtil类代码在后面***

## 1.WiFiModeUtil简介

>以下5种情况发生时
>
>1.正在连接
>
>2.连接失败
>
>3.连接成功
>
>4.接收到数据
>
>5.连接断开
>
>都会发出相应的广播

**各广播的ACtion如下：**

```java
/**
 * 正在连接广播Action:	"WiFiModeUtil.Connecting"
 * 连接失败广播Action:	"WiFiModeUtil.Connect.Fail"
 * 连接成功广播Action:	"WiFiModeUtil.Connect.Succeed"
 * 收到数据广播Action:	"WiFiModeUtil.Connect.ReceiveMessage"
 * 连接断开广播Action:	"WiFiModeUtil.Disconnected"
 */
```

> 你可以在接收到不同广播时
>
> 去完成相应动作



## 2.如何使用

### (1)在Activity中进行广播注册

> 广播注册
> 并给WiFiModeUtil工具类中的本地广播管理器赋值

这里我也已经写好了，直接**copy**到你的**Activity中**就行:

```java
/**
 * 广播注册
 * 并给WiFiModeUtil工具类中的本地广播管理器赋值
 */
private void registerBroadcastReceiver() {
    IntentFilter intentFilter = new IntentFilter();
    intentFilter.addAction("WiFiModeUtil.Connecting");//正在连接
    intentFilter.addAction("WiFiModeUtil.Connect.Succeed");//连接成功
    intentFilter.addAction("WiFiModeUtil.Connect.Fail");//连接失败
    intentFilter.addAction("WiFiModeUtil.Connect.ReceiveMessage");//接收到数据
    intentFilter.addAction("WiFiModeUtil.Disconnected");//接收到数据
    localBroadcastReceiver = new MyLocalBroadcastReceiver();
    localBroadcastManager = LocalBroadcastManager.getInstance(this);
    WiFiModeUtil.localBroadcastManager=localBroadcastManager;//给WiFiModeUtil工具类中的本地广播管理器赋值
    localBroadcastManager.registerReceiver(localBroadcastReceiver,intentFilter);
}
```

>对不同广播进行处理

直接**copy**到你的**Activity中**就行:

```java
	/**
     * 广播接收者
     */
    class MyLocalBroadcastReceiver extends BroadcastReceiver {
        @Override
        public void onReceive(Context context, Intent intent) {
            switch (intent.getAction()) {
                case "WiFiModeUtil.Connecting":
              		
                    break;
                case "WiFiModeUtil.Connect.Succeed":
                   
                    break;
                case "WiFiModeUtil.Connect.Fail":
                    
                    break;
                case "WiFiModeUtil.Connect.ReceiveMessage":
                    
                    break;
                case "WiFiModeUtil.Disconnected":
                    
                    break;
            }
        }
    }
```

### (2)进行连接

调用**静态**方法 ：

```java
WiFiModeUtil.connetByTCP(String Ip,int Port);
```

如：

```java
private String mIp;//硬件的IP
private int mPort = 5000;//硬件的端口
WiFiModeUtil.connetByTCP(mIp,mPort);//进行连接
```

### (3)发送数据

调用静态方法:

```java
WiFiModeUtil.sendData(String data);
```

如:

```java
WiFiModeUtil.sendData(et_msg.getText().toString());//发送数据
```

### (4)获取接收的数据

硬件发来的数据会被存在一个List中，访问数据如下：

```java
WiFiModeUtil.DataList.get(WiFiModeUtil.DataList.size()-1);
```

### (5)关闭连接

调用如下**静态**方法，关闭Socket释放资源:

```java
WiFiModeUtil.closeSocketAndStream();
```

当然也别忘了**注销广播**，如：

```java
@Override
protected void onDestroy() {
    localBroadcastManager.unregisterReceiver(localBroadcastReceiver);//注销广播
    WiFiModeUtil.closeSocketAndStream();//关闭Socket释放资源
    super.onDestroy();
}
```

## 3.WiFiModeUtil类代码

```java
package com.example.wifidemo;

import android.annotation.SuppressLint;
import android.content.Intent;
import android.os.CountDownTimer;
import android.os.Handler;
import android.os.Looper;
import android.os.Message;
import android.util.Log;

import androidx.localbroadcastmanager.content.LocalBroadcastManager;

import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.Socket;
import java.nio.ByteBuffer;
import java.nio.charset.StandardCharsets;
import java.util.ArrayList;
import java.util.List;

/**
 * 正在连接广播Action:"WiFiModeUtil.Connecting"
 * 连接失败广播Action:"WiFiModeUtil.Connect.Fail"
 * 连接成功广播Action:"WiFiModeUtil.Connect.Succeed"
 * 收到数据广播Action:"WiFiModeUtil.Connect.ReceiveMessage"
 * 连接断开广播Action:"WiFiModeUtil.Disconnected"
 */
public class WiFiModeUtil {
    private static String mIp;//硬件的IP
    private static int mPort = 5000;//硬件的端口
    public static Socket mSocket = null;//连接成功可得到的Socket
    public static OutputStream outputStream = null;//定义输出流
    public static InputStream inputStream = null;//定义输入流
    public static StringBuffer DataRecivice = new StringBuffer();//数据
    public static List<String> DataList=new ArrayList<>();//数据
    public static boolean connectFlage = true;//连接成功或连接3s后变false
    //    private static int ShowPointSum = 0;//连接时显示 连接中.. 后面点的计数
    private static final String TAG = "WifiDemoLogESP8266ClientActivity";

    /**
     * 本地广播管理器 从外面用:
     * WiFiModeUtil.localBroadcastManager=localBroadcastManager;
     * 进行赋值
     */
    public static LocalBroadcastManager localBroadcastManager;

    /**
     * 处理消息的Handler
     */
    @SuppressLint("HandlerLeak")
    public static Handler mHandler = new Handler(Looper.myLooper()) {
        @Override
        public void handleMessage(Message msg) {
            super.handleMessage(msg);
            switch (msg.what) {
                case 0://接收到数据
                    DataRecivice.append(msg.obj.toString());
                    DataRecivice.append("\n");
                    DataList.add(msg.obj.toString());//添加数据
                    Intent intent = new Intent("WiFiModeUtil.Connect.ReceiveMessage");
                    localBroadcastManager.sendBroadcast(intent);//发送收到数据广播
                    break;
                case 1://连接成功
                    Intent intent2 = new Intent("WiFiModeUtil.Connect.Succeed");
                    localBroadcastManager.sendBroadcast(intent2);//发送连接成功广播
                    readData();//开启接收线程
                    connectFlage = true;
                    break;
                case 2://连接断开
                    Intent intent3 = new Intent("WiFiModeUtil.Disconnected");
                    localBroadcastManager.sendBroadcast(intent3);//发送连接失败广播
                    connectFlage = true;
                    break;
            }
        }
    };

    /***
     * 延时3s的定时器
     * 在开始连接时计时3s
     * 3s未连接上视为连接失败
     */
    private final static CountDownTimer tcpClientCountDownTimer = new CountDownTimer(3000, 300) {
        @Override
        public void onTick(long millisUntilFinished) {//每隔300ms进入
            if (connectFlage) {
                Intent intent = new Intent("WiFiModeUtil.Connecting");
                localBroadcastManager.sendBroadcast(intent);
            }
        }

        @Override
        public void onFinish() {//3s后进入(没有取消定时器的情况下)
            if (connectFlage) {
                connectFlage = false;//连接失败
                closeSocketAndStream();
            }
            tcpClientCountDownTimer.cancel();//关掉定时器
            Intent intent = new Intent("WiFiModeUtil.Connect.Fail");
            localBroadcastManager.sendBroadcast(intent);
            Log.d(TAG,"连接失败");
        }
    };

    /**
     * 关掉Socket和输入输出流
     */
    public static void closeSocketAndStream() {
        if (outputStream != null) {
            try {
                outputStream.close();
                Log.d(TAG,"关闭输出流");
            } catch (IOException e) {
                e.printStackTrace();
            }
            outputStream = null;
        }
        if (inputStream != null) {
            try {
                inputStream.close();
                Log.d(TAG,"关闭输入流");
            } catch (IOException e) {
                e.printStackTrace();
            }
            inputStream = null;
        }
        if (mSocket != null) {
            try {
                mSocket.close();
                Log.d(TAG,"关闭Socket");
            } catch (IOException e) {
                e.printStackTrace();
            }
            mSocket = null;
        }
    }

    /**
     * 连接服务器任务
     */
    static class ConnectSeverThread extends Thread {
        @Override
        public void run() {
            Log.d(TAG,"连接线程开启");
            while (connectFlage) {
                try {
                    Log.d(TAG,"正在连接...");
                    mSocket = new Socket(mIp, mPort);//进行连接
                    connectFlage = false;//已连接
                    tcpClientCountDownTimer.cancel();//关掉计时器
                    /*连接成功更新显示连接状态的UI*/
                    Message msg = new Message();
                    msg.what = 1;
                    mHandler.sendMessage(msg);
                    Log.d(TAG,"连接成功");
                    inputStream = mSocket.getInputStream();//获取输入流
                    Log.d(TAG,"获取输入流");
                    outputStream = mSocket.getOutputStream();////获取输出流
                    Log.d(TAG,"获取输出流");
                } catch (IOException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                    Log.d(TAG,"连接过程中出错");
                }
            }
        }
    }

    /**
     * 传入硬件服务端IP建立TCP连接
     * 正在连接每200ms          发送一条‘正在连接’广播
     * 3秒后还未连接则连接失败    发送一条‘失败广播’
     * 连接成功会              发送一条‘成功广播’
     *
     * @param IPAdress
     */
    public static void connetByTCP(String IPAdress,int Port) {
        mIp = IPAdress;
        mPort=Port;
        ConnectSeverThread connectSeverThread = new ConnectSeverThread();
        connectSeverThread.start();
        tcpClientCountDownTimer.start();
    }

    /**
     * 向硬件发送数据
     */
    public static void sendData(String data) {
        if(mSocket!=null){
            byte[] sendByte = data.getBytes();
            new Thread() {
                @Override
                public void run() {
                    try {
                        Log.d(TAG,"发送线程开启 正在发送中...");
                        DataOutputStream writer = new DataOutputStream(outputStream);
                        writer.write(sendByte, 0, sendByte.length);
                        Log.d(TAG,"已发送数据:"+data);
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                    Log.d(TAG,"发送线程结束");
                }
            }.start();
        }
    }

    /**
     * 连接成功后开启
     * 接收硬件发送的数据
     */
    public static void readData() {
        new Thread() {
            @Override
            public void run() {
                Log.d(TAG,"接收线程开启");
                try {
                    while (true) {
                        Thread.sleep(200);
                        //如果连接断开 尝试重连
                        try {
                            /*
                                sendUrgentData()方法
                                它会往输出流发送一个字节的数据，
                                只要对方Socket的SO_OOBINLINE属性没有打开，
                                就会自动舍弃这个字节，
                                就会抛出异常，
                                而SO_OOBINLINE属性默认情况下就是关闭的
                             */
                            mSocket.sendUrgentData(0xFF);//发送1个字节的紧急数据，默认情况下，服务器端没有开启紧急数据处理，不影响正常通信
                        } catch (Exception ex) {
                            Log.d(TAG,"连接已断开，请重新进行连接");
                            Message msg=new Message();
                            msg.what=2;
                            msg.obj="连接已断开，请重新进行连接";
                            mHandler.sendMessage(msg);
                        }
                        DataInputStream reader = new DataInputStream(inputStream);
                        byte[] buffer = new byte[1024];
                        int len;
                        while ((len = reader.read(buffer)) != -1) {
                            String data = new String(buffer, 0, len);
                            Log.d(TAG,"接收到数据:"+data);
                            Message msg = new Message();
                            msg.what = 0;
                            msg.obj = data;
                            mHandler.sendMessage(msg);
                        }
                    }
                } catch (IOException | InterruptedException e) {
                    e.printStackTrace();
                }
                Log.d(TAG,"接收线程结束");
            }
        }.start();
    }
}
```

# WiFiDemo地址

[KWJYES/WiFiDemo (github.com)](https://github.com/KWJYES/WiFiDemo)

