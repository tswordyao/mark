## 运行下面命令可以实现下载submodule源码
git submodule update --init --recursive


# Git 1.8.2增加了跟踪分支的可能性。

## add submodule to track master branch
git submodule add -b branch_name URL_to_Git_repo optional_directory_rename

## update your submodule
git submodule update --remote 

## 标准配置
```
[submodule "CommonLogin"]
	path = CommonLogin
	url = https://g.hz.netease.com/ykt-adult-front/commonlogin.git
```


## 相对仓库配置
```
[submodule "src/components/SuperTable"]
	path = src/components/SuperTable
	url = ../SuperTable.git
```    

## 分支配置   
```
[submodule "SubmoduleTestRepo"] 
    path = SubmoduleTestRepo 
    url = https://github.com/jzaccone/SubmoduleTestRepo.git 
    branch = master
```    
