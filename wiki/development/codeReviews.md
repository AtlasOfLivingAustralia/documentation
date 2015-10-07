# Code Style and Review guidelines

## General guidelines

1. Do not mix languages in a single file:
  1. Do not generate large amounts of JS in Groovy
  1. Do not dump large amounts of JS in .gsps
  1. Do not put CSS styles in .gsps
  1. Javascript belongs in .js files
  1. HTML belongs in .thml or .gsp files
  1. CSS belongs in .css files
1. Format your code consistently - this makes it easier to read and therefore easier to maintain
1. Take advantage of IntelliJ's code inspections: the right-hand margin of the editor will highlight potential bugs or maintenance issues with a little line. Your goal should be to have 0 warnings...much of our code looks like a barcode.
1. Structure your code cleanly:
  1. Separation of concerns - group code into classes/files based on the behaviour or purpose of that code
  1. Methods perform a single purpose - lots of small single-purpose methods are easier to test (and read) than one method that does multiple things
1. _Consistency is king_:
  1. Consistent formatting makes code easier to read
  1. Consistent design makes code easier to extend and integrate
  1. Consistent languages/frameworks makes code easier to pick up when moving onto a different product
  1. Consistent implementation makes code easier to maintain
  1. Consistent structure (i.e. classes, methods, packages etc) makes it easier to maintain and extend
  1. Consistent testing makes it easier (and safer) to refactor and extend
1. Self documenting - comments should ideally only be needed to explain _why_ something is done: the code itself says what and how
  1. Good structure (single purpose methods etc).
  1. Good naming conventions (NO SINGLE CHARACTER VARIABLES (except integer counters in loops))
  1. Clear and concise code (just because you can put everything on a single line doesn't mean you should)
  1. Use types where they are known - this helps explain what the code does by saying what types it expects/returns (it also helps the IDE provide code insight and autocomplete)
1. DO NOT CHECK IN COMMENTED OR DEAD (unused) CODE!

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

Mixing languages confuses IntelliJ, and leads to bittle and difficult to test code. Don't do it.

## Code Reviews

Remember: _Code reviews are an invitation for a conversation, NOT a criticism_.

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
    1. Are related areas clearly associated with each other (i.e. is it easy to identify which .js file contains the script for a paricular block of html?)
1. Review the naming conventions in use - do all classes/methods/variables have accurately descriptive names?
1. Is there any commented or dead (unused) code? This should be removed
1. Step through the code as if you were debugging it, trying to spot potential bugs. E.g.
  1. could the code result in a null pointer?
  1. could the code result in an endless loop?
  1. could a method return an unexpected value?
  1. is the code efficient?
  1. how are exceptions handled?
  1. are the correct html attributes being used (e.g. title, not alt, to get a tooltip on a link)?
1. Review the unit tests
  1. Do they exist? Should they?
  1. Do they sufficiently cover the code being tested?
  1. Do they test boundary conditions?
  1. Have existing tests been updated to reflect the code change?
1. For a large change, check out the branch and run it, doing some basic spot checks.