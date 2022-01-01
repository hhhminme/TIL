# flexbox

**Sample felxbox example**
https://mdn.github.io/learning-area/css/css-layout/flexbox/flexbox0.html

**Specifying what elements to lay out as flexible boxes**

special value of display on the parent element of the elements you want to affect.

```css
section {
  display: flex;
}
```

this caues the section element to become a flex container and its children to become flex items. the result of this should be someting.

then multipe column layout with equal-sized columns, and the columns are all the same height.
this is beacause the default value given to flex items (the children of the flex container) are set up to solve common problems such as this.

the element we've given a display value of lex to is **acting like a block-level element is termsin terms of how it interacts with the rest of the page**
but children are laid out as flex items.

# the flex model

<img src= "https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Flexbox/flex_terms.png"/>

- The main axis is the axis running in the direction the flex items are laid out in (for example, as rows across the page, or columns down the page.) The start and end of this axis are called the main start and main end
- The cross axis is the axis running perpendicular to the direction the flex items are laid out in. The start and end of this axis are called the cross start and cross end.
- The parent element that has display: flex set on it (the section in our example) is called the flex container.
- The items laid out as flexible boxes inside the flex container are called flex items (the article elements in our example).
  # Columns or rows?
  ```css
  flex-direction: column;
  ```
  flex-direction that specifies which direction the main axis runs.
  By default this is set to row.(left to right, in the case of an English browser)

lay out flex utems in a reverse direction using the row-reverse and clumn-reverse value

# Wrapping

https://mdn.github.io/learning-area/css/css-layout/flexbox/flexbox-wrap0.html

One issue that arises when you have a fixed width or height in your layout is that eventually your flexbox children will overflow their container, breaking the layout.

Here we see that the children are indeed breaking out of their container. One way in which you can fix this is to add the following declaration to your section rule:

```css
section {
  flex-wrap: wrap;
}

article {
  flex: 200px;
}
```

We now have multiple rows. Each row has as many flexbox children fitted into it as is sensible.

the flex:200px declaration set on the articles means that each will be at least200px wide.

# flex-flow shorthand

```css
section {
  flex-direction: row;
  flex-wrap: wrap;
}
/* then */
section {
  flex-flow: row wrap;
}
```

# Flexible sizing of lex items

You can also specify a minimum size value within the flex value. Try updating your existing article rules like so:

```css
article {
  flex: 1 200px;
}

article:nth-of-type(3) {
  flex: 2 200px;
}
```

"Each flex item will first be given 200px of the available space. After that, the rest of the available space will be shared according to the proportion units."

# flex:shorthand vs longhand

flex is a shorthand property that can specify up to three different values

- The unitless proportion value we discussed above. This can be specified separately using the flex-grow longhand property.
- A second unitless proportion value, flex-shrink, which comes into play when the flex items are overflowing their container. This value specifies how much an item will shrink in order to prevent overflow. This is quite an advanced flexbox feature and we won't be covering it any further in this article.
- The minimum size value we discussed above. This can be specified separately using the flex-basis longhand value

**We'd advise against using the longhand flex properties unless you really have to (for example, to override something previously set). They lead to a lot of extra code being written, and they can be somewhat confusing.**

# Horizontal and vertical alignment

https://mdn.github.io/learning-area/css/css-layout/flexbox/flex-align0.html

```css
div {
  display: flex;
  align-items: center;
  justify-content: space-around;
}
```

Refresh the page and you'll see that the buttons are now nicely centered horizontally and vertically. We've done this via two new properties.

align-items controls where the flex items sit on the cross axis.

- The center value that we used in our above code causes the items to maintain their intrinsic dimensions, but be centered along the cross axis. This is why our current example's buttons are centered vertically.

justify-content controls where the flex items sit on the main axis.

- The value we've used above, space-around, is useful — it distributes all the items evenly along the main axis with a bit of s**pace left at either end.**
- There is another value, **space-between**, which is very similar to space-around except that **it doesn't leave any space at either end.**
