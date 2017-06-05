# Gulp Tasks

## Run


Gulp Run

``` php
<?php
// simple execution
$this->taskGulpRun()->run();

// run task 'clean' with --silent option
$this->taskGulpRun('clean')
     ->silent()
     ->run();
?>
```

* `noColor()`  adds `--no-color` option to gulp
* `color()`  adds `--color` option to gulp
* `simple()`  adds `--tasks-simple` option to gulp
* `dir($dir)`  Changes working directory of command
* `arg($arg)`  Pass argument to executable. Its value will be automatically escaped.
* `args($args)`  Pass methods parameters as arguments to executable. Argument values
* `rawArg($arg)`  Pass the provided string in its raw (as provided) form as an argument to executable.
* `option($option, $value = null, $separator = null)`  Pass option to executable. Options are prefixed with `--` , value can be provided in second parameter.
* `optionList($option, $value = null, $separator = null)`  Pass multiple options to executable. Value can be a string or array.
