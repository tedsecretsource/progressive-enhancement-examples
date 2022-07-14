# Display a Modal

One of the most common features of modern web applications is the use of "modals": a windoid that the user must interact with, normally by entering some kind of text in a text field and hitting Enter, or cancelling out, before being allowed to interact with anything else.

Until very recently, this type of UI element was not available natively in browsers. In this example, we provide a fully functional modal "feel", using native features wherever possible, but falling back to a standard form for browsers that don't support the native elements.

We say "feel" because, in the worst-case scenario, this will render as a form above the rest of the content as there simply is no way to do a dialog using old HTML.

## Resources

- [Safari adds native support](https://webkit.org/blog/12209/introducing-the-dialog-element/)

- [Github modal almost works](https://github.com/tedsecretsource?achievement=pull-shark&tab=achievements)