
<a href="#将本地项目上传到github" style="font-size:16px">1. 将本地项目上传到github<a>

<a href="#更新远程分支列表" style="font-size:16px">2. 更新远程分支列表</a>

#### 将本地项目上传到github

    1、创建本地仓库（文件夹）

    mkdir study//创建文件夹study
    cd study //进入study文件夹

    2、通过命令git init把这个文件夹变成Git可管理的仓库
    git init //把这个文件夹变成Git可以管理的仓库

    这时可以发现study里面多了个.git文件夹，它是Git用来跟踪和管理版本库的。如果你看不到，是因为它默认是隐藏文件，那你就需要设置一下让隐藏文件可见。

    3、把项目粘贴到这个本地Git仓库里面（粘贴后你可以通过git status来查看你当前的状态），然后通过

    git add . //把项目添加到仓库（或git add .把该目录下的所有文件添加到仓库，注意点是用空格隔开的）。在这个过程中你其实可以一直使用git status来查看你当前的状态。

    git commit -m "注释"// 把项目提交到仓库。

    4、在github上创建study文件夹后

    git remote add origin https://github.com/husterlihuijuan/study.git   //与github（远程仓库）建立联系

    git push -u origin master  //把本地库study的所有内容推送到远程仓库（也就是Github）上，此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；

    至此就完成了将本地项目上传到Github的整个过程。

#### 更新远程分支列表

    git remote update origin --prune 

#### 设置全局的用户名和邮箱
    git config --global user.name "Real Name"
    git config --global user.email "e-mail@domain.tld"