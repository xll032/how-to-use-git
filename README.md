
  1.创建版本库
  
    mkdir test

    cd test

    pwd  # 显示当前目录

    git init # 初始化当前目录为git仓库，会有一个隐藏的 .git 目录（用于跟踪管理版本库，不要去改它）


  2.添加、提交文件（要先add，再commit）
   # 中间创建了一个文件（readme.txt） 内容随意

   git add <fileName>  # 把文件加入到 git中（svn的添加）

   git commit -m "说明文字" # 提交文件到仓库中（svn的提交）

  3.git status # 查看仓库的状态

  4.git diff <fileName> # 查看文件的区别

  5.git log # 查看修改日志
    
    会多行显示内容，如果希望更加清晰简洁，可以使用 git log --pretty=oneline  # 显示的一长串字符是版本号的意思

  6.git reset --hard HEAD^ 
  
    HEAD 表示当前版本，HEAD^ 表示上一个版本，每多一个 ^ 就表示往上一个版本，很多的话可以使用 HEAD~100（往上100版本）

    这时候查看记录就会找不到之后的版本了，就像是时光倒流一样，如果记得未来的版本号，还是可以回去。原来窗口里能看到。

    使用 git reset --hard b722f070ac8409128affca23386da7a0a6fcfe0f （版本号，可以不写全也不能写的太少，git会自己去找）

    假如没有记住版本号，还是有办法找回的。 使用 git reflog 可以看到每一次的版本号，使用相应id就可以恢复了。

  7.git暂存区概念
               add               commit
    本地文件   -->   放入暂存区    -->    提交到仓库，并清空暂存区

    如果修改了文件，没有 add ，就不会被 commit 提交

    如果修改了多次，可以多次add，一起合并commit

  8.git checkout -- <fileName>  # 丢弃工作区的修改（简单的说，就是回到最近一次git commit 或 git add 时的状态）

  9.git reset HEAD <fileName> # 将已放入暂存区的文件撤销（撤销 git add 的状态）

  10.如果不小心也 git commit 也没有关系，可以使用之前的 git reset --hard HEAD^ 回退（但如果已经提交到远程版本库。。那就没有办法了）

  11.删除文件

    如果是误删，可以使用 git checkout -- <fileName> （从仓库里检出文件）

    的确要删除的话，使用 git rm <fileName> （相当于add，之后也需要提交）

    如果删除后，还是提交了也可以恢复，但是会丢失最近一次提交后修改的内容

  12.本地关联远程仓库

    在 github 上建一个新的仓库，完成后有提示如何将本地文件关联到github上

    关联：git remote add origin https://github.com/xll032/test.git # origin 后面是项目地址

    提交：git push -u origin master（第一次） 之后：git push origin master

    这是 git 比 svn 好的一个很大的优势，可以不用联网正常工作，之后只需要在有网络的时候推送到远程就完成同步了。svn 没有网络是没法用的。


  13.克隆远程仓库
  
    git clone https://github.com/xll032/test.git # clone 项目地址

  14.创建分支

    svn 的分支切换和创建非常慢。。所以大家都不喜欢用。 git 的分支切换、创建、删除都是非常快的。

    git checkout -b <devName> # 创建一个名为 <devName> 的分支   -b 表示创建并切换，相当于： 
    
    git branch <devName> # 创建分支        git checkout dev # 切换分支

    git branch  # 查看所有分支  当前分支前有一个 *
 
    git merge <devName> # 合并分支

    git branch -d dev # 删除分支

  15.解决冲突

    不同分支修改了相同地方会产生合并冲突，会显示（只是一个例子）：
    <<<<<<< HEAD
    master 修改的内容
    =======
    branch 修改的内容
    >>>>>>>

    可以直接打开文件选择需要的内容，然后提交

    git log --graph --pretty=oneline --abbrev-commit  # 查看日志的图形

  16.分支管理策略
 
    合并分支时，git 会尽可能使用 Fast forward 模式，这种模式会在分支删除后，丢失分支信息。

    强制禁止分支模式可以使用参数 --no-ff，这样就会在 merge 时生成一个新的 commit，这样就能从历史中看到分支信息

    实际使用： git merge --no-ff -m "merge with no-ff" <devName>



## 个人博客 - 介绍

>**使用技术： vue2 + +element-ui + sass + ~~font-awesome~~（已去除）**

>**博客地址： [个人博客 - 龙](https://xll032.github.io "https://xll032.github.io")**
###### 注：源码保存在本地不公开，如有需要可联系QQ：376013043 （请说明意图，谢谢）
**主要目的是收集记录以往遇到过的技术问题，方便以后再次遇到的时候可以快速找到解决的方法。**    
>之前一直本地用文本文档记录，一旦记录多了以后发现查找很不方便，而且当时记录下来的内容语言上比较粗糙。  
>既然记录了，那就应该好看，也要方便，不然就对不起自己的职业了啊，哈哈哈。:grin:  
>没有用 `hexo`的原因是觉得它的主题不是我想要的样子（我自己设计的网页虽然也一般般，我自己还是蛮喜欢的，不要喷我！:stuck_out_tongue_winking_eye:）  
>同时也希望能够在技术小伙伴们遇到类似问题的时候，可以快速准确地提供一些帮助。  
>Baidu Google 搜出来的肯定比我的全面，但我的优势在于我知道我曾经整理过，对症下药效果才是最好的。  
>由于水平有限，记录下来的东西可能也不难，没有强大的实力写出深入浅出的好文章。  

**一步一脚印，相信未来，不止于此。**


>### 博客功能更新、优化日志：
**界面显示效果优化 2017/07/01**
* 使用较多的 css3 动画，采用样式渐变以优化视觉效果


**回到顶部效果优化并组件化 2017/07/06**
* 速度优化
* 识别鼠标滚轮动作，可以立即阻止自动滚动的行为
* 组件化，使用如下方式引入即可
```html
<go-top></go-top>
```


**博客数据整合 2017/07/12**
* 将数据用 `vuex` 存放在 `$store` 中
* 使用 `$store.commit` 改变查询的数据


**blog 内容代码优化 2017/07/30**
* 提取出 blog 内容里的 js 部分，公用同一个 js 既减少代码量，又方便同意修改


**样式优化 2017/08/11**
* 提取出公共部分的 css 代码，方便统一管理
* 注：一开始在 App.vue、main.vue、home.vue 中都有公共的 css 代码，长期不修改分不清哪一个公共代码在什么地方，而实际中它们都是公用的，于是将它们整合提取到 public.css 中


**优化显示的内容 2017/9/20**
* 去除不必要的显示内容，并在 阿里巴巴矢量图标库 中找到了使用的图片，取代了使用并不多的 `font-awesome`
* 目的：减少代码体积


**图片压缩 2017/09/20**
* 使用 `tinypng` 在线压缩了图片


**回到顶部（go-top）按钮组件化 2017/10/02**
* 回到顶部的功能在每个页面中都有使用，组件化以后使用更加灵活，并且可以将它的样式都单独提取出去方便管理和修改
* 组件化之后简单的加入： `<go-top></go-top>` 即可


**blog 列表组件化 2017/10/6**
* 与回到顶部（go-top）按钮类似，组件化后使用更加灵活，多处使用可以统一修改显示的效果，方便管理和修改
* 组件化之后这样写即可：
```html
<my-blog v-for  = "item in $store.state.currentBlogData"
         :title = "item.title"
         :url   = "item.url"
         :key   = "item.url"
         :time  = "item.time"
         :tags  = "item.tags"
         :brief = "item.brief"></my-blog>
```


**搜索功能、页面完善 2017/11/06**
* PC 端搜索可以直接找到相应的 blog，方便查询已有的信息
* 移动端搜索独立呈现一个简单的页面，点击搜索结果与 PC 端效果相同


**首页轮播图懒加载 2017/11/20**
* 使用 `Photoshop cc2017` 给首页图片打上了高斯模糊，并缩小尺寸
* 清晰原图采用懒加载，在图片轮播到的时候才加载
* 使用了 `Promise` 加载，实际使用直接赋值在此处效果相同


**升级框架版本 2018/01/10**
* 升级 element-ui 至版本 2.0.11
* 升级 vue 至版本 2.5.13
* 修复了升级带来的代码兼容问题
* 根据 element-ui 文档按需引入组件
```javascript
npm install babel-plugin-component -D
```
修改 .babelrc 文件：
```javascript
{
  "presets": [
    ["es2015", { "modules": false }]
  ],
  "plugins": [["component", [
    {
      "libraryName": "element-ui",
      "styleLibraryName": "theme-chalk"
    }
  ]]]
}
```
然后引入：
```javascript
import { Button, Select } from 'element-ui';
 Vue.use(Button);
```


**使用 sass 管理样式并优化界面样式 2018/02/08**
* 整合样式，使样式文件结构清晰，便于阅读和管理


**头部、尾部组件化 2018/02/14**
* 组件化头部、尾部，便于代码区分管理，使用如下方式引入即可
```html
<Header></Header>
<Bottom></Bottom>
```
**头部、尾部组件化 2018/02/26**
* 优化页面效果，如：页面切换动画
* 移动端采用下拉加载数据

