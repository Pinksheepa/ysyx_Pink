[toc]

---
# 学习笔记
Keep It Simple, Stupid
笔记只会记下笔者认为必要的东西，仅供参考

[系统设计黄金法则：简单之美](https://blog.sciencenet.cn/blog-414166-562616.html)
[提问的智慧](https://github.com/ryanhanwu/How-To-Ask-Questions-The-Smart-Way/blob/main/README-zh_CN.md#%E7%AC%AC%E4%BA%8C%E6%AD%A5%E4%BD%BF%E7%94%A8%E9%A1%B9%E7%9B%AE%E9%82%AE%E4%BB%B6%E5%88%97%E8%A1%A8)和[别像弱智一样提问](https://github.com/tangx/Stop-Ask-Questions-The-Stupid-Ways/tree/master)

1. 简单，简单，先跑起来再说。
2. 精细化求助范围（对应社区）https://stackexchange.com/sites
3. 问题明确环境和差异，精简但要筛选出必要的可供推测的条件
4. 去情绪化，保持礼貌,  多多感谢
5. 目标+（环境）+差异
> 我正在尝试`···`，已知`···`，但是`···`

>请问一个关于 `···` 的问题。
我想要达到 `···` 效果，但是我这样做出现了 `···` 的问题。
报错日志是 `···` 的。（要 学会 画关键字）
我尝试过 `···` 方法来解决。
我尝试搜索过了 `···` 关键字，在里面找到了 这些 `URL` 的回答，尝试了还是没有解决问题。
我用的是 `···` 操作系统，版本号是多少。
我用的是 `···` 软件，版本号是多少。
谢谢

---
# 工具使用

## git使用
- 生成ssh私匙和公匙
  1. 查看现有的私匙 `cd ./.ssh`
  2. 生成密匙 `ssh-keygen -t rsa -C "xxx@xxx.com"`
  3. 查看公匙 `cat id_rsa.pub`
  4. 在远程仓库添加公匙

- 添加多个ssh密匙时，在.ssh文件夹中应配置config文件用于配置私匙对应的服务器。

    ```
    # gitlab
    Host gitlab.ylwnl.com  　
    HostName gitlab.xxx.com 　　
    PreferredAuthentications publickey  
    IdentityFile ~/.ssh/gitlab_id_rsa 

    # github
    Host github.com
    吧                                                                                     
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/github_id_rsa

    # gitee
    Host gitee.com
    HostName gitee.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/gitee_id_rsa
    ```
- 常用指令
    操作:   git init    git clone
            git add(--all)     git commit -m "xxx"     git push    
            git checkout (-b xxx)（创造并切换到xxx分支）
            git merge --no-f xxx（无论能否都提交到仓库）
            git reset --hard (xxx)（时间点的哈希值）
            git remote add      git push\pull
            git push --set-upstream （添加当前分支内容到远程仓库）
    ```

    ```
    状态：  git status  git log (--pretty==short简短信息) (xxx相关文件日志)
            git branch (xxx)（创造xxx分支）
            git log --graph
            git reflog（log为当前状态为终点，reflog为仓库操作日志）
    ```
- 在Github上查看分支差别（使用URL）
  > 两个分支比较        https://github.com/Pinksheepa/ysyx-2024 `/main...branch-A`
  > 七天前比较          https://github.com/Pinksheepa/ysyx-2024 `/main{@7.day.ago}...main`
  > 具体某一日期        https://github.com/Pinksheepa/ysyx-2024 `/main{@2024-12-12}...main`

- 现有仓库添加子仓库
  1. 添加 `git submodule add <子仓库URL> <路径>`
  2. 更新 `在子仓库中操作,再父仓库提交`
  在父仓库中，子仓库的内容默认是固定的，只会随着父仓库的提交而更新子仓库的引用。
  3. 删除
    - 删除子仓库引用  `git submodule deinit <子仓库路径>`
    - 删除子仓库的文件 `git rm <路>`
    - 删除子仓库 `rm -rf <子仓库>`
    - 报错`git submodule: already exists in the index`原因是暂存区还存在
      `git rm -r --cached <路径>`

## vscode 

- anaconda管理的虚拟环境要激活使用
  ```
  conda activate xxx(环境名或者绝对路径)
  
  conda env list(查看当前所处环境)
  ```



---

## Linux基础学习
1. apt依赖冲突问题，可改用aptitude安装
2. `source ~/.bashrc`可以强制重新加载 .bashrc 文件，使更改立即生效，而不需要关闭并重新打开终端
   如果你对 .bashrc 文件进行了修改（例如添加了新的别名或环境变量），这些更改不会立即生效，因为 .bashrc 只在启动新的 shell 时加载。
3. **函数中的arg是什么意思？**
   `int main(int argc, char *argv[])`
   arg——argument，参数
   |          名称          |        含义        |      例子      |
   | :--------------------: | :----------------: | :------------: |
   |          argc          | int	argument count |    参数个数    |
   | argv	char*[] 或 char** |  argument vector   | 参数字符串数组 |
4. **为什么printf()的输出要换行?**
    标准输出（stdout）默认采用行缓冲模式：当输出内容包含换行符\n时，缓冲区会立即刷新，将内容显示到终端；若不换行，则需等待缓冲区填满或程序正常结束才会刷新。
    ```
        #include <stdio.h>
        int main() {
            printf("No newline");  // 可能不显示
            // fflush(stdout);     // 手动刷新可解决
            return 0;               // 程序正常退出时，缓冲区自动刷新
        }
    ```

### 虚拟机VMvare

- :watermelon: 共享文件夹挂载方式
`echo '.host:/ /home/pink/share fuse.vmhgfs-fuse allow_other,defaults 0 0' >> /etc/fstab`


### VIM操作

- 常用指令
  ```
  - 文字移动 `hjkl`  `w` `e` `b`   (`%`符号移动) `o`
  - 修改 `i` `a` `A` `d` `r` `c`  `:(%)s/.../.../g`
    > `d + w // + $ // + e // + dd`
    > `y` `p`
  - 数字用于重复次数      `d2w` `2w` `0`
  - 撤销 `u` ctrl+r 恢复
  - `p` 将删除暂存的内容插入
  - 页面移动 ctrl+G `gg` `G` `...G` 
  - 查找 `/...` `n` `N`
  - 退出和保存 `:w ...` `:q!` 
  - 运行外部指令 `:!` 
  - 文本选择 `v`
  - 文本融合 `:r`
  ```

- 修改windows编写的shell脚本  `:set ff=unix`






### 命令行操作
```
文件管理 - cd, pwd, mkdir, rmdir, ls, cp, rm, mv, tar
文件检索 - cat, more, less, head, tail, file, find
输入输出控制 - 重定向, 管道, tee, xargs
文本处理 - vim, grep, awk, sed, sort, wc, uniq, cut, tr
正则表达式
系统监控 - jobs, ps, top, kill, free, dmesg, lsof
```
- 添加环境变量
  
  1. 进入bashrc文件
    ```
    cd ~
    vim .bashrc
    ```
  2. 在文件末尾添加环境变量
  ```
    变量名=变量值...=...
    export 变量名 ...
  如：JAVA_HOME=/opt/jdk-12.0.2
  　　CLASSPATH=.
  　　PATH=$JAVA_HOME/bin:$PATH
  　　export JAVA_HOME CLASSPATH PATH
  ```
    3. 最后运行.bashrc文件脚本，使立即生效（否则重启）
  `source .bashrc # 注：如果不执行 source 命令，则需重启系统才能生效`

- 一键把空格替换为Tab（wrong separator）
  `sed -i 's/^    /\t/' xxx.mk` (sudo)
 
### 安装和使用verilator

[参照官网手册-git安装](https://verilator.org/guide/latest/install.html#detailed-build-instructions)

https://zhuanlan.zhihu.com/p/672636291

[使用verilator和NVboard](https://blog.csdn.net/Naruto123456__/article/details/141272106)
此外还在此基础上编写了生成tb_top.cpp的Makefile文件，一键编译更加方便

---

# RISC-V学习

## 英文缩写释义
- **rd** destination register
- **rs** source register
- **imm** 立即数



## 基本指令集

### 基本指令
- **addi**
- **auipc**


- **jal**
- **jalr**

- **sw** 将源寄存器rs2的内容存储到内存地址(rs1+imm)处，存储4字节宽度的数据

- **ebreak**



### 伪指令
方便编程而存在的指令，由基本指令组成
- **li** `addi rd, x0, imm`
- **mv**  `addi a0, a0, 0`

- **ret** `jalr x0, 0(ra)` 跳转到ra寄存器中保存的地址
- **j** `jal x0, offset` 