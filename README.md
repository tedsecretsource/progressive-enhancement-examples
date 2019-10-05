# Progressive Enhancement Patterns and Examples

## Introduction

I have been a strong proponent of employing [progressive
enhancement (PE)](https://en.wikipedia.org/wiki/Progressive_enhancement) best
practices ever since I was introduced to feature detection in JavaScript
in Danny Goodman's JavaScript Bible in the late 90s (although, back then
the term hadn't been coined quite yet).

These are the reasons why everyone should be employing PE best practices.

- By following a PE approach, costs will never be more than not 
following one.
- By following a PE approach, costs will go down in the long run
because the software is more resilient and requires less maintenance.
- If it takes more time, you're doing it in the wrong order (graceful
degredation, or just trying to patch a thoroughly broken system)

## Patterns

There are two main PE patterns:

- Intercept an event to reduce latency (lightbox, ajax form)
- Leverage CSS to reduce DOM nodes / render latency (font formatting, 
CSS Grid) and enhance visual comprehension (not just legibility)

## Anti-Patterns (smells)

These are just a couple of smells that I use to spot code that hasn't
followed PE best practices.

- The abscense of a fallback when JavaScript isn't available
- Inaccessible or missing content when JS or CSS isn't available

## PE in the real world

Every now and then I [turn off JS and 
CSS](https://chrome.google.com/webstore/detail/disable-html/lfhjgihpknekohffabeddfkmoiklonhm?hl=en) 
and surf the web just to see what the experience can be like and remind
myself of the importance of providing easily consumable content. The
following big players provide usable fallbacks for their sites. In most
cases it seems to be that they were aware of these principles when they
built their sites:

- twitter.com (my personal favorite as the experience is nearly 
identical)
- facebook.com
- cnn.com
- bbc.co.uk (most news sites in general)
- stackoverflow.com
- wired.com
- news.ycombinator.com (no surprise there)
- reddit
- github.com
- probably _all_ e-commerce web sites, especially amazon.com
- many, many more — you get the idea

These sites do not provide any kind of experience without JavaScript:

- instagram.com
- linkedin.com
- google.com/maps/ (although it does have a funny no JS message)
- bitbucket.org (major fail if you ask me)

While doing this research, I noticed that netflix.com sort of works
without JavaScript but the first step in signing up seems to be a
JavaScript link when it could just as well have been a button and a
form. Oops!

## PE in Practice - A practical view

Everyone knows login forms require a visit to the server. No one would
even consider using JavaScript to authenticate users due to the
likelihood of the site being hacked. Likewise, login forms often verify
field values before executing the authentication function: there must be
a username and a password. These fields cannot be blank. Processing 
forms with empty fields is a waste of time and energy and opens a door
for potential abuse.

It is also common practice to add JavaScript, or even HTML attributes on
the client to make sure the form isn't submitted without some value in
the username and password fields. This is done for two reasons: 1) it is
a waste of server resources to process a login form with no data and,
more importantly, 2) the user experience is so much better when this
kind of validation happens on the client.

This is an industry standard of PE because it "enhances" the end user
experience with no loss of critical functionality should the JavaScript
fail to work. Note that the first step in implementing this
functionality is always going to be getting the basic form working (and
testing it requires testing _without_ JavaScript).

But what about a form to add a picture to your social network feed? What
would it look like to do this in a PE way? Before we answer, let's look
at what it would look like to do it in a non-PE way…

Assuming we're using React, we would probably create a component
consisting of a FORM element and the inputs necessary to allow giving
the image a title and description and a FILE input for uploading the 
actual file. We might then have to assign a className to one or more of
the elements to tell React how to display them and assign a handler for
the form submission. At this point we'd have to write the form handler
which would include validating the input for obvious reasons and then
some server-side code to handle the upload and produce a response that
React will know how to handle (the response handler).

So, to summarize, the steps would be:

1. Write a form in HTML
2. Style the form with CSS
3. Write a handler in JavaScript that sends the form to the server
4. Write another handler in JavaScript that processes the server response
5. Update the page display depending on the response (normally, a call
to a page or another component)

If you were to do this same thing in a non-JavaScript way, the steps
would be:

1. Write a form in HTML
2. Style the form with CSS
3. _(nothing - hitting Enter suffices)_
4. Process the form on the server and decide what response to send
5. _(nothing - the server sends the whole page)_

Five steps vs. three steps.

Let that sink in for a moment.

**More work to end up with code that is more fragile!**

## But, but… React is Just so Cool!

And I agree, actually, and hope is not lost, at all. If you're willing
to put in the extra work to do it in React, why not just do it in a PE
manner for the same amount of effort? We have two steps we can use to 
progressively enhance the experience. Let's start with step 3:

Just like in the non-PE version, we're going to write a handler that
sends the request and handles the response (normally, show the result
of the operation).

```js
let theForm = document.querySelector('#theform');
theForm.addEventHandler('submit', function(event) {
	event.preventDefault();
	// serialize the data, maybe
	// handle file uploads
	// add a "processing" spinner to let the user know something is
	// happening
	// submit the form and Bob's your uncle, mostly
});
```

Before going any further, please note that *the response from the server
has already been written* and can be used _as is_ with **no** additional
_server-side_ work if we so choose. Just by handling the form submission
via ajax we are already significantly cutting down on the request time
and thus, the entire request/response time. Modifying the response on
the server will also decrease it significantly but it is OPTIONAL, so we
won't do it _for now_.

But we still need to handle the response. We still need to determine if
the form was accepted by the HTTP server and what the application 
response was (either the image was added or failure, the image was not 
added).

### Handling the Response - Our Step 5

`successPromise()` is a function that is executed whenever the server
responds with anything other than an error, typically when the response
is 200, 201, or maybe even 302. It does not mean the image was added to
the profile (this is not the application response, normally, but the
HTTP server response code). We still need to inspect the body of the 
response to determine if the image was added or not. Normally, the body
will contain some kind of success message that you know, because you
wrote it.

If the application response is not success, like, maybe the user only 
entered periods in the image title and our client-side validation did
not take that possibility into account, then we need to redisplay the
form with the submitted data and an error message. Since we have all of
this data (both in the request that we sent and the response we 
received), then we just update the display accordingly.

If the application response _is_ success, then, in a case such as this,
we can either window.location.href redirect the user to the new image
details page or notify the user that the image was added, clear the form
and let them add another, or something similar. The possibilities are
endless but the point I'm trying to make is that how you choose to 
hanlde the response does not mean more work just because you are
following PE principles.

`failurePromise()` is a function that runs when the server responds with
HTTP codes other than 200, 201, and maybe 302. In this instance, we 
notify the user that there was an unknown error and that they should 
give up.

Just kidding… but you get the idea.

## Summing Up

At this piont you've seen how you can progressively enhance an existing
form and how it is not more work than just using React (for example).
But here's where PE shines… What if you have a typo in your JavaScript
that avoids detection in testing (especially since it is hard to test
client-side code) and what if that typo causes a JavaScript error or
exception and the whole runtime engine comes to a grinding halt? If
you've followed PE, __nothing bad happens__ because the form will be 
submitted like always. In fact, if you are very clever about how you set
it up, users might not even notice there *was* an error.

Now, I know what many of you are thinking… React won't compile if there
is a syntax error due to a typo so this will never happen, and you're
right. React will not let you deploy code with syntax errors. But let's
say it wasn't a syntax error, but simply a network failure, or a
misconfigured server failure, or the user went into a tunnel on the 
train and the request timed out interrupting the response for the
JavaScript. What then?

## Coda

What follows are links to repositories that demonstrate different types
of progressive enhancement. The goal is to give you patterns you can
follow in your own day-to-day work (and play).

To me, the current React/Vue/Vanilla, etc. landscape is beginning to
smell a lot like the era of Flash at the end of the last century or the
animated gif of the period before it. When all you have is a hammer,
everything looks like a nail. Hopefully, after reading this article, and
examing the sample repositories, you now also have a sturdy table on
which to pound that hammer, and pound you shall!

The examples you'll find here include:

- [Legible Text](#legible-text)
- [Display a modal](#display-a-modal)
- [Display the main menu at the top of the screen instead of the bottom](#main-menu-at-top-of-page)
- [Using JavaScript to reduce network size when submitting a form](#ajaxify-a-form)
- [Add a Print Page button to a receipt or reservation confirmation](#add-a-print-page-button)
- [Using JS to provide an image preview prior to uploading](#enhance-image-upload-with-preview)

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
