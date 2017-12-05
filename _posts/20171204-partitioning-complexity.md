---
title: Partitioning complexity
layout: post
permalink: /2017/12/partitioning-complexity
---

Kent Beck has a great article about "[One Bite At A Time: Partitioning Complexity](https://www.facebook.com/notes/kent-beck/one-bite-at-a-time-partitioning-complexity/1716882961677894/)".

Here is a summary of the main points:

### One problem at a time

* Make it **work**, make it **right**, make it **fast**.
* Work on **structure** XOR **behavior**.
* Work on **interface** XOR **implementation**.
* Work on **test** XOR **code**.

### Code structure

* **Composed methods** provide an outline of what's happening.
* Mutable core/immutable leaves - not sure how this applies to working with ClojureScript where data is immutable by default.
* **Move logic to data** so that operations and data are located close together.
* **One pile** for the messy stuff, rather than having it scattered everywhere.

### Workflow

* Start with a **constant** for complex input data, then **compute** it in a second step.
* **Predict test results** and use surprise as a pointer that your understanding is lacking, and you need to re-think the problem.
* **Red/red/revert**. Go back to the drawing board if the first attempt at fixing it didn't work.
* Red/red/red/green/revert/red/green/red/green retrace the steps taken to tackle a complex problem for maximum learning.
* **Wait** let ugly code get more ugly before trying to fix it.
* Stories to separate thoughts of outcome from those of implementation.

A good quote from the end:

> Many of the strategies above sacrifice short-term efficiency in order to keep my feet moving. What really slows me down is not programming slowly, it is getting overwhelmed, losing my confidence, and not programming at all.
