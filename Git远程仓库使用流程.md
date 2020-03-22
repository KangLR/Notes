# Git远程仓库使用流程

1. git init  初始化本地仓库所在目录

2. git remote add github git@github.com:KangLR/MybatisLearning.git

   github是你可以自己命名的	

   后面的要点击Use SSH然后复制（前提是你要把自己的公钥给添加再网站上）

   公钥在类似于C:\Users\klr10的.ssh文件夹下

   id_rsa是密钥

   id_rsa.pub是公钥

   ![image-20200307193951950](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200307193951950.png)

3. git pull --rebase github master

   拉取网站上的仓库的内容与本地合并

   --rebase参数不加的话可能会报错

4. git add XXX(你仓库中的内容）

   ...

   git add XXX

5. git commit -m 'XXX'

   一次性把暂存区的内容提交到版本库

6. git checkout -b version1

   此时你可以创建一个分支也可以不创建

7. git push github master

   把master分支的内容上传到网站上的仓库

8. git push github version1

   这是网站上的仓库会自动创建一个分支存储version1分支的内容

   其实此时的master和version1两个分支的内容是相同的，因为创建version1后并没有对它进行修改

9. git checkout master

   可以把HEAD切换到master分支

10. git branch

    查看此时的分支状态