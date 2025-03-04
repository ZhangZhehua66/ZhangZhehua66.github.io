---
layout: post
title: Jupyter在al-folio的使用
date: 2025-3-3 02:15:13
description: 想要JYY网站里那种，可以通过键盘快捷键翻页的效果
tags: 编码
categories: 计算机
tabs: true
giscus_comments: true

---

要在文章中嵌入 Jupyter Notebook，你可以使用以下代码：

{% raw %}

```liquid
{::nomarkdown}
{% assign jupyter_path = 'assets/jupyter/blog.ipynb' | relative_url %}
{% capture notebook_exists %}{% file_exists assets/jupyter/blog.ipynb %}{% endcapture %}
{% if notebook_exists == 'true' %}
  {% jupyter_notebook jupyter_path %}
{% else %}
  <p>抱歉，您查找的 Notebook 不存在。</p>
{% endif %}
{:/nomarkdown}
```

{% endraw %}

让我们来解析一下：这得益于 [Jekyll Jupyter Notebook 插件](https://github.com/red-data-tools/jekyll-jupyter-notebook)，它允许你在文章中嵌入 Jupyter Notebook。该插件本质上调用 [`jupyter nbconvert --to html`](https://nbconvert.readthedocs.io/en/latest/usage.html#convert-html) 将 Notebook 转换为 HTML 页面，然后嵌入到文章中。由于 [Kramdown](https://jekyllrb.com/docs/configuration/markdown/) 是 Jekyll 的默认 Markdown 渲染器，我们需要使用 [`::nomarkdown`](https://kramdown.gettalong.org/syntax.html#extensions) 标签包裹插件调用，以防止 Kramdown 处理该部分内容，并确保其按原样输出。

该插件的输入是 Notebook 的路径，但它默认假设该文件存在。如果你希望在调用插件之前检查文件是否存在，可以使用 `file_exists` 过滤器。这可以避免插件返回 404 错误，并导致主页面被错误地嵌入其中。如果文件不存在，你可以向用户输出一条消息。上面的代码将会输出如下内容：

{::nomarkdown}
{% assign jupyter_path = "assets/jupyter/blog.ipynb" | relative_url %}
{% capture notebook_exists %}{% file_exists assets/jupyter/blog.ipynb %}{% endcapture %}
{% if notebook_exists == "true" %}
{% jupyter_notebook jupyter_path %}
{% else %}

<p>抱歉，您查找的 Notebook 不存在。</p>

{% endif %}
{:/nomarkdown}

请注意，Jupyter Notebook 支持浅色和深色主题。
