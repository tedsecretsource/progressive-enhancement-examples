# Progressive Enhancement Examples

This repo contains a series of examples of 
[progressive enhancment](https://en.wikipedia.org/wiki/Progressive_enhancement). The 
reason behind this repo is multiple people I've met recently, including
some of our developers, struggle to understand what PE is. Graceful
degredation makes sense to them but not PE.

The examples you'll find here include:

- [Display a modal](#display-a-modal)
- [Display the main menu at the top of the screen instead of the bottom](#main-menu-at-top-of-page)
- [Using JavaScript to reduce network size when submitting a form](#ajaxify-a-form)
- [Add a Print Page button to a receipt or reservation confirmation](#add-a-print-page-button)
- [Using JS to provide an image preview prior to uploading](#enhance-image-upload-with-preview)

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
