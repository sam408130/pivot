## 目的

通过检测主要Telegram群的消息，把消息映射到币种，根据币种被讨论的频次，做两件事情：
1. 每天生成一条快讯，列出每天讨论最多的10个币种，例如：今日Telegram群热议币种：BTC, ADA, QLS, HT....
2. 作为币种页面【热议榜】的依据


## 开发流程

#### 服务部署
消息监测服务只在国际版执行，国内，国际共用一套数据。

#### 监测Telegram群消息方法

主要使用```telethon```这个库

```
from telethon import TelegramClient

#初始化服务
#api_id, api_hash是我新申请的，可以直接用
api_id = 490459  
api_hash = 'a3f39c507ae0f37062c70e085f2ec166'

#初始化client，这里pivot是随便起的名字，对应一个telegram账号
client = TelegramClient('pivot',
    api_id,
    api_hash,
    proxy=(socks.SOCKS5, 'localhost', 1086) #在国内测试时需要加代理
).start()

#监听消息
#chats表示要监听的Telegram群名称，可以是多个，逗号隔开
@client.on(events.NewMessage(chats=('t.me/binanceexchange'), incoming=True))
def my_event_handler(event):
    #处理消息    
    print(event.raw_text)

```

#### 消息处理
处理消息的逻辑：
1. 收到新消息
2. 去除消息中的标点符号
3. 将消息按照空格分开，逐个单词检查，看是否出现在【币种列表】里
4. 每匹配一次就记录一次：时间，币种，来源，例如：创建时间, BTC, 币种id, Huobi


#### 问题
1. 监听的群会陆续增加
2. 消息过来是阻塞试的，会不会影响主接口服务










