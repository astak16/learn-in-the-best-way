[`iterm2`配置](https://lhajh.github.io/mac/2018/04/25/Iterm2-usage-and-skills.html)

## `homebrew`安装

如果官方教程没法安装，可参考：[mac 下镜像飞速安装 Homebrew 教程](https://zhuanlan.zhihu.com/p/90508170)

## `node`安装

`node`需要用`brew`安装，否则全局安装东西时没有权限

1、`brew`搜索`node`

```js
brew search node
```

2、安装`node`

```js
brew install node<@版本号>
```

3、解除`node`软连接

```js
brew unlink node
```

4、切换`node`，`--overwrite`覆盖冲突文件，`--force`强制执行

```js
brew link --overwrite node@10 --force
```

## `React Native`开发环境搭建

1、安装`node`

```js
brew install node
```

2、安装`watchman`，它是文件监听的工具

```js
brew install watchman
```

3、安装`React Native`脚手架工具

```js
npm install -g react-native-cli
```

4、拉远程项目

```js
cd ios && pod install

// pod install -bash: pod: command not found
sudo gem install cocoapods
```

## 设置`ssh key`

1. 在终端输入`ssh-keygen`，一路回车，会在当前用户名下生成一个`.ssh`的文件夹。
2. `cat ~/.ssh/id_rsa.pub`就可以看到`ssh key`

## 安装`oh my zsh`

```js
curl -L https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh | sh
```

查看`~`目录下有没有`.zshrc`文件，没有创建

```js
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
```

`zsh`语法高亮

1. `brew install zsh-syntax-highlighting`
2. 在`./zshrc`中追加`source /usr/local/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh`

修改主题

1. 下载主题`https://github.com/robbyrussell/oh-my-zsh/wiki/themes`
2. 放置在`~/.oh-my-zsh/themes`
3. `vim ~/.zshrc`
4. 找到`ZSH_THEME="robbyrussell"`修改成自己喜欢的主题

## 设置分支名

1. 编辑`/etc/profile`

2. 添加下面代码

```js
find_git_branch () {

local dir=. head

until [ "$dir" -ef / ]; do

if [ -f "$dir/.git/HEAD" ]; then

head=$(< "$dir/.git/HEAD")

if [[ $head = ref:\ refs/heads/* ]]; then

git_branch=" (${head#*/*/})"

elif [[ $head != '' ]]; then

git_branch=" → (detached)"

else

git_branch=" → (unknow)"

fi

return

fi

dir="../$dir"

done

git_branch=''

}

PROMPT_COMMAND="find_git_branch; $PROMPT_COMMAND"

black=$'\[\e[1;30m\]'

red=$'\[\e[1;31m\]'

green=$'\[\e[1;32m\]'

yellow=$'\[\e[1;33m\]'

blue=$'\[\e[1;34m\]'

magenta=$'\[\e[1;35m\]'

cyan=$'\[\e[1;36m\]'

white=$'\[\e[1;37m\]'

normal=$'\[\e[m\]'

PS1="$white[$white@$green\h$white:$cyan\W$yellow\$git_branch$white]\$ $normal"
```

3. `source /etc/profile`

## `Git`

`git status`出现中文乱码

```js
git config --global core.quotepath false
```

## 命令行使用

```bash
lsof -i:8888  // 端口是否被占用
kill -9 PID   // 释放端口
```
