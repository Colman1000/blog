---
title: "Flutter Layout: Row and Container"
date: 2022-06-05T20:07:01+01:00
description: "Discover interesting ways to use the Row & Column widget in your flutter app"
tags: ["flutter", "layout", "row", "column", "android","ios"]
categories: ["Layout", "Flutter"]
draft: false
author: "@Colman1000"
cover:
    image: "row-and-column.png"
    alt: "Row & Column Layout | Cover Image"
    caption: "Flutter Layout: Row and Container"
images: ['row-and-column.png', 'row-column-diagram.png', 'ui-plain.png', 'ui.gif', 'ui-simplified.gif']
keywords: ["flutter", "row", "column", "layout", "android","ios"]
summary: "The first step to being comfortable and confident with either replicating a User Interface or creating a custom widget is mastering Flutter's layout widgets. Let's look at Rows and Columns in flutter "
---

You have most probably heard this phrase: *Almost everything in Flutter is a Widget*. That also applies to layout. From things you can see like *Images* and *Icons* to things you cannot see such as
*Rows*, *Grids*, *Spaces*, *Alignment* etc. 

Consider the User Interface (**UI**) below;

{{< figure align=center src="ui-plain.png" >}}

This can be broken down into smaller and simple widgets which can then be composed back up. This is done by identifying each piece of the UI  and deciding the type of layout widget to use.

The UI above may look complex, but it's just a clever arrangement of a few widgets in either *Rows* or *Columns* as shown;

{{< figure align=center src="ui.gif" >}}

**LEGEND**:

- **Green**: Consists of *Magenta* `ROWS`
- **Magenta**: Consists of *Yellow* `COLUMNS`
- **Yellow** : Consists of *Red* `ROWS`
- **Red** : Consists of *Blue* `COLUMNS` 

See a simplified version below;

{{< figure align=center src="ui-simplified.gif" >}}


As you can see from the above image, It all boils down to arrangement.

You can accomplish lots of complex layouts using only the basic `ROW` and `COLUMN` widgets

## ROW vs COLUMN
The `Row` and `Column` widget are very similar. The only difference is in how they layout their children.
You control how a Row or Column lays out its children using the `mainAxisAlignment` and `crossAxisAlignment` properties.
For a `Row`, the main axis runs *horizontally* and the cross axis runs *vertically*.
For a `Column`, the main axis runs *vertically* and the cross axis runs *horizontally*.

{{< figure align=center src="row-column-diagram.png" >}}

The `MainAxisAlignment` and `CrossAxisAlignment` enums offer a variety of constants for controlling alignment.

#### MainAxisAlignment
The `MainAxisAlignment` is an `enum` that controls how the children should be placed along the main axis in a flex layout.

It has the following options:

* `start`: This option places the children of the widget ( *usually a Row or Column* ) as close to the start of the *main axis* as possible.

* `end`: This option is used to place all the children as close to the end of the *main axis* as possible.

* `spaceBetween`: This option simply places free space evenly between the children.

* `spaceAround`: This option places free space evenly between the children as well as half of that space before and after the first and last child.

* `spaceEvenly`: This option evenly spaces all the children


#### CrossAxisAlignment

This is an `enum` that controls how the children should be placed along the cross axis in a flex layout.

It has the following options:

* `start`: This option places the children of the widget as close to the start of the *cross axis* as possible.

* `end`: This option is used to place all the children as close to the end of the *cross axis* as possible.

* `center`: This option is used to place the children so that their centers align with the middle of the *cross axis*. This is the default cross-axis alignment.

* `stretch`: This option requires the children to fill the cross axis. This causes the constraints passed to the children to be tight in the cross axis.

* `baseline`: This option places the children along the cross axis such that their baselines match.
