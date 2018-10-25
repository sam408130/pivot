## MYKEY 是什么？

MYKEY实际是基于Key Id智能合约的一个实际应用，它是基于多条公链的自主身份系统。

MYKEY从【资产维度】，【社会关系维度】，【数据维度】描述的具体的应用场景。

![](/assets/屏幕快照 2018-10-25 下午5.03.15.png)


## 多链钱包

#### 多链钱包能干啥？

多链钱包可以让你在不同的公链上都能使用Key(这个Key是以太坊上的Key)

#### 怎么实现的？

key id智能合约被部署在多条智能合约公链上，例如以太坊，EOS, ETC, ADA, AE等公链，并在相应的公链上设计的对应的token, 例如EOSKey, ADAKey等。这些不同的通证之间可以1：1兑换，以此来实现Key这个代币在不同公链上的使用，以及代币Key与其他公链上的代币无障碍的兑换。

#### 什么是身份，什么是身份名称？

身份其实是key id智能合约中的账户，在使用MYKEY这款App时，像平时注册一样，你会获得一个MYKEY APP的账户，注册完成后你就获得了一个身份，相当于你在区块链世界创建了另外一个你。登录后你就可以使用多链钱包，使用信任网络提供的身份认证功能，使用数据存储提供的存储功能。

身份名称可以理解为用户名，是你在区块链世界里的数字名称，这个名称一旦和你的MYKEY账户绑定，例如Ding Sai，那么你在其他公链下的数字名称就都叫Ding Sai了。这种身份与身份名称的绑定动作就好比于你申请了一个护照，那么在中国我被大家认识为Ding Sai，我在美国，别人也知道我叫Ding Sai。

那么问题来了，我在EOS, AE的公链上，都申请的钱包地址，如果让MYKEY知道这些地址也是我呢？怎么样让MYKEY知道，我在EOS, AE公链上的活动（例如转账等），其实是Ding Sai这个身份名称在活动呢？

这个问题实质是身份名称和其他公链地址的绑定问题。

MYKEY提出了一种【抵押声明 + 挑战期】的策略。

发起人须抵押一定数量的 KEY 作为保 证金，来发起非确权链上的映射关系的声明;随后，是一定时长的挑战期，在这期间，任何 人都可以通过抵押等量的 KEY 保证金来发起针对该对应关系的挑战。如果挑战期内无人挑战，则账户与身份名称在非确权链上的绑定关系确立。如果挑战期内有人发起挑战，则挑战 者须抵押等量的 KEY 保证金，由仲裁者根据相应数据仲裁。

这里其实我也没太明白，为什么要有这个流程？在创建MYKEY账户之初，直接在MYKEY多链钱包功能下，重新创建一个其他公链上的地址不就解决了吗，干嘛还要费力的去做这种证明？我只能理解为他们强行创建了这条key的流通场景。（后面再多研究一下）

#### 设置身份名称需要什么？

身份名称命名规则如下:
1. 身份名称可包含 1-63 个字符。
2. 字符的范围包含:a-z的26个小写英文字母，A-Z的26个大写英文字母，数字0-9，
以及连字符“-”。
3. 身份名称不区分大小写英文字母。
4. 身份名称不允许以连字符“-”开头或结尾。

身份名称不是自由设置，而是不定期释放，由用户使用Key竞拍：

• 单字符:每 120 天释放 1 个名称
• 2字符:每7天释放1个名称
• 3 字符:每天释放 5 个名称
• 4 字符:每天释放 125 个名称
• 5 字符:每天释放 3000 个名称
• 6 字符:每天释放 85000 个名称
• 7 字符及以上:无单日上限

每个身份名称至少支付 0.1 KEY，对于同一个身份名称的拍卖，每次叫价必须至少是前一次价格的110%，他妹的，这个卖域名有啥区别。。。


#### 什么是KeyPoint

前面说到在MYKEY中创建身份，这个可不是免费注册的。新用户可以通过KYC身份认证获得平台奖励的KeyPoint（一种消耗凭证）来实现免费创建身份，KeyPoint在之后的App使用中可以作为消耗的凭证，是一种使用权，不能转让，可以用key兑换，1KP = 1美元。


#### 多链钱包的安全体系

区块链世界使用私钥来管理账户，MYKEY也是。身份创建完成后，和普通钱包不一样的是，它会帮你创建1个管理私钥，和多个操作私钥。

管理私钥掌握最高权限，可以修改其他操作私钥。

操作私钥分多种：

##### 紧急协助任务响应私钥

这个私钥是用来找回身份用的，防止一旦丢失管理私钥，身份再也无法找回的情况。每个账户都可以设置多个紧急联系人，当你丢失管理私钥时，可以通过紧急联系人的【紧急协助任务响应私钥】来帮助你找回账户。具体操作流程可查阅白皮书。

##### 资产管理私钥

这个私钥用于MYKEY账户对账户下资产的转账，抵押等操作。

##### 特殊目的子账户的操作私钥

用于储蓄账户、外部应用专用子账户等。我理解相当于1Password，是用来管理其他隐秘信息的，如平台账号密码，私密信息等。

##### 登录私钥

以 MYKEY 账户的身份授权登录各种外部应用。

##### 投票私钥

以 MYKEY 账户的身份对某些议案进行投票操作。

##### 可验证声明的操作私钥

这个主要应用在信任网络中，作为其他节点动作证明的凭证。

由此可见，MYKEY多链钱包不只是满足于资产的管理，也设计了其他场景下的应用，包括资料存储，Dapp授权登录，投票等。通过MYKEY，在数字世界，你的钱就是你的钱，你就是你，没人可以冒充你。


## 信任网络





