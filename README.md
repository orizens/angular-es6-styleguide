
# Angular 1.5 - ES6/ES2015 Style Guide

## Endorsements
The idea for this styleguide is inspired from [john papa's angular style guide](https://github.com/johnpapa/angular-styleguide) which is focused on angular version 1.x, written with ES5.
Some sections are currently taken from john papa's styleguide and are subject to change in the future (as this stylguide grows).
This styleguide is also inspired from [@angularclass's boilerplates](http://github.com/angularclass) for angular & es2015.

## Purpose
*Opinionated Angular.js written with ES6/ES2015 and module loading style guide for teams by [@orizens](//twitter.com/orizens)*

If you are looking for an opinionated style guide for syntax, conventions, and structuring Angular.js applications v 1.x with ES2015 and module loading, then this is it. These styles are based on my development experience with [Angular.js](//angularjs.org) and the [Echoes Player](http://echotu.be), [Open Source Project Development (currently the es2015 branch)](https://github.com/orizens/echoes/tree/es2015).

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
1. [Components](#components)
1. [Services](#services)
1. [Testing](#testing)
1. [Testing Guidelines](#testing-guidelines)
1. [Write Tests with Stories](#write-tests-with-stories)
1. [Testing Library](#testing-library)
1. [Test Runner](#test-runner)
1. [Testing Controllers](#testing-controllers)
1. [Organizing Tests](#organizing-tests)
1. [Comments](#comments)
1. [ES Lint](#es-lint)
1. [Use an Options File](#use-an-options-file)
1. [Routing](#routing)
1. [Contributing](#contributing)
1. [License](#license)
1. [Copyright](#copyright)


## Single Responsibility

### Rule of 1

- Define 1  class per file.

The following example defines the several classes in the same file.

```javascript
// now-playlist.services.js
/* avoid */
export class NowPlaylistProvider {}
export class NowPlaylistCreator {}
```

The same classes are now separated into their own files.

```javascript
/* recommended */

// now-playlist-provider.service.js
export default class NowPlaylistProvider {}
```

```javascript
/* recommended */

// now-playlist-creator.service.js
export default class NowPlaylistCreator {}
```

**[Back to top](#table-of-contents)**

## Modules

Use [Proposed Loader Specification](https://whatwg.github.io/loader/) (Former ES6/ES2015)

**Why?**: it assists in bundling the app and promotes the seperation of concerns. In Addition, Angular 2 is also based on Loader's standards.

```javascript
import NowPlaylist from './now-playlist';
import { NowPlaylistComponent } from './now-playlist.component';
```

### Module Loaders Tools

- [System.js](https://github.com/systemjs/systemjs)
- [Browserify](http://browserify.org/)
- [Webpack](https://webpack.github.io/)
- [Typescript CLI](http://www.typescriptlang.org/)
  - [tsd is a type definition package manager](http://definitelytyped.org/tsd/)

### Module Folder Structure

- each module directory should be named with a dash seperator (kebab notation).

**Why?**: it follows the web-component notation of seperating a tag name with a dash. It is easier to read and follow in the code editor.

```
// name of directory for <now-playlist></now-playlist> component
- now-playlist
```

### Module files

  - each module should contain the following:
  1. index.js - it should contain:
  1. module-name.component.js - a component (directive) file defintion with class as a controller
  1. template: 
    1. Long html template: an html template file
    2. Small html template: inline backtick template in component file
  1. a spec file

#### index.js - module entry point

  should contain:
  * the module defintion
  * components/directives angular wrappers
  * its dependencies
  * config phase & function
  * angular's entities wrappers - services, factories, additional components/directives, other..

**Why?**: this is the file where we can hook vanilla js files into angular. This is the main point to see what this module is composed of.

```javascript
import angular from 'angular';
import AngularSortableView from 'angular-sortable-view/src/angular-sortable-view.js';
import { NowPlaylistComponent } from './now-playlist.component';

export default angular.module('now-playlist', [
      'angular-sortable-view'
    ])
    .config(config)
    .component(NowPlaylistComponent.selector, NowPlaylistComponent)
    // OR - if you defined controllerAs similary to the component's element:
    .component(NowPlaylistComponent.controllerAs, NowPlaylistComponent)
;
// optional
/* @ngInject */
function config () {

}
```
#### module-name.component.js - component defintion with controller class

  this file should contain:
  1. the component/directive **definition** as a literal object, with export.
  2. the **"controller"** property should be defined as a **class**.
  3. inlined with template string es6/es2015 syntax.
  4. if template is huge (30 lines), consider:
    4.1 break the template to smaller reusable components.
    4.2 fallback to a template that should be imported from external file.

**Why?**: It's easy to understand the bigger picture of this component: what are the inputs and outputs retrieved from scope. Everything is in one place and easier to reference. Moreover, this syntax is similar to angular 2 component definion - having the component configuration above the "controller" class.

```javascript
import template from './now-playlist.tpl.html';

export let NowPlaylistComponent = {
  selector: 'nowPlaylist',
  template,
  // if using inline template then use template strings (es2015):
  template: `
    <section class="now-playlist">
        ....
    </section>
  `,
  // Optional: controllerAs: 'nowPlaylist',
  bindings: {
      videos: '<',
      filter: '<',
      nowPlaying: '<',
      onSelect: '&',
      onRemove: '&',
      onSort: '&'
  },
  controller: class NowPlaylistCtrl {
    /* @ngInject */
    constructor () {
        // injected with this.videos, this.onRemove, this.onSelect
        this.showPlaylistSaver = false;
    }

    removeVideo($event, video, $index) {
        this.onRemove && this.onRemove({ $event, video, $index });
    }

    selectVideo (video, $index) {
        this.onSelect && this.onSelect({ video, $index });
    }

    sortVideo($item, $indexTo) {
        this.onSort && this.onSort({ $item, $indexTo });
    }
  }
}
```

## Components (as of Angular 1.5)
* Use ES2015 class for controller
* Use **Object.assign** to expose injected services to a class methods (make it public)

**Why?** - ```Object.assign``` is a nice one liner usage for overloading services on "this" context, making it available to all methods in a service (i.e., **playVideo** method). 
**Why?** - it follows the paradigm of angular 2 component class (controller), where you would define "public"/"private" on constructor's argument in order to create references on "this" context.

```javascript
export let NowPlaylistComponent = {
  // ....
  controller: class YoutubeVideosCtrl {
    /* @ngInject */
    constructor (YoutubePlayerSettings, YoutubeSearch, YoutubeVideoInfo) {
      Object.assign(this, { YoutubePlayerSettings, YoutubeVideoInfo });
      this.videos = YoutubePlayerSettings.items;

      YoutubeSearch.resetPageToken();
      if (!this.videos.length) {
        YoutubeSearch.search();
      }
    }

    playVideo (video) {
      this.YoutubePlayerSettings.queueVideo(video);
      this.YoutubePlayerSettings.playVideoId(video);
    }

    playPlaylist (playlist) {
      return this.YoutubeVideoInfo.getPlaylist(playlist.id).then(this.YoutubePlayerSettings.playPlaylist);
    }
  }
}
``` 

### Component Options
#### controllerAs
if starting a new component, use the default **$ctrl**. 
if migrating and want to keep the **controllerAs** custom name, define a 'selector' property the same as the camelCased controllerAs value, which will be used with the **angular.component** factory.

**WHY?**: 'selector' in angular 2 indicates the "css" selector which will be used in html. When migrating to Angular 2, it will be easier to transform it to the kebab case.

```
// when controllerAs isn't explicitly defined
// and used as the default "$ctrl"
export let NowPlaylistComponent = {
  selector: 'nowPlaylist'
  ...
}

// when controllerAs is explicitly defined
// and NOT used as the default "$ctrl"
export let NowPlaylistComponent = {
  // when migrating to angular 2 => selector: 'now-playlist'
  selector: 'nowPlaylist'
  controllerAs: 'nowPlaylist'
}
```

## Services
It's a best practice to write all logic in services.

**Why?**: logic can be reused in multiple files. logic can be tested easily when in a service object.

### angular.service
Use **angular.service** api with a class for a service.

**Why?**: Services in angular 2 are classes. it'll be easier to migrate the code.

### angular.factory
use **angular.service** instead.

### angular.provider
export a function as provider as in angular with ES5.

## Application Structure

### Overall Guidelines
[-] - a folder
an * - a file

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
#### src/components
This directory includes **SMART components**. It consumes the **app.core** services and usually doesn't expose any api in attributes.
It's like an app inside a smart phone. It consumes the app's services (ask to consume it) and knows how to do its stuff.

Usage of such smart component is as follows:

```html
<now-playing></now-playing>
```

Example definition in **index.js** can be:

```javascript
import angular from 'angular';
import AppCore from '../core';
import { NowPlayingComponent } from './now-playing.component.js';
import nowPlaylist from './now-playlist';
import nowPlaylistFilter from './now-playlist-filter';
import playlistSaver from './playlist-saver';
import YoutubePlayer from '../youtube-player';

export default angular.module('now-playing', [
      AppCore.name,
      nowPlaylist.name,
      nowPlaylistFilter.name,
      playlistSaver.name,
      YoutubePlayer.name
    ])
    .config(config)
    .component(NowPlayingComponent.selector, NowPlayingComponent)
;
/* @ngInject */
function config () {

}
```

#### src/core/components
This directory includes system wide **DUMB components**. A Dumb Component gets data and fire events. It is communicating only through events.
This is example:
```javascript
<dropdown items="vm.presets" on-select="vm.handlePresetSelect(item, index)"></dropdown>
```

## Testing
Unit testing helps maintain clean code, as such I included some of my recommendations for unit testing foundations with links for more information.

### Testing Guidelines
These are guidelines to follow for setup an environment for testing.

#### Write Tests with Stories

- Write a set of tests for every story. Start with an empty test and fill them in as you write the code for the story.

**Why?**: Writing the test descriptions helps clearly define what your story will do, will not do, and how you can measure success.

```javascript
it('should have a collection of media items', function() {
    // TODO
});

it('should find fetch metadata for a youtube media', function() {
    // TODO
});

```

#### Testing Library

- Use [Jasmine](http://jasmine.github.io/) or [Mocha](http://mochajs.org) for unit testing.

**Why?**: Both Jasmine and Mocha are widely used in the Angular community. Both are stable, well maintained, and provide robust testing features.

Note: When using Mocha, also consider choosing an assert library such as [Chai](http://chaijs.com). I prefer Mocha.

#### Test Runner

- Use [Karma](http://karma-runner.github.io) as a test runner.

**Why?**: Karma is easy to configure to run once or automatically when you change your code.

**Why?**: Karma hooks into your Continuous Integration process easily on its own or through Grunt or Gulp.

**[Back to top](#table-of-contents)**

### Testing Controllers
Since controllers are written as classes, unit tests should be written to check functionality.
Follow this post for [testing angular 1.x components written with ES2015](http://orizens.com/wp/topics/testing-angular-1-x-es2015-component-with-jasmine/)
Steps to follow:
1. import the component javascript file
1. setup a mocked module
1. setup spies (if needed)
1. use ```$controller``` to instanciate a new controller.
  1. 1st argument is the component's controller ("class").
  1. 2nd argument is an object with a new scope and possibly anh mocked objects/spies from previous steps.
1. use ```scope.$digest``` to apply compilation and mock an "angular" data digest cycle (optional in some cases - TBD)

### Organizing Tests

- Place unit test files (specs) side-by-side within the component's code.
- Place mocks in a **tests/mocks** folder

**Why?**: Unit tests have a direct correlation to a specific component and file in source code.
**Why?**: Mock files (json) should be agnostic to the component which is using them. Multiple components specs might use the same jsom mocks.

**[Back to top](#table-of-contents)**

## Comments
TBD

## ES Lint

### Use an Options File
- Use [eslint.org](http://eslint.org/) to deifne es2015 support
- Use **.eslintrc.json** file for linting and support es2015 features

**Why?**: Provides a first alert prior to committing any code to source control.
**Why?**: Provides consistency across your team.

```javascript
{
  "parserOptions": {
    "sourceType": "module",
    "ecmaVersion": 6,
    "ecmaFeatures": {
      "modules": true,
      "arrowFunctions": true,
      "blockBindings": true,
      "destructuring": true,
      "classes": true
    }
  },
  "rules": {
    "indent": [
      "error",
      2
    ],
    "quotes": [
      2,
      "single"
    ],
    "linebreak-style": [
      2,
      "unix"
    ],
    "semi": [
      2,
      "always"
    ]
  },
  "env": {
    "es6": true,
    "browser": true,
    "jasmine": true,
    "commonjs": true
  },
  "globals": {
    "angular": true,
    "inject": true
  },
  "extends": "eslint:recommended"
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

**[Back to top](#table-of-contents)**
