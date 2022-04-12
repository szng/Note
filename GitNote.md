# 远程仓库

在本地建立一个裸仓库，用以同步其他各处的代码。  
1. 建立本地原始仓库
2. 从原始仓库克隆裸仓库(命令中原始仓库和裸仓库在同一目录下)

        git clone --bare ../project ./project-bare.git
3. 设置本地原始仓库的远程仓库

        git remote add origin ../project/project-bare/project-bare.git
4. 其他仓库克隆裸仓库

        git clone ssh://username@ip/path ./project