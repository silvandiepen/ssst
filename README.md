# ssst
### Sass & Scss Seatbelt Tests

When everything is ok, I'll be silent...



### Installation

```npm install ssst````

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


