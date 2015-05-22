title: AngularJS unit testing with Jasmine
date: 2015-03-01 09:42:09
tags:
- TDD
- JavaScript
- AngularJS
---
I am currently working on a hybrid mobile app using [Ionic](http://ionicframework.com/) which is based on [AngularJS](http://angularjs.org/). [AngularJS](http://angularjs.org/), just like my favorite ActionScript 3 framework [Robotlegs](http://www.robotlegs.org/), was designed to be testable. With helps of dependency injection and built in mocks, it is quite easy to write tests but still can be painful to get everything sorted out. Here are some tips to make life easier. Like a template (in [WebStorm](http://www.jetbrains.com/webstorm/)) to setup a test for a service:

```javascript
describe('$MODULE$.$NAME$', function () {
    var $NAME$;
    beforeEach(module('$MODULE$'));
    beforeEach(inject(function (_$NAME$_) {
        $NAME$ = _$NAME$_;
    }));
});
```

## Mock dependencies

At start of tests when we load the module we can mock dependencies at the same time:

```javascript
var $VALUE$;
beforeEach(module('$MODULE$', function ($provide) {
    $VALUE$ = {$END$};
    $provide.value('$VALUE$', $VALUE$);
}));
```

## Test controllers

Unlike services, controllers can not be injected so we need to instantiate them ourselves.
<!--more-->

```javascript
var $NAME$, $rootScope, $scope;
beforeEach(inject(function ($controller, _$rootScope_) {
    $rootScope = _$rootScope_;
    $scope = $rootScope.$new();
    $NAME$ = $controller('$NAME$', {
        '$scope': $scope
    });
}));
```

## Spies

It is quiet handy to use spies for mocking and stubbing in Jasmine. Here is a template to return a value from a mocked method.

```javascript
var $VALUE$;
beforeEach(function () {
    $VALUE$ = {};
    spyOn($TARGET$, '$METHOD$').and.returnValue($VALUE$);
});
```

## $q and promise

It was a pain for me to test promises at first. I tried to use Jasmine's asynchronous support by adding done method argument but no luck. Now I just go with Jasmine's spies.

To save time I have a `test_helpers.js` and I firstly define $q and $timeout there.

```javascript
var $q, $timeout;
var $_q = function (_$q_, _$timeout_) {
    $q = _$q_;
    $timeout = _$timeout_;
};
var $_resolve = function (value) {
    var defer = $q.defer();
    defer.resolve(value);
    return defer.promise;
};
```

Use this template to inject $q, $timeout then you can use them in your test:

```javascript
beforeEach(inject(function (_$q_, _$timeout_) {
    $_q(_$q_, _$timeout_);
}));
```

This template is to mock a method and return a promise:

```javascript
var $TARGET$_$METHOD$_defer;
beforeEach(function () {
    $TARGET$_$METHOD$_defer = $q.defer();
    spyOn($TARGET$, '$METHOD$').and.returnValue($TARGET$_$METHOD$_defer.promise);
});
```

Then I use spies to verify results and carefully call `$timeout.flush();` first.

```javascript
var onSuccess = jasmine.createSpy('onSuccess');
$END$
$timeout.flush();
expect(onSuccess).toHaveBeenCalled();
```

## $translate

Controllers using $translate can be hard to test so I put this to my 'test_helpers.js':

```javascript
var $_translate = function ($provide, value) {
    var $translate = function () {
        return $_resolve(value || {})
    };
    $translate.storageKey = function () {
        return '';
    };
    $translate.storage = function () {
        return {
            get: function () {
                return '';
            }
        };
    };
    $translate.preferredLanguage = function () {
        return 'zh';
    };
    $translate.use = function () {
    };
    $provide.value('$translate', $translate);
};
```

And a template to save typing:

```javascript
beforeEach(module('$MODULE$', function ($provide) {
    $_translate($provide);
}));
beforeEach(inject(function (_$q_, _$timeout_) {
    $_q(_$q_, _$timeout_);
}));
```
