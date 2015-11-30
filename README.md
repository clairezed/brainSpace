* [System](#system)
* [Development](#development)
 * [Static Websites](#staticWebsites)
 * [Javascript](#javascript)
  * [Javascript debugging tips](#javascriptDebuggingTips)
 * [Meteor](#meteor)
  * [Meteor tips](#meteorTips)
  * [Meteor generic resources](#meteorResources)
* [Design](#design)
 * [Font](#font)
 * [Patterns](#patterns)
 * [Divers](#designdiverse)
* [Books](#design)
 * [Javascript](#javascript)
  * [Notes Eloquent.js](books/eloquentJsNotes.md)

___

# <a name="system">System

- http://itsfoss.com/how-to-fix-no-sound-through-hdmi-in-external-monitor-in-ubuntu/

# <a name="development">Development

## <a name="staticWebsites">Static Websites

- [Modern Static website generators - Smashing magazine](http://www.smashingmagazine.com/2015/11/modern-static-website-generators-next-big-thing/) : list of usefull complementary ressources for building sites usng static generators

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

__File organisation__

- https://medium.com/unexpected-token/how-to-organize-your-files-on-your-meteor-projects-ef7f34373ed

__Monitoring your application__

- [Mongol](https://github.com/msavin/Mongol) : Visual Editing Tool for MongoDB Collections
- [JetSetter](https://github.com/msavin/JetSetter) : Visual Get/Set Tool for Meteor Session Variables

__Fast development utilities__

- how to create scaffolding : https://github.com/Elao/meteor-admin-generator
- [create a yeoman generator](http://www.mightymeteorites.com/articles/create-a-basic-yeoman-generator-for-meteor-in-20-minutes)

__Routing__
- Watch flow router ?

__Reactivity__
- [Reining in the reactivity with Meteor](http://tomkelsey.co.uk/reining-in-the-reactivity-with-meteor/) : limiting reactivity for ux purpose

__Making and using its own packages__

- [check telescope package system](http://www.telescopeapp.org/blog/previewing-telescope-big-refactor/)
- [Writing a package, by Meteor Chef](http://themeteorchef.com/recipes/writing-a-package/)
- [How I develop MeteorJS apps - part 1. Package for everything](http://howwedoapps.com/2015/07/16/how-i-develop-meteorjs-apps-part1)
- [https://github.com/Compy/meteor-mobile-desktop](https://github.com/Compy/meteor-mobile-desktop) : example use of packages to develop an app running on both desktop and mobile

__Developping mobile apps__

- [The illustrated guide to mobile apps with Meteor](https://www.yauh.de/the-illustrated-guide-to-mobile-apps-with-meteor/) : huge doc but quite small app, no routing, no design framework, but explains store submission
- [Meteor Point mobile developing page](http://meteorpoint.com/do-you-know-about/mobile-developing) : list of links about meteor mobile app development
- [Meteor-React-Ionic Mobile App Part 1: The Basic Template](https://medium.com/@SamCorcos/meteor-react-ionic-mobile-app-part-1-the-basic-template-9355ebf3397f)
- [1st app in meteor](https://medium.com/@jracollins/my-first-app-carspot-an-instagram-clone-for-cars-was-approved-for-the-itunes-store-on-the-18-8-f4668f8b17de) : codeship, modulus, settings.json, compose.io, cordova, WkWebView, Appcache package, imgix...
- [Building an ultra responsive UI sharing nearly all code between web and mobile](https://hansoftx.com/blog/building-an-ultra-responsive-ui) : example of a desktop and mobile app built with meteor
- [Offline app with Meteor and Cordova](http://rafaelquintanilha.com/offline-app-with-meteor-and-cordova/) : ionic, grounddb, link to an app

__Communicating with outside world : API, etc__

- [Authenticating a Meteor.js app using a Chrome Extension](https://medium.com/meteor-js/authenticating-a-meteor-js-app-using-a-chrome-extension-321a5e3a18e) : building a RESTful API and authenticating server side through a chrome extension
- [How to connect Meteor.js to an external API](https://medium.com/meteor-js/how-to-connect-meteor-js-to-an-external-api-93c0d856433b)
- [How to make Meteor web apps communicate together](https://medium.com/unexpected-token/how-to-make-meteor-web-apps-communicate-together-a-comparison-with-the-rest-api-method-acef91040faf)
- [Writing an API](http://themeteorchef.com/recipes/writing-an-api/)
- [Server Side Route Authentication in Meteor](https://blog.kayla.com.au/server-side-route-authentication-in-meteor/)

__Debugging__
- [EASILY DEBUGGING METEOR.JS](http://joshowens.me/easily-debugging-meteor-js/)

__Testing__

__Scaling__
- [Building Large Apps: Tips](https://meteor.hackpad.com/Building-Large-Apps-Tips-d8PQ848nLyE)
- [First experience scaling a meteor app](https://medium.com/@eliezer/first-experiences-scaling-a-meteor-app-14a48e62a4af) : talks about different meteor stacks (cost, pros and cons..), horizontal scaling with nginx, with explanations for noobs and useful links

__Deployment__

- [When a Meteor finally hits production](https://medium.com/@davidyahalomi/when-a-meteor-finally-hits-production-6c37b81f795b) : deployment configuration
- [Continuous delivery with MeteorJS](https://sungwoncho.io/meteorjs-continuous-delivery/)
- [Continuous Integration and Delivery of your Meteor “package for everything” project](http://howwedoapps.com/2015/07/27/how-i-develop-meteorjs-apps-part-3-continuous-integration-and-delivery-of-your-meteor-package-for-everything-project) : with codeship and meteorup
- [Taking meteor apps into production with modulus compose and codeship](https://medium.com/@david_sykora/taking-meteor-apps-into-production-with-modulus-compose-and-codeship-54236d7f0cc) : with modulus, compose, codeship

__Security__
- [Allow & Deny: A Security Primer](https://www.discovermeteor.com/blog/allow-deny-a-security-primer/)

__Good practices__
- [Keeping App State on the URL](https://meteorhacks.com/meteor-ui-pattern-keeping-app-state-in-the-url)
- [Fat models, skinny templates](http://joshowens.me/fat-models-skinny-templates/)

__Animation__

- [Meteor template animation](https://github.com/gwendall/meteor-template-animations) : simple DOM animations for Meteor


__Alerts and notifications__

- https://github.com/juliancwirko/meteor-s-alert/

### <a name="meteorResources">Meteor generic resources

- https://github.com/Urigo/awesome-meteor : A curated, community driven list of awesome Meteor packages, libraries, resources and shiny things



# <a name="design">Design

## <a name="font">Font

- http://fontpair.co/ Font Pair helps designers pair Google Fonts together

## <a name="patterns">Patterns

- http://thepatternlibrary.com/ few patterns to use in websites

## <a name="designdiverse">Divers, à classer

- http://miguel-perez.github.io/smoothState.js/index.html : Add page transitions to your site an boost performance
- http://www.contentsnippets.com/ : Content Snippets collects specific copy examples from websites and applications, to serve as inspiration for writing professionals


# <a name="books">Books

## <a name="javascript">Javascript

- [Notes Eloquent.js](books/eloquentJsNotes.md)


