---
title: UI is a state machine
layout: post
permalink: /2017/05/ui-is-a-state-machine
---

[This article](http://blog.cognitect.com/blog/2017/5/22/restate-your-ui-using-state-machines-to-simplify-user-interface-development) does a good job in demonstrating the benefits of modeling a UI as a state machine. After showing how quickly the app state can turn into an unmanageable mess, this section formulates really well how a state machine can help:

> There is a better way. The overall goal -- one that is common to all user interfaces and independent of whatever library or framework you are employing -- is that as a user navigates a UI, we allow only the actions that make sense in light of the current application state. In other words, as the user does stuff the application should move from one state to another while the set of available actions should change based on the state.
>
> The key word here is state. In computer science, State Machines are abtract entities that have a finite number of states. Associated with each state is a set of possible transitions. Each transition allows the machine to move to a new state. Sound familair?
>
> Even better, the idea of a state machine fits well with the Clojure prime directive of focusing on the data -- to describe your problem declaratively -- in order to leverage Clojure's powers of data transformation.

This approach should work really well with the [re-frame](https://github.com/Day8/re-frame) CLJS library, combined with a state machine library like [stately](https://github.com/nodename/stately). I will continue to explore this.
