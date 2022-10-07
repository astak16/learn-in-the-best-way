要在 github 上预览页面，需要使用 GitHub Pages 功能

## 新建仓库

访问 github 首页，在右边点击加号，选择 New repoitory

![1.png](https://i.loli.net/2019/08/24/rmi7RhM8XZ3QFwA.png)

第一步：输入 Repository name。

第二步：点击 Create repository。

其他不用啥都用点
![2.png](https://i.loli.net/2019/08/24/ApGIvg5uEDZ4eF1.png)

## 初始化本地仓库

上一步点了 Repository name，进入到了下面页面

![3.png](https://i.loli.net/2019/08/24/WSoD9ZTVXsKJwAY.png)

如果你的本地没有仓库，按照图中步骤 1 进行操作。

如果你的本地有仓库，按照图中步骤 2 进行操作，如果只是本地的一个文件夹，在进行步骤 2 操作前先进行一些操作：

> git init
> git add .
> git commit -m '提交信息'

tip:

1. 这里要看清楚使用的是`ssh`
2. 每一步操作都不能出错，否则得重新再来一次操作。

当看看下面如下信息是，说明已经上传成功了，刷新下页面就能看到我们提交的代码了。

![3.png](https://i.loli.net/2019/08/24/UmShTv9lCdsGJHL.png)

## Github Page

仓库上传上去了，那怎么预览网页呢？

点击图中红框中的 Settings

![4.png](https://i.loli.net/2019/08/24/HVnKXOzUSY6olRh.png)

进入后，往下滚动，看到 Github Page 停下来。

![5.png](https://i.loli.net/2019/08/24/frnAuQ4heHT9YiZ.png)

按照图中步骤 1、步骤 2 依次点击操作。

![6.png](https://i.loli.net/2019/08/24/6xqwiV71OknZTUm.png)

操作成功后，会跳回页面顶部，此时再次往下滚动 Github Page，会看到一条蓝色的链接，如下图所示，点击它即可预览页面。

![7.png](https://i.loli.net/2019/08/24/ngANQhjy5BWpx7s.png)

## 在仓库中预览

如果预览每次都需要这波操作也是挺麻烦的，有没什么快捷方法呢？

进入到 Settings 页面，滚动到 Github Page，复制这条蓝色链接。

先点击 Code，回到我们的仓库，在右上方有个 Edit 的按钮，如图所示。

![8.png](https://i.loli.net/2019/08/24/x2PS5zLv8qnKf6o.png)

点击 Edit 按钮后，出现了 Description 和 Website。

将刚刚复制的链接填入 Website，然后在 Description 中这个网址的描述点击保存即可

![9.png](https://i.loli.net/2019/08/24/q4htXvzlBIsndSG.png)

下次在进入这个仓库时，就能看到预览链接了，直接点击即可

![10.png](https://i.loli.net/2019/08/24/wbMdOrPQ7Hsihku.png)
