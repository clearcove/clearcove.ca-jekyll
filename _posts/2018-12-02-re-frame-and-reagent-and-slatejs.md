---
title: Re-frame and reagent and SlateJS
layout: post
permalink: /2018/12/re-frame-and-reagent-and-slatejs
---

I created an example app to demonstrate how to integrate the [SlateJS](https://www.slatejs.org/) editor into a [re-frame](https://github.com/Day8/re-frame) (and [reagent](https://github.com/reagent-project/reagent)) ClojureScript app.

It's an advanced use case with the following features:

* Multiple editable sections exist on the page. One can be edited at a time. Select a section to edit by clicking on its card.
* Shared toolbar, separate from the editor. Acts on the currently selected editor.
* Bold and italic formatting via toolbar or keyboard shortcuts.
* Persistence of edited text into re-frame app-db (as HTML).
* Used in production with 1K editable sections on a single page.

## Demo

You can view the [demo](https://jhund.github.io/re-frame-and-reagent-and-slatejs/index.html).

## Source code

View the [source code](https://github.com/jhund/re-frame-and-reagent-and-slatejs).
