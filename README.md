# $parsers and $formatters

## Overview

Parsers and formatters allow us to have different values in our view and our model for the same item. Don't be confused, they're really simple!

## Objectives

- Describe $parsers and $formatters
- Use a $parser to change Model to View data
- Use a $formatter to change View to Model data

## $parsers and $formatters

We can add parsers and formatters by requiring in the `ngModel` controller in our directive.

This can solve some simple issues and save us processing data down the line - for instance, our model might be minutes, but we might want to display the data as hours and minutes. It saves us processing the data later on in our controllers - the data is simply formatted there and then.

```js
function changeCase() {
	return {
		restrict: 'A',
		require: 'ngModel',
		link: function (scope, element, attrs, ngModel) {

		}
	}
}

angular
	.module('app')
	.directive('changeCase', changeCase);
```

Here we can push functions into two different arrays - `$parsers` and `$formatters`.

### $parsers

Parsers parse incoming data from the `ng-model` directive before saving them to the model. Let's change all values to be stored in lowercase in our model.

```js
function changeCase() {
	return {
		restrict: 'A',
		require: 'ngModel',
		link: function (scope, element, attrs, ngModel) {
			ngModel.$parsers.push(function (str) {
				return str.toLowerCase();
			});
		}
	}
}

angular
	.module('app')
	.directive('changeCase', changeCase);
```

Now, regardless of what case we actually type in inside our input, it'll be saved into the model in lowercase.

### $formatters

Formatters are the opposite of parsers - they format the data from the model into the view. Let's change all of our model values to be displayed in uppercase in our view.

```js
function changeCase() {
	return {
		restrict: 'A',
		require: 'ngModel',
		link: function (scope, element, attrs, ngModel) {
			ngModel.$formatters.push(function (str) {
				return str.toUpperCase();
			});
		}
	}
}

angular
	.module('app')
	.directive('changeCase', changeCase);
```

This will take our model values and display them in uppercase in our field. Combine this with our previous `$parser`, and all our of actual model values will be in lowercase, but displayed in uppercase in our view.

## Using our parsers/formatters

You might have noticed that our directive is restricted to being used as an attribute. This means that we can apply it to any input that we have `ng-model` attached on.

```html
<input ng-model="ctrl.username" change-case />
```

By using `change-case`, we add our parsers/formatters to that specific input!