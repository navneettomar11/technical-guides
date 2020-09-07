# Selector
- Id Selector - #
- Class Selector - .
- Attribute Selector - [attr=value]
-

# Absolute versus Relative Units
Pixels are a type of length unit, which is what tells the browser how to size or space an item. In addition to px, CSS has a number of different length unit options that you can use.
The two main types of length units are absolute and relative. Absolute unit tie to physical unit of length. For example, `in` and `mm` refer to inches and millimeters, respectievely. Absolute length units approximate the actual measurement on a screen, but there are some differences depending on screen's resolution.

Relative units such as `em` or `rem` are relative to another length value. For example, em is based on the size of an element's font. If you use it to set the font-size property itself; it's relative to the parent's `font-size`.


# Padding
An element's padding controls the amount of space between the element's content and its broder.

These four values work like a clock: top, right, bottom, left, and will produce the exact same result as using the side-specific padding instructions.

> padding: 10px 20px 10px 20px;

# Margin
An element's margin control the amount of space between an element's border and surrounding elements.
If you set an element's margin to a negative value, the element will grow larger.

# CSS inheritance
Every HTML page has a `body` element. We can prove that the body element exists here by giving it a `background-color` of back.

precedence -
 !Important Attribute --> Inline Style --> ID --> Class

# CSS Variable
CSS variables are powerful way to change many CSS style properties at once by changing only one value.

```CSS
.penguin {

    /* Only change code below this line */
    --penguin-skin: gray;
    --penguin-belly: white;
    --penguin-beak: orange;
    /* Only change code above this line */
     position: relative;
    margin: auto;
    display: block;
    margin-top: 5%;
    width: 300px;
    height: 300px;
}
.penguin-top {
    top: 10%;
    left: 25%;
    background: var(--penguin-skin, gray);
    width: 50%;
    height: 45%;
    border-radius: 70% 70% 60% 60%;
  }
```
To create a CSS variable, you just need to give it a name with two hyphens in front of it and assign it a value like this:
 > --penguin-skin: gray;

After you create your variable, you can assign its value to other CSS properties by referencing the name you gave it.

> background: var(--penguin-skin);

This will change the background of whatever element you are targeting to gray because that is the value of the --penguin-skin variable. Note that styles will not be applied unless the variable names are an exact match.

## Inherit CSS Variables
When you create a variable, it is available for you to use inside the selector in which you create it. It also is available in any of that selector's descendants. This happends because CSS varaibles are inherited , just like ordinary properties.
To make use of inheritance, CSS variables are often defined in the `:root` element.
`:root` is a pseudo-class selector that matches the root element of the document, usually the `html` element. By creating your variables in `:root`, they will be available globally and can be accessed from any other selector in the stylesheet.

## Change a variable for a specific area
When you create your variable in `:root` they will set the value of that variable for the whole page.

You can then over-write these variables by setting them again within a specific elememt.

## Use a media query to change a variable
CSS Variables can simplify the way you use media queries.

For instance, when your screen is smaller or larger than your media query break point, you can change the value of a variable, and it will apply its style wherever it is used.


# Improve Compatibility with Browser Fallbacks
When working with CSS you will likely run into browser compatibility issues at some point. This is why it's important to provide browser fallbacks to avoid potential problems.

When your browser parses the CSS of a webpage, it ignores any properties that it doesn't recognize or support. For example, if you use a CSS variable to assign a background color on a site, Internet Explorer will ignore the background color because it does not support CSS variables. In that case, the browser will use whatever value it has for that property. If it can't find any other value set for that property, it will revert to the default value, which is typically not ideal.

This means that if you do want to provide a browser fallback, it's as easy as providing another more widely supported value immediately before your declaration. That way an older browser will have something to fall back on, while a newer browser will just interpret whatever declaration comes later in the cascade.


# Applied Visual Design
Visual Design in web development is a broad topic. It combines typography, color theory, graphics, animation and page layout to help deliver a site's message. The definition of good design is a well-discussed subject, with many books on the theme.

At a basic level, most web content provides a user with information. The visual design of the page can influence its presentation and a user's experience. In web development, HTML gives structure and semantics to a page's content, and CSS controls the layout and appearance of it.

## box-shadow
The box-shadow property applies one or more shadows to an element.

The box-shadow property takes values for
- `offset-x`(how far to push the shadow horizontally from the element)
- `offset-y` (how far to push the shadow vertically from the element)
- `blur-radius`
- `spread-radius` and
- `color` in that order

The `blur-radius` and `spread-radius` values are optional.

Multiple box-shadows can be created by using commas to separate properties of each `box-shadow` element.

Here's an example of the CSS to create multiple shadows with some blur, at mostly-transparent black colors.
 > box-shadow: 0 10px 20px rgba(0,0,0,0.19), 0 6px 6px rgba(0,0,0,0.23);


 # line-height
 line-height property to change the height of each line in a block of text. 

 # Learn about Complementary Colors
 Color theory and its impact on design is a deep topic and only the basics are covered in the following challenges. On a website, color can draw attention to content, evoke emotions, or create visual harmony. Using different combinations of colors can really change the look of a website, and a lot of thought can go into picking a color palette that works with your content.

 The color wheel is a useful tool to visualize how colors relate to each other - it's a circle where similar hues are neighbors and different hues are farther apart. When two colors are opposite each other on the wheel, they are called  complementary colors. They have the characteristics that if they are combined they "cancel" each other out and create a gray color. However, when placed side-by-side, these colors appear more vibrant and produce a strong visual constrast.

Some examples of complementary colors with their hex codes are:
```css
red (#FF0000) and cyan (#00FFFF)
green (#00FF00) and magenta (#FF00FF)
blue (#0000FF) and yellow (#FFFF00)
```
This is different than the outdated RYB color model that many of us were taught in school, which has different primary and complementary colors. Modern color theory uses the additive RGB model (like on a computer screen) and the subtractive CMY(K) model (like in printing). Read here for more information on this complex subject.

There are many color picking tools available online that have an option to find the complement of a color.

# Tertiary Colors
Computer monitors and device screens create different colors by combining amounts of red, green, and blue light. This is known as the RGB additive color model in modern color theory. Red (R), green (G), and blue (B) are called primary colors. Mixing two primary colors creates the secondary colors cyan (G + B), magenta (R + B) and yellow (R + G). You saw these colors in the Complementary Colors challenge. These secondary colors happen to be the complement to the primary color not used in their creation, and are opposite to that primary color on the color wheel. For example, magenta is made with red and blue, and is the complement to green.

Tertiary colors are the result of combining a primary color with one of its secondary color neighbors. For example, within the RGB color model, red(primary) and yellow(secondary) make orange tertiary. This add six more colors to a simple color wheel for a total of twelve.

The are various methods of selecting different colors that result in a harmonious combination in design. One example that can use tertiary color is called the split-complementary color scheme. This scheme start with a base color, then pairs it with the two colors that are adjacent to its complement. The three colors provide strong visual contrast in design, but are more subtle than using two complementary colors.

# Hue of a Color
Color have several characteristics including hue, saturation, and lightness. CSS3 introduce `hsl()` property as an alternative way to pick a color by directly stating these characteristics.

**Hue** is what people generally think of as `color`. If you picture a spectrum of colors starting of colors starting with red on the left, moving through green in the middle, and blue on right, the hue is where a color fits along this line. In `hsl()`. hue uses a color wheel concept instead of the  spectrum, where the angle of the color on the circle is given as value between 0 and 360.

**Saturation** is the amount gray in color. A fully saturated color has no gray in it, and minimally saturated color is almost completely gray. This is given as a percentage with 100% being fully saturated.

**Lightness** is the amount of white or black in a color. A percentage is given ranging from 0%(black) to 100% white, where 50% is the normal color.

Here are a few examples of using `hsl()` with fully-saturated, normal lightness color:

|Color|HSL|
|-----|---|
|red|hsl(0, 100%, 50%)|
|yellow|hsl(60, 100%, 50%)|
|green|hsl(120, 100%, 50%)|
|cyan|hsl(180, 100%, 50%)|
|blue|hsl(240, 100%, 50%)|
|magenta|hsl(300, 100%, 50%)|

## Adjust the tone of a color
The `hsl()` option in CSS also makes it easy to adjust the tone of a color. Mixing white with a pure hue creates a tint of that color and adding black will make a shade. Alternatively, a tone is produced by adding gray or by both tinting and shading. Recall that the 's' and 'l' of `hsl()` stand for saturation and lightness, respectively. The saturation percent change the amount of gray and the lightness percent determines how much white or black is in the color. This is useful when you have a base hue you like, but need different variations of it.

# Create a Gradual CSS Linear Gradient
Applying a color on HTML elements is not limited to one flat hue. CSS provides the ability to use color transitions, otherwise known as gradients on elements. This is accessed through the `background` property's `linear gradient()` function. Here is the general syntax:
```css
background: linear-gradient(gradient_direction, color 1, color 2, color 3, ...);
```

The first argument specifies the direction from which color transition starts - it can be stated as a degree, where 90deg makes a vertical gradient and 45deg angled like backslash.

The `repeating-linear-gradient()` function is very similar to `linear-gradient()` with the major difference that it repeats the specified gradient pattern. `repeating-linear-gradient()` accepts a variety of values, but for simplicity, you'll work with an angle value and color stop values in this challenge.

# Transform property
To change the scale of an element, CSS has the `transform` property, along with its `scale()` function. The following code examples doubles the size of all paragraph element on the page:
```css
p {
  transform: scale(2);
}
```

The next function of the `transform` property is `skewX()` which skews the selected element along its X(horizontal) axis by a given degree.
```css
p {
  transform: skewX(32deg);
}
```
Given that the `skewX()` functions skews the selected element along the X-axis by a given degree, it is no surprise that the skewY() property skews an element along the Y(vertical) axis.

# @keyframes and animation properties
TO animate an element, you need to know about the animation properties and the `@keyframes` rule. The animation properties control how the animation should behave and the `@keyframes` rule control what happens during that animation. There are eight animation properties in total. This challenge will keep it simple and cover the two most important one first:
- `animation-name`: set the name of the animation, which is later used by `@keyframes` to tell CSS which rule go with which animations.
- `animation-duration`: set the length of time for the animation.
- `@keyframes`: is how to specify exactly what happens within the animation over the duration. This is done by giving CSS properties for specific "frames" during the animation, with percentanges ranging from 0% to 100%. If you compare this to a movie, the CSS properties for 0% is how the element displays in the opening scene. The CSS properties for 100% is how the element appears at the end, right before the credits roll. Then CSS applies the magic to transition the element over the given duration to act out the scene. Here's an example to illustrate the usage of `@keyframes` and the animation properties:
```css
#anim {
  animation-name: colorful;
  animation-duration: 3s;
}

@keyframes colorful {
  0% {
    background-color: blue;
  }
  100% {
    background-color: yellow;
  }
}
```

# Accessibility
"Accessibility" generally means having web content and a user interface that can be understood navigated and interacted with by a broad audience. This include people with visual, auditory, mobility, or cognitive disabilities.

Websites should be open and accessible to everyone, regardless of a user's abilities or resources. Some users rely on assistive technology such as a screen reader or voicce recognition software. Other users may be able to navigate through site only using a keyboard. Keeping the needs of various users in mind when developing your project can go a long way towards creating as open web.

Here are there general concepts:
1. have well-organized code that uses appropriate markup.
2. ensure text alternatives exists for non-text and visual content.
3. create an easily navigated page that's keyboard-friendly.

Having accessible web content is an ongoing challenge. A great resource for your project going forward is the W3C Consortium's Web Content Accessibilty Guidelines(WCAG). They set the international standard for accessibility and provide a number of criteria you can use to check your work.

## Text Alternative to Image
It's likely that you've seen `alt` attribute on an `img` tag in other challenges. `Alt` text describe the content of the image and provides a text-alternative for it. This helps in case where the image fails to load or can't be seen by a user. It's also used by search engines to understand what an image contains to include it in search result. 

People with visual impairments rely on screen readers to convert web content into an audio interface. They won't get information if it's only presented visually. For images, screen reader scan access the `alt` attribute and read its content to deliver key information.

## Using Headings to show hierarchial relationship of content
Heading (h1 through h6 elements) are workhorse tages that help provide structure and labelling to  your content. Screen readers can be set to read only the headings on a page so the user gets a summary. This means it is important for the heading tags in your markup to have semantic meaning and relate to each other, not be picked merely for their size values.

Semantic meaning means that the tag you use around content indicates the type of information it contains.

## Audio tag
HTML5's `audio` element gives semantic meaning when it wraps sound or audio stream content in your markup. Audio content also needs a text alternative to be accessible to people who are deaf or hard of hearing. This can be done with near by text on the page or link to a transcript.

The `audio` tag supports the `controls` attribute. This show the browser default play, pause, and other controls and supports keyboard functionaility. This is a boolean attribute, meaning it doesn't need a value, it presence on the tag turens the setting on.

```html
<audio id="meowClip" controls>
  <source src="audio/meow.mp3" type="audio/mpeg" />
  <source src="audio/meow.ogg" type="audio/ogg" />
</audio>
```

## Figure Element
HTML5 introduce the `figure` element along with the related `figcaption`. Used together, these item wrap a visual representation (like an image, diagram or chart) along with its caption. This gives a two-fold accessibility boost by both semantically grouping related content, and providing a text alternative that explains the `figure`.

For data visualization like charts, the caption can be used to briefly note the trends or conclusions for users with visual impairments. Another challenge covers how to move a table version of the chart's data off-screen(using CSS) for screen reader users.

## Time with datetime attribute
HTML5 introduce the `time` element along with a `datetime` attribute to standardize times. This is an inline element that can wrap a date or time on a page. A valid format of the date is held by the `datetime` attribute. This is the value accessed by assistive devices. It helps avoid confusion by stating a standarized version of a time, even if it's written in an informal or colloquial manner in the text.

 # Responsive Web Design
 Today, there are many types of devices that can access the web. They range from large desktop computers to small mobile phones. These devices have different screen sizes, resolutond and processing power. Responsive Web design is an approach to designing web content that responds to the constraints of different devices. The page structure and CSS rules should be flexible to accommodate these differences. In general, design the page's CSS to your target auidence. If you expect most of your traffic to be from mobile users, take a `mobile-first` approach. Then add conditional rules for large screen sizes. If your visitors are desktop users, then design for larger screens with conditional rules for smaller sizes. CSS gives you  the tools to write different style rules, then apply them depending on the device displaying the page. 
 
 ## Media Queries
 Media Queries are a new technique introduced in CSS3 that change the presentation of content based on different depending on the device used to access the site.

 Media Queries consists of a media type, and if that media type matches the type of device the document is displayed on, the styles are applied. You can have as many selectors and styles inside your media query as you want.

 Here's example of a media query that returns the content when the device's width is less than or equal to 100px:

```css
@media (max-width: 100px) {/* CSS Rules */}
```

## Make an Image Responsive
Making images responsive with CSS is actually very simple. You just need to add these properties to an image:
```css
img {
  max-width: 100%;
  height: auto;
}
```
## Typography Responsive
Instead of using `em` or `px` to size text, you can use viewport units for responsive typography. Viewport units, like percentage, are relative units, but they are based off different items. Viewport units are relative to the viewport dimensions(width or height) of a device, and percentages are relative to the size of the parent container element.

The four different viewport units are:
- `vw` (viewport width): 10vw would  be 10% of the viewport's width.
- `vh` (viewport height): 3vh would be 3% of the viewport's height.
- `vmin` (viewport minimum): 70vmin would be 70% of th viewport's smaller dimension (height or width).
- `vmax` (viewport maximum): 100vmax would be 100% of the viewport's bigger dimension (height or width).

# Flexbox
A website's User Interface (UI) has two components. First, there are the visual elements, such as colors, font and images. Second, there is the placement or positioning of those elements. In Responsive Web Design, a UI layout must accomodate many different browsers and devices.

CSS3 introducd Flexible Boxes or flexbox to create page layouts for a dynamic UI. It is a layout mode that arranges elements in a predictable way for different screen sizes and browsers. While somewhat new, all popular modern browsers support flexbox.
 
Adding `display:flex` to element turn it into flex container. 

 ## flex-direction
 This makes it possible to align any children of that element into rows or columns. You do this by adding the `flex-direction` property to the parent item and setting it to row or column. Creating a row will align the children horizontally and creating a column will align the children vertically.

 Other options for `flex-direction` are row-reverse and column-reverse.

 ## justify-content
 Sometimes the flex items within a flex container do not fill all the space in the container. It is common to want to tell CSS how to align and space out the flex items a certain way. Fortunately, the `justify-content` property has several option to do this. But first, there is some important terminology to understand before reviewing those options.

![Here is a useful image showing a row to illustrate the concepts below.](https://www.w3.org/TR/css-flexbox-1/images/flex-direction-terms.svg)

Recall that setting a flex container as a row places the flex item side-by-side from left-to-right. A flex-container set a column places the flex items in a vertical stack from top-to-bottom. For each, the direction the flex items are arranged is called **main axis**. For a row this a horizontal line that cuts through each item. And for a column, the main axis is a vertical line through the items.

There are several options for how to space the flex items along the line that is the main axis. One of the most commonly used is `justify-content: center;` which aligns all the flex item to the center inside the flex container. Other options include:
- `flex-start`: align items to the start of the flex container. For a row, this pushes the items to the left of the container. For a column, this pushes the items to the top of the container. This is the default alignment if no `justify-content` is specified.
- `flex-end`: align items to the end of the flex container. For a row, this pushes the items to the right of the container. For a column, this pushes the items to the bottom of the container.
- `space-between`: aligns items to the center of the main axis, with extra space placed between the items. The first and last items are pushed to the very edge of the flex container.For example, in a row the first item is against the left side of the container, the last item is against the right side of the container, then the remaining space is distributed evenly among the other items.
- `space-around`: similar to `space-between` but the first and last items are not locked to the edges of the container, the space is distributed around all the items with a half space on either end of the flex container.
- `space-evenly`:  Distributes space evenly between the flex items with a full space at either end of the flex container.

## align-items
The `align-items` property is similar to `justify-content`. The `justify-content` properly aligned flex items along the main axis. For rows, the main axis is a horizontal line and for columns it is a vertical line.

Flex containers also have a **cross axis** which is the opposite of the main axis. For rows, the cross axis is a vertical line and for columns it is horizontal line.

CSS offers the align-items property to align flex items along the cross axis. For a row, it tells CSS how to push the items in the entire row up or down within the container. And for a column, how to push all the items left or right within the container.

The different values available for align-items include:
- `flex-start`: aligns items to the start of the flex container. For rows, this aligns items to the top of the container. For columns, this aligns items to the left of the container.
- `flex-end`: aligns items to the end of the flex container. For rows, this aligns items to the bottom of the container. For columns, this aligns items to the right of the container.
- `center`: align items to the center. For rows, this vertically aligns items (equal space above and below the items). For columns, this horizontally aligns them (equal space to the left and right of the items).
- `stretch`: stretch the items to fill the flex container. For example, rows items are stretched to fill the flex container top-to-bottom. This is the default value if no align-items value is specified.
- `baseline`: align items to their baselines. Baseline is a text concept, think of it as the line that the letters sit on.

## flex-wrap
CSS flexbox has a feature to split a flex item into multiple rows (or columns). By default, a flex container will fit all flex items together. For example, a row will all be on one line.

However, using the `flex-wrap` property tells CSS to wrap items. This means extra items move into a new row or column. The break point of where the wrapping happends depends on the size of the items and the size of the container.

CSS also has options for the direction of the wrap:

- `nowrap`: this is the default setting, and does not wrap items.
- `wrap`: wraps items from left-to-right if they are in a row, or top-to-bottom if they are in a column.
- `wrap-reverse`: wraps items from right-to-left if they are in a row, or bottom-to-top if they are in a column.

## flex-shrink
The `flex-shrink` property used, it allows an item to shrink if the flex-container is too small. Item shrink when the width of the parent container is smaller than the combined widths of all the flex items within it.

The `flex-shrink` property takes number as value. The higher the number, the more it will shrink compared to the other items in the container. For example, if one item has a `flex-shrink` value of `1` and the other has a `flex-shrink` value of `3` will shrink three times as much as the other.

## flex-grow
The opposite of `flex-shrink` is the `flex-grow` property. Recall that `flex-shrink` controls the size of the items when the contaoner shrinks. The `flex-grow` property controls the size of items when the parent container expands.

Using a similar example from the last challenge, if one item has a `flex-grow` value of `1` and the other has a `flex-grow` value of `3` the one with the value of `3` will grow three times as much as the other.

## flex-basis
The `flex-basis` property specifies the intial size of the item before CSS makes adjustments with `flex-shrink` or `flex-grow`.

The units used by the `flex-basis` property are the same as other size properties (px, em, %, etc). The value `auto` sizes item bases on the content.

> flex shorthand - there is a shorthand available to set several flex properties at once. The `flex-grow`, `flex-shrink` and `flex-basis` properties can all be set together by using the `flex` property.

## order
The `order` property is used to tell CSS the order of how flex items appear in the flex container. By default, items will appear in the same order they come in the source HTML. The property takes number as value and negative numbers can be used.

## align-self
The final property for flex items is `align-self`.The property  allows you to adjust each item's alignment individually, instead of setting them all at once. This is useful since other common adjustment techniques using the CSS properties `float`, `clear` and `vertical-align` do not work on flex items.

`align-self` accepts the same values as `align-items` and will override any value set by the `align-items` property.

# CSS Grid 
CSS Grid helps you easily build complex web designs. It work by turning an HTML element into a grid container with rows and columns for you to place children elements where you want within the grid.

## grid-template-columns
Simply creating a grid element doesn't get you very far. You need to define the structure of the grid as well. To add some columns to the grid, use the `grid-template-columns` property on a grid container as demonstrated below:
```css
.container {
  display: grid;  
  grid-template-column: 50px 50px;
}
```
This will give you grid two columns that are each 50px wide. The number of parameters given the the `grid-template-columns` property indicates the number of columns in the grid, and the value of each parameter indicates the width of each column.

To adjust the rows manually, use the `grid-template-rows` property in the same way you used `grid-template-columns`.

You can use absolute and relative units like `px` and `em` in CSS grid to define the size of rows and columns. You can use these as well:
`fr`:set the column or row to a fraction of the available space.
`auto`: set the column or row to the width or height of its content automatically.
`%`: adjusts the column or row to the percent width of its container.

So far in the grids you have created, the columns have all been tight up against each other. Sometimes you want a gap in between the columns. To add a gap between the columns, use the `grid-column-gap` property like this:
```css
grid-column-gap: 10px;
```
This creates 10px of empty space between all of our columns.

You can add a gap in between the rows of a grid using `grid-row-gap` in the same way that you added a gap in between columns

`grid-gap` is a shorthand property for `grid-row-gap` and `grid-column-gap`.

 If `grid-gap` has one value, it will create a gap between all rows and columns. However, if there are two values, it will use the first one to set the gap between the rows and the second value for the columns.

 The `grid-column` property is the first one for use on the grid items themselves.

 The hypothetical horizontal and vertical lines that create the grid are referred to as lines. These lines are numbered starting with 1 at the top left corner of the grid and move right for columns and down for rows, counting upward.

To control the amount of columns an item will consume, you can use the grid-column property in conjunction with the line numbers you want the item to start and stop at.

```css
grid-column: 1 / 3;
```

This will make the item start at the first vertical line of the grid on the left and span to the 3rd line of the grid, consuming two columns.

You can group cells of your grid together into an area and give the area a custom name. Do this by using `grid-template-areas` on the container like this:

```css
grid-template-areas:
  "header header header"
  "advert content content"
  "footer footer footer";
```

The code above merges the top three cells together into an area named header, the bottom three cells into a footer area, and it makes two areas in the middle row; advert and content. Note: Every word in the code represents a cell and every pair of quotation marks represent a row. In addition to custom labels, you can use a period (.) to designate an empty cell in the grid.

You can place an item in your custom area by referencing the name you gave it. To do this, you use the `grid-area` property on an item like this:

```css
.item1 {
  grid-area: header;
}
```
This lets the grid know that you want the item1 class to go in the area named header. In this case, the item will use the entire top row because that whole row is named as the header area.

 If your grid doesn't have an areas template to reference, you can create an area on the fly for an item to be placed like this:

 ```css
 .item1 { grid-area: 1/1/2/4; }
 ```
 This is using the line numbers you learned about earlier to define where the area for this item will be. The numbers in the example above represent these values:

 > grid-area: horizontal line to start at / vertical line to start at / horizontal line to end at / vertical line to end at;

So the item in the example will consume the rows between lines 1 and 2, and the columns between lines 1 and 4.

When you used grid-template-columns and grid-template-rows to define the structure of a grid, you entered a value for each row or column you created.

Let's say you want a grid with 100 rows of the same height. It isn't very practical to insert 100 values individually. Fortunately, there's a better way - by using the repeat function to specify the number of times you want your column or row to be repeated, followed by a comma and the value you want to repeat.

Here's an example that would create the 100 row grid, each row at 50px tall.

> grid-template-rows: repeat(100, 50px);

You can also repeat multiple values with the repeat function and insert the function amongst other values when defining a grid structure. Here's what that looks like:

> grid-template-columns: repeat(2, 1fr 50px) 20px;

This translates to:

grid-template-columns: 1fr 50px 1fr 50px 20px;

There's another built-in function to use with `grid-template-columns` and `grid-template-rows` called `minmax`. It's used to limit the size of items when the grid container changes size. To do this you need to specify the acceptable size range for your item. Here is an example:

> grid-template-columns: 100px minmax(50px, 200px);

In the code above, `grid-template-columns` is set to create two columns; the first is 100px wide, and the second has the minimum width of 50px and the maximum width of 200px.

The repeat function comes with an option called `auto-fill`. This allows you to automatically insert as many rows or columns of your desired size as possible depending on the size of the container. You can create flexible layouts when combining `auto-fill` with `minmax`, like this:

> repeat(auto-fill, minmax(60px, 1fr));

When the container changes size, this setup keeps inserting 60px columns and stretching them until it can insert another one. Note: If your container can't fit all your items on one row, it will move them down to a new one.

`auto-fit` works almost identically to `auto-fill`. The only difference is that when the container's size exceeds the size of all the items combined, auto-fill keeps inserting empty rows or columns and pushes your items to the side, while auto-fit collapses those empty rows or columns and stretches your items to fit the size of the container.