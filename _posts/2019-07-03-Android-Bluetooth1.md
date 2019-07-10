---
layout: post
title:  "Android 蓝牙开发（1）"
date:   2019-07-03 15:14:54
categories: Android
tags: 蓝牙
excerpt: 
mathjax: true
author: sydMobile
---
* content
{:toc}












### 普通蓝牙设备官方文档

**Android 平台包含蓝牙网络堆栈支持**，凭借此支持，设备能以无线方式与其他蓝牙设备交换数据。应用框架提供了通过 Android Bluetooth API 访问蓝牙功能的途径。使用 Bluetooth API Android 应用可以执行下面的操作：

- 扫描其他蓝牙设备
- 查询本地蓝牙适配器的配对蓝牙设备
- 建立 RFCOMM 通道
- 通过服务发现连接到其他设备
- 与其他设备进行双向数据传输
- 管理多个连接

传统蓝牙适用于电池使用强度较大的操作，例如 Android 设备之间的流传输和通信等。针对具有低功耗要求的蓝牙设备，Android 4.3（API 18）中引入了面向低功耗蓝牙的 API 支持。

#### 基础知识

使用 Android Bluetooth API 来完成使用蓝牙进行通信的四项主要任务：**设置蓝牙**、**查找局部区域内的配对设备**或可用设备、**连接设备**，以及在**设备之间传输数据**。

关于蓝牙的 API 在 `android.bluetooth` 包中，下面介绍一下和蓝牙相关的主要类

**BluetoothAdapter**

本地蓝牙适配器，是所有**蓝牙交互的入口点**，表示蓝牙设备自身的一个蓝牙设备适配器，**整个系统只有一个蓝牙适配器**。通过它可以发现其他蓝牙设备，查询绑定（配对）设备的列表，使用已知的 Mac 地址实例化 `BluetoothDevice` 以及创建 `BluetoothServerSocket` 用来侦听来自其他设备的通信。

**BluetoothDevice**

表示远程的蓝牙设备。利用它可以通过 `BluetoothSocket` 请求与某个远程设备建立连接，或查询有关该设备的信息，例如设备的名称、地址、类和绑定状态等。

**BluetoothSocket**

表示蓝牙套接字接口（与 TCP Socket 相似）。这是允许应用通过 `InputStream` 和 `OutputStream` 与其他蓝牙设备交换数据的节点。正是利用这个对象来完成蓝牙设备间的数据交换，

**BluetoothServerSocket**

表示用于侦听传入请求的开发服务器套接字（类似于 TCP ServerSocket）要连接两台 Android 设备，其中一台设备必须使用此类开发的一个服务器套接字。当一台远程蓝牙设备向此设备发出连接请求时，`BluetoothServerSocket` 将会在接受连接后返回已连接的 `BluethoothSocket`。

**BluetoothClass**

描述蓝牙设备的一般特性和功能。这是一组只读属性，用于定义设备的主要和次要设备类及其服务。不过，它不能可靠地描述设备支持的所有蓝牙配置文件和服务，而是适合作为设备类型提示。

**BluetoothProfile**

表示蓝牙配置文件的接口。蓝牙配置文件是适用于设备间**蓝牙通信的无线接口规范**。免提配置文件便是一个示例。如需了解关于配置文件的详细讨论，参考下面配置文件的讲解

**BluetoothHeadset**

提供蓝牙耳机支持，以便与手机配合使用。其中包括蓝牙耳机和免提（1.5版）配置文件。**BluetoothProfile 的实现类**

**BlutoothA2dp**
定义**高质量音频**如何通过蓝牙连接和流式传输，从一台设备传输到另一台设备。“A2DP”代表高级音频分发配置文件。是 **BluetoothProfile 的实现类**

**BluetoothHealth** 
表示用于控制蓝牙服务的健康设备配置文件代理。 **BluetoothProfile 的实现类。**

**BluetoothGatt**

**BluetoothProfile 的实现类**。与低功耗蓝牙通信有关的配置文件代理

**BluetoothHealthCallback**
用于实现 BluetoothHealth 回调的抽象类。必须扩展此类并实现回调方法，以接收关于应用注册状态和蓝牙通道状态变化的更新内容。
**BluetoothHealthAppConfiguration** 
表示第三方蓝牙健康应用注册的应用配置，以便与远程蓝牙健康设备通信
**BluetoothProfile.ServiceListener**
在 BluetoothProfile IPC 客户端连接到服务（即，运行特定配置文件的内部服务）或断开服务连接时向其发送通知的接口。

#### 蓝牙权限

使用蓝牙必须声明权限 **BLUETOOTH** 才可以执行蓝牙通信。

```xml
 <mainifest>
    <uses-permission android:name = "android.permission.BLUETOOTH"/>
    <!--启用应用启动设备发现或者操作蓝牙设备的超级管理员-->
    <uses-permission android:name="android.permission.BLUETOOTH_ADMIN"/>
</mainifest>
```

#### 设置蓝牙

1. **获取蓝牙适配器**
   所有的蓝牙 Activity 都是需要 BluetoothAdapter 的。获取 BluetoothAdapter 调用 BluetoothAdapter 的静态方法 `getDefaultAdapter()` 方法。会返回一个表示设备自身的蓝牙适配器（蓝牙无线装置）的 BluetoothAdapter。
   整个系统有一个蓝牙适配器，我们的应用可以通过 BluetoothAdapter 这个对象与之交互。如果 `getDefaultAdapter()`返回 **null 则说明该设备不支持蓝牙**。

```java
    
    BluetoothAdapter mBluetoothAdapter = BluetoothAdapter.getDefaultAdapter();
    if(mBluetoothAdapter == null){
        // 说明此设备不支持蓝牙操作
    }
```

2. **启用蓝牙**
   需要确认是否已经开启蓝牙`isEnabled()`。返回 false 则说明蓝牙处于关闭状态。请求启用蓝牙。使用 `ACTION_REQUEST_ENABLE` 操作 Intent 调用 `startActivityForResult()`将通过系统设置发出启用蓝牙的请求。也可以通过 `mBluetoothAdapter.enable()` 直接打开蓝牙。

```java
   // 没有开始蓝牙
   if(!mBluetoothAdapter.isEnabled()){
        Intent enableBtIntent = new Intent(BluetoothAdapter.ACTION_REQUEST_ENABLE);
        startActivityForResult(enableBtIntent,REQUEST_ENBLE_BT);
   }
```

我们的应用也可以选择侦听 `ACTION_STATE_CHANGED` 广播 Intent。每当蓝牙状态发生变化时，系统会广播此 Intent。此广播包含额外字段 `EXTRA_STATE` 和 `EXTRA_PREVIOUS_STATE` 分别表示新的和旧的蓝牙状态。

#### 查找设备

使用 BluetoothAdapter 可以通过设备发现或通过查询配对设备的列表来查找远程蓝牙设备。
设备发现是一个扫描过程，它会搜索局部区域内已启用蓝牙功能的设备，然后请求一些关于各台设备的信息。这个过程也称为发现、查询、扫描。**局部区域内的蓝牙设备仅在其当前已启用可检测性时才会响应发现请求**。如果设备可以检测到，它将通过共享一些信息（例如设备名称、类及其唯一MAC地址）来响应发现请求。利用此信息，执行发现的设备可以选择发起到被发现设备的连接。

在首次与远程设备建立连接后，将会自动向用户显示配对请求。设备完成配对后，将会保存关于该设备的基本信息（如 设备名称、MAC 地址）。并且可以使用 Bluetooth API 读取这些信息。利用远程设备的已知 Mac 地址可以随时向其发起连接，而不需执行发现操作（假定该设备处于有效范围内）。

**被配对和被连接之间存在差别**。**被配对意味着两台设备知晓彼此的存在，具有可用于身份验证的共享链路密钥，并且能够与彼此建立加密连接。被连接意味着设备当前共享一个 RFCOMM 通道，并且能够向彼此传输数据。**当前的 Android Bluetooth API 要求对设备进行配对，然后才能建立 RFCOMM 连接（在使用 Bluetooth API 发起加密连接时，会自动执行配对）。**Android 设备是默认处于不可检测状态的。**

#### 查询配对的设备

在执行设备发现之前，有必要查询已配对的设备集合。用来了解设备是否处于已知状态。通过 `getBondedDevices()` 来实现，这将返回表示已配对设备的一组 `BluetoothDevice` 。

例如：我们可以查询所有已配对的设备，然后使用 ArrayAdapter 向用户显示每台设备的名称：

```java
    
    Set<BluetoothDevice> pairedDevices = mBlutooothAdapter.getBondedDevices();
    
    if(pairedDevices.size() > 0){
        for(BluetoothDevice device:pairedDevices){
            // 把名字和地址取出来添加到适配器中
            mArrayAdapter.add(device.getName()+"\n"+ device.getAddress());
        }
    }

```

要发起连接仅需要知道目标蓝牙设备的 Mac 地址就可以了。

#### 发现设备

发现设备使用 `startDiscovery()`该进程为异步进程。该方法会立刻返回一个布尔值，指示是否已成功启动发现操作。发现进程通常包含约 12 秒的查询扫描，之后对发现的设备进行扫描，以检索其蓝牙设备的名字。

我们的应用必须针对 `ACTION_FOUND` Intent 注册一个 BroadcastReceiver，以便接受每台发现的设备的信息。针对每台设备，系统会广播 ACTION_FOUND Intent。这个 Intent 会携带额外的字段 EXTRA_DEVICE 和 EXTRA_CLASS。这两者分别包含 BluetoothDevice 和 BluetoothClass。

```java
   // 创建一个接受 ACTION_FOUND 的 BroadcastReceiver
   private final BroadcastReceiver mReceiver = new BroadcastReceiver(){
   
        public void onReceive(Context context,Intent intent){
            String action = intent.getAction();
            // 当 Discovery 发现了一个设备  
            if(BluetoothDevice.ACTION_FOUND.equals(action)){
                // 从 Intent 中获取发现的 BluetoothDevice 
                BluetoothDevice device = intent.getParcelableExtra(BluetoothDevice.EXTRA_DEVICE);
                // 将名字和地址放入要显示的适配器中
                mArrayAdapter.add(device.getName + "\n" + device.getAddress());
                
            }
        }
   };
   // 注册这个 BroadcastReceiver
   IntentFilter filter = new IntentFilter(BluetoothDevice.ACTION_FOUND);
   registerReceiver(mReiver,filter);
   
   // 在 onDestroy 中 unRegister
```

**注意** 执行 discovery 对于蓝牙适配器来说是一个非常繁重的过程，并且会消耗大量资源。在找到要连接的设备后，**要确保使用 `cancelDiscovery()` 来停止发现，然后尝试连接**。如果您已经和某台设备进行连接，那么这个时候执行发现操作会大幅度的减少此连接可用的带宽！因此不应该在处于连接状态的时候执行发现操作！

#### 启用可检测性

如果我们希望我们的设备是可以被其他设备检测到的，可以使用 `ACTION_REQUEST_DISCOVERABLE` 来操作 Intent 调用 `startActivityForResult(Intent,int)`。这样会通过系统设置发出启用可检测到模式的请求（无需停止我们的应用）。默认情况下，**设备会变为可检测状态并且持续 120 秒钟。我们还可以通过 `EXTRA_DISCOVERABLE_DURATION` Intent Extra** 
**来定义不同的持续时间。可以设置的最大持续时间为 3600 秒。**值为 **0 表示始终可以被检测到**。任何小于 0 或者大于 3600 的值都会自动设置为 120 秒钟。

例如：

```java
   
   Intent discoverableIntent = new Intent(BluetoothAdapter.ACTION_REQUEST_DISCOVERABLE);
   discoverableIntent.putExtra(BluetoothAdapter.EXTRA_DISCOVERABLE_DURATION,300);
   startActivityForResult(discoverableIntent);

```

将显示对话框，请求用户允许将设备设为可检测到。如果用户响应为 YES，则设备将变为可检测到并持续指定的时间量。然后您的 Activity 将会收到对 onActivityResult() 回调的调用，其结果代码等于设备可检测到的持续时间。如果用户响应 NO 或者出现错误，结果代码为 RESULT_CANCELED

如果设备没有打开蓝牙，则启用设备可检测性的时候会自动启用蓝牙。

设备将在分配的时间内以静默方式保持可检测到模式。如果我们希望在可检测到模式发生变化时收到通知，可以利用 `ACTION_SCAN_MODE_CHANGED` Intent 注册 BroadcastReceiver。它将包含额外字段 EXTRA_SCAN_MODE 和 EXTRA_PREVIOUS_SCAN_MODE,两者分别告诉我们新的和旧的扫描模式。每个字段可能包括SCAN_MODE_CONNECTABLE_DISCOVERABLE(可检测到模式)、SCAN_MODE_CONNECTABLE(未处于可检测模式但可以接受连接)、SCAN_MODE_NOE(未处于可检测到模式并且无法连接)

#### 连接设备

要在两台设备上的应用之间创建连接，必须同时实现服务端和客户端机制，因为其中一台设备必须开放服务器套接字，而另一台设备必须发起连接（使用服务器设备的 MAC 地址发起连接）。当服务器和客户端在同一 RFCOMM 通道上分别拥有已连接的 BluetoothSocket 时，二者将被视为彼此连接。在这种情况下每台设备都能获得输入和输出流式传输，并且可以开始传输数据。

服务端和客户端分别以不同的方式来获得 BluetoothSocket 。服务器将在传入连接被接受时收到套接字。客户端将在其打开到服务器的 RFCOMM 通道时收到该套接字。

一种实现方式是自动将每台设备准备为一个服务器，从而使每台设备开发一个服务器套接字并侦听连接。然后任一设备可以发起与另一台设备的连接，并成为客户端。或者其中一台设备可显示“托管”连接并按需开放一个服务器套接字，从而另一台设备则直接发起连接。

**在连接之前如果两个设备没有配对，则系统会自动发出配对请求**

##### 连接为服务器

当连接两台设备时，其中一台必须保持开发的 `BluetoothServerSocket` 来充当服务器，用于监听传入的连接请求，在接受了请求后提供一个已经连接的 `BluetoothSocket`。从 `BluetoothServerSocket` 连接获取 `BluetoothSocket` 后就可以调用 close 来关闭这个等待了。

##### 关于 UUID 

通用唯一标识符（UUID），用于表示唯一标识信息的字符串ID，128位。可以使用网络上众多的随机 UUID 生成器，然后通过 `formString(String)` 来初始化一个 UUID。

**服务器套接字接受连接的基本过程**

- 通过 `listenUsingRfcommWithServiceRecord(String,UUID)获取 BluetoothServerSocket`

  字符串是我们自己定义的服务的可识别名称，可以直接使用包名。系统会自定将其写入到设备上的新服务发现协议（SDP）数据库条目中。UUID 也在 SDP 中，作为与客户端设备连接协议的匹配规则。只有客户端和这里的UUID 一样了才可以会被连接

- `accept()` 侦听连接请求

  阻塞调用，将在连接被接受或者发生异常的时候返回，操作成功后，会返回 `BluetoothSocket`。

- 除非要接受更多的连接，否则调用 `close()` 来关闭这个监听

  这样会释放服务器套接字及其所有资源，但不会关闭已经连接的 BluetoothSocket。与 TCP/IP 不同的是，RFCOMM 一次只允许每个通道有一个已经连接的客户端。

放在子线程中去执行。

**例子：**

```java
private class AcceptThread extend Thread{
    private final BluetoothServerSocket mServerSocket;
    public AcceptThread(){
        mServerSocket = mBluetoothAdapter.listenUsingRfcommWithServiceRecord(NAME,MY_UUID);
    }
    
    public void run(){
        BluetoothSocket socket = null;
        while(true){
            socket = mServerSocket.accept();
            if(socket!=null){
                // 自定义方法
                manageConnectedSocket(socket);
                mServerSocket.close();
                break;
            }
              
        } 
    }
    public void cancle(){
        mServerSocket.close();
    }
    
}
```

##### 连接为客户端

要想和保持开发服务器套接字的设备建立连接，必须首先要获取该设备的 BluetoothDevice 对象。然后使用这个对象来获取 BluetoothSocket 并发起连接。

**客户端连接的基本过程**

- 通过 BluetoothDevice 的 createRfcommSocketToServiceRecord(UUID) 获取 BluetoothSocket 对象

  这里的 UUID 要和服务器端的一致

- 通过 connect() 发起连接

  执行此方法后，系统将会在远程设备上执行 SDP 查找，来匹配 UUID。如果查找成功了并且远程设备接受了该连接，它将共享 RFCOMM 通道在连接期间使用。这个时候 connect() 就会返回。这个方法也是阻塞的，如果失败或者超时（12秒之后），将引发异常。

  **调用 connect() 的时候要确保客户端没有执行发现操作。如果执行了会大幅度降低连接的速度，增加失败的可能性**

**例子**

```java
private class ConnectThread extend Thread{
    private BluetoothDevice mDevice;
    private BluetoothSocket mSocket;
    public ConnectThread(BluetoothSocket device){
        mDevice = device;
        // 这里的 UUID 需要和服务器的一致
        mSocket = device.createRfcommSocketToServiceRecord(My_UUID);
    }
    
    public void run(){
        // 关闭发现设备
        mBluetoothAdapter.cancelDiscovery();
        try{
             mSocket.connect();
        }catch(IOException connectException){
            try{
                mSocket.close();
            }catch(IOException closeException){
                return;
            }
        }
        // 自定义方法
        manageConnectedSocket(mmSocket);
       
    }
    
    public void cancle(){
        try{
            mSocket.close();
        }cathc(IOException closeException){
            
        }
    }
    
}
```

在连接之前调用 `cancleDiscovery()` 在进行连接之前应该始终调用这个方法，而且调用的时候无需检测是否正在扫描。

#### 管理连接

建立连接后的两个设备都有一个 `BluetoothSocket` 通过这个 Socket 就可以在这两个设备间传输数据了。

**过程：**

- 获取 InputStream 和 OutputStream 
- 使用 read（byte[]）和 write（byte []）读取或者写入流式传输

#### 使用配置文件

从 Android 3.0 开始， Bluetooth API 便支持使用蓝牙配置文件。蓝牙配置文件是适用于设备间蓝牙通信的无线接口规范。

**蓝牙配置文件就是设备间通信（蓝牙设备）的一种规范**

免提配置文件便是一个示例，对于连接到无线耳机的手机，两台设备都必须支持免提配置文件。我们也可以通过实现接口 `BluetoothProfile` 来写入自己的类来支持特定的蓝牙配置文件。Android API 提供了以下的几种蓝牙配置文件的实现：

- **耳机**：耳机配置文件提供了蓝牙耳机的支持。也就是这个配置文件提供了手机和蓝牙耳机进行通信的一种规范。使用 **BluetoothHeadset** 类，用于进程间通信来控制蓝牙耳机服务的代理。这个类包含 AT 命令支持。
- **A2DP:** 高级音频分发配置文件（A2DP）。定义了高质量音频如何通过蓝牙连接和流式传输，从一个设备传输到另一个设备。**BluetoothAdp** 类，是用于通过进程间通信（IPC）来控制蓝牙 A2DP 服务的代理。
- **健康设备：** Android 4.0（API 14）引入了对蓝牙健康设备配置文件（HDP）的支持。这样就允许我们创建的应用可以使用蓝牙与支持蓝牙功能的健康设备进行通信。（心率检测仪、血糖仪、温度计等等）。详解的配置要查看健康设备配置文件。

**使用配置文件的基本步骤**

- 获取默认适配器 BluetoothAdapter
- 使用 `getProfileProxy()` ，建立到配置文件所关联的配置文件代理对象的连接。
- 设置监听`BluetoothProfile.ServiceListener`。这个监听会在客户端连接到服务或者断开服务连接的时候发送通知。
- 在 `onServiceConnected()` 中获取配置文件代理对象的句柄。
- 获取配置文件代理对象后，可以里脊将其用于监听连接状态和执行其他与该配置文件相关的操作。

**例子：** 如何连接到 BluetoothHeadset 代理对象，以便能够控制耳机配置文件：

```java
BluetoothHeadset mBluetoothHeadset;
// 获取默认蓝牙适配器
BluetoothAdapter mBluetoothAdapter = BluetoothAdapter.getDefaultAdapter();
// 设置监听（监听连接状态）
private BluetoothProfile.ServiceListener mProfileListener = new BluetoothProfile.ServiceListener(){
    public void onServiceConnected(int profile,BluetoothProfile proxy){
        if(profile == BluetoothProfile.HEADSET){
            mBluetoothHeadset = (BluetoothHeadset)
        }
    }
    public void onServiceDisconnected(int profile){
        if(profile == BluetoothProfile.HEADSET){
            mBluetoothHeadset = null;
        }
    }
}

// 建立与配置文件代理的连接
mBluetoothAdapter.getProfileProxy(contenxt,mProfileListener,BluetoothProfile.HEADSET);
// 使用 mBluetoothHeadset 代理内部的方法

// 使用完毕后关闭
mBluetoothAdapter.closeProfileProxy(mBluetoothHeadset);
```

##### 供应商特定的 AT 命令

从 Android 3.0 开始。应用可以注册接受耳机所发送的预定义的供应商特定 AT 命令的系统广播（例如 Plantronics +XEVENT命令）（也就是说我们的应用可以接受耳机蓝牙商预定义的命令）。如：应用可以接受指示所连接设备的电池电量的广播，并根据需要通知用户或采取其他操作。使用 **ACTION_VENDOR_SPECIFIC_HEADSET_EVENT**  intent 创建广播接收器，用来处理耳机供应商特定的 AT 命令。

##### 健康设备配置文件

Android 4.0 引入了对蓝牙健康设备配置文件（HDP）的支持。这可以使用我们的应用使用蓝牙与支持蓝牙功能的健康设备进行通信（心率检测仪、血糖仪、温度计、台秤）

创建 HDP 应用：

- 获取 BluetoothHealth 代理对象

  与常规耳机和 A2DP 类似。使用 ServiceListener 和 HEALTH 配置文件类型参数来调用 getProfileProxy()，来和配置文件代理对象建立连接。

- 创建 BluetoothHealthCallback 并注册充当健康汇集设备的应用配置（BluetoothHealthAPPConfiguration）

- 建立到健康设备的连接

- 成功连接到健康设备后，使用文件描述符对健康设备执行读写操作

- 完成后，关闭健康通道并取消注册该应用，该通道长时间闲置也会关闭。

#### 总结：

关于普通蓝牙设备和普通蓝牙设备之间的连接通信

- 通过 BluetoothAdapter 的 getDefaultAdapter 方法获取系统唯一的蓝牙适配器（如果返回为 null 则说明此设备不支持蓝牙）
- 通过 BluetoothAdapter 的 isEnable 方法判断是否已经打开蓝牙
- 可以通过 BluetoothAdapter.ACTION_REQUEST_ENABLE  intent 来开启蓝牙，也可以直接 .enable 开启蓝牙
- 通过 调用 startDiscovery 开启发现周边设备（持续 12 秒），这个时候需要注册广播接收器来接受发现的蓝牙设备（及时关闭这个操作）
- 通过 BluetoothDevice 就可以进行建立连接，（分为客户端和服务端类似于 Socket 连接）
- 然后建立连接后就可以通过 BluetoothSocket获取里面的流来进行读写通信

关于蓝牙设备和蓝牙仪器（蓝牙耳机、电子秤等等类似产品）

这种之间的通信是通过配置文件代理来实现的。

都有一个对应的配置文件代理类。具体的操作是通过这个对象来完成。

