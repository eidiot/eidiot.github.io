title: Angular event and action timer
tags:
  - AngularJS
  - JavaScript
  - Event
  - Timer
date: 2015-12-21 20:06:46
---

[Angular 2][1] is already in Beta but I am now still working on a project with [ionic][2] which is based on angular 1.4.3. Maybe too late but there are two small pieces of code I think worthing sharing, so I put them into two tiny libs for angular 1: [angular-event][3] and [angular-action-timer][4]. 

# angular-event

Some people are using `$rootScope.$emit()` for event based communication. [angular-event][3] is basicly a wrapper arount it but no more string event names typing around. 

<!-- more -->

## Use as global events

### Define the event service

```javascript
angular.module('demo')
  .factory('evtDemoTriggered', evtDemoTriggered);
  
function evtDemoTriggered(event) {
  return event('demo:triggered');
}  
```

### Inject and dispatch the event

```javascript
angular.module('demo')
  .factory('demoTrigger', demoTrigger);
  
function demoTrigger(evtDemoTriggered) {
  function trigger() {
    evtDemoTriggered.emit();
  }
}  
```
  
### Inject and listen to the event

```javascript
angular.module('demo')
  .factory('demoModel', demoModel);

function demoModel(evtDemoTriggered) {
  evtDemoTriggered.on(onDemoTriggered);
  function onDemoTriggered() {
    //...
  }
}
```

## Use as an instance event

### Define and dispatch the instance event

```javascript
angular.module('demo')
  .factory('demoModel', demoModel);
  
function demoModel(event) {
  var model = {
    value: 0,
    evtUpdate: event('demo:model:update')
  };
  return model;
  
  function demo() {
    model.value++;
    model.evtUpdate.emit(model.value);
  }
}
```

### Inject and listen to the event

```javascript
angular.module('demo')
  .controller('demoController', demoController);

function demoController(demoModel) {
  demoModel.evtUpdate.on(onModelUpdate);
  function onModelUpdate(event, value) {
    alert('Model updated: ' + value);
  }
}
```

# angular-action-timer

[angular-action-timer][4] is a wrapper of `$timeout` and useful when some function needs to be invoked after user "stop" doing something continually. Here is a [Demo][5] for it working together with [angular-event][3]. 

```javascript
angular.module('demo', ['ng.event', 'ng.actionTimer'])
  .factory('demoTrigger', demoTrigger);
  
function demoTrigger(evtDemoTriggered, actionTimer) {
  var timer = actionTimer(trigger, 500);
  return {
    execute: execute
  };

  function execute() {
    timer.schedule();
  }

  function trigger() {
    evtDemoTriggered.emit();
  }
}  
```

[1]: https://angular.io/
[2]: http://ionicframework.com/
[3]: https://github.com/evan-liu/angular-event
[4]: https://github.com/evan-liu/angular-action-timer
[5]: http://plnkr.co/Lvw8BG
