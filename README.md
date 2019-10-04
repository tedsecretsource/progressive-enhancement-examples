# Progressive Enhancement Examples

This repo contains a series of examples of 
[progressive enhancment](https://en.wikipedia.org/wiki/Progressive_enhancement). The 
reason behind this repo is multiple people I've met recently, including
some of our developers, struggle to understand what PE is. Graceful
degredation makes sense to them but not PE.

The examples you'll find here include:

- [Legible Text](#legible-text)
- [Display a modal](#display-a-modal)
- [Display the main menu at the top of the screen instead of the bottom](#main-menu-at-top-of-page)
- [Using JavaScript to reduce network size when submitting a form](#ajaxify-a-form)
- [Add a Print Page button to a receipt or reservation confirmation](#add-a-print-page-button)
- [Using JS to provide an image preview prior to uploading](#enhance-image-upload-with-preview)

## But What is It?

For me, PE starts with the most basic presentation imaginable. Developers
then "enhance" the presentation such that should an enhancment fail, for
whatever reason, the basic presentaiton is still available and the reason
the user came to the site is not invalidated.

## But PE is More Expensive

*Only if you do it as an afterthought*, in most cases. Think
about it: even in today's "modern" world, the majority of what we're
doing with React is presenting data, sometimes with a UI that makes
consumption of that data easier. And how does React do this? It _tricks_
the browser into thinking it is looking at an H1, or a P, or a FOOTER
element. In other words, in memory, it recreates the very HTML that
many believe we can dispose of. Why not just display the damn HTML?!

This first example demonstrates a right way, and a wrong way, to follow
the principles of PE.

## Legible Text

In this example, a web page has a large amount of white text on it and
a colorful background, much like the desktop of a computer. Done as an
afterthought, PE will be shown to require more time. Done first, it will
be evident it is the right way to do things.

## Display a Modal

Perhaps the most common use case for PE is the display of some bit of content, 
like an enlarged photo, in a "modal" or "lightbox" display without leaving
the page you are currently on. The method for doing this in a progressivley
enhanced way is pretty straight forward:

1. Create the enlargement page as a separate URL
2. Mark up the enlargement page as required using symmantic HTML and as 
few elements as possible
3. On the "index" page, create an A element that points to the enlargement
4. In a linked JS file, interrupt the default behavior for A elements
that point to the enlargement
5. On click, make an ajax call to the enlargement page and parse out the
HTML of the enlargement as required and display it on top of the current
page as a "modal"

Step 5 is more complex than it seems unless you use jQuery, in which case
it's about 4 lines of code. In these examples I'll try to do it without
jQuery, though, to provide dependency free examples.

## Main Menu at Top of Page

## Ajaxify a Form

## Add a Print Page Button

This example comes from this description of [Progressive Enhancement vs.
Graceful Degredation](https://www.w3.org/community/webed/wiki/Graceful_degredation_versus_progressive_enhancement)

## Enhance Image Upload with Preview
