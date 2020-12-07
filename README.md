# update_github_per_day
从github每日定时爬虫更新的文件 使用python脚本调用github的方式 
## step1:
- git clone 下来你要的资源（注意里面的.git文件 ls -a）

	git clone https://github.com/stamparm/maltrail.git>
这时候你的本地就会有所有的代码，找到你要的文件夹，

- git fetch 可以把本地的信息与远程仓库对比 本地的叫master 远程仓库叫origin/master



在文件加中找到不同的文件
```
 grep -rn "diff_name_only" /data/
```
找到不同的文件（香港服务器，并且写到/data/sftp/weihai_feed/maltrail/diff_name_only.txt）
```
[root@iZj6cjas64u6t81eu372kvZ maltrail]# cat  /data/weihai_feed/update_git_maltrail.py
import git
import re
repo = git.Repo('/data/sftp/weihai_feed/maltrail')
remote = repo.remote()
t=remote.fetch()
res = repo.git.diff(t)
diff_name_only = re.findall('diff --git a/trails/static/malware/(.*) b/', res)
print("update file",diff_name_only)
if len(diff_name_only)>0:
    with open("/data/sftp/weihai_feed/maltrail/diff_name_only.txt","w") as f_new:
        for i in diff_name_only:
            f_new.write(i)
            f_new.write("\n")
p=remote.pull()
```
