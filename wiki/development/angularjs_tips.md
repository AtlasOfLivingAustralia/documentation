# AngularJS Tips and Tricks

## Hide content until bindings have been processed

### The problem
AngularJS processes its bindings after the DOM has been loaded. This means that the browser will display the markup prior to AngularJS doing its magic, and so you can end up with a screen with partially populated or empty views that flash up before the end result is displayed (you'll often see {{...}} text on the screen). AngularJS provides the ```ng-cloak``` directive to resolve this, but there is a slight catch when you defer the loading of JS files to after the page load: ```ng-cloak``` uses CSS to hide the content, but that CSS is inline in the Angular JS file, so it is not available when the page loads.

### The solution
Add the following to your application CSS:
```
/**
 * Angular uses ng-cloak to hide dynamic content until the data is
 * available. This causes screen flicker that shows the {{}} tags.
 * Angular (as per best practise) is loaded at the bottom of the
 * page. The CSS it uses to hide dynamic content in in-lined in the
 * AngularJS script file, so it also loads at the bottom of the page.
 * This means that even with ng-cloak we still get the flicker.
 *
 * Our CSS is loaded earlier, so we can specify the rule here and
 * avoid most of the flicker.
 */
[ng-cloak] {
    display: none !important;
}
```

## Clean and readable scoping for controllers

### The problem
When using multiple controllers on the same page, it can be difficult to see clearly which controller owns which variables.

### The solution
Use the "Controller-as" pattern, where you define a name for each controller in your markup, and prefix all variables with that name.

E.g.
```
<div ng-controller="Controller1 as firstCtrl">
...
  {{ firstCtrl.someVar }}
</div>

<div ng-controller="Controller2 as secondCtrl">
...
  {{ secondCtrl.someVar }}
</div>
```

## Use ```var self = this``` inside controllers

This makes it very obvious, and safe, as to which variables are within which scope. Also, anything prefixed with ```self.``` will be accessible in the view as a variable or function.
