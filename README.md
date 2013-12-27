# uxGenie

Directive for [GenieJS](http://www.github.com/kentcdodds/genie)

[Demo](http://kentcdodds.github.com/genie)

## Vernacular

See the original genie [vernacular documentation](https://github.com/kentcdodds/genie#vernacular). Words specific to this directive:

 - Lamp: The home of Genie. In Arabian folklore, the genie is imprisoned in a lamp until summoned by rubbiux the lamp to perform wishes. In a GenieJS context, this is the interface between GenieJS and the user.
 - UX: The `ux-` prefix stands for "User eXperience." This was used in an effort to follow the [Best Practices](https://github.com/angular/angular.js/wiki/Best-Practices) proposed by the AngularJS Team.

## Setup

The directive is called `uxLamp` in a module called `uxGenie`. It is restricted to attributes ([Angular default](http://docs.angularjs.org/guide/directive)). Here is an example of it's use:

### Load Script

```html
<script src="./vendor/genie.js"></script> <!-- uxGenie depends on the global genie variable -->
<script src="./vendor/uxGenie.js"></script>
```

### Add Dependency

```javascript
angular.module('yourApp', ['uxGenie']);
```

### Directives

#### ux-lamp

**Short Version**

```html
<div ux-lamp></div>
```

This will provide you will the default lamp functionality. The lamp is rubbed with <kbd>Ctrl</kbd> + <kbd>Space</kbd> and it will simply appear/disappear.

**Long Version**

```html
<div 
  ux-lamp
  lamp-visible="genieVisible"
  rub-class="visible"
  rub-shortcut="32"
  rub-modifier="ctrlKey"
  rub-event-type="keypress"
  wish-callback="wishMade(wish)"
  local-storage="true">
</div>
```

The attributes of interest:

 - `ux-lamp` - The directive itself.
 - `lamp-visible` - A doubly bound variable which controls the visibility of the lamp. (Toggles between `true` and `false` when the lamp is rubbed.)
 - `rub-class` - The class to give the lamp when it should be visible. Useful for CSS transitions. If excluded, the lamp will simply appear/disappear when rubbed.
 - `rub-shortcut` - The character code or character to bind as a shortcut to rub the lamp. Defaults to 32 (<kbd>space</kbd> keyCode).
 - `rub-modifier` - A modifier key to be pressed to rub the lamp (ie `ctrlKey`, `metaKey`, `shiftKey`, `altKey`). Defaults to `ctrlKey`.
 - `rub-event-type` - The type of event to bind the lamp rubbing shortcut to (like `keypress`, `keyup`, or `keydown`).
 - `wish-callback` - A function to call when a wish is made (i.e. the user clicks or presses <kbd>enter</kbd> on a wish). The wish which was made is passed to this as an argument.
 - `local-storage` - Defaults to false, but if it is set to true (or a truthy variable) then the user's preferences will be saved to their local storage and reloaded whenever they refresh the browser.

##### Data

The text displayed for each wish is either what is contained in the `displayText` property of the wish's `data` property.
If this is null, then it uses the first item in the `magicWords` array of the wish.

An `img` tag will be created and added prior to each wish which has a `data.imgIcon` property (this will be assigned to the `img` tag's `src` property).

An `i` tag will be created and added prior to each wish which has a `data.iIcon` property (this will be assigned to the `i` tag's `class` property).

#### genie-wish

**Short Version**
<a href="/home" genie-wish="Go Home">Home</a>

**Long Version**
<a href="/home"
   genie-wish="Go Home"
   wish-id="go-home-wish-id"
   wish-context="comma separated,wish contexts"
   wish-data="scopeVariable"
   wish-event="click"
   wish-action="scopeFunction(wish)"
   event-modifiers="ctrlKey,altKey,shiftKey,metaKey">Home</a>

The attributes of interest:

 - `genie-wish` - The directive itself. This can be a comma separated list for multiple magic words.
 - `wish-id` - The id of the wish
 - `wish-context` - A comma separated list of contexts
 - `wish-data` - A scope object for any wish data. This is doubly bound. More info about data below.
 - `wish-event` - The dom event to trigger on the element.
 - `wish-action` - A scope function to be executed after the event is triggered. More info about this below.
 - `event-modifiers` - a comma separated list of modifiers to add to the event triggered on the element.

If for some reason you would rather not have the `genie-wish` attribute be the magic words, you can leave
that attribute empty and the `genie-wish` directive will look for a name or id (in that order) and assign
the magicWords to that value (split by commas). If none of these have values, you will get an error.

##### Events and Actions

If no wish-action or wish-event is present, a 'click' event will be triggered. If a wish-event is present then
that event will be triggered on the element. If both a wish-event and wish-action are present, the wish-action
will be called after the wish-event is triggered.

##### Data

This directive uses the following wish data items:

 - `element` - This is assigned by the directive. It is the element to perform the action on.
 - `event` - This is assigned by the given `wish-event` or the given `wish-data`'s data.event property.
     If neither of those are present, it defaults to 'click'.

## Other Stuff

### View All Wishes
If the first character typed in the lamp input field is <kbd>'</kbd> (apostrophe), then it is stripped from the input when genie is queried for matching magic words. This is primarily to enable a user to see all the available wishes.

### CSS
If all you do is follow the instructions above you'll be disappointed to see that the lamp looks nothing like the demo. This is because I've opened it up to you to do custom CSS. You can borrow from the css I've made in the demo, or you can be creative and use the classes available to you:

```css
.genie-container {/* */}

.genie-container > input {/* */}

.genie-wishes {/* */}

.genie-wish {/* */}

.genie-wish > img {/* */}

.genie-wish > i {/* */}

.genie-wish.focused {/* */}
```

## Contributing

I'd love to accept [pull requests](https://github.com/kentcdodds/ux-genie/pulls).
When you clone the repo, make sure to run `bower install` to get the dependencies.
Also, please uglify `uxGenie.js` to `uxGenie.min.js` using [UglifyJS2](https://github.com/mishoo/UglifyJS2)
and this command: `uglifyjs uxGenie.js -o uxGenie.min.js --comments` thanks!

## Issues

If you have a problem with GenieJS please don't hesitate to use GitHub's [issue tracker](https://github.com/kentcdodds/ux-genie/issues)
to report it. I'll do my best to get it resolved as quickly as I can.

## The Future...

... [is as bright as your faith](https://www.lds.org/general-conference/2009/04/be-of-good-cheer?lang=eng).
*And* I plan on adding the following features in the future

 - Make tests... :)
 - Allow custom template for wishes
 - Make a jQuery plugin version of this...? I'd love to see someone else make this if you like :)

## License

The MIT License (MIT)

Copyright (c) 2013 Kent C. Dodds

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
the Software, and to permit persons to whom the Software is furnished to do so,
subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/kentcdodds/ux-genie/trend.png)](https://bitdeli.com/free "Bitdeli Badge")

