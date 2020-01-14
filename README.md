# ipaDemo


## 前言

  `iOS` 支持以无线方式安装自定的企业内部应用，而无需使用 `iTunes` 或 `App Store`。应用的格式必须为`.ipa`，并且使用企业内部 预置描述文件进行构建。

   为了安装应用，用户使用特殊的 `URL` 前缀从您的网站上下载 清单文件。您可以通过短信或电子邮件分发用于下载清单文件的 `URL`，或将其嵌入创建的另一企业应用中。
    
   您负责设计和托管用于分发应用的网站。请确定用户已通过 认证（可能是使用基本认证或基于目录的认证），并确定网站可 通过内联网或互联网进行访问。您可以将应用和清单文件放入隐藏目录或任何可使用` HTTPS `读取的位置。
   
1. 无线安装要求

    - `XML` 清单文件

    - 可让设备访问 `Apple iTunes` 服务器的网络配置

    - 使用 `HTTPS`
  
2. 准备基础架构

    - 证书验证
        > 用户首次打开应用时，系统会通过联系`Apple`的 `OCSP` 服务 器来验证分发证书。如果证书已撤销，应用将不会启动。为了验证状态，设备必须能够访问 `ocsp.apple.com`。

   `OCSP` 响应会在设备上缓存一段时间(由 `OCSP` 服务器指定)， 当前为 `3` 到 `7` 天之间。在重新启动设备和缓存的响应过期之前， 将不会再次检查证书的有效性。如果当时收到撤销命令，系统 将阻止应用运行。
   
   【`警告`】撤销分发证书会导致使用该证书签名的所有应用失 效。只有万不得已时才应撤销证书，比如确定专用秘钥已丢失 或确信证书已遭破解。
  
    - 构建网站
   
        > 将应用(`.ipa`)文件和清单(`.plist`)文件上传到网站上可供已认证的 用户访问的区域,以下是示例链接:
   
          请确定`.ipa`文件可通过`HTTPS`进行访问，并且您的站点已使 用`iOS`信任的证书进行了签名。如果自签名证书没有受信任的锚 点并且无法由`iOS`设备验证，安装会失败。
   
    - 设定服务器 `MIME` 类型
  
      对于服务器，请将 `MIME` 类型添加到网页服务的 `MIME` 类型设置:
   
      * application/octet-stream.ipa
 
      * text/xml plist
   
      对于微软的互联网信息服务器(`IIS`)，请使用`IIS Manager`在服务 器的“`属性`”页面中添加`MIME`类型:
   
      * .ipa application/octet-stream
 
      * .plist text./xml
   
    - 网络配置要求
  
      如果设备连接到封闭式内部网络，那么您必须允许它访问以下站点:
   
      * [https://ax.init.itunes.apple.com](https://ax.init.itunes.apple.com)使用蜂窝移动网络下载应用时，设备会限制其当前文件大小。如果无法访问此站点，安装可能会失败。
   
      * [https://ppq.apple.com](https://ppq.apple.com)设备会联系此网站，检查用来给预置描述文件签名的分发证书状态。
   
3. 创建清单文件
 
   清单文件是一个`XML plist`文件，可供`Apple`设备用来从您的 网页服务器上查找、下载和安装应用。以下栏是必填项:
    
   1. URL 应用(`.ipa`)文件的完全限定`HTTPS URL`。
   
   2. display-image `57 * 57`像素的`PNG`图像，在下载和安装过程中显示。指定图像的完全限定`URL`。
    
   3. full-size-image `512 * 512`像素的`PNG`图像，表示`iTunes`中相应的应用
    
   4. bundle-identifier 应用的包标识符，与`Xcode`项目中指定的完全一样
    
   5. bundle-version 应用的包版本，在`Xcode`项目中指定
    
   6. title 下载和安装过程中显示的应用的名称
    
 4. 无线 iOS 应用分发故障诊断
 
    如果无线应用分发失败，并显示“`无法下载`”信息:
    
    1. 请确定应用已正确进行签名。测试方法是使用`Xcode-USB`连接到设备上，然后查看是否发生错误。
    
    2. 请确定清单文件的链接是否正确，清单文件是否可供网络用户访问。
    
    3. 请确定`.ipa`（在清单文件中）的`URL`是否正确，并且该`.ipa`文件是否可供网络用户通过`HTTPS`访问。
    
5. 下载链接

 > 把下面的链接放置在网页上，用户点击即可或者把链接生成二维码，用户拿`iPhone`手机自带相机扫码即可。
   生成二维码网站:[http://tool.chinaz.com/qrcode](http://tool.chinaz.com/qrcode)
    
6. 注意点

    - `manifest`文件路径必须是`https`
   
    - `ipa`文件路径必须是`https`
    
