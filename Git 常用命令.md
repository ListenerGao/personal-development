# Git 命令

## 用户设置

* 查看用户配置

```
// 查看当前配置, 在当前项目下面查看的配置是全局配置+当前项目的配置, 使用的时候会优先使用当前项目的配置
git config --list
```

* Git 全局用户名邮箱设置

```
git config --global user.name "ListenerGao"
git config --global user.email "hnthgys@gmail.com"
```


* Git 指定仓库用户名邮箱设置 (在仓库路径下执行一下命令)

```
git config user.name "ListenerGao"
git config user.email "hnthgys@gmail.com"
```