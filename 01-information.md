
# Chapter01 InformationCollection

## 隐藏目录泄露
### 1.1 scrabble 对.git文件进行恢复
https://github.com/denny0223/scrabble
./scrabble https://example.com
git init
git add index.php
git commmit -m "first"


### 1.2 backroll
git log
git diff HEAD commit-id
git reset --hard HEAD^

### 1.3 branch

修改分支
git checkout master #切换到基础分支
git checkout -b newBranch # 创建新的分支
git add *
git commit -m "init newBranch"
git push origin newBranch
git branch -a #查看所有分支
git branch # 查看当前分支, 带*
git checkout branch_name # swith branch

### 1.4 .git/config文件中含括access_token信息，可访问别的仓库
现有多数工具不支持branch, git log只查看当前分支
例如工具https://github.com/WangYihang/GitHacker,
python GitHacker.py http://127.0.0.1:8000/.git/
git log --all # 查看所有记录
git brance -v # 只可查看master分支
git reflog 查看checkout记录， 可看到其他分支secret

wget http://127.0.0.1:8000/.git/refs/heads/secret # 获取其他分支文件
```修改GitHacker.py, 直接调用fixmissing函数进行恢复
  if __name__ == "__main__":
     main()
    baseurl = complete_url('http://127.0.0.1:8000/.git/')
    temppath = replace_bad_chars(get_prefix(baseurl))
    fixmissing(baseurl, temppath)
```
调用python GitHacker.py进行恢复, 
在文件夹内，调用git log -all, git branch -v 可查看到分支secret
git diff HEAD commit-id, 查看分支差异

### 2.svn
.svn/entries 或 wc.db文件获取服务器源码
两类工具http://github.com/kost/dvcs-ripper, Seay-svn(windows下的源代码备份漏洞)

### 3.HG
初始化项目时，存在.hg隐藏文件夹
https://github.com/kost/dvcs-ripper

### 4.经验
目录泄露多数来自目录扫描, 开源的目录扫描工具https://github.com/maurosoria/dirsearch




### 敏感备份文件
### 1. gedit 备份文件以`~结尾, 例如flag~`
### 2.vim, 文件格式为".文件名.swap"
  # 当存在.flag.swap文件时，如何有效恢复文件
  torch flag #创建空的文件
  vim -r flag # 恢复

### 3.常规文件
robots.txt: 记录目录和一些CMS版本文件
readme.md: 记录CMS版本和地址
www.zip/rar/tar.gz 网站备份源码

### 4.总结
存在线上运维题目，需实时监看文件夹
```
vim第一次异常文件为"*.swp", 
  第二次异常为"*.swo",
  第三次异常为"*.swn"
还存在"*.un.文件名.swp"
```

# Banner识别
# 1. github上的CMS指纹
# 2.Wappalyzer工具:
```
代码实例
pip install python-Wappalyzer
from Wappalyzer import Wappalyzer, WebPage
wappalyzer = Wappalyzer.latest()
webpage = WebPage.new_from_url('http://example.com')
wappalyzer.analyze(webpage)

data/apps.json为规则，可自行添加
```

