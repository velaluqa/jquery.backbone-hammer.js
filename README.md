# jquery.backbone-hammer.js
[Backbone.js](http://backbonejs.org/) + [Hammer.JS](http://hammerjs.github.io/)

A tiny monkey patch for ```Backbone.View``` and ```Hammer.Manager```
loosely based on
[jbottigliero/backbone-hammerjs-view](https://github.com/jbottigliero/backbone-hammerjs-view)
superseeding the Hammer.JS jQuery plugin.

### Usage

Include Backbone.js and Hammer.JS (v2) first, but don't add the Hammer.JS
jQuery plugin, as it patches ```Hammer.Manager::emit``` differently.

To use Hammer with default settings:

```coffeescript
class MyView extends Backbone.View
  events:
    'tap .button': 'handleTap'
    'swipeleft': 'handleSwipeLeft'
  hammerjs: true
```

To let you customize your Hammer instance, any truthy ```hammerjs```
attribute different from ```true``` will get passed to the Hammer
initializer:

```coffeescript
class MyView extends Backbone.View
  events:
    'tap .button': 'handleTap'
    'swipeleft': 'handleSwipeLeft'
  hammerjs:
    recognizers: [
      [Hammer.Rotate, { enable: false }],
      [Hammer.Pinch, {}, ['rotate']],
      [Hammer.Swipe,{ enable: false }],
      [Hammer.Pan, { direction: Hammer.DIRECTION_ALL, threshold: 1 }, ['swipe']],
      [Hammer.Tap, { threshold: 5 }],
      [Hammer.Tap, { event: 'doubletap', taps: 2, posThreshold: 20, threshold: 5 }, ['tap']],
      [Hammer.Press, { enable: false }]
    ]
```

You can access the Hammer object with ```@hammer```:

```coffeescript
handlePinchEnd: (e) =>
  # Do not call pan handlers
  @hammer.stop()
```

### Caveat

The event delegation implemented by the ```Hammer.Manager::emit```
monkey patch works pretty well for me, but I am sure it will not cover
all use cases. So please ...

### Contribute :)

Pull requests and contributions are greatly appreciated.
