# callback-loader

Webpack loader that parse your JS-file, call specified functions and replace their call expressions with the results.

## Installation

`npm install callback-loader`

## Usage

Source file:

``` javascript
var a = multBy2(10);
var b = mult(10, 3);
var c = concat("foo", "bar");
```

Webpack config:

``` javascript
{
    ...
    callbackLoader: {
        multBy2: function(num) {
            return num * 2;
        },
        mult: function(num1, num2) {
            return num1 * num2;
        },
        concat: function(str1, str2) {
            return '"' + str1 + str2 + '"';
        }
    }
}
```

Result:

``` javascript
var a = 20;
var b = 30;
var c = "foobar";
```

Notice that quotes was added in **concat** function.

### Loader parameters

You can choose which functions will be processed in your query:

``` javascript
'callback?mult&multBy2!example.js'
```

Result for this query will be this:

``` javascript
var a = 20;
var b = 30;
var c = concat("foo", "bar");
```

### Different configs

Webpack config:

``` javascript
{
    ...
    callbackLoader: {
        concat: function(str1, str2) {
            return '"' + str1 + str2 + '"';
        }
    },
    anotherConfig: {
        concat: function(str1, str2) {
            return '"' + str1 + str2 + '-version2"';
        }
    }
}
```

Loader query:

``` javascript
'callback?config=anotherConfig!example.js'
```

Result for this query will be this:

``` javascript
var a = multBy2(10);
var b = mult(10, 3);
var c = "foobar-version2";
```

## Restrictions

No expressions in function arguments, sorry. Only raw values.