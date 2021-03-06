---
layout:     post   				    # 使用的布局（不需要改）
title:      获取App在AppStore中的版本号 				# 标题 
subtitle:   操作步骤详解 #副标题
date:       2019-07-25 				# 时间
author:     wozyao 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - iOS
---

### 导语

>开发中我们可能会遇到这样的需求，当AppStore中有新版本迭代更新，在用户点开APP的时候弹框提醒客户去AppStore更新APP。这里面就有个关键点，判断当前APP与AppStore中的版本高低，若一样，则无需进行提示；反之则弹框提示（用户使用版本不会比AppStore版本高）。下面就讲一下如何获取APP在AppStore中的版本，惯例直接上代码，O(∩_∩)O

```objectivec
- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    AFHTTPRequestOperationManager *manager = [AFHTTPRequestOperationManager manager];
    [manager POST:@"https://itunes.apple.com/lookup?id=414478124" parameters:nil success:^(AFHTTPRequestOperation * _Nonnull operation, id  _Nonnull responseObject) {
        NSArray *array = responseObject[@"results"];
        NSDictionary *dict = [array lastObject];
        NSLog(@"当前版本为：%@", dict[@"version"]);
    } failure:^(AFHTTPRequestOperation * _Nullable operation, NSError * _Nonnull error) {
        [SVProgressHUD showErrorWithStatus:@"请求失败！" ];
    }];
}
```
简单介绍一下上面的代码，请求链接中的id为APP在AppStore中的一个序号，你可以看成一个唯一标识符，下面放几张图片给还不清楚如何获取的同学科普下，我们以微信为例
![355579-4ba31e8846499890.png](https://i.loli.net/2020/12/18/B249ebfuI7qv16i.png)
![355579-9ac87ee688ad6640.png](https://i.loli.net/2020/12/18/3NynaHoB1PhsVpm.png)

***

返回的responsObjet返回的是一个字典，results键取出来是一个单个元素的数组，所以我用lastObject这个方法取的元素（当然你也可以firstObject或者[0]），数组取出的元素同样是一个字典，version键对应的值就是AppStore中的版本号。字典里的所有键值对几乎涵盖了APP在AppStore中的各种信息，有兴趣的小伙伴可以打个断点在控制台po一下，就酱紫啦~

---
**2019.06.06更新**
1、有小伙伴反馈用[https://itunes.apple.com/lookup?id=](https://itunes.apple.com/lookup?id=)获取版本号，会出现延迟或请求回来的版本号不稳定还有就是与刚刚发布的版本号对不上。这是因为连接国外的服务器，所以会有延迟。 解决：使用[https://itunes.apple.com/cn/lookup?id=](https://itunes.apple.com/cn/lookup?id=)即可；
2、获取的App的id，可以直接在App Store贵司的App的App，这里以微信为例，参照下图，即可获取：
![blob](https://i.loli.net/2020/12/18/cWBOGMa89ElzKjr.png)
![blob](https://i.loli.net/2020/12/18/GJ1t6i74ekbDVPY.png)
![blob](https://i.loli.net/2020/12/18/bHk5KNj92WLirzI.png)
