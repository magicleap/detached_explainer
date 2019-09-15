# Page Decomposition

[//]: # (## Authors:)
[//]: # ([Author 1])
[//]: # ([Author 2])
[//]: # ([etc.])

[//]: # (Table of Contents [if the explainer is longer than one printed page])
[//]: # ([You can generate a Table of Contents for markdown documents using a tool like doctoc.])

## Introduction
[//]: # ([The “executive summary” or “abstract”. Explain in a few sentences what the goals of the project are, and a brief overview of how the solution works. This should be no more than 1-2 paragraphs.])

With the advent of Virtual and Augmented reality, a new style of 3D interfaces is emerging. We are no longer limited to flat monitors; instead the user interface can be positioned within your 3D space (for VR) or the real world (for AR).

Browser vendors are developing WebXR for WebGL rendered content and are experimenting with [basic 3D models](https://developers.google.com/web/updates/2019/02/model-viewer) and other [interactive experiences](https://creator.magicleap.com/learn/guides/prismatic-getting-started). 

## proposal

We want to add a new feature to CSS: Page decomposition.
This feature would enable breaking off elements of web page into the 3-d space around the browser surface. This would make viewing web pages a much more immersive experience than looking at a rectangular surface would be. 
Here is an example rendering of the effect:
![scene](https://github.com/rcabanier/detached_explainer/raw/master/src/detached.gif "Scene")

Because we don't want to render content anywhere in the user's space, we also propose to define a "safe" space around the browser.
Content can not be place outside this space.
![prism](https://github.com/rcabanier/detached_explainer/raw/master/src/prism.gif "Prism")


[//]: # (## Goals [or Motivating Use Cases, or Scenarios])
[//]: # ([What is the end-user need which this project aims to address?])

#
## Key scenarios
[//]: # ([If there are a suite of interacting APIs, show how they work together to solve the key scenarios described.])

### 1. Transform-style 'detached' behaves like 'flat' on platforms which do not support page decomposition.

Detaching a surface involves flattening the node and its children to a render context, compositing to a separate render surface and applying necessary transforms.
This pattern closely resembles transform style flat, which also creates a render context, renders first to a temporary texture and is then composited to root render surface with necessary transforms.

We do not default to preserve-3d as its behavior is significantly different from that of detached. Render contexts are not created for nodes with preserve-3d and their children are composited onto the closest flattened parent with aggregated transforms.

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