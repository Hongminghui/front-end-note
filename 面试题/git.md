### 1. git fetch和git pull的区别

- `git pull`：相当于是从远程获取最新版本并`merge`到本地
- `git fetch`：相当于是从远程获取最新版本到本地，不会自动`merge`

### 2. git reset git revert

git reset [commit]: 回到某个版本

git revert [commit]: 撤销某个版本的变化，但是不影响后面的版本变化

[Git恢复之前版本的两种方法reset、revert（图文详解）_游笑天涯-CSDN博客_git revert](https://blog.csdn.net/yxlshk/article/details/79944535)