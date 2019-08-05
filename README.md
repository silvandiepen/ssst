# ssst
### Sass & Scss Seatbelt Tests

When everything is ok, I'll be silent...



### Installation

```npm install ssst```

### Run

Create a test file in for instance `test/index.scss`. In this file you can write the tests. Include your library or function file(s). 


### Example

#### functions.scss

```scss
// A sample function to add number
// Sample use: sampleAddFunction(1 2 5); returns 8

@function sampleAddFunction($args){
  $value: 0;
  @for $arg in $args{
    @if type-of($arg) == 'number'{
      $value: $value + $arg;
    }
    @else{
      @warn '#{$arg} is not a number';
    }
  }
  @return $value;
}
```

#### test/index.scss

```scss
// Import your functions file
@import '../functions.scss';

// Import the Ssst suite.
@import '../node_modules/ssst/index.scss';

// Write your tests.
// You create a test by defining a name, description and include the tests in the @content.
// For instance a 'is-equal' which checks two values, your function and the value it should return. 
// If your function returns the same value as your example, Ssst won't do anything. If not, Ssst will let you know whats wrong and pass a warning.

@include test(
	'sampleAddFunction() function',
	'Returns the number'
) {
	@include is-equal(sampleAddFunction(1 2 5), 8);
}  
```

#### package.json

Add tests to your package.json, in order to be able to run your tests. You could also add these to your 
prepublish or precommit. The test will fail if there is something wrong and will prevent you from publishing or committing your code. `npm run ssst`

```json
...
 "scripts":{
  ...
  "ssst": "sass test/index.scss test/dist/index.css" // Use either sass or node-sass 
  ...
 }
...
```

#### .gitignore

Don't forget to igore your dists, you won't need them in your package.

```
test/dist
```


## Tests

There are different tests available for now. Any other tests can be requested (please create an issue or a PR). 

### 'is-equal'

Test if if a function returns a certain value

```
@include test(
	'sampleAddFunction() function',
	'Returns the number'
) {
	@include is-equal(sampleAddFunction(1 2 5), 8);
}  
```

### 'is-not-equal'

Test if if a function doesn't returns a certain value. Giving back a false can be an issue, thats why we check if the value is Not the same. 

```
@include test(
	'is-number() function',
	'Returns that it\'s not a number'
) {
	@include is-not-equal(is-number('test'), true);
}  
```

### 'is'

Is has an array of different types which can be checked. 

```
@include test(
	'Another function()',
	'Check if the result of myFunction is an angle'
) {
	@include is('angle', myFunction(342)); // This will test of the result of myFunction will be an angle.
} 

```

Other options for is:

| Option          | Description                    | Example types                                                  | pass   | fail  |
| --------------- | ------------------------------ | -------------------------------------------------------------- | ------ | ----- |
| number          | Is the result a number?        | any number                                                     | 343    | a4343 |
| time            | Is the result a time?          | ms, s                                                          | 123s   | 123   |
| duration        | Is the result a duration?      | ms, s                                                          | 123s   | 123   |
| angle           | Is the result an angle?        | deg, rad, grad or turn                                         | 123deg | 123px |
| frequency       | Is the result a frequency?     | Hz or kHz                                                      |        |       |
| int, integer    | Is the result an integer?      | A round number                                                 | 232    | 10.35 |
| relative length | Is the result a frequency?     | em, ex, ch, rem, vw, vh, vmin or vmax                          | 10vh   | 10px  |
| absolute length | Is the result a frequency?     | cm, mm, in, px, pt, or pc                                      | 10px   | 10vh  |
| length          | Is the result a frequency?     | em, ex, ch, rem, vw, vh, vmin, vmax, cm, mm, in, px, pt, or pc | 10px   | 10deg |
| resolution      | Is the result a frequency?     | dpi, dpcm, dppx                                                | 10dpi  | 10px  |
| position        | Is the given value a position? | top, right, bottom, left, or center                            | top    | above |






### 'exist'

Check whether your function, mixin or variable exists. Usually you know this, but because of a import fest, you could check if the imports are done right. 

```
// For a function

@include test(
	'Check if function exists',
	'Does my Rem() function exist?'
) {
	@include exist('function','rem'); // define the type and the name of type you want to search for
} 

// For a Mixin

@include test(
	'Check if function exists',
	'Does my borders() mixins exist?'
) {
	@include exist('mixin','borders'); // define the type and the name of type you want to search for
} 

// For a Variable

@include test(
	'Check if my variable exists',
	'Does my $base variable exist?'
) {
	@include exist('variable','base'); // define the type and the name of type you want to search for
} 

```

| Option   | Description              | Example types                                                        |
| -------- | ------------------------ | -------------------------------------------------------------------- |
| variable | Does the variable exist? | Name of a variable; `$testVariable` becomes `testVariable` to check  |
| function | Does the function exist? | Name of a variable; `@function testFunc` becomes `testFunc` to check |
| mixin    | Does the mixin exist?    | Name of a mixin; `@mixin testMixin` becomes `testMixin` to check     |

