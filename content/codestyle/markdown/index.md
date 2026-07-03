---
title: "Markdown Style Guide"
params:
  menuPre: "<i class=\"devicons devicons-markdown\"></i> "
weight: 7
---

## 1 Introduction

This document serves as the complete definition of MineKot's coding standards for source code in `Markdown (*.md)`. A Markdown source file is described as being in MineKot Style if and only if it adheres to the rules herein.

All projects under the MineKot umbrella are open-source and licensed under the `Apache License 2.0`. This guide enforces precise architectural boundaries and stylistic consistency to support a scalable, maintainable, and community-driven ecosystem.

## 2 Source file basics

### 2.1 File encoding

Source files are encoded in UTF-8 without BOM.

### 2.2 Line endings

Source files end with an `LF (Line Feed)` line ending and insert a single final newline character at the end of the file.

### 2.3 Language standard

Code identifiers, documentation, and user-facing strings are written in American English.

## 3 Formatting

### 3.1 Indentation

The global default indent size is two spaces, and the tab width is two spaces. Continuation indent size is four spaces.

### 3.2 Column limit

Markdown code has a column limit of `999 characters` to preserve the fluidity of documentation layouts across various display formats and split-screen development tools.

### 3.3 Formatter control

Formatter control comments are enabled. Code regions can be excluded from formatting using HTML comments:

`````markdown
<!-- Correct -->
<!-- prettier-ignore -->
<div>Content</div>
```

### 3.4 Hard line breaks

Automatic hard line-breaks inserted during active editing or typing are disabled completely. Paragraph blocks must flow continuously as unbroken lines to ensure responsive rendering in web interfaces.

### 3.5 Spacing

One space is used after list markers, after headings, and around emphasis markers. No spaces are used before list markers or before emphasis markers.

## 4 Headings

### 4.1 Heading hierarchy

Headings follow a logical hierarchy without skipping levels:

`````markdown
# Correct

# Main Title

## Section Title

### Subsection Title

#### Sub-subsection Title

# Incorrect

# Main Title

### Subsection Title

##### Sub-sub-subsection Title
`````

### 4.2 Heading spacing

One blank line is placed before and after headings:

`````markdown
# Correct

Previous paragraph.

# Heading

Next paragraph.

# Incorrect

Previous paragraph.

# Heading

Next paragraph.
`````

### 4.3 Heading case

Headings use sentence case (first letter capitalized, rest lowercase except proper nouns):

`````markdown
# Correct

# Installation guide

# Configuration options

# API reference

# Incorrect

# Installation Guide

# CONFIGURATION OPTIONS

# API Reference
`````

### 4.4 ATX-style headings

ATX-style headings (`#`) are preferred over Setext-style headings (`===` or `---`):

`````markdown
# Correct

# Heading

## Subheading

# Incorrect

Heading
=======

Subheading
-------
`````

## 5 Lists

### 5.1 Unordered lists

Unordered lists use hyphens (`-`) as markers:

`````markdown
# Correct

- Item 1
- Item 2
- Item 3

# Incorrect

* Item 1
* Item 2
* Item 3

+ Item 1
+ Item 2
+ Item 3
`````

### 5.2 Ordered lists

Ordered lists use numbers followed by periods:

`````markdown
# Correct

1. First item
2. Second item
3. Third item

# Incorrect

1) First item
2) Second item
3) Third item
`````

### 5.3 List indentation

Nested lists are indented by two spaces:

`````markdown
# Correct

- Item 1
  - Nested item 1
  - Nested item 2
- Item 2

# Incorrect

- Item 1
  - Nested item 1
  - Nested item 2
- Item 2
`````

### 5.4 List spacing

One blank line is placed between list items when they contain multiple paragraphs:

`````markdown
# Correct

- Item 1

  Additional paragraph for item 1.

- Item 2

# Incorrect

- Item 1
  Additional paragraph for item 1.
- Item 2
`````

## 6 Links

### 6.1 Link syntax

Inline links are used for short links. Reference-style links are used for repeated links or long URLs:

`````markdown
# Correct - Inline link

See the [documentation](https://example.com/docs) for details.

# Correct - Reference-style link

See the [documentation][docs] for details.

[docs]: https://example.com/docs

# Incorrect - Unnecessary reference

See the [documentation](https://example.com/docs) for details.

[documentation]: https://example.com/docs
`````

### 6.2 Link text

Link text is descriptive and indicates the destination:

`````markdown
# Correct

[Installation guide](https://example.com/install)
[API reference](https://example.com/api)

# Incorrect

[click here](https://example.com/install)
[link](https://example.com/api)
`````

### 6.3 URL formatting

URLs are not enclosed in angle brackets unless necessary:

`````markdown
# Correct

https://example.com/docs

# Acceptable - When necessary for parsing

<https://example.com/docs>
`````

## 7 Images

### 7.1 Image syntax

Images use the standard Markdown image syntax with descriptive alt text:

`````markdown
# Correct

![Screenshot of the application interface](./screenshots/app.png)

# Incorrect

![screenshot](./screenshots/app.png)
![app.png](./screenshots/app.png)
`````

### 7.2 Alt text

Alt text is descriptive and conveys the image's purpose:

`````markdown
# Correct

![Diagram showing the system architecture](./architecture.png)

# Incorrect

![diagram](./architecture.png)
![architecture.png](./architecture.png)
`````

### 7.3 Image paths

Relative paths are used for images within the repository. Absolute URLs are used for external images:

`````markdown
# Correct

![Logo](./assets/logo.png)
![External image](https://example.com/image.png)

# Incorrect

![Logo](/absolute/path/to/logo.png)
`````

## 8 Code blocks

### 8.1 Fenced code blocks

Fenced code blocks with language identifiers are used for code:

`````markdown
# Correct

```kotlin
fun main() {
    println("Hello, World!")
}
```

# Incorrect – No language identifier

```
fun main() {
    println("Hello, World!")
}
```

# Incorrect – Indented code

    fun main() {
        println("Hello, World!")
    }
`````

### 8.2 Language identifiers

Language identifiers are specified for all code blocks:

`````markdown
# Correct

```kotlin
// Kotlin code
```

```javascript
// JavaScript code
```

```yaml
# YAML configuration
```

# Incorrect

```
// Code without language identifier
```
`````

### 8.3 Inline code

Inline code uses backticks for short code snippets:

`````markdown
# Correct

Use the `main()` function to start the application.

# Incorrect

Use the main() function to start the application.
Use "main()" function to start the application.
`````

### 8.4 Code escaping

When embedding raw illustrative source snippets or configurations inside Markdown documents, block boundaries must use clear language flags.

## 9 Emphasis

### 9.1 Bold and italic

Bold is used for strong emphasis. Italic is used for mild emphasis:

`````markdown
# Correct

**Important note**: This is critical.
*Note*: This is additional information.

# Incorrect

__Important note__: This is critical.
_Note_: This is additional information.
`````

### 9.2 Emphasis consistency

Emphasis markers are used consistently throughout the document:

`````markdown
# Correct

**Bold text**
*Italic text*

# Incorrect - Mixed markers

**Bold text**
_Italic text_
`````

### 9.3 Overuse

Emphasis is used sparingly. Overuse reduces readability.

## 10 Blockquotes

### 10.1 Blockquote syntax

Blockquotes use the `>` character:

`````markdown
# Correct

> This is a blockquote.
> It can span multiple lines.

# Incorrect

This is a blockquote.
It can span multiple lines.
`````

### 10.2 Nested blockquotes

Nested blockquotes use multiple `>` characters:

`````markdown
# Correct

> This is a blockquote.
>
> > This is a nested blockquote.
`````

### 10.3 Blockquote spacing

One blank line is placed before and after blockquotes:

`````markdown
# Correct

Previous paragraph.

> This is a blockquote.

Next paragraph.

# Incorrect

Previous paragraph.
> This is a blockquote.
Next paragraph.
`````

## 11 Tables

### 11.1 Table syntax

Tables use the standard Markdown table syntax:

`````markdown
# Correct

| Header 1 | Header 2 | Header 3 |
|----------|----------|----------|
| Cell 1   | Cell 2   | Cell 3   |
| Cell 4   | Cell 5   | Cell 6   |

# Incorrect

Header 1 | Header 2 | Header 3
Cell 1 | Cell 2 | Cell 3
Cell 4 | Cell 5 | Cell 6
`````

### 11.2 Table alignment

Table alignment is specified in the separator row:

`````markdown
# Correct

| Left | Center | Right |
|:-----|:------:|------:|
| Cell |  Cell  |  Cell |

# Incorrect

| Left | Center | Right |
|------|--------|-------|
| Cell |  Cell  |  Cell |
`````

### 11.3 Table readability

Tables are kept simple. Complex tables are split into multiple tables or presented in alternative formats.

## 12 Horizontal rules

### 12.1 Horizontal rule syntax

Horizontal rules use three or more hyphens:

`````markdown
# Correct
---

# Incorrect

___
***
`````

### 12.2 Horizontal rule spacing

One blank line is placed before and after horizontal rules:

`````markdown
# Correct

Previous section.

---

Next section.

# Incorrect

Previous section.
---
Next section.
`````

## 13 Technical documentation standards

### 13.1 Content guidelines

All technical concepts, script commands, and internal APIs must be documented in comprehensive, plain-text explanations.

### 13.2 Code examples

Code examples are complete and runnable. Explanations accompany complex examples.

### 13.3 Command examples

Command examples use code blocks with appropriate language identifiers:

`````markdown
# Correct

```bash
./gradlew build
```

```kotlin
./gradlew build
```
`````

## 14 Architecture and design

### 14.1 Document structure

Markdown documents follow a clear structure:

1. Title (heading level 1)
2. Introduction
3. Main content (organized with headings)
4. Conclusion or summary
5. References or additional resources

### 14.2 Section organization

Related content is grouped under appropriate headings. Sections are logically ordered.

### 14.3 Cross-references

Cross-references use links to other sections or documents:

`````markdown
# Correct

See the [Installation](#installation) section for details.
See the [API documentation](./api.md) for more information.
`````

## 15 Documentation and comments

### 15.1 HTML comments

HTML comments are used for notes that should not appear in the rendered output:

`````markdown
<!-- This is a comment that will not be rendered -->
`````

### 15.2 Front matter

Front matter is used for metadata in documents that support it:

`````markdown
---
title: "Document Title"
date: 2024-01-15
author: "John Doe"
---
`````

## 16 Performance and optimization

### 16.1 File size

Markdown files are kept concise. Unnecessary content is avoided.

### 16.2 Image optimization

Images are optimized for web display. Appropriate formats and sizes are used.

### 16.3 Link checking

Links are checked regularly to ensure they are valid and up to date.

## Conclusion

Adherence to this style guide ensures that the MineKot ecosystem remains consistent, readable, and maintainable. By following these standards, contributors help uphold the quality and performance expectations of the project.
