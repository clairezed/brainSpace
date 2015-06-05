* [Development](#development)
 * [Javascript](#javascript)
  * [Javascript debugging tips](#javascriptDebuggingTips)
 * [Meteor](#meteor)
  * [Meteor tips](#meteorTips)
* [Design](#design)
 * [Font](#font)
 * [Patterns](#patterns)
* [Books](#design)
 * [Javascript](#javascript)
  * [Notes Eloquent.js](books/eloquentJsNotes.md)

___

# <a name="development">Development

## <a name="javascript">Javascript

### <a name="javascriptDebuggingTips">Javascript debugging tips

- The selected element via the inspector can be displayed with `$0`. Following siblings can be displayed with `$1`, `$2`...
- `console.table(complexObject)` displays a list of objects as a table, in browser console
- `console.trace('string')` in your code will show in the console the entire stack trace that resulted in the current function call.
- typing `debug(functionName)` in the console will make your script stop in debug mode when it gets a function call functionName.
- typing `monitor(functionName)` in the console enables to watch specific function calls and it's arguments.
- `$('css-selector')` will return the first match of a CSS selector. `$$('css-selector')` will return all of them.

## <a name="meteor">Meteor

### <a name="meteorTips">Meteor tips

Things to try and implement in a next meteor project

__Monitoring packages__

- [Mongol](https://github.com/msavin/Mongol) : Visual Editing Tool for MongoDB Collections
- [JetSetter](https://github.com/msavin/JetSetter) : Visual Get/Set Tool for Meteor Session Variables

__Fast development utilities__

- how to create scaffolding : https://github.com/Elao/meteor-admin-generator
- [create a yeoman generator](http://www.mightymeteorites.com/articles/create-a-basic-yeoman-generator-for-meteor-in-20-minutes)

__Developping mobile apps__

- [The illustrated guide to mobile apps with Meteor](https://www.yauh.de/the-illustrated-guide-to-mobile-apps-with-meteor/) : huge doc but quite small app, no routing, no design framework, but explains store submission
- [Meteor Point mobile developing page](http://meteorpoint.com/do-you-know-about/mobile-developing) : list of links about meteor mobile app development

__Alerts and notifications__

- https://github.com/juliancwirko/meteor-s-alert/

__Making and using its own packages__

- [check telescope package system](http://www.telescopeapp.org/blog/previewing-telescope-big-refactor/)
- [Writing a package, by Meteor Chef](http://themeteorchef.com/recipes/writing-a-package/)

__File organisation__

- https://medium.com/unexpected-token/how-to-organize-your-files-on-your-meteor-projects-ef7f34373ed


__Animation__

- [Meteor template animation](https://github.com/gwendall/meteor-template-animations) : simple DOM animations for Meteor

__Good practices__

- [Keeping App State on the URL](https://meteorhacks.com/meteor-ui-pattern-keeping-app-state-in-the-url)


# <a name="design">Design

## <a name="font">Font

- http://fontpair.co/ Font Pair helps designers pair Google Fonts together

## <a name="patterns">Patterns

- http://thepatternlibrary.com/ few patterns to use in websites

# <a name="books">Books

## <a name="javascript">Javascript

- [Notes Eloquent.js](books/eloquentJsNotes.md)


