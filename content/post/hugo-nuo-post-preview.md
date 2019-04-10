---
title: "Hugo 主题 Nuo 文章样式预览"
tags: ["Markdown", "Theme", "Hugo"]
date: 2014-07-17
draft: true
---

这篇文章集中说明本人博客主题所支持的 Markdown 语法和 Hugo Shortcodes 插件，你也可以在这里预览到他们的样子。如果你不喜欢某些部分的样式，可以去修改 `content.scss` 和 `shortcodes.scss` 这两个文件。预告一下，我所用的这个名为 `Nuo` 的 `Hugo` 也将于近期发布，敬请期待。



## 1. 标题

# H1
## H2
### H3
#### H4
##### H5
###### H6

## 2. 段落

使用单引号 `*` 或者单下划线 `_` 标记 *斜体强调* 或者 _斜体强调_

使用两个引号 `**` 或者两个下划线 `__` 标记 **加粗强调** 或者 __加粗强调__

引号和下划线可叠加使用 → **只是加粗 _斜体并加粗_**

使用两个波浪线 `~~` 标记 ~~已删除文字~~

插入文字暂无 `Markdown` 标记，直接使用 `HTML` 标签 `<ins>` 标记 <ins>插入文字</ins>

行内代码使用反引号标记 → `print("hello world")`

上标 X<sup>2</sup> / 下标 X<sub>2</sub>

按键 <kbd>Ctrl</kbd>

外链 [chekun's blog](https://chekun.me)

页面内段落 [图片](#section-07)

*注意：你可以通过 `{#section-id}` 方式自定义段落锚点*
参考资料 <sup>[[1]](#ref01)</sup><sup>[[2]](#ref02)</sup>

数字引用 [编号为 1 的链接](1)

## 3. 列表
以下的无序、有序和任务列表均支持二级嵌套，不建议使用二级以上嵌套。

### 3.1 无序列表

* 无序列表
  - 嵌套的无序列表
  - 嵌套的无序列表
* 无序列表
  1. 嵌套的有序列表
  2. 嵌套的有序列表
* 无序列表

### 3.2 有序列表

1. 有序列表
  1. 嵌套的有序列表
  2. 嵌套的有序列表
2. 有序列表
  - 嵌套的无序列表
  - 嵌套的无序列表
3. 有序列表

### 3.3 定义列表

CSS
: 层叠样式表

### 3.4 任务列表

- [ ] Cmd Markdown 开发
  - [ ] 改进 Cmd 渲染算法，使用局部渲染技术提高渲染效率
  - [ ] 支持以 PDF 格式导出文稿
  - [x] 新增Todo列表功能 [语法参考](https://github.com/blog/1375-task-lists-in-gfm-issues-pulls-comments)
  - [x] 改进 LaTex 功能
  - [x] 修复 LaTex 公式渲染问题
  - [x] 新增 LaTex 公式编号功能 [语法参考](http://docs.mathjax.org/en/latest/tex.html#tex-eq-numbers)
- [ ] 七月旅行准备
  - [ ] 准备邮轮上需要携带的物品
  - [ ] 浏览日本免税店的物品
  - [x] 购买蓝宝石公主号七月一日的船票

## 4. 引用

> 野火烧不尽，春风吹又生。
>
> <cite>-- 白居易《赋得古原草送别》</cite>

## 5. 代码

以本站的一段 `JavaScript` 代码做演示。

```javascript
// Initialize video.js player
if (document.getElementById('my-player') !== null) {
  /* eslint-disable no-undef */
  videojs('#my-player', {
    aspectRatio: '16:9',
    fluid: true,
  });
}
```

## 6. 分割线

---

中间能写字的分割线，如果你修改了分割线中字的内容，请配合修改 `CSS` 样式。

## 7. 图片 {#section-07}

不带标题的图片，如下图👇

![这是一只梅花鹿](/media/posts/hugo-nuo-post-preview/01.jpg)

带标题的图片（zoomable），如下图👇

{{% figure src="/media/posts/hugo-nuo-post-preview/01.jpg" alt="这是一只梅花鹿" title="" class="zoomable" %}}

## 8. 表格

使用 `Markdown` 画的表格，如下表👇

| Tables        |      Are      |  Cool |
| :------------ | :-----------: | ----: |
| col 3 is      | right-aligned | $1600 |
| col 2 is      |   centered    |   $12 |
| zebra stripes |   are neat    |    $1 |

使用 `HTML` 画的表格，如下表👇

*注意：下表叠加应用了 `is-centered` `is-striped` `is-bordered` `is-narrow` 四种表格样式。*

<table class="is-centered is-striped is-bordered is-narrow">
  <tr>
    <th rowspan="2">值班人员</th>
    <th>星期一</th>
    <th>星期二</th>
    <th>星期三</th>
  </tr>
  <tr>
    <td>李强</td>
    <td>张明</td>
    <td>王平</td>
  </tr>
</table>

## 9. 数学公式

主题使用了 [MathJax](https://www.mathjax.org/) 开源库来实现对数学公式的支持，使用 `$$` 标记。

<div>$$
\left\{
\begin{aligned}
N & = pq \\
\varphi(n) & = (p-1)(q-1)\\
\end{aligned}
\right.
\Rightarrow
N - \varphi(n) + 1 = p + q
$$</div>

## 10. JSFiddle

引入 [JSFiddle](https://jsfiddle.net/) 网站的代码范例，在主题目录 `layouts/shortcodes` 文件夹下的 `jsfiddle.html` 对该标签进行定义。

{{% jsfiddle "laozhu/L479wueo" "html,css,result" %}}

## 11. Codepen

引入 [Code Pen](https://codepen.io/) 网站的代码演示，在主题目录 `layouts/shortcodes` 文件夹下的 `codepen.html` 对该标签进行定义。

{{% codepen "RoaWdE" "🐍 Snake Rush" "laozhu" "Ritchie Zhu" "600" %}}

## 12. 声享 PPT

引入 [声享](https://ppt.baomitu.com/) PPT 演示文稿，在主题目录 `layouts/shortcodes` 文件夹下的 `shengxiang.html` 对该标签进行定义。

{{% shengxiang "a8a49a00" %}}

## 13. 本地视频

主题使用了 [video.js](http://videojs.com/) 播放视频文件，你还可以自己定义视频的封面，在主题目录 `layouts/shortcodes` 文件夹下的 `video.html` 对该标签进行定义。

{{% video
  "/media/posts/hugo-nuo-post-preview/videojs.mp4"
  "/media/posts/hugo-nuo-post-preview/videojs.webm"
  "/media/posts/hugo-nuo-post-preview/videojs.ogv" %}}

## 14. 网易云音乐

主题文章中可以轻松插入 [网易云音乐](https://music.163.com/) 的指定音乐，你可以根据需要将音乐设置为自动播放，在主题目录 `layouts/shortcodes` 文件夹下的 `music.html` 对该标签进行定义。

**注意：由于版权问题，网易已经禁止外站分享版权音乐，该 shortcode 已经无法正常使用。**

## 15. Gist 代码片段

除了本地的代码片段，主题中可使用 [GitHub](https://github.com/) 的 [Gist](https://gist.github.com/) 服务轻松插入代码片段。

{{% gist "laozhu" "8285349" %}}

## 16. Tweet

由于不明原因可能无法访问。

## 17. YouTube

由于不明原因可能无法播放。

{{% youtube "w7Ft2ymGmfc" %}}

## 18. Instagram
![](https://raw.githubusercontent.com/yscoder/vscode-qiniu-upload-image/master/features/demo.gif)

由于不明原因可能无法访问。

## 文章更新

### [2017年9月8日](#inline-mathjax)

支持行内的数学公式，使用标记 `$` 包裹公式，如下：

When `\(a \ne 0\)`, there are two solutions to `$ax^2 + bx + c = 0$` and they are
<div>$$x = {-b \pm \sqrt{b^2-4ac} \over 2a}.$$</div>

## 参考资料

1. <p id="ref01">[Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)</p>
2. <p id="ref02">[Markdown 语法手册](https://www.zybuluo.com/EncyKe/note/120103)</p>
