# Hide content until bindings have been processed

## The problem
KnockoutJS, like AngularJS, processes its bindings after the DOM has been loaded. This means that the browser will display the markup prior to KnockoutJS doing its magic, and so you can end up with a screen with partially populated or empty views that 'flashes' before the end result is displayed. AngularJS provides the ng-clock directive to resolve this, but KnockoutJS doesn't seem to have an equivalent.

## The solution
You can work around this behaviour with the following markup:

```
<div data-bind="visible: false">
  <span class="fa fa-spin fa-spinner"></span>&nbsp;Loading...
</div>
<div style="display: none" data-bind="visible: true">
  <!-- normal view content, with KnockoutJS bindings, goes here -->
</div>
```

What does it do?

* Hide the content that is being manipulated by KnockoutJS (you need to use inline styles rather than CSS rules because KO's ```visible``` binding uses inline styles, and so will override your CSS rule)
* Displays a 'Loading...' message and spinner icon
* Uses KO to hide the spinner, and show the content - this is triggered _by KnockoutJS_, and so it will happen after KO is loaded and has processed all the data bindings.

To make this a bit nicer to use, put it into a Grails taglib (see the ```KoLoadingTagLib.groovy``` in Biocollect for an example).
