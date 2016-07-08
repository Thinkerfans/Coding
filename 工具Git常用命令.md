#Git 工具常用命令记录
### git基本命令
1. ```git add <file or path>```
>Add file contents to the index;将要提交的文件或目录信息保存到索引库。
 
2. ```git commit -m "message"```
>Record changes to the repository;将本地修改保持到本地仓库

3. ```git push origin master ```
>Update remote refs along with associated objects;更新远程索引和相关的对象。即将本地仓库的修改上传到服务器仓库。


### 标签处理命令 git tag
1. 创建标签: ```git tag -a v1.4 -m 'tag message' ```
2. 删除标签: ```git tag -d v1.4 ```
3. 查看标签: ```git tag ```
4. 修改标签: ``` ```
5. 提交标签: ```git push origin v1.4```



