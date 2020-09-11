# CSS Fundamentals

## 3 Pillars of Good HTML and CSS

### Responsive Design

Responsive design is building 1 website that works smoothly on all screen sizes. - Fluid Layouts - Media Queries - Responsive Images - Correct Units - Desktop-first vs mobile-first

### Maintainable and Scalable Code

Maintainable and scalable code is code that can continue to be used by yourself and others the future.

- Clean
- easy-to-read/understand
- Grows and scales easily
- Reusable

We'll cover

- How to organize files
- How to name Classes
- How to structure HTML

### Web Performance

Web Performance is writing efficient and small code so that the website is faster/more responsive and doesn't require as much data to be sent to the client.

- Minimize HTTP requests
- Less code
- Compress code
- use a CSS preprocessor aka Sass
- Less Images
- Compress Images

## How CSS Works Behind the Scenes

### What happens when we load a webpage

- load html
- parse html
  - Load CSS
  - parse CSS
    - Resolve CSS conflicts (cascade)
      - figures out which declaration applies to an element if the same attribute is declared more than once
    - process final CSS values
      - converts abstract units (%, rem, etc.) into concrete units (pixels)
  - Build CSS Object Model (CSSOM)
- Build Document Object Model (DOM)
  - Where decoded html is stored
- DOM and CSSOM combined to create Render tree
- visual formatting model applied to render tree
- Final rendered website is displayed

### How CSS is parsed: Value Processing

Understanding value processing is important because it is how all abstract values are converted to pixels.

- Step 1. Declared value
  - Comes from the author declarations. conflicts are resolved via cascade.
- Step 2. Cascaded value
  - The result of the cascade.
- Step 3. Specified value
  - This is the default value provied by the CSS specification should no value be declared.
- Step 4. Computed value
  - The converted value from relative values to absolute. Done for inheritance.
- Step 5. Used value (only happens during rendering)
  - final calculation value from the rendered layout
- Step 6. Actual value
  - browser and device restrictions (decimals rounded to whole pixels)

Initial value - the default value provided by the CSS specification
rem - relative unit equal to the computed value of the root font-size.

- How relative units are converted to pixels (rem -> px)
  - % for fonts = % \* parent's computed font-size
  - % for lengths = % \* parent's computed **width**
  - em - uses current element parent font-size
    - for fonts = X \* **parent** computed font-size
    - for lengths = X \* **current element** computed font-size
  - rem - uses root font-size
    - for fonts or lengths = X \* root computed font-size
  - vh = % of viewport height
  - vw = % of viewport width

### Inheritance

Every CSS property must have a value.

Is there a cascaded value?

- Yes - use that value
- No
  - can that property be inherited? (specific to each property)
    - Yes - use computed value of parent element
    - No - use initial value (default for that property)

inheritance makes for more maintainable code.
In general properties related to text are inherited.
spacing properties are generally not inherited.
Computed value is what gets inherited, not the declared value.
Inherit keyword can be used to force inheritance on a certain property.
Initial keyword resets a property to it's initial value.

### Converting px to rem: an effective workflow

We should use rem as much as possible in projects because we want an easy way to change all of our sizes in our project just by changing one thing.

- Set root font-size in html selector
  - use a percent to use the browser default font-size

### How CSS renders with the visual formatting model

Visual Formatting Model: an algorithm that calculates boxes and determines the layout of these boxes, for each element in the render tree, in order to determine the final layout of the page.
Considers the following:

- dimensions of boxes
- box type
- positioning scheme
- stacking contexts
- other elements in the render tree
- viewport size, dimensions of images, etc.

#### Box Model

Each element on a web page can be seen as a rectangular box. These boxes have the following properties:

- content area - where text, images, etc resides
- width - the width of the content area
- height - the height of the content area
- padding - the transparent area around the content but still inside the box
- border - goes around the padding and the content
  - outside the width and height UNLESS the box-sizing property is set to border-box
- margin - the space between boxes
- fill area - the area that gets filled with background color and background image. everything except the margin.

Default calculation

- total width = r-border + r-padding + specified width + l-padding + l-border
- total height = t-border + t-padding + specified height + b-padding + b-border

Using box-sizing: border-box

- total width = specified width
- total height = specified height
- padding and border are then subtracted from the specified height and width

#### Box Types: Inline, Block-Level, and Inline-Block

Box type is defined by the display property

Block-level boxes (most elements default) - display: block
also produce block-level boxes - display: flex - display: list-item - display: table

- Elements formatted visually as blocks
- 100% of parent's width
- vertically one after another with line breaks
- box-model applies as shown

Inline-block boxes - display: inline-block

- a mix of block and inline
- occupies only content's space
- no line-breaks
- box-model applies as shown

Inline boxes (opposite of block-level boxes) - display: inline

- content distributed in lines
- occupies only content's space
- no line-breaks
- no heights and widths (cannot be set)
- paddings and margins only horizontal

#### Positioning Schemes

Normal Flow (default) - position: relative

- not floated
- not absolutely positioned
- elements laid out according to their source order

Floats - float: left - float: right

- Element is removed from the normal flow
- text and inline elements will rap around the floated element
- container will not adjust its height to the element
- moves element as far left or right as possible until it touches the edge of the parent element or another float.

Absolute positioning - position: absolute - position: fixed

- element is removed from normal flow
- no impact on surrounding elements
- use _top_, _bottom_, _left_, and _right_ to offset the element from it's relatively positioned container

#### Stacking Contexts

Allows elements to overlap. Does so by creating a stacking context - z-index: 1 -> n - allows for making multiple layers where 1 is the bottom and n is the top

- other properties that created stacking contexts
  - opacity other than 1
  - transform
  - filter
  - etc.
  - these can cause problems with z-index contexts

### CSS Architecture, Components, and BEM

Think, Build, Architect mindset

- **Think** about the layout of your webpage or web app before writing code.
- **Build** your layout in HTML and CSS with a consistent structure for naming classes.
- Create a logical **Architecture** for your CSS with files and folders

**Think**

- Component driven design
  - modular building blocks that make up interfaces
  - held together by the layout of the page
  - re-usable across a project, and between different projects
  - independant, allowing us to use them anywhere on the page
  - similar to Atomic Design (google it)

**Build**

- Block Element Modifier (BEM)
  - **Block**: A standalone component that is meaningful on its own (can be reused)
  - **Element**: a part of a block that has no standalone meaning (needs the context of the block)
  - **Modifier**: a different version of a block or an element
  - class selectors
    - .block {}
    - .block\_\_element {}
    - .block--modifier {}
    - .block\_\_element--modifier {}

**Architect**

- 7-1 Pattern
  - 7 different folders for partial Sass files
    - base/
    - components/
    - layout/
    - pages/
    - themes/
    - abstracts/
    - vendors/
  - 1 main Sass file to import all other files into a compiled stylesheet
