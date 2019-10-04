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

In this very basic example, a web page has a large amount of white text 
on it and a colorful background, much like the desktop of a computer.
Clicking a button on the page changes the background to another picture.
Done as an afterthought, PE will be shown to require more time. Done
first, it will be evident it is the right way to do things.

### Steps required to create the page

#### Doing it right

1. Create an HTML document
2. Add the content (the text) as P elements
3. Add a submit "button" to a form whose action is empty (so it submits
to itself).
4. CSS style the text so it is white.
5. CSS style the background so it is a dark color so you can see the
white text.
6. Add the code to bring in a picture in the background.

#### Doing it wrong (but often how it is done)

1. Create an HTML document
2. Add the content (the text) as P elements
3. Add a button (without a form)
4. Add JS to handle the button click
5. Add JS to update the display when the button is clicked (since we're 
already in the JS)

## Display a Modal

Perhaps the most common use case for PE is the display of some bit of
content, like an enlarged photo, in a "modal" or "lightbox" display
without leaving the page you are currently on. The method for doing this
in a progressivley enhanced way is pretty straight forward:

1. Create the enlargement page as a separate, unique URL
2. Mark up the enlargement page as required using symmantic HTML and as
few elements as possible
3. On the "index" page, create an A element that points to the
enlargement
4. In a linked JS file, interrupt the default behavior for A elements
that point to the enlargement
5. On click, make an ajax call to the enlargement page and parse out the
HTML of the enlargement as required and display it on top of the current
page as a "modal"

Step 5 is more complex than it seems unless you use jQuery, in which
case it's about 4 lines of code. In these examples I'll try to do it
without jQuery, though, to provide dependency free examples.

## Main Menu at Top of Page

One school of thought in SEO-land is that the main content of the page
should come first in document order. Accessibility also benefist from
this approach as screen readers start right in reading the content
rather than reading all the menu items, ads or whatever, and _then_ the
content.

## Ajaxify a Form

This is probably the most common use case for progressive enhancement.
Submitting forms is often "costly" in terms of network resources. If the
response is an error ("try again") or success ("your information has 
been saved"), loading an entirely new page is overkill for such a small
amount of information.

However, in order to do this via HTTP, you need a form or some way to
send a POST request with a serialized payload but guess what, forms are
already a part of the standard selection of HTML elements. No need to 
reinvent the wheel. All we really need to do is interrupt the submit
event, send the request with the smallest of key/value pairs, and then
process the response, which can be as small as 1 or 0 given the right
circumstances. The enhanced form can then update the DOM accordingly, or
even, god forbid, redirect to a new page!

## Add a Print Page Button

This example comes from this description of [Progressive Enhancement vs.
Graceful Degredation](https://www.w3.org/community/webed/wiki/Graceful_degredation_versus_progressive_enhancement)

## Enhance Image Upload with Preview
