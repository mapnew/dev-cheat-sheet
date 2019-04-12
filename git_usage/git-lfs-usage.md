# Git LFS

## 简介

[git lfs](<https://git-lfs.github.com/>) 将大文件内容（如音/视频，图片和模型等）存储在远程服务器上，在git仓库仅用占用空间极小的文本指针替换大型文件。

- 大文件存储在 Git 仓库之外，减小 Git 仓库体积，使克隆 Git 仓库的速度加快，避免Git性能损耗
- 在默认情况下，只有当前签出的 commit 下的 LFS 对象的当前版本会被下载，在git仓库仅用占用空间极小的文本指针替换大型文件，如：

```
version https:``//git-lfs.github.com/spec/v1
oid sha256:74317704769af0bd58bde36009bcb8a83ab6701426b2a32782ad83a6f5d9ef90
size ``99884
```

在下载当前签出的commit下的lfs对象时正是依赖该文本指针找到的。

## 安装和配置

> 要求 git  >= 1.8.2

下载和安装：<https://git-lfs.github.com/>

如果是自建的 LFS 服务器， 需替换服务器 URL: `git config --global lfs.url  "your lfs url"`

自建的LFS服务器如果是采用 Basic Authentication，为了避免重复输入用户名和密码，可以打开 git-credential 保存： `git config --global credential.helper store`, 设置后，该密码会被保存在 `~/.git-credentials` 文件中，而用户名被添加到 `~/.gitconfig` 文件中



## 基本使用

1. 确保本机安装过 LFS, 可以通过 `git lfs env`确认

2. 使用 `git lfs track` 追踪需要使用 Git LFS 管理的文件，如下，该命令会修改`.gitattribute`文件，个人不推荐手动修改 `.gitattribute`

   ```shell
   # 假设仓库中新增了两个大文件 video.h264 和 common.a;
   # 使用LFS来管理 .a 和 .h264 类型文件
   $ git lfs track "*.a"
   $ git lfs track "*.h264"
   # 使用引号，可以确保将来添加的 .h264 和 .a 也会用 LFS 管理
    
    
   # 确保 .gitattributes 被git追踪
   $ git add .gitattributes
   $ git add video.h264 common.a
   $ git commit "add static library and video"
   
   ```

3. 运行`git lfs ls-files` 可以查看已经用 lfs 追踪的大文件，也可以运行 `git lfs track` 查看当前使用 Git LFS 管理的匹配列表

   ```shell
   $ git lfs ls-files
   a3aa7f5654 * common.a
   c99115880a * video.h264
   
   $ git lfs track
   Listing tracked paths
       *.a (.gitattributes)
       *.h264 (.gitattributes)
   ```

4. 如果要取消使用Git LFS来管理相应的大文件，如：

   ```shell
   $ git lfs untrack "*.a"
   Untracking *.a
   ```

5. 获取当前对应的 git lfs 对象文件

   ```shell
   $ git lfs pull
    
   # 该命令等同于
   $ git lfs fetch
   $ git lfs checkout
   ```

   

## 更多使用

- 如果想要只获取仓库本身，而不获取任何 LFS 对象, 比如当你的工作不涉及 LFS对象时，可以跳过LFS的下载。

  ```shell
  GIT_LFS_SKIP_SMUDGE=1 git clone git@your repository url.git
  # 或
  git -c filter.lfs.smudge= -c filter.lfs.required=false clone git@your repository url.git
  ```

  注意 `GIT_LFS_SKIP_SMUDGE=1` 和 `git -c filter.lfs.smudge= -c filter.lfs.required=false`也适用于其他 git 命令，如 checkout  和 pull.

## 使用中可能遇到的问题

- Q: 涉及LFS文件下载时，报错信息如下："batch response: Git LFS is not enabled on this GitLab server, contact your admin."

  该问题是因为没有配置正确的 LFS 服务器地址导致，可以先通过运行`git lfs env` 检查其中的 `Endpoint`字段，再重新配置远程服务器地址： `git config --global lfs.url  "your lfs url"`

- Q: 在安装 Git LFS 之前，克隆了使用 Git LFS 的仓库，则被 Git LFS 管理的文件会被显示为文本指针，而非具体的文件。查看这些文件指针，会发现类似如下内容：

  ```
  version https:``//git-lfs.github.com/spec/v1
  oid sha256:74317704769af0bd58bde36009bcb8a83ab6701426b2a32782ad83a6f5d9ef90
  size ``99884
  ```

  解决办法：安装 GIT lFS 后手动下载 LFS 对象:  `$ git lfs pull`

- Q: 出现网络原因导致下载 LFS 文件中断，解决方法: `$ git lfs pull`

- Q:  用LFS 管理的文件都显示为被修改 modified 状态，导致每次要提交代码前需要 reset 回未修改状态，可以先运行 `git lfs env`查看 filter 配置

  ```shell
  $ git lfs env
  ...
  git config filter.lfs.smudge = ``"git-lfs smudge -- %f"
  git config filter.lfs.clean = ``"git-lfs clean -- %f"
  ```

  如果未配置，可以通过以下命令配置：

  ```shell
  git config --global filter.lfs.required ``true
  git config --global filter.lfs.clean ``"git-lfs clean -- %f"
  git config --global filter.lfs.smudge ``"git-lfs smudge -- %f"
  ```

  或是在 `~/.gitconfig` 中修改

  ```
  [filter "lfs"]
          clean = git-lfs clean -- %f
          smudge = git-lfs smudge -- %f
          process = git-lfs filter-process
          required = true
  ```

  

## 参考

<https://github.com/git-lfs/git-lfs/wiki/Tutorial>

<https://github.com/git-lfs/git-lfs/tree/master/docs>