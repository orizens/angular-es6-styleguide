
# Angular ES2015 Style Guide

## Endorsements
The idea for this styleguide is inspired from [john papa's angular style guide](https://github.com/johnpapa/angular-styleguide) which is focused on angular version 1.x, written with ES5.  
Some sections are currently taken from john papa's styleguide and are subject to change in the future (as this stylguide grows).  
This styleguide is also inspired from [@angularclass's boilerplates](http://github.com/angularclass) for angular & es2015.

## Purpose
*Opinionated Angular.js written with ES2015 and module loadeing style guide for teams by [@orizens](//twitter.com/orizens)*

If you are looking for an opinionated style guide for syntax, conventions, and structuring Angular.js applications v 1.x with ES2015 and module loading, then this is it. These styles are based on my development experience with [Angular.js](//angularjs.org) and the [Echoes Player](http://echotu.be) [Open Source Project Development](http://github.com/orizens/echoes).

The purpose of this style guide is to provide guidance on building Angular applications with ES2015 and module loading by showing the conventions I use and in order to prepare angular v 1.x code to angular v 2.

## Inspirations
[NG6-Starter](https://github.com/AngularClass/NG6-starter)  
[Angular, Gulp, Browserify](https://github.com/jakemmarsh/angularjs-gulp-browserify-boilerplate)  

## See the Styles in a Sample App
This guide is based on my [open source Echoes Player application](http://echotu.be) that follows these styles and patterns. You can clone/fork the code at [echoes repository](https://github.com/orizens/echoes).  

##Translations
TBD  

## Table of Contents

  1. [Single Responsibility](#single-responsibility)
  1. [Modules](#modules)
  1. [Services](#services)

## Single Responsibility

### Rule of 1

  - Define 1  class per file.

  The following example defines the several classes in the same file.

  ```javascript
  // now-playlist.controllers.js 
  /* avoid */
  export class NowPlaylistCtrl {}
  export class NowPlaylistFilterCtrl {}
  ```

  The same classes are now separated into their own files.

  ```javascript
  /* recommended */

  // now-playlist.ctrl.js
  export default class NowPlaylistCtrl {}
  ```

  ```javascript
  /* recommended */

  // now-playlist-filter.ctrljs
  export default class NowPlaylistFilterCtrl {}
  ```

**[Back to top](#table-of-contents)**

## Modules

  - Use ES2015 module loader standards

  *Why?*: it assists in bundling the app and promotes the seperation of concerns. In Addition, Angular 2 is also based on ES2015 module loader standards.

  ```javascript
  import NowPlaylist from './now-playlist'
  ```  

### Module Loaders Tools
  - Browserify
  - Webpack
  - Typescript CLI

### Module Folder Structure
  
  - each module directory should be named with a dash seperator (kebab notation).

  *Why?*: it follows the web-component notation of seperating a tag name with a dash. It is easier to read and follow in the code editor.

  ```
  // name of directory of <now-playlist></now-playlist> component
  - now-playlist
  ```  

#### Module files
  
  - each module should contain the following:  
  1. index.js - it should contain:  
    1. angular module definition  
    1. loading sub modules (if exist)  
    1. module config function  
    1. module directive definition  
    1. module services definition  
  1. a component (directive) file defintion  
    1. should load a controller file  
    1. should load a template file  
  1. a controller ES2015 class  
  1. a template file  
  1. a spec file  

## Application Structure

### Overall Guidelines

```
[-] src
  [-] components
  [-] core
    [-] components
    [-] services
  [-] css
  * app.js
  * index.html
```

## Testing
Unit testing helps maintain clean code, as such I included some of my recommendations for unit testing foundations with links for more information.

### Write Tests with Stories

  - Write a set of tests for every story. Start with an empty test and fill them in as you write the code for the story.

    *Why?*: Writing the test descriptions helps clearly define what your story will do, will not do, and how you can measure success.

    ```javascript
    it('should have a collection of media items', function() {
        // TODO
    });

    it('should find fetch metadata for a youtube media', function() {
        // TODO
    });

    ```

### Testing Library

  - Use [Jasmine](http://jasmine.github.io/) or [Mocha](http://mochajs.org) for unit testing.

    *Why?*: Both Jasmine and Mocha are widely used in the Angular community. Both are stable, well maintained, and provide robust testing features.

    Note: When using Mocha, also consider choosing an assert library such as [Chai](http://chaijs.com). I prefer Mocha.

### Test Runner

  - Use [Karma](http://karma-runner.github.io) as a test runner.

    *Why?*: Karma is easy to configure to run once or automatically when you change your code.

    *Why?*: Karma hooks into your Continuous Integration process easily on its own or through Grunt or Gulp.

**[Back to top](#table-of-contents)**

### Organizing Tests

  - Place unit test files (specs) side-by-side within the component's code.
  - Place mocks in a **tests/mocks** folder

    *Why?*: Unit tests have a direct correlation to a specific component and file in source code.

    *Why?*: Mock files (json) should be agnostic to the component which is using them. Multiple components specs might use the same jsom mocks.

**[Back to top](#table-of-contents)**

## Comments
TBD  

## ES Lint

### Use an Options File
  - Use [eslint.org](http://eslint.org/) to deifne es2015 support
  - Use **.eslintrc** file for linting and support es2015 features

    *Why?*: Provides a first alert prior to committing any code to source control.

    *Why?*: Provides consistency across your team.

    ```javascript
    {
        arrowFunctions: true,
        classes: true,
        modules: true,
        restParams: true,
        spread: true,
        defaultParams: true
    }
    ```
More To Come...  

**[Back to top](#table-of-contents)**

## Routing
TBD  

## Contributing

1. Open an issue for discussion  
2. Create a pull request to suggest additions or changes  

## License

Share your thoughts with an issue or pull request  

### Copyright

Copyright (c) 2015-2016 [Oren Farhi](http://orizens.com)

### (The MIT License)
Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[Back to top](#table-of-contents)**
