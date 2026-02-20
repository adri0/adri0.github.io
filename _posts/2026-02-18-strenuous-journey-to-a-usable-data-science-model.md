---
layout: post
title: The strenuous journey from a useful to a usable data science model
categories: 
    - code
last_modified_at: 2026-02-20T11:45:00+01:00
excerpt_separator: "<!--more-->"
---

> This article is co-authored by yours truly and [Haseeb Tariq](https://mhaseebtariq.com), an outstanding engineer and dear colleague of trade. It has also been [published on his blog](https://medium.com/@mhaseebtariq/the-strenuous-journey-from-a-useful-to-a-usable-data-science-model-ecc5064f613e), which you should also check out.

Python being one of the most accessible programming languages — when it was taking off in around 2010, gave a lot of beginners a _false_ sense of competence. Today _vibe coding_ is doing the same, just at a whole new level. This type of widespread and accelerated adoption comes at a cost. Prototyping an idea becomes as easy as a walk in the park, while translating the same idea into a production-ready software becomes ever so challenging. The challenges are further exacerbated in the areas of data science and ML (Machine Learning), where the vast majority of practitioners do not have a background in software engineering or (in some cases) _even_ computer science.

<!--more-->

Unlike regular software, the complexity in ML-powered systems tend to accumulate much faster due to their statistical nature and the blurred boundaries between data and code. After working on countless production-grade systems in a variety of industries, we have come up with a list of the most common bad practices, anti-patterns, and flawed designs which could hinder the success of any _high-potential_ data science model.

<figure>
    <img src="/assets/images/silos.webp" alt="How data science teams are organized can have far-reaching consequences on the success of a project." width="500" />
    <figcaption>How data science teams are organized can have far-reaching consequences on the success of a project.</figcaption>
</figure>

## Separation of usefulness and usability efforts

Despite being common knowledge now that it is a bad organizational setup, this is still an ubiquitous practice. A team of data scientists work tirelessly on making a data science model _functionally_ **useful**, while a separate team of ML or MLOps (Machine Learning Operations) engineers later try to turn the same model _operationally_ **usable**. From our experience, broadly speaking, there are 3 different ways data science efforts can be organized:

1. [**Ideal**] One coherent team of data scientists and ML engineers with a unified set of goals and timelines, protecting both usefulness and usability metrics
2. [Less ideal] A well documented model design is handed over to the engineering team, who then translate it into a production-grade solution
3. [Least ideal] Model implementation is handed over to the engineering team, as code, with the purpose of deploying it in its existing form

Option 2 actually has a positive side effect where, during the translation phase, some of the bugs or inconsistencies can be rectified. With the least ideal structure, the downsides are numerous:

1. During the experimentation/prototyping phase, aspects of operational usability might be de-prioritized at best or ignored altogether at worst
2. During the deployment phase, because of the lack of contextual information about the business problem, the functional usefulness of the model might be compromised
3. While sharing knowledge, translating requirements, and handing over code — the cognitive load is exacerbated for both teams
4. Finally, this separation also creates unnecessary friction in the teams — while both teams are chasing their own independent timelines and distinct goals


## The model thinking trap

An _accurate_ model is sort of an oxymoron. A model is a “representation” (or simplification) of a physical object; a process; a system; or a phenomenon. A representation is therefore, by definition, not accurate. Hence the aphorism,

> All models are wrong, but some are useful.


<figure>
<img src="/assets/images/climb.webp" alt="Model thinking can be an obstacle when striving for making a model usable." width="500">
<figcaption>Model thinking can be an obstacle when striving for making a model usable.</figcaption>
</figure>

Model thinking is a data scientist’s super power to navigate complex real-world problems. Finding a delicate balance between usefulness and accuracy; bias and variance; generalization and customization; or overfitting and underfitting can only be achieved by having a model thinking _mindset_. Although, when there is a separation of usefulness and usability efforts, this _exclusive_ mindset leads to two counter-productive and contradicting phenomena.

### 1. Over-abstraction

How many times have you come across a piece of code with complex hierarchies of classes and abstractions, which could be refactored into a sequence of simple function calls? This premature or excessive creation of generic code structures leads to overly complex, hard-to-maintain, or unnecessary software. The intended goal is to _model_ the software in a way that is future-proof, a future that might never come, inadvertently increasing the cognitive load of maintaining the project for all the existing and _future_ developers.

### 2. Cutting corners

On the other hand, the model thinking mindset also leads to a tendency of cutting corners. If you really think about it, every data science model is optimizing on time. Any problem can be solved _without_ a model, when you have unlimited time to solve it. Therefore, when you are optimizing for time, every poor design decision; untested/undocumented code; or suboptimal algorithm etc. is tomorrow’s problem.

<figure>
<img src="/assets/images/jenga.webp" alt="Pushing a small code change should not feel like removing a block from an unstable Jenga tower." width="500">
<figcaption>Pushing a small code change should not feel like removing a block from an unstable Jenga tower.</figcaption>
</figure>


## The best practices trap

Data scientists love to bring up the one-model-fits-all fallacy, while at the same time love chasing best practices from the industry leaders. Although, to be fair, engineers fall into this trap as much (or even more in some cases). Adopting best practices may sound like a good idea, however, in software, many of the so-called best practices rarely fit every situation. What was considered best a week ago, might not be in today’s state of affairs. Practices evolve out of a particular context and conditions which could change over time. The trap lies in reproducing practices without really understanding their motivations and intentions, leading to [_cargo cult software engineering_](https://en.wikipedia.org/wiki/Cargo_cult_programming) and unnecessary complexity. Instead, in order to be useful, practices must come from an explicit intent (e.g. minimize mental models, standardize components, encapsulate complexity, etc.); and be adaptable to a changing reality.

Nick Hawn from Atomic Spin [proposes](https://spin.atomicobject.com/best-practices/):

> The term “leading practices” was something I was introduced to a few years ago and aligns more closely with the inherent change and fast-paced software space. Unlike a “best practice,” which suggests we have reached the pinnacle of achievement, “leading practices” better help us embrace continual learning and evolution of our field.


<figure>
<img src="/assets/images/yamls.webp" alt="Following best or common practices without proper considerations can prove counter-productive." width="500">
<figcaption>Following best or common practices without proper considerations can prove counter-productive.</figcaption>
</figure>

To give an example, using yaml files for anything _remotely_ related to configuration management has become a standard in ML projects. Although, in our opinion, the actual use cases for yaml files are rare. When weighing a technology choice, a crucial exercise is to list all the cons. In the case of yaml files when used in a Python codebase (for instance), here are some of the potential cons:

1. Unnecessary (de)serialization
2. The overly flexible format of yaml can also mean [more ways to make mistakes](https://ruudvanasseldonk.com/2023/01/11/the-yaml-document-from-hell)
3. The format does not provide additional benefits when you don’t have complex hierarchies and nested structures in your configurations
4. No huge benefits when programmers are maintaining the configurations _exclusively_
5. Navigation and refactoring is simpler with plain Python

## Turning a blind eye on (accumulating) technical debt

The set of practices that constitute the discipline of MLOps evolved to address problems that specifically arise from maintaining ML systems. Adopting MLOps standards is normally a good default, although understanding the motivation behind them is key for navigating a project through uncertainty; while avoiding finding oneself in unmanageable situations.

<figure>
<img src="/assets/images/creditcards.webp" alt="Ward Cunningham coined the term technical debt in 1992 to describe the long-term consequences of prioritizing quick software delivery over ideal, more time-consuming code design." width="500">
<figcaption>Ward Cunningham coined the term technical debt in 1992 to describe the long-term consequences of prioritizing quick software delivery over ideal, more time-consuming code design.</figcaption>
</figure>

Back in 2014 a group of Google engineers published, [_Machine Learning: The High Interest Credit Card of Technical Debt_](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/43146.pdf), where they analyze the unsuspected challenges of developing and maintaining ML systems using the framework of _technical debt_. Although ML has come a long way since its publishing, the paper still reads incredibly fresh. Below are some of the issues highlighted in the paper:

***Changing anything changes everything*** — This is the effect that a change in any input (or rather its distribution) can cause unpredictable changes in the behavior of all other inputs. Reminding that in an ML system every change requires end-to-end reevaluation.

***Hidden feedback loops*** — This happens when, inadvertently (or deliberately), consequences of the model output cause a change in the distribution of its input data, accelerating concept drift. Simply retraining a model that contains hidden feedback loops typically won’t hold its performance for long.

***The cost of data dependencies*** — A model is not only made of a static picture of its algorithm structure and its weights, but also its training data. Just like any system, a model must be able to evolve. Excessive or unmanageable dependencies, be code or data, add inertia to its evolutive process. Data signals must also be traceable, available, and versioned to be properly tamed — concepts like [slowly changing dimensions](https://en.wikipedia.org/wiki/Slowly_changing_dimension), and tools like [data version control](https://dvc.org/) exist to address this. Developers avoid adding _low_ added-value libraries to a code base. In the same way, one should rethink whether maintaining features that have infinitesimal impact to the model output is worth the effort. In other words, feature selection is not only about model performance.

***Glue code and pipeline jungles*** — In an ML system, the majority of the effort is not spent on implementing ML algorithms or calling ML packages. Instead, it is on defining data transformation pipelines; making the data ready to be input into ML algorithms; CI/CD jobs; compatibility layers; and glueing different systems and data sources together. This is commonly referred to as glue code. While it _ideally_ must be minimized, it cannot be completely avoided. Out of control glue code tends to spell doom of an ML project.

***Configuration debt*** — ML projects tend to accumulate configuration due to the volume of parameters and settings that are subject to change as a model evolves. While separating parameters from code comes from good intentions (organization and clarity), it is very easy to overuse it. Too much or overly complex configurations can end up doing more harm than good.

***Experimentation paths*** — Experimentation plays a big role in making successful ML models. An experiment (be it a feature or an analytical process) must be very _clearly_ distinguishable from production. You don’t want, for example, to be in a situation where there’s confusion about whether a trained model artifact (or a set of metrics, or some given feature) is experimental or production-ready.

***Changes in the external world*** — Data distributions shift, and assumptions written inside the model stop being valid (think of fixed thresholds, for example). The system supporting the model must be able to recognize such changes when the model is not able to adapt by itself. That’s where sane monitoring comes into place.

---

Building (and maintaining) ML systems is a challenging undertaking, requiring expertise from a variety of different disciplines. The people management aspect is in itself an even bigger part of the puzzle. Therefore, summarizing all the lessons, challenges, recommendations, and solutions in this single blog post is an impossible task. Hopefully it can still provide the practitioners with a set of guidelines to avoid the most detrimental pitfalls.
