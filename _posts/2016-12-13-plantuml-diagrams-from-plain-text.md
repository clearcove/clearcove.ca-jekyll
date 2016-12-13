---
title: Use PlantUML to create diagrams from plain text
layout: post
permalink: /2016/12/plantuml-diagrams-from-plain-text
---

[PlantUML](http://plantuml.com/) is a tool that allows you to create all kinds of UML diagrams from plain text. I am particularly interested in StateCharts.

Below is an example statechart diagram and the related source code:

![PlantUML example state chart](/images/2016/PlantUML-statechart-diagram-exampl.png "PlantUML example state chart"){: .img-polaroid}

```
@startuml
title Simple Orthogonal Composite State Model
[*] --> NeilDiamond
state NeilDiamond 

state "Neil Diamond Onstage" as NeilDiamond {
  state Dancing
  state Singing
  state Smiling
  Dancing --> Singing
  Singing --> Smiling
  Smiling --> Dancing
}

state NDoff
state "Neil Diamond in Dressing Room" as NDoff {
  state ThinkingAboutAmerica
  state WatchingGlee
  ThinkingAboutAmerica --> WatchingGlee
  WatchingGlee --> ThinkingAboutAmerica
}

NeilDiamond -Right-> NDoff : Walking
NDoff -Left-> NeilDiamond :Running

@enduml
```

You can try PlantUML online at [PlantText](https://www.planttext.com/).
