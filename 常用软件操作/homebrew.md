```
homebrew

替换 brew.git:
git -C "$(brew --repo)" remote set-url origin https://mirrors.ustc.edu.cn/brew.git

替换 homebrew-core.git:
git -C "$(brew --repo homebrew/core)" remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git

替换 homebrew-cask.git：
git -C "$(brew --repo homebrew/cask)" remote set-url origin https://mirrors.ustc.edu.cn/homebrew-cask.git

替换homebrew-bottles：
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile
source ~/.bash_profile


Homebrew 主要由四个部分组成, 对应的功能如下：

组成				功能
Homebrew		源代码仓库
homebrew-core	Homebrew 核心源
homebrew-cask	提供macos应用和大型二进制文件的安装
homebrew-bottles	预编译二进制软件包
```