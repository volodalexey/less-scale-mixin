# LessJS mixin to scale values

Initial information [CSS locks](https://fvsch.com/code/css-locks/) or [Flexible typography with css locks](http://blog.typekit.com/2016/08/17/flexible-typography-with-css-locks/)

Re-implementation of [PostCSS plugin to scale values](https://github.com/bramstein/postcss-scale) in [{less}](http://lesscss.org/)

Input:

```less
h1 {
  font-size: 1.5rem;
}

@media (min-width: 20rem) {
  h1 {
    .scale(font-size, 20rem, 60rem, 1.5rem, 2.5rem, 100vw);
  }
}

@media (min-width: 60rem) {
  h1 {
    font-size: 2.5rem;
  }
}
```

Output:

```css
h1 {
  font-size: 1.5rem;
}

@media (min-width: 20rem) {
  h1 {
    font-size: calc( ((2.5 - 1.5) * ((100vw) - 20rem) / (60 - 20)) + 1.5rem );
  }
}

@media (min-width: 60rem) {
  h1 {
    font-size: 2.5rem;
  }
}
```

The `scale` mixin takes six values:
  * property name
  * min base (defines the lower limit of the base range)
  * max base (defines the upper limit of the base range)
  * min target (defines the lower limit of the target range)
  * max target (defines the upper limit of the target range)
  * input (expression)
  
 ```less
.scale(@property, @minBase, @maxBase, @minTarget, @maxTarget, @expression) {
  @lessMinBase: unit(@minBase);
  @lessMaxBase: unit(@maxBase);
  @lessMinTarget: unit(@minTarget);
  @lessMaxTarget: unit(@maxTarget);

  @{property}: ~"calc( ((@{lessMaxTarget} - @{lessMinTarget}) * ((@{expression}) - @{minBase}) / (@{lessMaxBase} - @{lessMinBase})) + @{minTarget} )"
}
 ```