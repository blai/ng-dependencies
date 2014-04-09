#ng-dependencies

> Analyze javascript code using [esprima](https://github.com/ariya/esprima) and return a list of objects representing the module dependencies in the code.

This is based on [gulp-angular-filesort](https://github.com/klei/gulp-angular-filesort). I extracted the module dependency code because I need to find a way to dynamically generate a root angular module that depends on a list of angular modules as Bower packages.

### Usage
```js
var fs = require('fs');
var ngDeps = require('ng-dependencies');

var deps = ngDeps(fs.readFileSync('./someNgModule.js'));

console.log(deps);
```

If the content of `./someNgModle.js` is as following:
```js
angular.module('test', ['one']).run(function () {
  // run some logic
});
angular.module('another', []);
angular.module('another').Controller('Ctrl', ['$scope', function ($scope) {
  // some controller logic
}]);
angular.module('useThis').run(function () {
  // ...
});
```

This will output:
```js
[
  {
    uses: ['one', 'useThis']
  },
  {
    name: 'test',
    uses: ['one']
  },
  {
    name: 'another',
    uses: []
  }
]
```