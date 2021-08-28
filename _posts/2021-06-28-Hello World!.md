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
// C++
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



```xml
<!-- Rouge 不支持 xaml，这里先用 xml 高亮 -->
<DockPanel>
    <Button DockPanel.Dock="Bottom" Width="100" Height="30"></Button>
    <Button DockPanel.Dock="Left" Width="100" Height="30"></Button>
    <Button DockPanel.Dock="Right" Width="100" Height="30"></Button>
    <Button DockPanel.Dock="Top" Width="100" Height="30"></Button>
</DockPanel> 
```



```c#
// C#
using System;
using System.Threading.Tasks;

namespace Foo
{
    class Bar
    {
        
    }
}
```



```kotlin
// kotlin
import androidx.compose.ui.Modifier
import androidx.compose.ui.geometry.Size
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.graphics.Path
import androidx.compose.ui.graphics.drawscope.Stroke
import androidx.compose.ui.platform.AmbientAnimationClock
import androidx.compose.ui.platform.setContent
import androidx.compose.ui.unit.dp
import com.example.sinuswave.ui.SinusWaveTheme
import kotlin.math.sin

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            SinusWaveTheme(darkTheme = true) {
                Surface(color = MaterialTheme.colors.background) {
                    App()
                }
            }
        }
    }
}
```



```c
// 什么也不选就完全没颜色，这里试一下用 c放纯文本
asdfghjkl;''
```



