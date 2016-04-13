# JigSass Utils Color
[![NPM version][npm-image]][npm-url]  [![Dependency Status][daviddm-image]][daviddm-url]   

  > A collection of dynamically generated `color` and `background-color` utility classes.

JigSass Utils Color works in tandem with [JigSass Tools Color](https://txhawks.github.io/jigsass-tools-color)
and allows defining color and background-color classes based on the color palette defined in
[$jigsass-colors](https://txhawks.github.io/jigsass-tools-color/#variable-jigsass-colors) as well as
adjusting them on the fly.

Color utils can be limited to a certain state (i.e., `hover`, `focus`,  etc.)

The syntax for generating a color util is: `$modifier: '<color-name>[(<adjustment-function>,<arg>)][:state]'`

Class names follow the [Emmet](http://docs.emmet.io/cheat-sheet/) abbreviation
syntax, with colons (':') replaced by two dashes (`--`) to follow BEM naming
conventions.

### Available classes

  - `u-c--<color-name>` (example: u-c--brand)
  - `u-c--<color-name>(<adjustment-func>,<arg>)` (example: u-c--brand(tint-2))
  - `u-c--<color-name>:<state>` (example: u-c--brand:focus)
  - `u-c--<color-name>(<adjustment-func>,<arg>):<state>` (example: u-c--brand(shade-20%):focus)


  - `u-bgc--<color-name>`
  - `u-bgc--<color-name>(<adjustment-func>,<arg>)`
  - `u-bgc--<color-name>:<state>`
  - `u-bgc--<color-name>(<adjustment-func>,<arg>):<state>`


## Installation

Using npm:

```sh
npm i -S jigsass-utils-color
```


## Usage
Import JigSass Utils Color into your main scss file near its very end, together with all
other utilities (utilities should always be the last to be imported).

```scss
@import 'path/to/jigsass-utils-color/scss/index';
```

Like all other JigSass Utils, JigSass Utils Color does not automatically generate any CSS
when imported. You would need to explicitly indicate that each individual color
class should actually be generated in each component or object it is used in
(clarification: This will include style declarations inside `.foo` and .`bar`):

```scss
// _c.foo.scss
.foo {
  @include jigsass-util(u-c, $modifier: primary); // <-- color: #09a5d9

  ...
}
```

```scss
// _c.bar.scss
.bar {
  @include jigsass-util(u-c, $modifier: 'black(tint,10%)');  // <-- color: #191919
  @include jigsass-util(u-bgc, $modifier: 'white(shade,30%)', $from: large); // <-- background-color: #b2b2b2 from large bp and on.

  ...
}
```

Doing so helps us a great deal with portability, as no matter where we import component or object
partials, the correct utility classes will be generated. Think of it as a poor man's dependency
management.

Developer communication is also assisted by including "dependencies" wherever they are required,
as anyone going through a partial, can easily understand how it should be marked up with just a
glance.

As far as bloat goes, just don't worry about it - the actual styles will only be generated once,
at the location in the cascade where the Jigsass Utils Color partial was imported into the main file.


JigSass Utils Color classes are responsive-enabled, using [JigSass MQ](https://txhawks.github.io/jigsass-tools-mq/)
and the breakpoints defined in the [$jigsass-breakpoints](https://txhawks.github.io/jigsass-tools-mq/#variable-jigsass-breakpoints) variable.

Based on the breakpoint arguments passed to `jigsass-util` when including a class,
responsive modifiers are generated according to the following logic:

```scss
.u-c-<modifier>[-[-from-<breakpoint-name>][-until-<breakpoint-name>][-misc-<breakpoint-name>]]
.u-bgc-<modifier>[-[-from-<breakpoint-name>][-until-<breakpoint-name>][-misc-<breakpoint-name>]]
```

So, assuming the `medium`, `large` and `landscape` breakpoints are defined in `$jigsass-breakpoints`
as `600px`, `1024px` and `(orientation: landscape)` respectively,

```scss
@include jigsass-util(u-c, $modifier: secondary);
```
will generate the `.u-c--secondary` class, which is not limited to any media-query.

```scss
@include jigsass-util(u-bgc, $modifier: primary, $until: medium);
```

will generate the `.u-bgc--primary--until-medium` class, which will be in effect at
`(max-width: 37.49em)` and will override styles in the default class until that point.

```scss
@include jigsass-util(u-c, $modifier: primary, $from: large, $misc: landscape);
```

will generate the `.u-c--primary--from-large-when-landscape` class, which will go into
effect at `(min-width: 64em) and (orientation: landscape)` and will override styles in the default
class under these  conditions.


## Documentation

The full documentation was generated using mdcss, and is available at 
[https://txhawks.github.io/jigsass-utils-color/](https://txhawks.github.io/jigsass-utils-color/)

## Contributing

It is a best practice for JigSass modules to *not* automatically generate css on `@import`, but 
rather have the user explicitly enable the generation of specific styles from the module.

Contributions in the form of pull-requests, issues, bug reports, etc. are welcome.
Please feel free to fork, hack or modify JigSass Color in any way you see fit.

#### Writing documentation

Good documentation is crucial for usability, scalability and maintainability. When 
contributing, please do make sure that both its Sass functionality (functions, mixins, 
variables and placeholder selectors), as well as the CSS it generates (selectors, 
concepts, usage exmples, etc.) are well documented.

Jigsass Color uses Jonathan Neal's [mdcss](//github.com/jonathantneal/mdcss).

When styles and documentation comments are not automatically generated by your module on `@import`,
please use the `sgSrc/sg.scss` file to enable their generation.

In addition, any file in `sgSrc/assets` will be available for use in the style guide.



## File structure
```bash
┬ ./
│
├─┬ scss/ 
│ └─ index.scss # The module's importable file.
│
├─┬ sgSrc/      # Style guide sources
│ │
│ ├── sg.scc    # It is a best practice for JigSass 
│ │             # modules to not automatically generate 
│ │             # css and documentation on `@import.` 
│ │             # Please use this file to enable css
│ │             # and documentation comments) generation.
│ │
│ └── assets/   # Files in `sgSrc/assets` will be 
│               # available for use in the style guide
│
└─┬ docs/      # Documention
  │
  └── styleguide/ # Generated documentation 
                  # of the module's CSS
```



**License:** MIT



[npm-image]: https://badge.fury.io/js/jigsass-utils-color.svg
[npm-url]: https://npmjs.org/package/jigsass-utils-color

[daviddm-image]: https://david-dm.org/TxHawks/jigsass-utils-color.svg?theme=shields.io
[daviddm-url]: https://david-dm.org/TxHawks/jigsass-utils-color
