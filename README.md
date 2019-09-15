# Page Decomposition

[//]: # (## Authors:)
[//]: # ([Author 1])
[//]: # ([Author 2])
[//]: # ([etc.])

[//]: # (Table of Contents [if the explainer is longer than one printed page])
[//]: # ([You can generate a Table of Contents for markdown documents using a tool like doctoc.])

## Introduction
[//]: # ([The “executive summary” or “abstract”. Explain in a few sentences what the goals of the project are, and a brief overview of how the solution works. This should be no more than 1-2 paragraphs.])

With the advent of Virtual and Augmented reality, a new style of 3D interfaces is emerging. We are no longer limited to flat monitors; instead the user interface can be broken up in sections and positioned within your 3D space (for VR) or the real world (for AR).

Browser vendors are developing WebXR for WebGL rendered content and are experimenting with [basic 3D models](https://developers.google.com/web/updates/2019/02/model-viewer) and other [interactive experiences](https://creator.magicleap.com/learn/guides/prismatic-getting-started).
However, until now it was not possible to position CSS generated content in 3D.

## proposal

We want enable breaking off parts of web page into the 3-d space around the browser surface because this would make viewing web pages a much more immersive experience than looking at a rectangular surface would be and we want to do this by introducing the smallest change to CSS as possible

We think this can be accomplished by introducing a new value for `transform-style`: `detached`.
This new property builds upon the existing 3D transforms that are shipping in all browsers and extends the behavior of `transform-style: preserve-3d`. The main difference is that instead of flattening the transformed elements back to the page, the transformed element stays in 3D space.


Here is an example rendering of the effect:
![scene](https://github.com/rcabanier/detached_explainer/raw/master/detached.gif "Scene")

Because we don't want to render content anywhere in the user's space, we also propose to define a "safe" space around the browser.
Content can not be place outside this space.
![prism](https://github.com/rcabanier/detached_explainer/raw/master/prism.gif "Prism")


[//]: # (## Goals [or Motivating Use Cases, or Scenarios])
[//]: # ([What is the end-user need which this project aims to address?])

#
## Key scenarios
[//]: # ([If there are a suite of interacting APIs, show how they work together to solve the key scenarios described.])

### 1. Transform-style 'detached' behaves like 'preserve-3d' on platforms which do not support page decomposition.

We don't want authors to design different CSS for immersive vs flat browsers. If a user agent renders on a flat surface, it can use the 'preserve-3d' value

[//]: # ([Description of the end-user scenario])

[//]: # (// Sample code demonstrating how to use these APIs to address that scenario.)
### 2. When detached elements are nested, the ancestor is detached while the child is composited to detached surface with transform style flat.

Allowing detached descendants of detached nodes significantly complicates the implementation and introduces multiple corner cases.

[//]: # (TODO: We need to explan this, but I cant think of the best explanation)


### 3. Effects (transparency, clip) applied to parents of detached nodes are not applied to detached nodes as well.

The detached elements and their descendants are rendered onto surfaces that are distinct from their parents. Any effect applied to parents will be applied during the compositing stage and will not be applied to the detached elements.

To apply effects to detached elements, the effects would have to be applied on the detached nodes themselves.

### 4. Transforms and transform-styles applied to children of detached element will behave as if render surface of detached element is root render surface.

## Detailed design discussion

[//]: # ([Tricky design choice #1])
[//]: # ([Talk through the tradeoffs in coming to the specific design point you want to make.])

[//]: # (// Illustrated with example code.)
[//]: # ([This may be an open question, in which case you should link to any active discussion threads.])

[//]: # ([Tricky design choice 2])
[//]: # ([etc.])

## Considered alternatives

[//]: # ([This should include as many alternatives as you can, from high level architectural decisions down to alternative naming choices.])

[//]: # ([Alternative 1])

[//]: # ([Describe an alternative which was considered, and why you decided against it.])

[//]: # ([Alternative 2])
[//]: # ([etc.])

## Stakeholder Feedback / Opposition

[//]: # ([Implementors and other stakeholders may already have publicly stated positions on this work. If you can, list them here with links to evidence as appropriate.])

[//]: # ([Implementor A] : Positive)
[//]: # ([Stakeholder B] : No signals)
[//]: # ([Implementor C] : Negative)
[//]: # ([If appropriate, explain the reasons given by other implementors for their concerns.])

## References & acknowledgements

[//]: # ([Your design will change and be informed by many people; acknowledge them in an ongoing way! It helps build community and, as we only get by through the contributions of many, is only fair.])
[//]: # ([Unless you have a specific reason not to, these should be in alphabetical order.])

Many thanks for valuable feedback and advice from:

[//]: # ([Person 1])
[//]: # ([Person 2])
[//]: # ([etc.])