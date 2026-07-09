---
title: "HTML Style Guide"
params:
  menuPre: "<i class=\"devicons devicons-html-5\"></i> "
weight: 3
---

## 1 Introduction

This document serves as the complete definition of MineKot's coding standards for source code in the `HyperText Markup Language (HTML)`. An HTML source file is described as being in MineKot Style if and only if it adheres to the rules herein.

All projects under the MineKot umbrella are open-source and licensed under the `Apache License 2.0`. This guide enforces precise architectural boundaries and stylistic consistency to support a scalable, maintainable, and community-driven ecosystem.

## 2 Source file basics

### 2.1 File encoding

Source files are encoded in UTF-8 without BOM.

### 2.2 Line endings

Source files end with an `LF (Line Feed)` line ending and insert a single final newline character at the end of the file.

### 2.3 Language standard

Code identifiers, documentation, and user-facing strings are written in American English.

## 3 Document structure

### 3.1 Doctype declaration

All HTML documents must begin with the HTML5 doctype declaration:

[//]: # (@formatter:off)
`````html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document Title</title>
</head>
<body>
<!-- Content -->
</body>
</html>
`````
[//]: # (@formatter:on)

### 3.2 HTML element

The root `<html>` element must include a `lang` attribute specifying the document language:

[//]: # (@formatter:off)
`````html

<html lang="en">
`````
[//]: # (@formatter:on)

### 3.3 Head section

The `<head>` section must include the following elements in order:

1. Character encoding meta tag
2. Title element
3. Viewport meta tag (for responsive design)
4. Additional meta tags as needed
5. Stylesheets
6. Scripts (if needed in head)

[//]: # (@formatter:off)
`````html

<head>
  <meta charset="UTF-8">
  <title>Page Title</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="Page description">
  <link rel="stylesheet" href="styles.css">
</head>
`````
[//]: # (@formatter:on)

### 3.4 Body section

The `<body>` section contains the document content. Scripts are placed at the end of the body unless they need to load before content rendering.

## 4 Formatting

### 4.1 Column limit

HTML code has a column limit of `120 characters`. Except as noted, any line that would exceed this limit is line-wrapped.

### 4.2 Formatter control

Formatter control comments are enabled. Code regions can be excluded from formatting using `<!-- prettier-ignore -->`:

[//]: # (@formatter:off)
`````html
<!-- Correct -->
<!-- prettier-ignore -->
<div class="special"><span>Content</span></div>
`````
[//]: # (@formatter:on)

### 4.3 Indentation

The global default indent size is two spaces, and the tab width is two spaces. Continuation indent size is four spaces.

### 4.4 Tag and attribute case

All HTML element tags and attribute names must be written in lowercase:

[//]: # (@formatter:off)
`````html
<!-- Correct -->
<div class="container">
  <input type="text" name="username">
</div>

<!-- Incorrect -->
<DIV CLASS="container">
  <INPUT TYPE="text" NAME="username">
</DIV>
`````
[//]: # (@formatter:on)

### 4.5 Attribute quoting

All attribute values must be enclosed in double quotes. Single quotes are forbidden:

[//]: # (@formatter:off)
`````html
<!-- Correct -->
<div class="container">
  <a href="https://example.com">Link</a>
</div>

<!-- Incorrect -->
<div class='container'>
  <a href='https://example.com'>Link</a>
</div>
`````
[//]: # (@formatter:on)

### 4.6 Self-closing tags

Self-closing tags must include a space before the trailing slash:

[//]: # (@formatter:off)
`````html
<!-- Correct -->
<img src="image.jpg" alt="Description" />
<br />
<input type="text" />

<!-- Incorrect -->
<img src="image.jpg" alt="Description">
<br>
<input type="text">
`````
[//]: # (@formatter:on)

### 4.7 Attribute ordering

Attributes are ordered in the following sequence:

1. `class`
2. `id`
3. `data-*` attributes (alphabetical)
4. `src`, `href`, `for`, `type`, `value`
5. Other attributes (alphabetical)

[//]: # (@formatter:off)
`````html
<!-- Correct -->
<div class="container" id="main" data-role="navigation" data-theme="dark">
  <a href="https://example.com" class="link">Link</a>
</div>
`````
[//]: # (@formatter:on)

### 4.8 Line wrapping

Attributes are wrapped to multiple lines when the line exceeds 120 characters:

[//]: # (@formatter:off)
`````html
<!-- Correct -->
<button
  type="submit"
  class="btn btn-primary"
  data-loading="false"
  disabled
>
  Submit
</button>

<!-- Incorrect -->
<button type="submit" class="btn btn-primary" data-loading="false" disabled>Submit</button>
`````
[//]: # (@formatter:on)

## 5 Semantic HTML

### 5.1 Semantic elements

Semantic HTML5 elements must be used instead of generic `<div>` elements when appropriate:

[//]: # (@formatter:off)
`````html
<!-- Correct -->
<header>
  <nav>
    <ul>
      <li><a href="/">Home</a></li>
      <li><a href="/about">About</a></li>
    </ul>
  </nav>
</header>
<main>
  <article>
    <h1>Article Title</h1>
    <p>Article content...</p>
  </article>
  <aside>
    <h2>Sidebar</h2>
    <p>Sidebar content...</p>
  </aside>
</main>
<footer>
  <p>&copy; 2024 MineKot</p>
</footer>

<!-- Incorrect -->
<div class="header">
  <div class="nav">
    <ul>
      <li><a href="/">Home</a></li>
      <li><a href="/about">About</a></li>
    </ul>
  </div>
</div>
<div class="main">
  <div class="article">
    <div class="title">Article Title</div>
    <div class="content">Article content...</div>
  </div>
  <div class="sidebar">
    <div class="title">Sidebar</div>
    <div class="content">Sidebar content...</div>
  </div>
</div>
<div class="footer">
  <p>&copy; 2024 MineKot</p>
</div>
`````
[//]: # (@formatter:on)

### 5.2 Heading hierarchy

Heading elements must follow a logical hierarchy without skipping levels:

[//]: # (@formatter:off)
`````html
<!-- Correct -->
<h1>Main Title</h1>
<h2>Section Title</h2>
<h3>Subsection Title</h3>

<!-- Incorrect -->
<h1>Main Title</h1>
<h3>Subsection Title</h3>
<h4>Sub-subsection Title</h4>
`````
[//]: # (@formatter:on)

### 5.3 Lists

Lists must use appropriate list elements (`<ul>`, `<ol>`, `<dl>`):

[//]: # (@formatter:off)
`````html
<!-- Correct -->
<ul>
  <li>Item 1</li>
  <li>Item 2</li>
</ul>

<ol>
  <li>First item</li>
  <li>Second item</li>
</ol>

<dl>
  <dt>Term</dt>
  <dd>Definition</dd>
</dl>

<!-- Incorrect -->
<div class="list">
  <div class="item">Item 1</div>
  <div class="item">Item 2</div>
</div>
`````
[//]: # (@formatter:on)

## 6 Accessibility

### 6.1 Alt text

All images must include descriptive `alt` text. Decorative images use empty `alt` attributes:

[//]: # (@formatter:off)
`````html
<!-- Correct - Descriptive image -->
<img src="logo.png" alt="MineKot Logo" />

<!-- Correct - Decorative image -->
<img src="divider.png" alt="" role="presentation" />

<!-- Incorrect -->
<img src="logo.png" />
<img src="logo.png" alt="logo" />
`````
[//]: # (@formatter:on)

### 6.2 Form labels

All form inputs must have associated labels using the `for` attribute matching the input's `id`:

[//]: # (@formatter:off)
`````html
<!-- Correct -->
<label for="username">Username</label>
<input type="text" id="username" name="username" />

<!-- Incorrect -->
<label>Username</label>
<input type="text" name="username" />

<input type="text" name="username" placeholder="Username" />
`````
[//]: # (@formatter:on)

### 6.3 ARIA attributes

ARIA attributes are used to enhance accessibility when semantic HTML is insufficient:

[//]: # (@formatter:off)
`````html
<!-- Correct - ARIA for interactive elements -->
<button aria-expanded="false" aria-controls="menu">Toggle Menu</button>
<div id="menu" role="menu" aria-hidden="true">
  <!-- Menu items -->
</div>

<!-- Correct - ARIA for live regions -->
<div role="status" aria-live="polite">
  Status message
</div>
`````
[//]: # (@formatter:on)

### 6.4 Focus management

Interactive elements must be keyboard-accessible. Custom interactive elements must include appropriate ARIA roles and keyboard event handlers:

[//]: # (@formatter:off)
`````html
<!-- Correct - Native button with keyboard support -->
<button type="button" onclick="handleClick()">Click me</button>

<!-- Correct - Custom element with keyboard support -->
<div role="button" tabindex="0" onclick="handleClick()" onkeydown="handleKeyDown(event)">
  Click me
</div>
`````
[//]: # (@formatter:on)

## 7 Script loading

### 7.1 Script placement

Scripts are placed at the end of the `<body>` element unless they need to execute before content rendering:

[//]: # (@formatter:off)
`````html
<!-- Correct -->
<body>
<div class="content">Content</div>
<script src="main.js"></script>
</body>

<!-- Incorrect -->
<head>
  <script src="main.js"></script>
</head>
<body>
<div class="content">Content</div>
</body>
`````
[//]: # (@formatter:on)

### 7.2 Async and defer

The `async` or `defer` attributes are used for script loading optimization:

[//]: # (@formatter:off)
`````html
<!-- Correct - Async for independent scripts -->
<script src="analytics.js" async></script>

<!-- Correct - Defer for scripts that depend on DOM -->
<script src="main.js" defer></script>

<!-- Correct - Inline script with defer -->
<script defer>
  // Inline script
</script>
`````
[//]: # (@formatter:on)

### 7.3 Inline scripts

Inline scripts are forbidden except for specific cases where external scripts are not feasible (e.g., CSP restrictions). Inline scripts must use nonce or hash-based CSP when required.

## 8 Architecture and design

### 8.1 Zero hardcoding

Hardcoding raw user-facing textual values directly inside HTML element structures is strictly prohibited. All plain text, label contexts, and interface strings must be managed dynamically via localized template systems or configurations.

### 8.2 Structural decoupling

Markup templates must cleanly isolate structural presentation from style configurations and behavioral logic. Inline style specifications (`style="..."`) and embedded inline scripting blocks (`onclick="..."`) are strictly banned across all repositories.

[//]: # (@formatter:off)
`````html
<!-- Correct -->
<div class="container">
  <button id="submit-button" class="btn btn-primary">Submit</button>
</div>

<!-- Incorrect -->
<div class="container" style="padding: 20px;">
  <button onclick="handleSubmit()" style="background: blue;">Submit</button>
</div>
`````
[//]: # (@formatter:on)

### 8.3 Data attributes

Data attributes are used for storing custom data for JavaScript access:

[//]: # (@formatter:off)
`````html
<!-- Correct -->
<div class="user-card" data-user-id="123" data-role="admin">
  <span class="username">John Doe</span>
</div>

<!-- Incorrect -->
<div class="user-card" id="user-123-admin">
  <span class="username">John Doe</span>
</div>
`````
[//]: # (@formatter:on)

### 8.4 Template systems

HTML templates use server-side or client-side template systems for dynamic content. Logic is not embedded in HTML:

[//]: # (@formatter:off)
`````html
<!-- Correct - Template syntax -->
<div class="user-card">
  <h2>{{ user.name }}</h2>
  <p>{{ user.email }}</p>
</div>

<!-- Incorrect - Embedded logic -->
<div class="user-card">
  <h2><?php echo $user['name']; ?></h2>
  <p><?php echo $user['email']; ?></p>
</div>
`````
[//]: # (@formatter:on)

## 9 Documentation and comments

### 9.1 HTML comments

Complex blocks, template injection areas, and non-intuitive layout containers must be clearly denoted using explicit comment wrappers:

[//]: # (@formatter:off)
`````html
<!-- Correct -->
<!-- Main content area -->
<main>
  <article>
    <h1>Article Title</h1>
  </article>
</main>

<!-- End of main content area -->

<!-- Incorrect -->
<div class="main">
  <!-- content -->
</div>
`````
[//]: # (@formatter:on)

### 9.2 Comment formatting

Comments are placed on their own lines. Inline comments are forbidden:

[//]: # (@formatter:off)
`````html
<!-- Correct -->
<!-- Navigation menu -->
<nav>
  <ul>
    <li><a href="/">Home</a></li>
  </ul>
</nav>

<!-- Incorrect -->
<nav> <!-- Navigation menu -->
  <ul>
    <li><a href="/">Home</a></li>
  </ul>
</nav>
`````
[//]: # (@formatter:on)

## 10 Performance and optimization

### 10.1 Resource loading

Resources are loaded efficiently using appropriate techniques:

1. Images use modern formats (WebP, AVIF) with fallbacks
2. Fonts use `font-display: swap` for faster rendering
3. CSS is loaded in the `<head>` to prevent flash of unstyled content
4. JavaScript is deferred or loaded asynchronously

### 10.2 Critical CSS

Critical CSS is inlined for above-the-fold content to improve perceived performance.

### 10.3 Lazy loading

Images and iframes use lazy loading for below-the-fold content:

[//]: # (@formatter:off)
`````html
<!-- Correct -->
<img src="image.jpg" alt="Description" loading="lazy" />
<iframe src="content.html" loading="lazy"></iframe>
`````
[//]: # (@formatter:on)

## Conclusion

Adherence to this style guide ensures that the MineKot ecosystem remains consistent, readable, and maintainable. By following these standards, contributors help uphold the quality and performance expectations of the project.
