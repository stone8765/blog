---
layout:	post
title:	Github Blog 搭建
author: StoneLi
description:	使用frp内网穿透工具可以让内网中的电脑能够像访问公网电脑一样方便，比如将公司或个人电脑里面的Web项目让别人能够访问、或进行电脑远程连接、或ssh连接
catalog: true
tags: [jekyll,github pages]
---


# 1. 安装Ruby
https://rubyinstaller.org/
# 2. 下载安装gem (Ruby的包管理器)
下载：https://rubygems.org/pages/download
解压之后 在目录中执行以下命令
ruby setup.rb

# 3. 安装jekyll
在命令行执行gem install jekyll

# 4. 运行
jekyll new myblog
cd myblog
jekyll server

在浏览器输入http://127.0.0.1:4000/即可浏览刚刚创建的blog
# 5. Jekyll 主题选择
进入网站 http://jekyllthemes.org/
选择主题，下载对应的仓库代码到本地即可

# 6.文件中文名本地无法显示的问题
修改安装目录\Ruby22-x64\lib\ruby\2.2.0\webrick\httpservlet下的filehandler.rb文件，建议先备份。
找到下列两处，添加一句（+的一行为添加部分）
```
path = req.path_info.dup.force_encoding(Encoding.find("filesystem"))
+ path.force_encoding("UTF-8") # 加入编码
if trailing_pathsep?(req.path_info)
```

```
break if base == "/"
+ base.force_encoding("UTF-8") # 加入编码
break unless File.directory?(File.expand_path(res.filename + base))
```

修改完重新jekyll serve即可支持中文文件名。 