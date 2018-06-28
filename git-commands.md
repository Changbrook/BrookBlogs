#### git 从远程仓库获取所有分支



```
git branch -r | grep -v '\->' | while read remote; do git branch --track "${remote#origin/}" "$remote"; done
git fetch --all
git pull --all
```



#### git 新建远程仓库

##### Create a new repository

git clone git@code-sh.huawei.com:OnapDevelopers/aaf.cadi.git

cd aaf.cadi

touch README.md

git add README.md

git commit -m "add README"

git push -u origin master

##### Existing folder

cd existing\_folder

git init

git remote add origin git@code-sh.huawei.com:OnapDevelopers/aaf.cadi.git

git add .

git commit

git push -u origin master

##### Existing Git repository

cd existing\_repo

git remote add origin git@code-sh.huawei.com:OnapDevelopers/aaf.cadi.git

git push -u origin --all

git push -u origin --tags

