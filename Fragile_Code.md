# Definition of Fragile

The following is a list of points of failure that can negatively affect
the form's behavior:

- Networking errors: for any reason, the network disappears and the form
is not sent, not received, or some mess inbetween, or an external
library fails to load, or doesn't load in time (see JavaScript errors
below) 
- HTML or CSS browser incompatibility: the user is using an older
version of X browser and it doesn't understand the elements or selectors
you're using in your HTML / CSS 
- JavaScript errors: unless you add a
window.onerror handler to every page that handles exceptions and errors
in an elegant manner (doesn't interrupt the interpreter), certain parts
of the page will stop working

## But, but… These Problems are Non-starters

### Little Can be Done with Networking Errors

True, little can be done. However, _something can_ be done so we might
as well do it. I can recall pre-loading images by setting their height
and width to 0px so they'd be cached by the browser for the next page
load. Fun times, but the lesson is clear: things can be done that will
help (and won't hurt).

### HTML and CSS Incompatibilities No Longer Exist

Mostly true, agreed, and you should definitely be using the newer HTML5
tags. However, browser manufacturers continue to introduce support for
new tags (that may not be a part of the specification) and that support
isn't always the same or consistent, so you find yourself having to
[fall back on older tags](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Cross_browser_testing/HTML_and_CSS#html_fallback_behavior).
We might as well plan (and code) accordingly.

### All Modern Browsers Ship with JavaScript (enabled)

"Uhg! God! FFS!! Internet Exploder 6 is long dead. Stop already with
this. JavaScript is always available!"

I know, I know, but let's just have a look at when it's _not_ available:

- Googlebot (and similar) can take years to index React applications (as
of this writing) if it ever even indexes them at all 

- You can't control the quality of the code included from third parties,
or the network. An error there can completely block the JS engine.

- We all write buggy code sometimes

#### React Won't Compile if there is a Syntax Error

True.

So, if you're willing to put in the extra work to do it in React, why
not just do it in a PE manner for the same amount of effort? In fact, React and Progressive Enhancement are not mutually exclusive, at all.

We have two
steps we can use to progressively enhance the experience. Let's start
with step 3:

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