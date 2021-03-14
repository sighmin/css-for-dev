# Module 0: Fundamentals recap

## Selectors

When using a comma to combine selectors, if even one selector is invalid, the
entire rule is ignored by the browser!

## Color

`hsl` and `hsla` both work in IE, however arguments must be separated with
comma's instead of spaces:

New syntax:
```
hsla(200deg 100% 50% 0.7)
```

Old syntax:
```
hsla(200deg, 100%, 50%, 0.7)
```

## Units

Use `rem` for typography as it allows users to change their default font size
and your text will scale appropriately. Zooming in will always work even if you
use pixels, but then the user will have to zoom in/out depending on what site
they are on to maintain a consistent comfortable font size.

## Typography

When using a web font, it's customary to surround it in quotation marks:

```
<link rel="preconnect" href="https://fonts.gstatic.com">
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">

<style>
  body {
    font-family: 'Roboto', sans-serif;
  }
</style>
```

## Debugging in the browser

Holding `shift` clicking a color in devtools, toggles through the different
types of color definitions – nice and easy to find `hsl` of a color.

Firefox has the ability to highlight when you use a property that depends on
another property and that property is not met – try to use Firefox more!





