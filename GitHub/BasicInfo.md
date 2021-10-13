# GitHub 之基础介绍



## 准备工作

1. 下载安装GitHub(会有两个dos命令窗口图标 git Bash,git GUI)
2. 在GitHub官网注册一个账号
3. git是版本控制，gitHub是平台托管
4. 编辑日期2021-04-20

## 个人工作

​		自己一个人负责一个项目，或者项目较小时，主要用于版本控制。

工作区(编辑代码)==>(通过**git add .**命令将代码存至)暂存区==>(通过**git commit -m "your mark message**提交至")代码库====>(git push)远端的生产环境(可存至你GitHub上的账号下面，但要先建一个项目，也可通过命令创建)



## 协同作业

​		协同作业时，多人负责统一项目开发，用于版本控制和分支合并。



## Git基本使用步骤(在GitBash窗口中操作)=>HTTPS
##### 1. 配置你的GitHub账号信息(第一次使用时配置)

```python
git config --global user.email	111@qq.com
git config --global user.name	GitHub's User Name

使用上述命令，在你的电脑C盘使用者或者管理员账号下会产生一个.gitcongig文件，保存了你刚刚配置的信息
```

##### 2.建立新的repository(本机，远端)

​		使用git init进行初始化，注意接下来的命令都是**在当前项目目录下进行** 

```python
# 本机
mkdir newgame
cd newgame
git init

# 远端：网页创建==》到第四步远端拉取成功，可在本地修改
```

##### 3. 进行一下个人作业步骤

```python
# 先自己在newgame下随便建个文件，可用
git add . # 添加newgame下所有的文件，也可指定名字添加=>将代码保存至暂存区
git commit -m "the first use of git" # 提交自己的代码至代码库，要加上备注信息
```

##### 4. 将网页创建的项目地址保存下来

###### 	4.1从远端拉取已存在的项目

```python
# 如果你用git clone 命令，那么(2.建立新的repository(本机)；4.将网页创建的项目地址保存下来)这一步就可以省略。git push应该是可以直接用的（没试过）。
# 此处也是成功拉取的，但是由于拉取的是一个空项目，所以有个警告
$ git clone https://github.com/KillenNolan/seeGitCmd.git
Cloning into 'seeGitCmd'...
warning: You appear to have cloned an empty repository.
    
# 成功拉取远端已存在的项目
Cloning into 'seeGitCmd'...
remote: Enumerating objects: 6, done.
remote: Counting objects: 100% (6/6), done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 6 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (6/6), done.
    
```

###### 	4.2将本地项目保存到远端（待验证）

```python
# 将本地仓保存远端地址为https://github.com/KillenNolan/newgameTest.git   
git remote add origin https://github.com/KillenNolan/newgameTest.git    


```

##### 5.将本地代码库的文件推送到远端(GitHub)

```python
# 直接推送，此時輸入遠端的賬號密碼
git push 

# ERROR 
fatal: The current branch master has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin master
# 由于没有upstream branch(上游分支)，我的理解：你是第一次对这个项目进行推送，需设定一个主干路径，好像不对。


# 将代码库的变动推送到主干(master)上
# 指定
git push -u origin master

# ERROR
fatal: unable to access 'https://github.com/KillenNolan/newgameTest.git/': Failed to connect to github.com port 443: Timed out
# 可能是我设了代理，或者电脑管控的原因
# 我在WEB上设置了代理，但是git上没有，于是我用git命令设置了代理，登陆了两次，才上传成功！！！


# SUCCESS ==>会出现一个百分比进度，done，最后还有一个total...
# 也可在远端网页查看，文件是否push成功
```

###### 5.1项目推送补充

- [参考:设置PersonalToken](https://segmentfault.com/a/1190000040544939)
- [参考:保存账号密码](https://www.jianshu.com/p/47a51aeeea62)

```python
# 2021-10-12补充报错
# git push 后会要求输入账号密码，但规则变了，账号密码验证被移除了。说让用Token
remote: Support for password authentication was removed on August 13, 2021. Please use a personal access token instead.
remote: Please see https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/ for more information.
fatal: Authentication failed for 'https://github.com/KillenNolan/CSharpPractise.git/'

# 需先设置个人访问牌
# git push https://github.com/KillenNolan/newgameTest.git要求输入账号密码时，将密码换成你自己创建的Token(个人访问牌)
    
```



##### 6. 查看代码的修改状况(git status)

```python
$ echo l love python >ss.txt # 向ss.txt中寫入数据(ss.txt不存在則創建)
$ echo lll >>Words # 向Words文档追加数据
$ git status	#查看修改

# 提示信息
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   Words

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        ss.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

##### 7. 查看提交的記錄(git log)

[參考文獻](http://strivingboy.github.io/blog/2014/09/29/better-git-log/)

[參考文獻](https://ruby-china.org/topics/939)

```python
$ git log

# 查看記錄高級用法
# 打開隱藏文件.gitconfig，沒有就自己建一個(我看的參考是說Home目錄下，我的是在administrator下)
# 創建.gitcongig文件是需要用命令創建的
# 方法一：直接複製到.gitcongig
[alias]
	lg=log --graph --pretty=online --abbrev-commit
  
#方法二：命令            
git config --global alias.lg  "log --graph --pretty=online --abbrev-commit" 
    
    
$ git lg # 我失敗了執行的時候

# ERROR
fatal: invalid --pretty format: online
        
# 我查看了下我的環境變量
C:\Program Files\Git\cmd;
C:\Program Files\TortoiseGit\bin;        
```

##### 设置代理

**可在 .gitconfig 中寫入或者刪除**

[參考文獻](https://www.jianshu.com/p/739f139cf13c)

```python
# 不帶賬號密碼的
git config --global http.proxy http://10.48.13.135:3128
git config --global https.proxy http://10.48.13.135:3128

# 帶賬號密碼的
git config --global http.proxy http://username:password@127.0.0.1:1080
git config --global https.proxy http://username:password@127.0.0.1:1080

# 只对github.com使用代理，其他仓库不走代理
git config --global http.https://github.com.proxy socks5://127.0.0.1:1080
git config --global https.https://github.com.proxy socks5://127.0.0.1:1080
```



## Git基本使用步骤(在GitBash窗口中操作)=>SSH

[參考前三步](https://www.itread01.com/content/1550493031.html)

[内外网ssh设置,暂未实践](https://www.it145.com/9/69685.html)


```
SSH 為 Secure Shell 的縮寫，由 IETF 的網路小組（Network Working Group）所制定；SSH 為建立在應用層和傳輸層基礎上的安全協議。SSH 是目前較可靠，專為遠端登入會話和其他網路服務提供安全性的協議。利用 SSH 協議可以有效防止遠端管理過程中的資訊洩露問題。SSH最初是UNIX系統上的一個程式，後來又迅速擴充套件到其他操作平臺。SSH在正確使用時可彌補網路中的漏洞。SSH客戶端適用於多種平臺。幾乎所有UNIX平臺—包括HP-UX、Linux、AIX、Solaris、Digital UNIX、Irix，以及其他平臺，都可執行SSH。 --百度百科
```

1. 開啓GitBash，**確認.ssh是否存在**==>(輸入	cd ~/.ssh)

2. 不存在則輸入(ssh-keygen -t rsa -C "youremail.qq.com")=>**創建.ssh文件夾**，并于該文件夾下產生公鑰和私鑰

3. 將公鑰文件(id_rsa.pub)下的内容copy到GitHub遠端【头像--》settings--》SSH and GPG keys--》New SSH key--》自己取个title--》copy内容】

4. **測試 github 外網 ssh 地址**

   ```python
   ssh -T git@github.com
   
   # ERROR  我可能是公司电脑进行了端口管控，说白了资安问题
   ssh: connect to host github.com port 22: Connection timed out 
           
   # Maybe Success
   The authenticity of host 'github.com (140.82.113.4)' can't be established.
   RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
   Are you sure you want to continue connecting (yes/no)? y
   Please type 'yes' or 'no': yes
   Warning: Permanently added 'github.com,140.82.113.4' (RSA) to the list of known hosts.
   Hi KillenNolan! You've successfully authenticated, but GitHub does not provide shell access.
   
   ```

## Git協同分支步驟

##### 1. 新建一個分支

```python
# 創建一個新的分支
git checkout -b "newbranch"

```

##### 2. 切換分支

```python
# 切換分支
git checkout master

```

##### 3.查看當前所在分支

```python
# 查看所有的分支 
$ git branch -a

# 輸出信息(小星星就是我當前所在分支)
  master
* newbranch
  remotes/origin/master   
    
```

##### 4.合并分支并處理衝突

```python
git merge
```



##  存儲連接GitHub密碼

[參考文獻](https://www.itread01.com/content/1546644067.html)

[參考文獻](https://officeguide.cc/git-save-username-and-password-tutorial/)

​		使用 **git config --global credential.helper store** 命令會生成 **.git-credentials**文件，此地會寫入賬號密碼明文。

```python
# 使用 git config --global credential.helper store 命令會生成 .git-credentials文件，此地會寫入賬號密碼明文。
# store 永久存儲
# cache 15min 
# 我的文件出來了，但是我依舊需要輸一次賬號密碼，之前需要輸入兩次，所以這是成功了吧。

```

## Git問題集

- 進行git add的時候報錯(warning: LF will be replaced by CRLF in)
  1. windows系統的換行符和Unix下的換行符不同，但是git會幫我們自動進行換行符的轉換，所以出現了這個warning
  2. 解決的方法就是，禁止自動轉換 git config --global core.autocrlf false
  
  ```python
  # 知識拓展
  # CR、LF、CR/LF為不同作業系統上使用的換行符 '\r','\n',
  # Windows/DOS系統：採用CR/LF表示下一行；
  # Unix/Linux系統：採用LF表示下一行；
  # Mac OS系統：採
  ```
  
- 









