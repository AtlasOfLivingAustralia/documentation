# Code Style and Review guidelines

*Rule #1: Always think about the poor sods who will come along after you and maintain your code - be nice, make their job as easy as possible.*

*Rule #2: Be consistent - to a large extent, it doesn't matter _how_ something is done as long as everything is consistent.*

*Rule #3: All code should be reviewed by at least one other developer, and all changes should be tested by someone other than the author.*

## General guidelines

1. Do not mix languages in a single file:
  1. Do not generate large amounts of JS in Groovy
  1. Do not dump large amounts of JS in .gsps
  1. Do not put large amounts of HTML inline in JS files
  1. Do not put CSS styles in .gsps (that also means do not use inline styles (```style="..."```))
  1. Javascript belongs in .js files
  1. HTML belongs in .html or .gsp files
  1. CSS belongs in .css files
1. _Consistency is king_:
  1. Consistent formatting makes code easier to read
  1. Consistent design makes code easier to extend and integrate
  1. Consistent languages/frameworks makes code easier to pick up when moving onto a different product
  1. Consistent implementation makes code easier to maintain
  1. Consistent structure (i.e. classes, methods, packages etc) makes code easier to maintain and extend
  1. Consistent testing makes code easier (and safer) to refactor and extend
1. Format your code consistently - this makes it easier to read and therefore easier to maintain
1. Take advantage of IntelliJ's code inspections: the right-hand margin of the editor will highlight potential bugs or maintenance issues with a little line. Your goal should be to have 0 warnings...much of our code looks like a barcode.
1. Structure your code cleanly:
  1. Separation of concerns - group code into classes/files based on the behaviour or purpose of that code
  1. Methods perform a single purpose - lots of small single-purpose methods are easier to test (and read) than one method that does multiple things
  1. Methods should be small: at the very most they should not exceed 1 screen of text, but ideally should be less than 20 or so lines. This keeps them succinct and focussed on a particular task.
  1. Methods should have 1 return at the end to improve readability. An exception to this rule would be trivial exit conditions, where you check for a basic precondition at the start of the method and return immediately if the precondition is not met.
1. Code should be self documenting - comments should ideally only be needed to explain _why_ something is done: the code itself says what and how
  1. Good structure (single purpose methods etc)
  1. Good naming conventions (NO SINGLE CHARACTER VARIABLES (except integer counters in loops))
  1. Clear and concise code (just because you can put everything on a single line doesn't mean you should)
  1. Use types where they are known - this helps explain what the code does by saying what types it expects/returns (it also helps the IDE provide code insight and autocompletion)
1. Do not check in commented or dead code (unused)!
1. Do not copy & paste code! Refactor it into a reusable form.
1. Do not include implementation details in names - i.e. do not call methods "ajaxSomething" because it may not always be used via ajax; do not call a variable "googleMap" because the implementation might change to another provider (e.g. Leaflet).
1. Remember: just because the language lets you do something doesn't mean you should.
1. Get a general understanding of basic accessibility requirements, and apply them to your UI code.
1. Never, ever, hardcode environment specific variables. Use externalised properties.
  1. This means never put URLs into the code: we write open source software than we want other countries to adopt - they don't want to point to ALA!

### Javascript guidelines

1. Defer loading of javascript to the end
1. When using Leaflet or Google maps, do not assume that the map container will always have a fixed id of "map" - this would limit you to 1 map per page.
  1. NEVER assign the map to a global variable: ```window.alaMap = map``` is WRONG.
  1. You are tying the hands of anyone who comes along later and wants/needs to use more than 1 map per page.
1. Do not use global variables/functions - scope your code properly.
1. Add ```"use strict";``` to the start of all Javascript files (see http://www.w3schools.com/js/js_strict.asp)
1. Use ```var self = this; ... self.foo = "bar";``` instead of ``` this.foo = "bar"``` to ensure that your bindings are consistent when functions are passed as/assigned to variables.

## Code Style

The default IntelliJ code format is good enough in almost all cases.

If you keep different language code in the correct files (i.e. don't dump copious amounts of Javascript in a .gsp file) then IntelliJ will format it correctly.

So write some code, then select it (if you want to only format that bit, otherwise the whole file will be formatted), and hit
```
Cmd + Alt + L
```
for Mac; or
```
Ctrl + Alt + L
```
for Windows.

Mixing languages confuses IntelliJ, and leads to brittle and difficult to test code. Don't do it.

## Code Reviews

Remember: _Code reviews are an invitation for a conversation, NOT a criticism_. Treat them as a learning opportunity for both parties. Try to keep egos on a leash.

This gives a good starting point: [Grails and Groovy Code Review Guidelines](https://tedvinke.wordpress.com/2015/03/15/basic-groovy-and-grails-code-review-guidelines/)

Code reviews are extremely important as they provide a number of benefits to the team and to the resulting product:

1. Team members are exposed to parts of the system that they are not directly working on by reviewing the work of team mates
1. Team members can learn new ways of doing things from reviewing the work of team mates
1. Team members can suggest better/cleaner/different ways of doing things when they review a team mate's work
1. Bugs can be found by properly reviewing a piece of code
  1. Code reviews are not just about style - you should mentally step through the code as if you were debugging it: look for bugs!
1. The end product is more likely to have consistent code if it being reviewed by the whole team - consistent in terms of formatting, but also in terms of implementation: e.g. I implement security via an annotation and filter, but I notice during a review that Bob used inline if statements....have a chat, agree on one approach or the other, and make the implementation consistent
1. Bugs will be found earlier - i.e. before they go to production


### What should you do in a code review?

1. Review the style and format of the code for consistency with the rest of the codebase (you should not be able to identify the author just by looking at how they format their code!)
1. Review the use of types - just because Groovy is dynamically typed doesn't mean that you shouldn't use types when you know them - typed code is more explanatory and easier to understand than something that declares everything as 'def'
1. Review the structure of the code:
  1. is the code appropriately separated into methods/classes/files/packages?
  1. are methods short and concise, performing just a single function?
  1. does the code follow separation of concerns guidelines?
  1. Are different languages isolated from each other so as to be more easily tested
    1. Are related areas clearly associated with each other (i.e. is it easy to identify which .js file contains the script for a particular block of html?)
1. Review the naming conventions in use - do all classes/methods/variables have accurately descriptive names?
1. Is there any commented or dead (unused) code? This should be removed
1. Step through the code as if you were debugging it, trying to spot potential bugs. E.g.
  1. could the code result in a null pointer?
  1. could the code result in an endless loop?
  1. could a method return an unexpected value?
  1. is the code efficient?
  1. how are exceptions handled?
  1. are the correct html attributes being used (e.g. title, not alt, to get a tooltip on a link)?
  1. Are environment specific values like URLs hardcoded?
1. Review the unit tests
  1. Do they exist? Should they?
  1. Do they sufficiently cover the code being tested?
  1. Do they test boundary conditions?
  1. Have existing tests been updated to reflect the code change?
1. For a large change, check out the branch and run it, doing some basic spot checks.
1. Don't only focus on negatives: if you see a nice bit of code, or something you didn't know how to do, say so! Code reviews are an opportunity for the reviewer to learn, too.


### How should you respond to a code review?

1. Remember that reviews are an invitation for a conversation, not an attack on your work
1. If you disagree with a comment, reply to it explaining why
  1. If the reviewer comes back again, go and have a talk with them to come to an agreement rather than bouncing comments back and forth, the summarise the agreement in a final comment
1. When you read a comment, remember that it might apply to more than just the point where the comment was made. It is your responsibility to apply the change to all relevant locations.
  1. For example, a comment saying to use types when known rather than defining everything with 'def' might be made against 1 variable in one class: it is your responsibility to apply the change to ALL of your code, not just that one variable.
1. Respond to all comments so the reviewer knows you have looked at it:
  1. Write 'done' if you apply the suggested change
  1. Explain your reasons if you decide not to apply the change
