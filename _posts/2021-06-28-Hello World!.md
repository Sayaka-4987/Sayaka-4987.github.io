---
layout:     post   				        # 使用的布局（不需要改）
title:      My First Post 			    # 标题 
subtitle:   Hello World, Hello Blog     # 副标题
date:       2021-06-28 				    # 时间
author:     YXWang 					    # 作者
header-img: img/post-bg-2015.jpg 	    # 这篇文章的标题背景图片
catalog: true 						    # 是否归档
tags:								    # 标签
    - First post
---

### Hello World! 
>这是我的第一篇博客。

进入博客主页，新的文章将会出现在主页上。

这篇文章也被用于测试代码高亮



```c++
FILE *fp = fopen("conf.ini","w");

fprintf(fp,"%s\n",conf.filesavepath);
fprintf(fp,"%s\n",conf.filename);
fprintf(fp,"%d\n",conf.maxvalue1);
fprintf(fp,"%d\n",conf.minvalue1);
fprintf(fp,"%d\n",conf.maxvalue2);
fprintf(fp,"%d\n",conf.minvalue2);
fprintf(fp,"%d\n",conf.recordcount1);
fprintf(fp,"%d\n",conf.recordcount2);
```



{% highlight cpp %}

FILE *fp = fopen("conf.ini","w");

fprintf(fp,"%s\n",conf.filesavepath);
fprintf(fp,"%s\n",conf.filename);
fprintf(fp,"%d\n",conf.maxvalue1);
fprintf(fp,"%d\n",conf.minvalue1);
fprintf(fp,"%d\n",conf.maxvalue2);
fprintf(fp,"%d\n",conf.minvalue2);
fprintf(fp,"%d\n",conf.recordcount1);
fprintf(fp,"%d\n",conf.recordcount2);

 {% endhighlight %}
