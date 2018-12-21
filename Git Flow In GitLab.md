## Git Flow In GitLab
### gitlab与code review
在了解如何review之前先明确几个观点：

1. master分支中的任何版本都认为是可以立即部署的
2. 每一次master分支的变更都来自于其它分支向master的合并操作。
3. 对master的修改需要review。

借助于gitlab的merge request机制，与上面说的工作流程，我们可以在release分支正式合并到master分支之前建立merge request，在其他人review完成这次合并之后再正式进行合并。

#### review的初步流程
1. 将需要合并到master的分支push到gitlab。
2. 进入工程-merge request -create new merge request
3. 选择源分支、目标分支，确定。
4. review负责人进入merge request，确认没有问题之后选择Auto Merge（或者手动在本地合并之后再push到gitlab），并关闭这个merge request，完成。
5. 如果发现问题那么在有问题的行下注释，并提醒request的发起人及时修改。

### 分配成员角色
首先来了解下，Git 中的五种角色：
1. Guest：访客，貌似没有什么权限；
2. Reporter：可以 拉代码，但是不能push 到仓库的默认分支；
3. Developer：项目的开发人员，能够推送和删除没有保护的分支，刚创建的分支 默认 都是没有保护的；
4. Master：项目管理人员，可以对没有保护和有保护的所有分支进行操作，几乎拥有所有权限；
5. Owner：系统管理员，拥有所有权限

我们需要做的是，**为项目成员分配恰当的角色，以限制其权限**。

### 锁定受保护的分支
在对 Git 不熟悉的时候，时常苦恼于各个分支不受约束，任何开发人员都可以向任何分支直接推送任何提交，各种未经审查的代码、花样百出的 Bug 就这样流窜在预发布分支上。
其实我们可以通过 GitLab 的受保护分支（Protected Branches）功能 解决该问题，该功能可用于：

阻止 Master 角色以外的开发人员 直接向此类分支推送代码，保持稳定分支的安全性；

在向受保护分支合并代码前，强制进行代码审查。
接下来我们就使用这项功能，锁定我们的受保护分支——主分支 `master` 和预发布分支 `release-*`，以阻止 `dev` 直接向这两类分支中推送代码。

若要求严格，则`dev`也要进行保护，没人新建一个自己的常驻分支，申请合并到`dev`

锁定后, `dev`推送代码将会报错

![](http://oec2003.qiniudn.com/15276055106321.jpg)

### 发起合并请求
没问题后就可以push到GitLab中了。接下来请求管理员把自己仓库的分支合并到原仓库的分支下，这就是pull request
![](https://upload-images.jianshu.io/upload_images/231599-409ef285fe886533.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)
点击这里进入merge request，并且点击New Merge Request：
![](https://upload-images.jianshu.io/upload_images/231599-f33a42423adaf881.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)
![](https://upload-images.jianshu.io/upload_images/231599-6a1609303719bb14.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)
提交一个Merge Request，请记得写清楚提交的理由，分配需要为你去做Review的同事。

此外，还可以在评论框中去@其他的同事，也可以在Commit和Change里看到最新的改变。

### 代码审核
被assign或者at的同事都会收到邮件要求review，那么也会进入到如上的界面中，各位就可以进行Code Review了
![](https://upload-images.jianshu.io/upload_images/231599-086ffa4507d81cea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

在这里，各位可以针对某一行提出自己的意见，也可以在评论里发表意见。如果没有问题，可以输入lgtm(looks good to me)，如果各位都认为没问题，就可以Accept Merge Request了。于是就会看到The Merge Request has been accepted，这时也就提交到了主代码上。

如果代码被评论过后，评论者也会收到消息，修改后push的代码会自动提交到同一个merge request里。

### 审核通过
审核通过后代码会自动合并到dev分支。并发送消息给当事人







