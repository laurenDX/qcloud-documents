## 腾讯短信服务
目前腾讯云短信为客户提供 **国内短信、国内语音** 和 **国际短信** 三大服务，腾讯云短信 SDK 支持以下操作：

### 国内短信
国内短信支持操作：
- 单发短信
- 指定模板单发短信
- 群发短信
- 指定模板群发短信
- 拉取短信回执和短信回复状态

> `Note`  短信拉取功能需要联系腾讯云短信技术支持(QQ:3012203387)开通权限，量大客户可以使用此功能批量拉取，其他客户不建议使用。

### 国际短信
国际短信支持操作：
- 单发短信
- 指定模板单发短信
- 群发短信
- 指定模板群发短信
- 拉取短信回执和短信回复状态

> `Note`  国际短信和国内短信使用同一接口，只需替换相应的国家码与手机号码，每次请求群发接口手机号码需全部为国内或者海外手机号码。

### 语音通知
语音通知支持操作：
- 发送语音验证码
- 发送语音通知

## 开发

### 准备
在开始开发云短信应用之前，需要准备如下信息：

- **获取 SDK AppID 和 AppKey**
云短信应用 SDK **AppID **和 **AppKey** 可在 [短信控制台](https://console.cloud.tencent.com/sms) 的应用信息里获取，如您尚未添加应用，请到 [短信控制台](https://console.cloud.tencent.com/sms) 中添加应用。

- **申请签名**
一个完整的短信由短信 **签名** 和 **短信正文内容** 两部分组成，短信 **签名** 须申请和审核，**签名** 可在 [短信控制台](https://console.cloud.tencent.com/sms) 的相应服务模块【内容配置】中进行申请。

- **申请模板**
同样短信或语音正文内容 **模板** 须申请和审核，**模板** 可在 [短信控制台](https://console.cloud.tencent.com/sms) 的相应服务模块【内容配置】中进行申请。

### 安装
qcloudsms_java 可以采用多种方式进行安装，我们提供以下三种方法供用户使用：

#### maven
要使用 qcloudsms_java 功能，需要在 pom.xml 中添加如下依赖，再执行 `maven update`。

```xml
<dependency>
  <groupId>com.github.qcloudsms</groupId>
  <artifactId>qcloudsms</artifactId>
  <version>1.0.2</version>
</dependency>
```

#### sbt
```
libraryDependencies += "com.github.qcloudsms" % "sms" % "1.0.2"
```

#### 其他

- 方法1
将 [源代码](https://github.com/qcloudsms/qcloudsms_java/tree/master/src) 直接引入到项目工程中。

- 方法2
将 [JAR包](https://github.com/qcloudsms/qcloudsms_java/releases) 直接引入到您的工程中。

> `Note` 由于 qcloudsms_java 依赖四个依赖项目 library： [org.json](http://central.maven.org/maven2/org/json/json/20170516/json-20170516.jar) , [httpclient](http://central.maven.org/maven2/org/apache/httpcomponents/httpclient/4.5.3/httpclient-4.5.3.jar), [httpcore](http://central.maven.org/maven2/org/apache/httpcomponents/httpcore/4.4.7/httpcore-4.4.7.jar) 和  [httpmine](http://central.maven.org/maven2/org/apache/httpcomponents/httpmime/4.5.3/httpmime-4.5.3.jar) 采用方法 1 需要将以上四个 jar 包导入工程。


### 用法
若您对接口存在疑问，可以查阅 [开发指南](https://cloud.tencent.com/document/product/382/13297) 、[API文档](https://qcloudsms.github.io/qcloudsms_java/) 和 [错误码](https://cloud.tencent.com/document/product/382/3771)。


- **准备必要参数**

```java
// 短信应用SDK AppID
int appid = 1400009099; // 1400开头

// 短信应用SDK AppKey
String appkey = "9ff91d87c2cd7cd0ea762f141975d1df37481d48700d70ac37470aefc60f9bad";

// 需要发送短信的手机号码
String[] phoneNumbers = {"21212313123", "12345678902", "12345678903"};

// 短信模板ID，需要在短信应用中申请
int templateId = 7839; // NOTE: 这里的模板ID`7839`只是一个示例，真实的模板ID需要在短信控制台中申请

// 签名
String smsSign = "腾讯云"; // NOTE: 这里的签名"腾讯云"只是一个示例，真实的签名需要在短信控制台中申请，另外签名参数使用的是`签名内容`，而不是`签名ID`
```

- **单发短信**

```java
import com.github.qcloudsms.SmsSingleSender;
import com.github.qcloudsms.SmsSingleSenderResult;
import com.github.qcloudsms.httpclient.HTTPException;
import org.json.JSONException;

import java.io.IOException;

try {
    SmsSingleSender ssender = new SmsSingleSender(appid, appkey);
    SmsSingleSenderResult result = ssender.send(0, "86", phoneNumbers[0],
        "【腾讯云】您的验证码是: 5678", "", "");
    System.out.print(result);
} catch (HTTPException e) {
    // HTTP响应码错误
    e.printStackTrace();
} catch (JSONException e) {
    // json解析错误
    e.printStackTrace();
} catch (IOException e) {
    // 网络IO错误
    e.printStackTrace();
}
```

> `Note` 如需发送海外短信，同样可以使用此接口，只需将国家码 `86` 改写成对应国家码号。
> `Note` 无论单发/群发短信还是指定模板ID单发/群发短信都需要从控制台中申请模板并且模板已经审核通过，才可能下发成功，否则返回失败。


- **指定模板ID单发短信**

```java
import com.github.qcloudsms.SmsSingleSender;
import com.github.qcloudsms.SmsSingleSenderResult;
import com.github.qcloudsms.httpclient.HTTPException;
import org.json.JSONException;

import java.io.IOException;

try {
    String[] params = {"5678"};
    SmsSingleSender ssender = new SmsSingleSender(appid, appkey);
    SmsSingleSenderResult result = sendWithParam("86", phoneNumbers[0],
        templateId, params, smsSign, "", "");  // 签名参数未提供或者为空时，会使用默认签名发送短信
    System.out.print(result);
} catch (HTTPException e) {
    // HTTP响应码错误
    e.printStackTrace();
} catch (JSONException e) {
    // json解析错误
    e.printStackTrace();
} catch (IOException e) {
    // 网络IO错误
    e.printStackTrace();
}
```

> `Note` 无论单发/群发短信还是指定模板ID单发/群发短信都需要从控制台中申请模板并且模板已经审核通过，才可能下发成功，否则返回失败。

- **群发**

```java
import com.github.qcloudsms.SmsMultiSender;
import com.github.qcloudsms.SmsMultiSenderResult;
import com.github.qcloudsms.httpclient.HTTPException;
import org.json.JSONException;

import java.io.IOException;

try {
    SmsMultiSender msender = new SmsMultiSender(appid, appkey);
    SmsMultiSenderResult result =  msender.send(0, "86", phoneNumbers,
        "【腾讯云】您的验证码是: 5678", "", "");
    System.out.print(result);
} catch (HTTPException e) {
    // HTTP响应码错误
    e.printStackTrace();
} catch (JSONException e) {
    // json解析错误
    e.printStackTrace();
} catch (IOException e) {
    // 网络IO错误
    e.printStackTrace();
}
```

> `Note` 无论单发/群发短信还是指定模板ID单发/群发短信都需要从控制台中申请模板并且模板已经审核通过，才可能下发成功，否则返回失败。

- **指定模板ID群发**

```java
import com.github.qcloudsms.SmsMultiSender;
import com.github.qcloudsms.SmsMultiSenderResult;
import com.github.qcloudsms.httpclient.HTTPException;
import org.json.JSONException;

import java.io.IOException;

try {
    String[] params = {"5678"};
    SmsMultiSender msender = new SmsMultiSender(appid, appkey);
    SmsMultiSenderResult result =  msender.sendWithParam("86", phoneNumbers,
        templateId, params, smsSign, "", "");  // 签名参数未提供或者为空时，会使用默认签名发送短信
    System.out.print(result);
} catch (HTTPException e) {
    // HTTP响应码错误
    e.printStackTrace();
} catch (JSONException e) {
    // json解析错误
    e.printStackTrace();
} catch (IOException e) {
    // 网络IO错误
    e.printStackTrace();
}
```

> `Note` 群发一次请求最多支持200个号码，如有对号码数量有特殊需求请联系腾讯云短信技术支持(QQ:3012203387)。
> `Note` 无论单发/群发短信还是指定模板ID单发/群发短信都需要从控制台中申请模板并且模板已经审核通过，才可能下发成功，否则返回失败。

- **发送语音验证码**

```java
import com.github.qcloudsms.SmsVoiceVerifyCodeSender;
import com.github.qcloudsms.SmsVoiceVerifyCodeSenderResult;
import com.github.qcloudsms.httpclient.HTTPException;
import org.json.JSONException;

import java.io.IOException;

try {
    SmsVoiceVerifyCodeSender vvcsender = new SmsVoiceVerifyCodeSender(appid,appkey);
    SmsVoiceVerifyCodeSenderResult result = vvcsender.send("86", phoneNumbers[0],
        "5678", 2, "");
    System.out.print(result);
} catch (HTTPException e) {
    // HTTP响应码错误
    e.printStackTrace();
} catch (JSONException e) {
    // json解析错误
    e.printStackTrace();
} catch (IOException e) {
    // 网络IO错误
    e.printStackTrace();
}
```

> `Note` 语音验证码发送只需提供验证码数字，例如当msg=“5678”时，您收到的语音通知为“您的语音验证码是5678”，如需自定义内容，可以使用语音通知。

- **发送语音通知**

```java
import com.github.qcloudsms.SmsVoicePromptSender;
import com.github.qcloudsms.SmsVoicePromptSenderResult;
import com.github.qcloudsms.httpclient.HTTPException;
import org.json.JSONException;

import java.io.IOException;

try {
    SmsVoicePromptSender vpsender = new SmsVoicePromptSender(appid, appkey);
    SmsVoicePromptSenderResult result = vpsender.send("86", phoneNumbers[0],
        2, 2, "5678", ""));
    System.out.print(result);
} catch (HTTPException e) {
    // HTTP响应码错误
    e.printStackTrace();
} catch (JSONException e) {
    // json解析错误
    e.printStackTrace();
} catch (IOException e) {
    // 网络IO错误
    e.printStackTrace();
}
```

- **拉取短信回执以及回复**

```java
import com.github.qcloudsms.SmsStatusPuller;
import com.github.qcloudsms.SmsStatusPullCallbackResult;
import com.github.qcloudsms.SmsStatusPullReplyResult;
import com.github.qcloudsms.httpclient.HTTPException;
import org.json.JSONException;

import java.io.IOException;

try {
    // Note: 短信拉取功能需要联系腾讯云短信技术支持(QQ:3012203387)开通权限
    int maxNum = 10;  // 单次拉取最大量
    SmsStatusPuller spuller = new SmsStatusPuller(appid, appkey);

    // 拉取短信回执
    SmsStatusPullCallbackResult callbackResult = spuller.pullCallback(maxNum);
    System.out.println(callbackResult);

    // 拉取回复
    SmsStatusPullReplyResult replyResult = spuller.pullReply(naxNum);
    System.out.println(replyResult);
} catch (HTTPException e) {
    // HTTP响应码错误
    e.printStackTrace();
} catch (JSONException e) {
    // json解析错误
    e.printStackTrace();
} catch (IOException e) {
    // 网络IO错误
    e.printStackTrace();
}
```

> `Note` 短信拉取功能需要联系腾讯云短信技术支持(QQ:3012203387)开通权限，量大客户可以使用此功能批量拉取，其他客户不建议使用。

- **拉取单个手机短信状态**

```java
import com.github.qcloudsms.SmsMobileStatusPuller;
import com.github.qcloudsms.SmsStatusPullCallbackResult;
import com.github.qcloudsms.SmsStatusPullReplyResult;
import com.github.qcloudsms.httpclient.HTTPException;
import org.json.JSONException;

import java.io.IOException;

try {
    int beginTime = 1511125600;  // 开始时间(unix timestamp)
    int endTime = 1511841600;    // 结束时间(unix timestamp)
    int maxNum = 10;             // 单次拉取最大量
    SmsMobileStatusPuller mspuller = new SmsMobileStatusPuller(appid, appkey);

    // 拉取短信回执
    SmsStatusPullCallbackResult callbackResult = mspuller.pullCallback("86",
        phoneNumbers[0], beginTime, endTime, maxNum);
    System.out.println(callbackResult);

    // 拉取回复
    SmsStatusPullReplyResult replyResult = mspuller.pullReply("86",
        phoneNumbers[0], beginTime, endTime, maxNum);
    System.out.println(replyResult);
} catch (HTTPException e) {
    // HTTP响应码错误
    e.printStackTrace();
} catch (JSONException e) {
    // json解析错误
    e.printStackTrace();
} catch (IOException e) {
    // 网络IO错误
    e.printStackTrace();
}
```

- **发送国际短信**

海外短信与国内短信发送类似, 发送海外短信只需替换相应国家码。

#### 使用连接池

多个线程可以共用一个连接池发送API请求，多线程并发单发短信示例如下：

```java
import com.github.qcloudsms.SmsSingleSender;
import com.github.qcloudsms.SmsSingleSenderResult;
import com.github.qcloudsms.httpclient.HTTPException;
import com.github.qcloudsms.httpclient.PoolingHTTPClient;
import org.json.JSONException;

import java.io.IOException;

class SmsThread extends Thread {

    private final SmsSingleSender sender;
    private final String nationCode;
    private final String phoneNumber;
    private final String msg;

    public SmsThread(SmsSingleSender sender, String nationCode, String phoneNumber, String msg) {
        this.sender = sender;
        this.nationCode = nationCode;
        this.phoneNumber = phoneNumber;
        this.msg = msg;
    }

    @Override
    public void run() {
        try {
            SmsSingleSenderResult result = sender.send(0, nationCode, phoneNumber, msg, "", "");
            System.out.println(result);
        } catch (HTTPException e) {
            // HTTP响应码错误
            e.printStackTrace();
        } catch (JSONException e) {
            // json解析错误
            e.printStackTrace();
        } catch (IOException e) {
            // 网络IO错误
            e.printStackTrace();
        }
    }
}

public class SmsTest {

    public static void main(String[] args) {

        int appid = 122333333;
        String appkey = "9ff91d87c2cd7cd0ea762f141975d1df37481d48700d70ac37470aefc60f9bad";
        String[] phoneNumbers = {
            "21212313123", "12345678902", "12345678903",
            "21212313124", "12345678903", "12345678904",
            "21212313125", "12345678904", "12345678905",
            "21212313126", "12345678905", "12345678906",
            "21212313127", "12345678906", "12345678907",
        };

        // 创建一个连接池httpclient, 并设置最大连接量为10
        PoolingHTTPClient httpclient = new PoolingHTTPClient(10);

        // 创建SmsSingleSender时传入连接池http client
        SmsSingleSender ssender = new SmsSingleSender(appid, appkey, httpclient);

        // 创建线程
        SmsThread[] threads = new SmsThread[phoneNumbers.length];
        for (int i = 0; i < phoneNumbers.length; i++) {
            threads[i] = new SmsThread(ssender, "86", phoneNumbers[i], "您验证码是：5678");
        }

        // 运行线程
        for (int i = 0; i < threads.length; i++) {
            threads[i].start();
        }

        // join线程
        for (int i = 0; i < threads.length; i++) {
            threads[i].join();
        }

        // 关闭连接池httpclient
        httpclient.close();
    }
}
```

### 使用自定义 HTTP client 实现

如果需要使用自定义的 HTTP client 实现，只需实现 `com.github.qcloudsms.httpclient.HTTPClient` 接口，并在构造 API 对象时传入自定义 HTTP client 即可，一个参考示例如下：

```java
import com.github.qcloudsms.httpclient.HTTPClient;
import com.github.qcloudsms.httpclient.HTTPRequest;
import com.github.qcloudsms.httpclient.HTTPResponse;

import java.io.IOException;
import java.net.URISyntaxException;

// import com.example.httpclient.MyHTTPClient
// import com.exmaple.httpclient.MyHTTPRequest
// import com.example.httpclient.MyHTTPresponse

public class CustomHTTPClient implements HTTPClient {

    public HTTPResponse fetch(HTTPRequest request) throws IOException, URISyntaxException {
        // 1. 创建自定义HTTP request
        // MyHTTPrequest req = MyHTTPRequest.build(request)

        // 2. 创建自定义HTTP cleint
        // MyHTTPClient client = new MyHTTPClient();

        // 3. 使用自定义HTTP client获取HTTP响应
        // MyHTTPResponse response = client.fetch(req);

        // 4. 转换HTTP响应到HTTPResponse
        // HTTPResponse res = transformToHTTPResponse(response);

        // 5. 返回HTTPResponse实例
        // return res;
    }

    public void close() {
        // 如果需要关闭必要资源
    }
}

// 创建自定义HTTP client
CustomHTTPClient httpclient = new CustomHTTPClient();
// 构造API对象时传入自定义HTTP client
SmsSingleSender ssender = new SmsSingleSender(appid, appkey, httpclient);
```

> `Note` 注意上面的这个示例代码只作参考，无法直接编译和运行，需要作相应修改。
