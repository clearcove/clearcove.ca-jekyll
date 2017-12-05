---
title: Architecture decision records
layout: post
permalink: /2017/12/architecture-decision-records
---

Here is a self referential example of an architecture decision record:

# Record architecture decisions

Date: 2017-12-05

## Status

Accepted

## Context

When developing software, architectural decisions have to be made at different stages of the project. The motivation for these decisions is relevant to current and future developers and needs to be documented somewhere for the following reasons:

* To explain to future developers why things are done in a certain way.
* To help current developers remember why a given decision was made that way, e.g., when re-visiting a piece of code after some time because of a strange bug, or new requirement.
* To prevent current or future developers from making a mistake because they are not aware of or forgot the factors that led to a decision.

## Decision

We will use Architecture Decision Records, as described by Michael Nygard in this [article](http://thinkrelevance.com/blog/2011/11/15/documenting-architecture-decisions):

* We keep a collection of records for "architecturally significant" decisions (ADR): those that affect the structure, non-functional characteristics, dependencies, interfaces, or construction techniques.
* We keep ADRs in the project repository under `doc/architecture-decisions/YYYY-MM-DD-title-of-decision.md`
* We use Markdown for formatting.
* If a decision is reversed, we will keep the old one around, but mark it's status as superseded. (It's still relevant to know that it was the decision, but is no longer the decision.)
* We add the following sections for each ADR:
    * **Title**: A short phrase that describes what this decision is about. For example, "Ruby on Rails Version", or "re-frame for front end".
    * **Status**: A decision may be "proposed" if the project stakeholders haven't agreed with it yet, or "accepted" once it is agreed. If a later ADR changes or reverses a decision, it may be marked as "deprecated" or "superseded" with a reference to its replacement.
    * **Context**: What is the issue we're seeing that is motivating this decision or change? What are the forces at play? Including technological, political, social, and project. These forces are probably in tension, and should be called out as such. The language in this section is value-neutral. It is simply describing facts.
    * **Decision** What is the change that we're making? This section describes the decision we made in full sentences, with active voice. "We keep..."
    * **Options**: Which options did we take into consideration, what are the pros and cons for each one? Why did we choose the way we did?
    * **Consequences**: What are the positive and negative consequences of this decision? A particular decision may have positive, negative, and neutral consequences, but all of them affect the team and project in the future.

Examples and Resources

* http://thinkrelevance.com/blog/2011/11/15/documenting-architecture-decisions
* https://github.com/npryce/adr-tools/tree/master/doc/adr
* https://stackoverflow.com/questions/7104735/template-for-documenting-architecture-alternatives-and-decisions
* https://github.com/joelparkerhenderson/architecture_decision_record

## Options

* No documentation.
    * Pro: No extra work today.
    * Con: Extra work, or setbacks in 6 months because we forgot, or were never aware of the reasons for a given decision.
* Architecture Decision Records.
    * Pro: Helps us retain important knowledge about the software with minimal effort for writing and reading it.
* More complex documentation.
    * Pro: Helps us retain detailed important knowledge about the software.
    * Con: Significant effort to write and read, reducing the likelihood of it being written, or it being read.

## Consequences

It will be a bit more work to document the reasons for decisions, however we believe this to be a worthwhile investment as it will help us remember why we made certain decisions, when we question them in the future, and it will help us prevent mistakes because we forget or are never aware of our motivations.
