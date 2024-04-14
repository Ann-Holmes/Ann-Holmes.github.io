---
title: 用VSCode打造一个LaTeX编辑器
toc: true
date: 2021-02-03 17:11:38
tags: 
- LaTeX
- vscode
- 配置
categories: 
- [软件配置,vscode,LaTeX]
---

## 准备工作

1.  安装VSCode

    VSCode安装请自行到官网查看教程. 

2.  安装LaTeX Workshop插件

    这个很简单, 直接进入到VSCode搜索LaTeX Workshop, 安装即可. 
    
    ![安装插件并且转到json配置](https://img-blog.csdnimg.cn/2019121911224248.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1NDkwMzkz,size_16,color_FFFFFF,t_70)

<!--more-->
## 配置编译选项

>   之前我查了很多资料想把VSCode配置成一个LaTeX编辑器, 但是困于自己LaTeX还没有入门很多配置看不懂, 就一直没有配置. 现在, 基本上能看懂说明的内容了. 
>
>   如果你是一个LaTeX新手, 建议先不要折腾编辑器, 先用TeXstudio, 等基本入门甚至熟练之后, 再来折腾编辑器, 最好可以把LaTeX Workshop的[说明文档](https://github.com/James-Yu/LaTeX-Workshop/wiki)看一遍, 这样还可以加深自己对于LaTeX的理解. 
>
>   下面是我自己折腾的过程的记录, 供新手小白参考. 

​	在LaTeX Workshop的插件支持下, 使用VSCode写LaTeX是非常方便的, 代码高亮很舒适, 智能提示也可以让写代码的效率提高, 还有很多非常实用的[snipets](https://github.com/James-Yu/LaTeX-Workshop/wiki/Snippets#Handy-mathematical-snippets), 都让写LaTeX代码变得很舒适. 但是最大的问题是**编译**. 

### 默认编译器

LaTeX Workshop默认的编译器是`pdflatex`, 这个编译器是不能编译中文文档的, 因此只要文档中有中文, 编译就会报错. 我还想用插件中支持的一个非常好用的功能, 这个功能是**编译之后的文档, 再有修改, 每次保存, 使用默认编译器编译**, 这就需要把默认的编译器设置成`xelatex`, 这样每次保存才不会报错. 

​	修改默认编译器的配置代码如下: 

```json
"latex-workshop.latex.recipes": [
	{
        "name": "xelatex",
        "tools": [
            "xelatex"
        ]
    }
]
```

​	解释一些上述代码, `"latex-workshop.latex.recipes"`表明要设置的选项, 该键对应的值就是要设置的部分, `"name"`只是表明一个recipe的名称, `"tools"`才是真正的编译器部分, 可以有多个编译器, 最终执行该recipe的时候, 按顺序使用`"tools"`中编译器, 编译文档. 

### Recipes

​	LaTeX在写论文时, 往往不只编译一次, 需要用不同的编译器按顺序编译多次. LaTeX Workshop插件支持创建一个recipe, 当使用这个recipe编译时, 就会按照recipe中规定的编译器顺序编译文档, 最终生成所需的pdf文档. 

​	配置recipe的代码如下: 

```json
"latex-workshop.latex.recipes": [
	{
        "name": "xelatex`x2", 
        "tools": [
            "xelatex",
            "xelatex"
        ]
    },
    {
        "name": "xelatex ➞ bibtex ➞ xelatex`×2",
        "tools": [
            "xelatex",
            "bibtex",
            "xelatex",
            "xelatex"
        ]
    }
]
```

​	解释上面的代码, 上面recipe的值是一个列表, 列表的第一个元素是默认使用的recipe, 我这里设置的是用`xelatex`编译两次, 因为一般情况下, 文档里面有交叉引用都是需要编译两次的. 列表的第二个元素是需要手动选用的recipe, 我是用来编译有参考文献的文档的. 如果有需要可以配置多个recipe, 分别用来编译不同的文档. 

![recipe在插件中的显示](https://img-blog.csdnimg.cn/20191219112357187.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1NDkwMzkz,size_16,color_FFFFFF,t_70)

### Tools

​	在recipe中设置的编译器, 都是需要在Tools选项中设置的. 设置代码如下: 

```js
"latex-workshop.latex.tools": [
    {
        "name": "latexmk",
        "command": "latexmk",
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "-pdf",
            "-outdir=%OUTDIR%",
            "%DOC%"
        ],
        "env": {}
    },
    {
        "name": "xelatex",
        "command": "xelatex",
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "%DOC%"
        ],
        "env": {}
    },
    {
        "name": "bibtex",
        "command": "bibtex",
        "args": [
            "%DOCFILE%"
        ],
        "env": {}
    }
]
```

​	上面的代码我是从LaTeX Workshop的文档中拷贝的, 其中我只把`pdflatex`的`"name"`和`"command"`的值修改成了`xelatex`, 我觉着仅仅这样修改应该是有问题的. 因为我再编译的时候发现有一个警告, 这个警告还没解决. 有看出问题的, 希望可以在评论区交流. 

![这就是那个警告的描述, 但是不影响编译的结果, 暂时不打算解决](https://img-blog.csdnimg.cn/20191219112425926.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1NDkwMzkz,size_16,color_FFFFFF,t_70)

## 完整的配置代码

​	我打开配置, 转成`json`配置模式(如何转成json配置, 参看第一幅图), 发现我的配置是空的, 刚开始我是一脸懵逼的, 后来才明白, 默认配置在这里不显示, 只有更改配置后这里才会显示. 下面就是我的完整配置代码, 供大家参考: 

```js
{
    "window.zoomLevel": 2,
    "latex-workshop.latex.recipes": [
        {
            "name": "xelatex`x2",
            "tools": [
                "xelatex",
                "xelatex"
            ]
        },
        {
            "name": "xelatex ➞ bibtex ➞ xelatex`×2",
            "tools": [
                "xelatex",
                "bibtex",
                "xelatex",
                "xelatex"
            ]
        }
    ],
    "latex-workshop.latex.tools": [
        {
            "name": "latexmk",
            "command": "latexmk",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-pdf",
                "-outdir=%OUTDIR%",
                "%DOC%"
            ],
            "env": {}
        },
        {
            "name": "xelatex",
            "command": "xelatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOC%"
            ],
            "env": {}
        },
        {
            "name": "bibtex",
            "command": "bibtex",
            "args": [
                "%DOCFILE%"
            ],
            "env": {}
        }
    ],
    "latex-workshop.view.pdf.viewer": "tab"
}
```

![完整的配置(其中window.room.level表示, 窗口的缩放, 我为了可以截图完整就把缩放改成了0.5, 我平时都设置的是2, 这样整个界面的字号都会变大, 我看着不太累)](https://img-blog.csdnimg.cn/20191219112509432.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1NDkwMzkz,size_16,color_FFFFFF,t_70)





​	

