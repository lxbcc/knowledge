本地文件上传到 github 步骤
1. 初始化本地仓库
   git init

2. 把文件添加到版本库中，使用命令 git add .添加到暂存区里面去，不要忘记后面的小数点“.”，意为添加文件夹下的所有文件
   git add .

3. 用命令 git commit告诉Git，把文件提交到仓库。引号内为提交说明
   git commit -m '提交信息'

4. 关联到远程仓库
   git remote add origin 你的远程库地址
