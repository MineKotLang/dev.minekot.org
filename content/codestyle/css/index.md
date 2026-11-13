---
title: "CSS Style Guide"
params:
  menuPre: "<i class=\"devicons devicons-css\"></i> "
weight: 4
---

## 1 Introduction

This document serves as the complete definition of MineKot's coding standards for source code in `Cascading Style Sheets (CSS)`. A CSS source file is described as being in MineKot Style if and only if it adheres to the rules herein.

All projects under the MineKot umbrella are open-source and licensed under the `Apache License 2.0`. This guide enforces precise architectural boundaries and stylistic consistency to support a scalable, maintainable, and community-driven ecosystem.

## 2 Source file basics

### 2.1 File encoding

Source files are encoded in UTF-8 without BOM.

### 2.2 Line endings

Source files end with an `LF (Line Feed)` line ending and insert a single final newline character at the end of the file.

### 2.3 Language standard

Code identifiers, documentation, and user-facing strings are written in American English.

## 3 Formatting

### 3.1 Column limit

CSS code has a column limit of `120 characters`. Except as noted, any line that would exceed this limit is line-wrapped.

### 3.2 Formatter control

Formatter control comments are enabled. Code regions can be excluded from formatting using `/* prettier-ignore */`:

[//]: # (@formatter:off)
`````css
/* Correct */
/* prettier-ignore */
.special-class { margin : 0;padding : 0; }
`````
[//]: # (@formatter:on)

### 3.3 Indentation

The global default indent size is two spaces, and the tab width is two spaces. Continuation indent size is four spaces.

### 3.4 Selector and declaration formatting

Selectors and declarations follow these formatting rules. Property values are aligned:

[//]: # (@formatter:off)
`````css
/* Correct */
.container {
  display         : flex;
  justify-content : center;
  align-items     : center;
}

.button {
  background-color : #ffffff;
  color            : #000000;
  padding          : 10px 20px;
}

/* Incorrect */
.container {display : flex;justify-content : center;align-items : center;}

.button { background-color : #ffffff; color : #000000; padding : 10px 20px; }
`````
[//]: # (@formatter:on)

### 3.4 Single-line blocks

Single-line blocks are allowed for simple rulesets:

[//]: # (@formatter:off)
`````css
/* Correct */
.hidden { display : none; }

/* Incorrect */
.container { display : flex; justify-content : center; align-items : center; }
`````
[//]: # (@formatter:on)

### 3.5 Spacing

Spaces are used after colons in property declarations, after commas in value lists, and before opening braces. No spaces are used before colons, before commas, or after opening braces.

[//]: # (@formatter:off)
`````css
/* Correct */
.selector {
  property    : value;
  font-family : Arial, sans-serif;
}

/* Incorrect */
.selector{property:value;font-family:Arial,sans-serif;}
`````
[//]: # (@formatter:on)

### 3.6 Quotes

Double quotes are enforced for string values (font names, URLs, etc.):

[//]: # (@formatter:off)
`````css
/* Correct */
font-family      : "Open Sans", sans-serif;
background-image : url("image.png");

/* Incorrect */
font-family      : 'Open Sans', sans-serif;
background-image : url('image.png');
`````
[//]: # (@formatter:on)

## 4 Property ordering

### 4.1 Declaration order

Properties are ordered in the following sequence to improve readability:

1. **Positioning:** `position`, `top`, `right`, `bottom`, `left`, `z-index`
2. **Box model:** `display`, `flex`, `grid`, `width`, `height`, `margin`, `padding`, `border`
3. **Typography:** `font`, `font-family`, `font-size`, `font-weight`, `line-height`, `text-align`, `color`
4. **Visual:** `background`, `background-color`, `border`, `box-shadow`, `opacity`
5. **Other:** `transition`, `transform`, `animation`

[//]: # (@formatter:off)
`````css
/* Correct */
.card {
  position         : relative;
  top              : 0;
  left             : 0;
  z-index          : 1;
  display          : flex;
  width            : 100%;
  height           : auto;
  margin           : 20px;
  padding          : 15px;
  border           : 1px solid #cccccc;
  font-family      : "Open Sans", sans-serif;
  font-size        : 16px;
  font-weight      : 400;
  line-height      : 1.5;
  text-align       : left;
  color            : #333333;
  background-color : #ffffff;
  box-shadow       : 0 2px 4px rgba(0, 0, 0, 0.1);
  opacity          : 1;
  transition       : all 0.3s ease;
}
`````
[//]: # (@formatter:on)

### 4.2 Shorthand properties

Shorthand properties are used when possible to reduce file size and improve readability:

[//]: # (@formatter:off)
`````css
  /* Correct */
  margin        : 10px 20px;
  padding       : 5px 10px 15px 20px;
  border        : 1px solid #cccccc;
  background    : #ffffff url("image.png") no-repeat center;

  /* Incorrect */
  margin-top    : 10px;
  margin-right  : 20px;
  margin-bottom : 10px;
  margin-left   : 20px;
`````
[//]: # (@formatter:on)

## 5 Naming conventions

### 5.1 Class names

Class names use `kebab-case` and are descriptive:

[//]: # (@formatter:off)
`````css
/* Correct */
.user-card
.primary-button
.navigation-menu 
/* Incorrect */
.userCard
.primary_button
.UserCard
`````
[//]: # (@formatter:on)

### 5.2 ID selectors

ID selectors are avoided in favor of class selectors. When IDs are necessary, they use `kebab-case`:

[//]: # (@formatter:off)
`````css
/* Correct - Class preferred */
.main-header
.footer-container
  /* Acceptable - ID when necessary */
#app-root
  /* Incorrect */
#mainHeader
`````
[//]: # (@formatter:on)

### 5.3 BEM methodology

BEM (Block Element Modifier) methodology is used for component styling:

[//]: # (@formatter:off)
`````css
/* Correct */
.block { }

.block__element { }

.block--modifier { }

.block__element--modifier { }

/* Example */
.card { }

.card__title { }

.card__content { }

.card--featured { }

.card__title--large { }

/* Incorrect */
.cardTitle
.card-title
.cardTitleLarge
`````
[//]: # (@formatter:on)

### 5.4 Custom properties

CSS custom properties (variables) use `kebab-case` with a prefix:

[//]: # (@formatter:off)
`````css
/* Correct */
:root {
  --color-primary   : #3498db;
  --color-secondary : #2ecc71;
  --spacing-unit    : 8px;
  --font-size-base  : 16px;
}

/* Incorrect */
:root {
  --colorPrimary : #3498db;
  --primaryColor : #3498db;
}
`````
[//]: # (@formatter:on)

## 6 Color formatting

### 6.1 Hex colors

Hexadecimal color values must be written in lowercase and use the long format:

[//]: # (@formatter:off)
`````css
/* Correct */
  color            : #ffffff;
  background-color : #000000;
  border-color     : #ff0000;

/* Incorrect */
  color            : #FFF;
  background-color : #000;
  border-color     : #F00;
`````
[//]: # (@formatter:on)

### 6.2 RGB and RGBA

RGB and RGBA values are used when transparency is needed:

[//]: # (@formatter:off)
`````css
/* Correct */
  background-color : rgba(255, 255, 255, 0.5);
  box-shadow       : 0 2px 4px rgba(0, 0, 0, 0.1);

/* Incorrect */
  background-color : rgba(255,255,255,0.5);
`````
[//]: # (@formatter:on)

### 6.3 Color variables

Colors are defined as CSS custom properties for consistency:

[//]: # (@formatter:off)
`````css
/* Correct */
:root {
  --color-primary    : #3498db;
  --color-secondary  : #2ecc71;
  --color-text       : #333333;
  --color-background : #ffffff;
}

.button {
  background-color : var(--color-primary);
  color            : var(--color-background);
}

/* Incorrect */
.button {
  background-color : #3498db;
  color            : #ffffff;
}
`````
[//]: # (@formatter:on)

## 7 Selector specificity

### 7.1 Low specificity

Selectors are kept as simple as possible to avoid specificity wars:

[//]: # (@formatter:off)
`````css
/* Correct */
.button { }

.card__title { }

/* Incorrect */
div.container .button { }

.card h2.title { }
`````
[//]: # (@formatter:on)

### 7.2 Avoid !important

The `!important` declaration is strictly forbidden except in specific utility classes:

[//]: # (@formatter:off)
`````css
/* Correct - Utility class */
.hidden { display : none !important; }

/* Incorrect */
.button { color : #ff0000 !important; }
`````
[//]: # (@formatter:on)

### 7.3 Qualifying selectors

Element qualifiers are avoided when class selectors are sufficient:

[//]: # (@formatter:off)
`````css
/* Correct */
.card { }

.button { }

/* Incorrect */
div.card { }

a.button { }
`````
[//]: # (@formatter:on)

## 8 Responsive design

### 8.1 Mobile-first approach

Styles are written mobile-first, with media queries for larger screens:

[//]: # (@formatter:off)
`````css
/* Correct */
.container {
  width   : 100%;
  padding : 10px;
}

@media (min-width : 768px) {
  .container {
    width   : 750px;
    padding : 20px;
  }
}

/* Incorrect - Desktop-first */
.container {
  width   : 1200px;
  padding : 20px;
}

@media (max-width : 767px) {
  .container {
    width   : 100%;
    padding : 10px;
  }
}
`````
[//]: # (@formatter:on)

### 8.2 Relative units

Relative units (`rem`, `em`, `%`, `vw`, `vh`) are preferred over absolute units (`px`):

[//]: # (@formatter:off)
`````css
/* Correct */
  font-size : 1rem;
  margin    : 1em;
  width     : 100%;
  height    : 50vh;

/* Incorrect */
  font-size : 16px;
  margin    : 16px;
  width     : 1200px;
  height    : 500px;
`````
[//]: # (@formatter:on)

### 8.3 Flexbox and Grid

Flexbox and Grid are used for layout instead of floats:

[//]: # (@formatter:off)
`````css
/* Correct */
.container {
  display         : flex;
  justify-content : center;
  align-items     : center;
}

.grid {
  display               : grid;
  grid-template-columns : repeat(3, 1fr);
  gap                   : 20px;
}

/* Incorrect */
.container {
  float : left;
  width : 50%;
}
`````
[//]: # (@formatter:on)

## 9 Architecture and design

### 9.1 Modular decoupling

Stylesheets maintain absolute decoupling between general framework layouts and unique interface implementations. Global overrides remain cleanly isolated from distinct components to prevent structural styling leaks.

### 9.2 Component-based architecture

Styles are organized by component, with each component having its own stylesheet:

[//]: # (@formatter:off)
`````tree
- styles | folder
  - base | folder
    - reset.css | devicons devicons-css
    - typography.css | devicons devicons-css
    - variables.css | devicons devicons-css
  - components | folder
    - button.css | devicons devicons-css
    - card.css | devicons devicons-css
    - navigation.css | devicons devicons-css
  - layouts | folder
    - header.css | devicons devicons-css
    - footer.css | devicons devicons-css
`````
[//]: # (@formatter:on)

### 9.3 Theme compliance

All color choices and systemic branding properties respect the project's selected user interface theme palette configuration. Hardcoded, mismatched color formulas are not allowed.

### 9.4 CSS custom properties

CSS custom properties are used for theming and configuration:

[//]: # (@formatter:off)
`````css
:root {
  /* Colors */
  --color-primary    : #3498db;
  --color-secondary  : #2ecc71;
  --color-text       : #333333;
  --color-background : #ffffff;

  /* Spacing */
  --spacing-xs       : 4px;
  --spacing-sm       : 8px;
  --spacing-md       : 16px;
  --spacing-lg       : 24px;

  /* Typography */
  --font-size-base   : 16px;
  --font-size-lg     : 20px;
  --font-size-xl     : 24px;
}
`````
[//]: # (@formatter:on)

## 10 Performance and optimization

### 10.1 Minification

Stylesheets are minified in production to reduce file size.

### 10.2 Critical CSS

Critical CSS is inlined for above-the-fold content to improve perceived performance.

### 10.3 Avoid expensive selectors

Expensive selectors (universal selectors, descendant selectors with deep nesting) are avoided:

[//]: # (@formatter:off)
`````css
/* Correct */
.button { }

.card__title { }

/* Incorrect */
* { }

.container div ul li a { }
`````
[//]: # (@formatter:on)

### 10.4 Will-change property

The `will-change` property is used sparingly and only for properties that will actually change:

[//]: # (@formatter:off)
`````css
/* Correct */
.modal {
  will-change : opacity;
}

/* Incorrect */
* {
  will-change : transform, opacity;
}
`````
[//]: # (@formatter:on)

## 11 Documentation and comments

### 11.1 Section comments

Major sections are documented with comments:

[//]: # (@formatter:off)
`````css
/* ===================================
   Button Component
   =================================== */

.button {
  /* Styles */
}
`````
[//]: # (@formatter:on)

### 11.2 Inline comments

Complex or non-obvious styles are documented with inline comments:

[//]: # (@formatter:off)
`````css
.container {
  /* Flexbox centering with fallback for older browsers */
  display                 : flex;
  display                 : -webkit-flex;
  justify-content         : center;
  -webkit-justify-content : center;
}
`````
[//]: # (@formatter:on)

### 11.3 Comment formatting

Comments are placed on their own lines. Inline comments are forbidden:

[//]: # (@formatter:off)
`````css
/* Correct */
/* Primary button styles */
.button {
  background-color : var(--color-primary);
}

/* Incorrect */
.button { /* Primary button styles */
  background-color : var(--color-primary);
}
`````
[//]: # (@formatter:on)

## 12 Browser compatibility

### 12.1 Vendor prefixes

Vendor prefixes are included only when necessary for supported browsers. Autoprefixer or similar tools are preferred over manual prefixing.

### 12.2 Feature detection

Feature detection (`@supports`) is used instead of browser detection:

[//]: # (@formatter:off)
`````css
/* Correct */
@supports (display: grid) {
  .container {
    display : grid;
  }
}

/* Incorrect */
/* IE-specific styles */
.container {
  display : flex;
}
`````
[//]: # (@formatter:on)

### 12.3 Progressive enhancement

Progressive enhancement is used to ensure basic functionality works in all browsers, with enhanced features for modern browsers.

## Conclusion

Adherence to this style guide ensures that the MineKot ecosystem remains consistent, readable, and maintainable. By following these standards, contributors help uphold the quality and performance expectations of the project.
