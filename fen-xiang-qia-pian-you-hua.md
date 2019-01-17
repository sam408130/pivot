## 社交平台分享卡片优化，Open Graph Markup

Pivot中分享到Facebook，twitter，telegram时，分享的卡片很单一：一个链接 + 一个图片，很影响点击转化率

经过调研，这些社交网络对于分享内容的呈现都有一套抓取规则

先看一下，链接分享出去的样式：

测试链接： https://www.buzzfeed.com/gabrielsanchez/ces-photos-montrent-les-braves-hommes-et-femmes-qu?utm_term=.yqBp3x2yq4#.yqBp3x2yq4

#### Facebook
![](/assets/17_24_41__01_07_2019.jpg)

#### Twitter

![](/assets/屏幕快照 2019-01-09 下午2.24.05.png)

#### Whatsapp

![](/assets/WechatIMG207.jpeg)

![](/assets/WechatIMG208.jpeg)

#### Telegram

![](/assets/WechatIMG209.jpeg)


单纯的分享链接，各个平台会自动抓取网页内容，呈现更丰富的信息。

要实现以上功能，需要在网页Head信息中添加相应的meta信息，主要分为3个部分

* 基本标签
* fb标签
* twitter标签
* AppLink标签


### 基本标签 Open Graph Markup

基本标签用于标识链接内容的类型，文章还是视频，标题，描述，以及封面图

```
<meta property="og:url"                content="http://www.nytimes.com/2015/02/19/arts/international/when-great-minds-dont-think-alike.html" />
<meta property="og:type"               content="article" />
<meta property="og:title"              content="When Great Minds Don’t Think Alike" />
<meta property="og:description"        content="How much does culture influence creative thinking?" />
<meta property="og:image"              content="http://static01.nyt.com/images/2015/02/19/arts/international/19iht-btnumbers19A/19iht-btnumbers19A-facebookJumbo-v2.jpg" />
```

type为video时，在whatsapp中分享可以直接播放视频

https://developers.facebook.com/docs/sharing/opengraph/object-properties#standard


### fb标签

```
  <meta property="fb:app_id" content="111569915535689" />
  <meta property="fb:pages" content="21785951839" />
```

facebook分享抓取规则遵循通用的开放图谱标记，可识别基本标签里的内容，这里添加app_id后可关联facebook提供的数据统计

fb:pages添加后，会在分享卡片上出现一个小问号，点击后显示facebook pages相关信心

https://developers.facebook.com/docs/sharing/webmasters#markup

### twitter标签

```
<meta name="twitter:card" content="summary" />
<meta name="twitter:site" content="@nytimesbits" />
<meta name="twitter:creator" content="@nickbilton" />
<meta property="og:url" content="http://bits.blogs.nytimes.com/2011/12/08/a-twitter-for-my-sister/" />
<meta property="og:title" content="A Twitter for My Sister" />
<meta property="og:description" content="In the early days, Twitter grew so quickly that it was almost impossible to add new features because engineers spent their time trying to keep the rocket ship from stalling." />
<meta property="og:image" content="http://graphics8.nytimes.com/images/2011/12/08/technology/bits-newtwitter/bits-newtwitter-tmagArticle.jpg" />
```

twitter:card标签用户表示分享卡片的内容，有summary, summary_large_image, app, player 4个类型

不同的类型，呈现不同的样式

https://developer.twitter.com/en/docs/tweets/optimize-with-cards/guides/getting-started


### AppLink标签

添加AppLink标签可以让用户在whatsapp里点击链接时，如果用户安装了app，可以通过scheme唤起App



```
<html>
<head>
    <meta property="al:ios:url" content="applinks://docs" />
    <meta property="al:ios:app_store_id" content="12345" />
    <meta property="al:ios:app_name" content="App Links" />
    <meta property="al:android:url" content="applinks://docs" />
    <meta property="al:android:app_name" content="App Links" />
    <meta property="al:android:package" content="org.applinks" />
    <meta property="al:web:url" content="http://applinks.org/documentation" />
</head>
<body>
    Hello, world!
</body>
</html>
```

facebook文档：https://developers.facebook.com/docs/applinks/metadata-reference/  

以下是9gag的头部信息：


```
 <meta property="al:ios:url" content="ninegag://9gag.com/gag/aLgppQ5" />
 <meta property="al:ios:app_store_id" content="545551605" />
 <meta property="al:ios:app_name" content="9GAG" />
 <meta property="al:android:url" content="ninegag://9gag.com/gag/aLgppQ5" />
 <meta property="al:android:package" content="com.ninegag.android.app" />
```





