# Module 1: Rendering logic I

## Built in declarations and inheritence

To improve the DX (developer experience), some CSS properties are inherited by
an elements children and grandchildren, like `color`, others do not, like
`border`.

`a` tags do not inherit `color` since they have user-agent stylesheet styles.

## Cardinality

CSS shares some mechanics with print, like pages are made up of vertical blocks
(block elements, top to bottom) and horizontal words (inline elements, left to
right).

## Logical properties 🤯

No IE support.

Since document flow directions for block and inline elements can change based on
the language, CSS defines logical equivalents of `top`, `right`, `bottom` and
`left`. For example, the built in styles for `p` in chrome:

```
p {
  display: block;
  margin-block-start: 1em;
  margin-block-end: 1em;
  margin-inline-start: 0px;
  margin-inline-end: 0px;
}

/* The same thing, but the absolute values for top-down left-right languages like English */
p {
  display: block;
  margin-top: 1em;
  margin-bottom: 1em;
  margin-left: 0px;
  margin-right: 0px;
}
```

Using the logical property will retain the 1em space no matter what
`writing-mode`, `direction` and `text-orientation` is.

Resources:
- [MDN: CSS Logical Properties](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Logical_Properties)
- [MDN: `writing-mode`](https://devdocs.io/css/writing-mode)

## The Box Model 😨

Consider how the `box-sizing` property changes the calculated size of the
following:

```
<style>
  section {
    width: 500px;
  }
  .box {
    width: 100%;
    padding: 10px;
    margin: 10px;
    border: 5px solid;
  }
</style>
<section>
  <div class="box"></div>
</section>
```

**`box-sizing: content-box` (default)**

Width would be 500 + (2x10) + (2x10) + (2x5) = 550px!

**`box-sizing: border-box` (default)**

Width would be 500px! This is because we generally/intuitively think of the
perimeter of boxes to be defined by their border.


### Border

If we don't specify a border color, it'll use the font's color by default!

I can combine this knowledge with the fact that children and grandchildren will
inherit the `color` property, therefore only having to specify the `border-size`
and `border-style` rule once over all elements I want to have a border.

To be ✨ explicit ✨ about this inheritence you can use `currentColor`:

```
.parent {
  color: hotpink;
  border: 1px solid currentColor;
}
```

💡 `outline` is like border, but does not affect the layout, it also doesn't
have a radius, but has a `outline-offset` property to create a gap between the
border and the outline.

⚠️  Never apply `outline: none` as it will break accessibility for keyboard-only
users.


## Flow Layout

**Replaced elements**

Apparently `img`, `video`, and `canvas` are "replaced elements" in that they
embed a foreign object into HTML doc. This is new to me!

These elements are technically inline elements however you can change their
width or height unlike other inline elements.

Another exception is `button` which isn't a replaced element but they function
in the same way.

Layout methods in CSS:
- Flow layout: the default with display modes `block`, `inline` and
  `inline-block`
- Flexible box layout "Flexbox"
- Grid layout: "CSS Grid"


**Inline magic space**

Example 1:

Inline elements appear to have magic space if you put an `img` in a `div` then
inspect their size you will see the div is wider as expected taking up the
entire width of it's "block", but is surprisingly also slightly taller.

This is because as an inline element, the browser applies some space after `img`
so that it's not right up against the next inline element in the stack.

This can be fixed with setting the img to `display: block` or `line-height: 0`.

Example 2:

If you have images next to each other but have whitespace between them in your
HTML, the browser preserves the whitespace thinking it's intentional. You can
remove it by taking out the whitespace.

## Width algorithms

✨ Using % widths ✨ will always take up a percentage of the parent's content
space even if it causes overflow, whereas `auto` will only fill the parents
content space not causing overflow.

By default, block elements have _dynamic sizing_, they're context-aware.

## Height algorithms

Percentage-based heights rarely work. When we set a percentage-based height or
min-height, the percentage is based on that parent height, which by default is
trying to be as small as possible to contain it's children, so the
percentage-based height is then ignored.

This isn't fixed by flexbox or css grid, rather it's to do with height itself.

The solution is to put `height: 100%` on all elements before the wrapper, then
`min-height: 100%` on the wrapper element, which will then fill the remainder of
the space in the viewport.

> Even after writing CSS for 15 years, I still get tripped up by this from time
> to time. height tends to look "down" the tree, to determine its size based on
> the natural size of its contents, while width tends to look "up" the tree,
> basing its size on the space made available by the parent.

💡 The `vh` unit doesn't quite work:

When you scroll on a mobile device, the address bar and footer controls slide
away, yielding their space to the content. This means that scrolling on a mobile
device changes the viewport height.

To avoid flickering UI issues, browsers like iOS Safari and Chrome Android will
set vh equal to the maximum viewport height, after scrolling. This means that
when the page first loads, 100vh will actually be quite a bit taller than the
viewable area.

In general, I recommend using the html/body height: 100% method described above.
It produces a better experience in most cases.

## Margin collapse

> Margin is meant to increase the distance between siblings. It is not meant to
> increase the gap between a child and its parent's bounding box; that's what
> padding is for.


There are some conditions that must be satisfied in order for the margin to be
transferred to the parent (and collapsed):

- No other elements in-between (see earlier rule, about the `<br/>`).
- The parent element doesn't have a height set.
- The parent element doesn't have any padding or border along the relevant edge.


