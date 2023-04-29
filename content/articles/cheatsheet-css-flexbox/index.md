---
title: "Cheatsheet CSS Flexbox"
description: "Flexbox is a CSS layout module that provides an efficient and flexible way to align and distribute space among items in a container."
date: 2023-04-29T22:04:57+01:00
draft: false
tags: ["cheatsheet", "css" ,"flexbox"]
categories: ["CSS", "Flexbox"]
draft: false
author: "@AI"
cover:
    image: "css-flexbox.png"
    alt: "CSS Flexbox | Cover Image"
    caption: "CSS Flexbox"
images: ['css-flexbox.png']
keywords: ["cheatsheet", "css" ,"flexbox"]
summary: "Flexbox is a CSS layout module that provides an efficient and flexible way to align and distribute space among items in a container."
---

### TL;DR;

| Property          | Description                                                                         | Values                                                                                        |
|-------------------|-------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| `display`         | Specifies that an element should be a flex container                                | `flex`, `inline-flex`                                                                         |
| `flex-direction`  | Specifies the direction of the flex container                                       | `row` (default), `row-reverse`, `column`, `column-reverse`                                    |
| `flex-wrap`       | Specifies whether flex items should wrap if they exceed the container's width       | `nowrap` (default), `wrap`, `wrap-reverse`                                                    |
| `justify-content` | Aligns flex items along the main axis of the flex container                         | `flex-start` (default), `flex-end`, `center`, `space-between`, `space-around`, `space-evenly` |
| `align-items`     | Aligns flex items along the cross axis of the flex container                        | `stretch` (default), `flex-start`, `flex-end`, `center`, `baseline`                           |
| `align-content`   | Aligns rows or columns of the flex container when there is extra space available    | `stretch` (default), `flex-start`, `flex-end`, `center`, `space-between`, `space-around`      |
| `flex-basis`      | Sets the initial size of a flex item along the main axis of the flex container      | `auto` (default), `<length>`, `<percentage>`, `content`                                       |
| `flex-grow`       | Specifies how much a flex item should grow if there is extra space available        | `0` (default), any positive number                                                            |
| `flex-shrink`     | Specifies how much a flex item should shrink if there is not enough space available | `1` (default), any positive number                                                            |
| `flex`            | Shorthand for setting `flex-grow`, `flex-shrink`, and `flex-basis` properties       | `<flex-grow> <flex-shrink> <flex-basis>`                                                      | 


### What is FlexBox

Flexbox (short for "Flexible Box Layout") is a CSS layout module that provides an efficient and flexible way to align and distribute space among items in a container, even when their sizes are unknown or dynamic. It allows you to create complex and responsive layouts with minimal code, without having to resort to floats or positioning.

With flexbox, you can specify how the container should distribute space along a single row or column (the "main axis") and how the items should align along the perpendicular axis (the "cross axis"). You can also control how much each item should grow or shrink relative to each other if there is extra or insufficient space available.

Flexbox has become increasingly popular and widely supported in modern browsers, and is often used in conjunction with other CSS layout modules such as Grid and Box Alignment to create even more sophisticated layouts.

#### Display
The `display` property is used to specify that an element should be a flex container.

```css
.container {
  display: flex;
}
```

#### Flex Direction
The `flex-direction` property is used to specify the direction of the flex container. The default value is `row`.

```css
.container {
  flex-direction: row; /* row (default), row-reverse, column, column-reverse */
}
```

#### Flex Wrap
The `flex-wrap` property is used to specify whether the flex items should wrap if they exceed the container's width. The default value is `nowrap`.

```css
.container {
  flex-wrap: nowrap; /* nowrap (default), wrap, wrap-reverse */
}
```

#### Justify Content
The `justify-content` property is used to align the flex items along the main axis of the flex container.

```css
.container {
  justify-content: flex-start; /* flex-start (default), flex-end, center, space-between, space-around, space-evenly */
}
```

#### Align Items
The `align-items` property is used to align the flex items along the cross axis of the flex container.

```css
.container {
  align-items: stretch; /* stretch (default), flex-start, flex-end, center, baseline */
}
```

#### Align Content
The `align-content` property is used to align the rows or columns of the flex container when there is extra space available.

```css
.container {
  align-content: stretch; /* stretch (default), flex-start, flex-end, center, space-between, space-around */
}
```

#### Flex Basis
The `flex-basis` property is used to set the initial size of a flex item along the main axis of the flex container.

```css
.item {
  flex-basis: auto; /* auto (default), width, height, content */
}
```

#### Flex Grow
The `flex-grow` property is used to specify how much a flex item should grow if there is extra space available.

```css
.item {
  flex-grow: 1; /* 0 (default), any positive number */
}
```

#### Flex Shrink
The `flex-shrink` property is used to specify how much a flex item should shrink if there is not enough space available.

```css
.item {
  flex-shrink: 1; /* 1 (default), any positive number */
}
```

#### Flex
The `flex` shorthand property is used to set the `flex-grow`, `flex-shrink`, and `flex-basis` properties in one declaration.

```css
.item {
  flex: 1 1 auto; /* flex-grow flex-shrink flex-basis */
}
```

I hope this cheat sheet helps you understand the basics of CSS flexbox!