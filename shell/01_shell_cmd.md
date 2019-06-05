# bash 命令瑞士军刀

```bash
# 统计文件夹下的文件夹个数（不包含子目录）
$ ls -l $dir |grep "^d"|wc -l

# 查找文件夹下修改时间为5天以前的文件夹（不包含子目录，通过maxdepth限定）然后删除
find $dir ! -path '.' -maxdepth 1 -mtime +5 -exec echo "Delete {}" \; -exec rm -rf "{}" \;
# 其中通过 '.' 过滤掉当前目录
```

