# [使用github issue 写博客 记录](https://github.com/yytmzys/blog/issues/2)

使用github issue 写博客 记录 , yihong0618/[gitblog](https://github.com/yihong0618/gitblog)

### 1 创建工程
<img width="717" alt="1" src="https://github.com/yytmzys/blog/assets/45475313/e7100ea3-af37-477d-b4cb-ea7c207b4cc2">

###  2 新建/上传文件 标红的两个文件, 文件内容不改 
<img width="851" alt="2" src="https://github.com/yytmzys/blog/assets/45475313/e3269b17-e3dd-4175-b593-17348bbe707f">

### 3 新建文件, 输入框输入 BACKUP/.gitkeep,  这样就会出现 文件夹 和空白文件 .gitkeep
<img width="831" alt="3" src="https://github.com/yytmzys/blog/assets/45475313/d04b0c63-5e7e-4fef-933d-0b8b9e1600af">


### 4 新建文件, 输入框输入 .github/workflows/generate_readme.yml
复制文件内容, 修改三项
master改为main, 因为新建的工程仓库默认是主分支名字main
env:
  GITHUB_NAME: qq
  GITHUB_EMAIL: qq@qq.com
<img width="467" alt="4" src="https://github.com/yytmzys/blog/assets/45475313/2f80f394-3fc0-4b62-9c40-57eea049725a">

这一项 secrets.G_T 改或不改, 
<img width="580" alt="5" src="https://github.com/yytmzys/blog/assets/45475313/27a058b3-a1ba-4550-b7d4-890a68dc39b5">

### 5 添加token, 全部权限 https://github.com/settings/tokens, 
完成复制token, 保存起来
<img width="1018" alt="6" src="https://github.com/yytmzys/blog/assets/45475313/7147d469-3b4c-4a17-9288-d4c120450007">

### 6 将token添加到 Actions secrets and variables, 我们创建 编辑 issue, 会触发 generate_readme 这里的方法, 
<img width="1047" alt="7" src="https://github.com/yytmzys/blog/assets/45475313/30f51f95-f1cc-4e8d-8b17-2701b1091b58">

### 7 为 Actions 添加读写权限, 
<img width="919" alt="8" src="https://github.com/yytmzys/blog/assets/45475313/fb1f910b-5ef1-4a69-8403-4ba27c7ad94f">
<img width="860" alt="9" src="https://github.com/yytmzys/blog/assets/45475313/4813d1e1-4e6b-472c-9547-4b7a58b8e653">
### 8 添加一条issue, 标题 内容, 触发 Action, 自动将issue添加到BACKUP文件夹, 创建/编辑issue会自动同步到 README.md, 形成目录

<img width="730" alt="10" src="https://github.com/yytmzys/blog/assets/45475313/98c1e08f-0b30-4316-8038-b6eabe0f82d1">
