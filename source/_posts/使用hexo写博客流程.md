---
title: 使用hexo发布博客流程
date: 2020-05-15 17:14:45
tags:
---
如果在一个新机器上发布博客，首先需要将hexo分支克隆到本地，然后`cd`到leol3.github.io目录，执行`npm install`  
<h4>新建文章</h4>

新建文章的命令为`hexo new page-name`，然后编辑`source/_posts/page-name.md`即可  
<h4>上传图片到相册</h4>

如果在新机器上，首先将`leol3/Blog_Album`代码克隆到本地，修改tool.py脚本中data.json对应的路径，并把照片放到photos目录下，执行`python tools.py`即可，另外需要将照片上传到阿里云oss的min_photos目录下（后续可以考虑通过脚本tools.py直接上传到阿里云oss）
<h4>部署到网站</h4>

执行以下命令：  
```
hexo clean
hexo g
hexo d
```
以上命令会将最新的网站静态文件push到github的master分支  

另外需要通过`git push`命令更新hexo分支下的博客源文件
