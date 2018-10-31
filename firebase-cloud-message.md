## Firebase cloud message

国际版还没有做批量推送以及个性推送的服务，例如向所有参与竞猜的用户发送通知，或向指定用户推送充币到账通知等。

以下内容是结合Firebase文档和我们的特点，整理的一些文档，方便以后要做这块服务时查询使用。


## Firebase Admin SDK

```
pip install firebase-admin
```

通过 Admin SDK，可以从特权环境与 Firebase 进行交互，以执行以下操作：

* 以完整的管理员权限读写实时数据库数据。
* 使用简单的替代方法以编程方式将 Firebase 云消息传递消息发送至 FCM 服务器协议。
* 生成并验证 Firebase 身份验证令牌。
* 访问 Google Cloud Platform 资源，例如与您的 Firebase 项目相关联的 Cloud Storage 存储分区和 Firestore 数据库。
* 创建您自己的简化管理控制台，以执行查询用户数据或更改用户用于身份验证的电子邮件地址等操作。


#### 完成身份认证

```
from firebase_admin import credentials

# 设置登录凭证
cred = credentials.Certificate("firebase.json")
firebase_admin.initialize_app(cred)

```


```firebase.json``` 是从Firebase后台生成的私钥文件


#### 设置用户分群

向指定用户群发消息时，跟极光一样，通过tag来发送，发给订阅了这个tag的所有用户。目前我们给现有用户打上了news的标签，这次发版后，所有的新用户也会订阅这个标签，以此表示所有用户。

除了news，从1.4版本开始，新用户还会打上以下标签：

* 设备属性  iOS/Android
* 版本号    1.4.0
* 国家     CN
* 语言     zh

也可以通过用户表中存储的pushID，我们主动给用户打标签

```
from firebase_admin import messaging

# 取出用户的pushID
registration_tokens = [
      'cIbUaxSewgM:...Tf',
      'cFZ817t9twP...060Wx9',
]

# 设置topic名称
topic = 'news'

# 给指定用户打标签
response = messaging.subscribe_to_topic(registration_tokens, topic)

```

这里要注意，在单个请求中，最多可以为 1000 台设备订阅主题。如果提供的数组所含的注册令牌超过 1000 个，则系统将无法处理此请求，并且会显示 messaging/invalid-argument 错误。





#### 向指定用户发送消息

发消息时同样需要messageing模块

```
from firebase_admin import messaging

# 取出用户的pushId
registration_token = 'cIbUaxSewgM:APA91bGmq4V...'
# 定义消息体
message = messaging.Message(
    notification=messaging.Notification(
        title='test message',
        body='test message topic news',
    ),
    data={
        'action': 'openPost',
        'pid': '5bd7bb40cd5ee77fbce1027a',
    },
    token=registration_token,
)
# 发送
response = messaging.send(message)
print('Successfully sent message:', response)

```

#### 向指定主题发消息

```
from firebase_admin import messaging

# 要发送的主题
topic = 'news'
# 定义消息体
message = messaging.Message(
    notification=messaging.Notification(
        title='test message',
        body='test message topic news',
    ),
    data={
        'action': 'openPost',
        'pid': '5bd7bb40cd5ee77fbce1027a',
    },
    topic=topic,
)
# 发送
response = messaging.send(message)
print('Successfully sent message:', response)

```



