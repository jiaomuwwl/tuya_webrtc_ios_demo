## Tuya WebRTC Demo


English Version：[English](README.md)


## 功能概述

涂鸦智能 WebRTC Demo 提供了 iOS App 通过 WebRTC 连接 IPC 的使用示例。 Demo 展示了 App 通过 WebRTC 连接 IPC 的整个流程，包括：接口、MQTT 连接、SDP 交互等。

主要包括了以下功能： 
* 获取token、获取 MQTT 登录信息和设备相关的 WebRTC 信息等。

* MQTT登录、订阅、消息发送和接收等。

* WebRTC、SDP 交互等。

* 通过app浏览摄像头的视频，并和摄像头进行语音通话。

## 使用步骤

1. 通过 IoT 账号获取 `clientid`。

2. 通过下方 URL 进行授权，获取到授权码。

    1). 先填充 clientid 和用户账号，然后在浏览器运行下面的链接进行授权。
    https://openapi-cn.wgine.com/login/open/tuya/login/v1/index.html?client_id=clientid(第1步获取到的clientId）clientid&redirect_uri=https://www.example.com/auth&state=1234&username=（用户账号）&app_schema=tuyasmart&is_dynamic=true

    2). 授权成功后返回如下地址：
    https://www.example.com/auth?code=xxxxxxxxxxxxxxxxxxxx&state=1234&platform_url=https://openapi-cn.wgine.com
    地址中的 code 即为授权码。
    
3. 通过 App 或其他方式获取到需要访问到摄像头的 `DID` 和 `localKey`。

4. 修改 Demo 中 `ARDAppClient.m` 文件中相应的参数：
   clientId_ =        修改为第 1 步获取到到 `clientid` 
   secret_   =        修改为第 1 步获取到到 `secret`
   authCode_ =        修改为第 2 步获取到到授权码
   deviceId_ =        修改为第 3 步获取到到设备 ID
   localKey_ =        修改为第 3 步获取到到设备 LocalKey。
   
5. 参数修改完成后，运行 Demo 进行调试。    

## Demo 主要流程

1. 获取 token。
    ```
      -(BOOL)getTokenWithClientId:(NSString*)clientId secret:(NSString*)secret  code:(NSString*)code 
    ```
      接口会返回比较重要的信息，比如 accessToken、 UID 等等。
      
2. 获取 MQTT 相关信息.
    ```
      -(BOOL)getMQTTConfig:(NSString*)clientId secret:(NSString*)secret access_token:(NSString*)access_token 
      ```
      返回的信息主要用于登录 MQTT 服务器。主要包含服务器地址、端口、用户名和密码等信息。
      
3. 获取设备 WebRTC 相关配置。
    ```
      -(BOOL)getRtcConfig:(NSString*)clientId secret:(NSString*)secret access_token:(NSString*)access_token deviceid:(NSString*)did
    ```  
4. 连接MQTT服务器。
  
5. MQTT 连接成功之后订阅相应的 topic，topic 的定义参见 Demo。
  
6. 订阅成功之后本地开启 peerconnection，然后就可以通过 MQTT 进行 SDP 交互工作。
  
7. 交互成功之后就可以看到摄像头的视频。
