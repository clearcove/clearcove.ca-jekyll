---
title: Editable inline diagrams in markdown
layout: post
permalink: /2020/10/editable-inline-diagrams-in-markdown
---

We use Markdown for our development documentation. One sticking point has long been the ability to conveniently insert diagrams and show them inline while editing the markdown file.

Here is the solution:

1. Download the [draw.io](https://www.diagrams.net/) desktop app for offline usage.
2. Create a diagram in draw.io, store it as **editable PNG**, yes, that's right. The diagram source is embedded in the PNG file.
3. Write your markdown document, include an image like so: `![alt text](./images/imageFileName.png)`.
4. If you are using SublimeText, install the [Markdown Images](https://packagecontrol.io/packages/Markdown%20Images) package. Now you can see your editable PNGs right inside your markdown document in SublimeText.

## Benefits

* draw.io is a powerful and open-source drawing tool.
* It can export to **editable!** SVG and PNG images. So the rendered output is also the source diagram. If you save your diagrams along with your source documentation as editable PNGs, you can display them inline in your text editor. The diagrams are stored along with the markdown documents under version control.
* You can update your PNG diagrams at a later time using draw.io. This is a big deal: Your rendered output (PNG) is also your editable source file.
