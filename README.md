# Detached Transform

[//]: # (## Authors:)
[//]: # ([Author 1])
[//]: # ([Author 2])
[//]: # ([etc.])

[//]: # (Table of Contents [if the explainer is longer than one printed page])
[//]: # ([You can generate a Table of Contents for markdown documents using a tool like doctoc.])

## Introduction
[//]: # ([The “executive summary” or “abstract”. Explain in a few sentences what the goals of the project are, and a brief overview of how the solution works. This should be no more than 1-2 paragraphs.])

With the advent of virtual and augmented reality, a new style of 3D interfaces is emerging.
We are no longer limited to flat screens; instead the user interface can be broken up in sections and positioned within the user's 3D space (for VR) or the real world (for AR).

Browser vendors are developing WebXR for WebGL rendered content and are experimenting with [basic 3D models](https://developers.google.com/web/updates/2019/02/model-viewer) and other [interactive experiences](https://creator.magicleap.com/learn/guides/prismatic-getting-started).
However, until now, positioning CSS generated content in 3D was not possible.

## Proposal

Our goal is to break off parts of web page into the 3D space around the browser surface.

This would make viewing web pages a much more immersive experience rather than looking at a rectangular surface.
We hope to accomplish this by introducing a small addition to CSS.

We propose to introduce a new value for `transform: detached`.

This new property builds upon the existing 3D transforms that are shipping in all browsers and extends the behavior of `transform` attribute.
The main difference is that instead of flattening the transformed elements back to the page, the transformed element stays in 3D space.
If `transform: detached` is specified on an element, the element will be rendered to a separate surface and positioned using the cumulative 3d transform applied to the element.

Example:

    <!DOCTYPE html>
    <html class="parent">
    <head>
        <style>
            .parent  { transform-style: preserve-3d; }
            .element { transform: detached rotateY(30deg) translateZ(25px); }
        </style>
    </head>
    <body class="parent">
        <div class="parent">
            <div class="element"> This is text on a detached surface </div>
        </div>
    </body>
    </html>

Here is an example rendering of the effect:
![scene](https://github.com/rcabanier/detached_explainer/raw/master/detached.gif "Scene")

As the page or a parent element scrolls, the detached element should scroll.
CSS animations and transitions will apply as well.

In order to disallow unrestricted access of user's space to the content developer, we also propose to define a "safe space" that shall be requested to the user by content developer and user consented.
Content can not be placed outside this space.
![prism](https://github.com/rcabanier/detached_explainer/raw/master/prism.gif "Prism")

[//]: # (## Goals [or Motivating Use Cases, or Scenarios])
[//]: # ([What is the end-user need which this project aims to address?])

#
## Key scenarios
[//]: # ([If there are a suite of interacting APIs, show how they work together to solve the key scenarios described.])

### 1. Transform `detached` is ignored on platforms which do not support detaching elements.

We don't want authors to design different CSS for immersive vs flat browsers.
If a user agent renders on a flat surface, transform `detached` is ignored and the element is rendered into the main surface

Thus the rendered page would appear visually similar on all platforms.

[//]: # ([Description of the end-user scenario])

[//]: # (// Sample code demonstrating how to use these APIs to address that scenario.)

### 2. Nodes with transform-style: `flat` prevent descendents from being detached.

Transform style `flat` and effects that cause flattening ([spec](https://drafts.csswg.org/css-transforms-2/#grouping-property-values)) will cause `transform: detached` to be ignored in descendant elements.

An implicit `flat` transform-style is applied to `<html>` tag. Implementing detached transform requires being able change `transform-style` of `<html>` tag to `preserve-3d` (as in above example).

### 3. Only the top-most frame can use this feature.

Contents of `<iframe>` elements should not be allowed to created detached content.
This feature will only be available to the main frame.

Within `<iframe>` elements, `detached` transform will be ignored.

### 4. Detached surfaces will not participate in stacking contexts.

This is a corollary as transform-stye: `preserve-3d` does not create a stacking context and for an element to be detached all its parents (including document root) should have transform-style: `preserve-3d`.

#
## Rejected alternate proposals
[//]: # ([This should include as many alternatives as you can, from high level architectural decisions down to alternative naming choices.])

### 1. Extending transform-style to include detached keyword.

Specifying transform-style: detached on an element would cause its children to be detached. For instance:

    <!DOCTYPE html>
    <html>
    <head>
      <style>
        .scene {
            transform-style: detached;
        }
        .element {
            transform: rotateY(30deg) translateZ(25px); }
        }
      </style>
    </head>
    <body>
        <div id="scene">
            <div id="element">This is detached text</div>
        </div>
    </body>
    </html>

In this alternative, on user agents that do not support detached elements, transform-style: detached would default to transform-style: `preserve-3d` to preserve visual similarity.

This introduces a ambiguity on how any children and grand-children with transform-style: `preserve-3d` will be treated.

#
## Feedback / Opposition
[//]: # ([Implementors and other stakeholders may already have publicly stated positions on this work. If you can, list them here with links to evidence as appropriate.])

### 1. Feedback from [csswg](https://github.com/w3c/csswg-drafts/issues/4242)

Concern was raised that a similar behavior could be achieved without introducing new transform or transform-style keywords.

In the alternative proposed by David Baron (with added details from Chris Harrelson), all surfaces that do not have a flattening parent will be detached. `<html>` tag would have to use transform-style: `preserve-3d` to allow detaching elements.

Chris Harrelson further proposed that surfaces which do not have detached parents and do not have transforms that would cause it to 'pop out' of the main surface can be left un-detached.

[//]: # ([Stakeholder B] : No signals)
[//]: # ([Implementor C] : Negative)
[//]: # ([If appropriate, explain the reasons given by other implementors for their concerns.])

## References & acknowledgements
[//]: # ([Your design will change and be informed by many people; acknowledge them in an ongoing way! It helps build community and, as we only get by through the contributions of many, is only fair.])
[//]: # ([Unless you have a specific reason not to, these should be in alphabetical order.])

[//]: # (Many thanks for valuable feedback and advice from:)

Josh Carpenter : Illustrations from [slide deck](https://docs.google.com/presentation/d/1ORdKs1wNe7QysRYSBtmW8LnMTFRu69gEwyOSrjIaZyA)