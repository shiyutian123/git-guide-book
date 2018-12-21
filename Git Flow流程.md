## Git Flow流程
### Git Flow 常用分支
1. **`master` 分支**: 这个分支最近发布到生产环境的代码，最近发布的Release， 这个分支只能从其他分支合并，不能在这个分支直接修改
2. **`dev`分支**: 这个分支是我们是我们的主开发分支，包含所有要发布到下一个`release`的代码，这个主要合并与其他分支，比如`feature`分支
3. **`feature`分支**: 这个分支主要是用来开发一个新的功能，一旦开发完成，我们合并回`dev`分支进入下一个`release`
4. **`release`分支**: 当你需要一个发布一个新`release`的时候，我们基于`dev`分支创建一个`release`分支，完成`release`后，我们合并到`master`和`dev`分支
5. **`hotfix`分支**: 当我们在`master`发现新的Bug时候，我们需要创建一个`hotfix`, 完成`hotfix`后，我们合并回`master`和`dev`分支，所以`hotfix`的改动会进入下一个`release`

### Git Flow工作模式
**`master分支`** 
> 所有在`master`分支上的commit应该Tag,并遵循[语义化版本规范](https://semver.org/lang/zh-CN/)

![](https://images.cnblogs.com/cnblogs_com/cnblogsfans/771108/o_git-workflow-release-cycle-1historical.png)

**`feature分支`**
>`feature`分支做完后，必须合并回`dev`分支, 合并完分支后一般会删点这个`feature`分支，核心功能的feature需要保留

![](https://images.cnblogs.com/cnblogs_com/cnblogsfans/771108/o_git-workflow-release-cycle-2feature.png)

**`release分支`** 
> `release`分支基于`dev`分支创建，打完`release`分支之后，我们可以在这个`release`分支上测试，修改Bug等。同时，其它开发人员可以基于开发新的`feature` (记住：一旦打了`release`分支之后不要从`dev`分支上合并新的改动到`release`分支)
> 发布`release`分支时，合并`release`到`master`和`dev`， 同时在Master分支上打个Tag记住`release`版本号，然后可以删除`release`分支了。

![](https://images.cnblogs.com/cnblogs_com/cnblogsfans/771108/o_git-workflow-release-cycle-3release.png)

**`hotfix分支`**
> `hotfix`分支基于`master`分支创建，开发完后需要合并回`master`和`dev`分支，同时在`master`上打一个Tag

![](https://images.cnblogs.com/cnblogs_com/cnblogsfans/771108/o_git-workflow-release-cycle-4maintenance.png)

### 命令实例
#### 创建dev分支

```mac
git branch develop
git push -u origin develop    
```

#### 开始新feature开发 (新功能开发)

```mac
git checkout -b feature-some dev
# Optionally, push branch to origin:
git push -u origin feature-some    

# 做一些改动    
git status
git add some-file
git commit    
```

#### 完成Feature (测试完成之后)

```mac
git pull origin dev
git checkout dev
git merge --no-ff feature-some
git push origin dev

git branch -d feature-some

# If you pushed branch to origin:
git push origin --delete some-feature 
```   

#### 开始relase

```mac
git checkout -b release-0.1.0 dev
# Optional: Bump version number, commit
# Prepare release, commit
git push -u origin release-0.1.0   
```

#### 完成relase

```mac
git checkout master
git merge --no-ff release-0.1.0
git push

git checkout dev
git merge --no-ff release-0.1.0
git push

git branch -d release-0.1.0

# If you pushed branch to origin:
git push origin --delete release-0.1.0   

git tag -a v0.1.0 master
git push --tags
```

#### 开始hotfix

```mac
git checkout -b hotfix-0.1.1 master    
```

#### 完成Hotfix

```mac
git checkout master
git merge --no-ff hotfix-0.1.1
git push


git checkout dev
git merge --no-ff hotfix-0.1.1
git push

git branch -d hotfix-0.1.1

git tag -a v0.1.1 master
git push --tags
```




    

