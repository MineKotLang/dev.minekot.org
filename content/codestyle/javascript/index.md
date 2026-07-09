---
title: "JavaScript Style Guide"
params:
  menuPre: "<i class=\"devicons devicons-javascript\"></i> "
weight: 2
---

## 1 Introduction

This document serves as the complete definition of MineKot's coding standards for source code in the `JavaScript Programming Language`. A JavaScript source file is described as being in MineKot Style if and only if it adheres to the rules herein.

All projects under the MineKot umbrella are open-source and licensed under the `Apache License 2.0`. This guide enforces precise architectural boundaries and stylistic consistency to support a scalable, maintainable, and community-driven ecosystem.

## 2 Source file basics

### 2.1 File encoding

Source files are encoded in UTF-8 without BOM.

### 2.2 Line endings

Source files end with an `LF (Line Feed)` line ending and insert a single final newline character at the end of the file.

### 2.3 Language standard

Code identifiers, documentation, and user-facing strings are written in American English.

## 3 Source file structure

### 3.1 Imports

Import statements are managed according to the following guidelines:

1. **Module system:** ES6 modules (`import`/`export`) are mandatory. CommonJS (`require`/`module.exports`) is forbidden.
2. **Import ordering:** Imports are ordered as follows: standard library imports, third-party library imports, local application imports, each group separated by a blank line.
3. **Named exports:** Named exports are preferred over default exports for clarity and tree-shaking.
4. **Import paths:** Relative imports must use explicit file extensions (`.js`).

### 3.2 File organization

Source files are organized in the following sequence:

1. Copyright and license header (if applicable)
2. Import statements
3. Type definitions (if using JSDoc)
4. Constants
5. Functions and classes
6. Exports

## 4 Formatting

### 4.1 Column limit

JavaScript code has a column limit of `120 characters`. Except as noted, any line that would exceed this limit is line-wrapped.

### 4.2 Formatter control

Formatter control comments are enabled. Code regions can be excluded from formatting using `// prettier-ignore` for single lines or `/* prettier-ignore-start */` and `/* prettier-ignore-end */` for multi-line blocks:

`````javascript
// Correct
// prettier-ignore
const matrix = [1, 2, 3, 4, 5, 6, 7, 8, 9];

/* prettier-ignore-start */
const complexObject = {
  // Custom formatting
  key1: value1,
  key2: value2,
};
/* prettier-ignore-end */
`````

### 4.3 Indentation

The global default indent size is two spaces, and the tab width is two spaces. Continuation indent size is four spaces.

### 4.4 Spacing

Spaces are used around binary operators, after commas, after colons in object literals, and before and after arrow function arrows. No spaces are used before commas, before colons in object literals, or around unary operators. Spaces are used within object literal braces and within imports:

`````javascript
// Correct
const user = { name: "John", age: 30 };
import { foo, bar } from "module";

// Incorrect
const user = { name: "John", age: 30 };
import { foo, bar } from "module";
`````

### 4.5 Semicolons

Semicolons are avoided where possible. Automatic Semicolon Insertion (ASI) is relied upon for statement termination. Semicolons are only used when required by the language (e.g., to avoid ambiguous statements). The formatter enforces semicolon style.

### 4.6 Quotes

Single quotes are used for string literals unless the string contains single quotes, in which case double quotes are used. Template literals (backticks) are used for string interpolation or multi-line strings. Quote style is enforced by the formatter.

### 4.7 Blank lines

One blank line is kept between top-level declarations. No blank lines are kept at the beginning or end of blocks.

### 4.8 Trailing commas

Trailing commas are enforced in multiline arrays, objects, and function calls to improve diff readability and match Kotlin style:

`````javascript
// Correct
const users = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
];

function createUser(
  name,
  email,
  role,
) {
  return { name, email, role };
}

// Incorrect
const users = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" }
];

function createUser(
  name,
  email,
  role
) {
  return { name, email, role };
}
`````

### 4.9 Parameter wrapping

Function declarations and function call parameters are wrapped such that every item is on its own line when multiline:

1. A new line is required immediately after the opening left parenthesis `(`.
2. The closing right parenthesis `)` is placed on a new line following the final parameter.

`````javascript
// Correct
function createUser(
  name,
  email,
  role,
) {
  return { name, email, role };
}

createUser(
  "John Doe",
  "john@example.com",
  "admin",
);

// Incorrect
function createUser(name, email, role) {
  return { name, email, role };
}

createUser("John Doe", "john@example.com", "admin");
`````

### 4.10 Template literals

When embedding variables, properties, or methods within a template literal, curly braces (`${}`) are required. Omission of curly braces for simple variables is not allowed:

`````javascript
// Correct
const greeting = `Hello, ${name}!`;
const message = `User ${user.id} has ${user.points} points.`;

// Incorrect
const greeting = `Hello, $name!`;
const message = `User $user.id has $user.points points.`;
`````

## 5 Language features and ecosystem

### 5.1 Modern JavaScript (ES6+)

Code must use modern JavaScript features:

1. **Variable declarations:** `const` is used for variables that are not reassigned. `let` is used for variables that are reassigned. `var` is forbidden.
2. **Arrow functions:** Arrow functions are preferred for callbacks and short functions. Function declarations are used for named functions and hoisting requirements.
3. **Template literals:** Template literals are used for string interpolation and multi-line strings. String concatenation with `+` is forbidden for interpolation.
4. **Destructuring:** Destructuring is used for object and array property extraction.
5. **Spread/rest:** Spread and rest operators are used for array/object manipulation and function parameters.
6. **Optional chaining:** Optional chaining (`?.`) is used for safe property access.
7. **Nullish coalescing:** Nullish coalescing (`??`) is used for null/undefined fallbacks.

### 5.2 Built-in APIs

Native JavaScript APIs are preferred over polyfills or external libraries unless the native API is not supported in target environments.

### 5.3 Versioning

Projects target the latest stable ECMAScript specification supported by the target runtime environment.

## 6 Programming practices

### 6.1 Variable declarations

1. **Const preference:** `const` is the default for all declarations unless reassignment is required.
2. **Let scope:** `let` is used only when reassignment is necessary and is scoped to the smallest possible block.
3. **No var:** `var` is strictly forbidden due to function scoping and hooting behavior.

### 6.2 Function declarations

1. **Named functions:** Function declarations are used for named functions that require hoisting.
2. **Arrow functions:** Arrow functions are used for callbacks, short functions, and when lexical `this` binding is desired.
3. **Function expressions:** Function expressions are used when a function needs to be assigned to a variable or passed as a callback.

### 6.3 Asynchronous operations

All non-trivial I/O operations, network tasks, and data queries **must** be executed asynchronously using `async/await` paradigms. Blocking execution loops on a shared execution thread is strictly forbidden.

### 6.4 Error handling

Uncaught exceptions or silent failures are completely banned. All asynchronous flows **must** safely wrap potential failure boundaries in `try-catch` blocks or robust promise chains (`.catch()`), ensuring errors are safely captured and routed to the logging system.

### 6.5 Equality checks

Strict equality (`===` and `!==`) is mandatory. Loose equality (`==` and `!=`) is forbidden.

### 6.6 Type coercion

Explicit type coercion is used instead of implicit coercion. Functions like `String()`, `Number()`, and `Boolean()` are preferred over implicit conversions.

## 7 Naming conventions

### 7.1 Variables and functions

Variables and functions use `camelCase` naming. Names are descriptive and indicate purpose or content.

`````javascript
// Correct
const userAccountCount = 42;

function getUserById(id) {
}

// Incorrect
const UserAccountCount = 42;

function get_user_by_id(id) {
}
`````

### 7.2 Constants

Constants that are never reassigned use `UPPER_SNAKE_CASE` naming.

`````javascript
// Correct
const MAX_CONNECTIONS = 100;
const API_BASE_URL = "https://api.example.com";

// Incorrect
const maxConnections = 100;
const apiBaseUrl = "https://api.example.com";
`````

### 7.3 Classes

Classes use `PascalCase` naming. Class instances use `camelCase`.

`````javascript
// Correct
class UserService {
}

const userService = new UserService();

// Incorrect
class userService {
}

const UserService = new UserService();
`````

### 7.4 Private members

Private members are prefixed with an underscore (`_`) to indicate internal use.

`````javascript
// Correct
class Database {
  constructor() {
    this._connection = null;
  }

  _connect() {
  }
}

// Incorrect
class Database {
  constructor() {
    this.connection = null;
  }

  connect() {
  }
}
`````

## 8 Architecture and design

### 8.1 Data-driven design

JavaScript scripts and utilities **must** be entirely data-driven. Logic routing, internal state thresholds, and functional options **must** be passed via objects or schemas instead of being hard-coded directly inside execution structures.

### 8.2 Zero hardcoding

Hardcoding structural keys, operational paths, or user-facing strings is strictly prohibited. Configuration parameters **must** be separated from functional source lines and pulled dynamically from designated management configurations.

### 8.3 Modular decoupling

Code is organized into small, focused modules with single responsibilities. Circular dependencies are forbidden. Modules export only what is necessary for public use.

### 8.4 Text and logging

Any user-facing text, console notification, or system logging **must** utilize the **MiniMessage** parsing paradigm, regardless of whether the specific repository operates within a Minecraft environment. The use of legacy formatting markers or direct concatenation for styled console text is banned.

### 8.5 Pure functions

Functions are preferred to be pure (no side effects) when possible. State mutations are explicit and localized.

## 9 Documentation and comments

### 9.1 JSDoc documentation

Every public-facing function, utility module, and custom class constructor **must** feature a comprehensive JSDoc block outlining its core objective, expected parameters (`@param`), return types (`@return` or `@returns`), and thrown exceptions (`@throws`).

`````javascript
/**
 * Retrieves a user by their unique identifier.
 * @param {string} id - The unique identifier of the user.
 * @returns {Promise<User>} A promise that resolves to the user object.
 * @throws {Error} Throws an error if the user is not found.
 */
async function getUserById(id) {
  // Implementation
}
`````

### 9.2 Variable documentation

JSDoc is required for "complicated" variables. This includes objects with complex structures, configuration objects, and instances where the behavior is not immediately evident from the property name alone. Simple, self-evident primitives do not require JSDoc.

### 9.3 Inline comments

Code logic **must** prioritize self-documenting syntax via descriptive names. Inline explanations (`//`) are restricted solely to areas containing inherently complex algorithms or mathematical boundaries that cannot be simplified through normal refactoring.

### 9.4 Comment formatting

Block comments (`/* */`) are used for multi-line explanations. Line comments (`//`) are used for single-line explanations. Comments are placed above the code they describe, not on the same line.

## 10 Performance and optimization

### 10.1 Event loops

Long-running synchronous operations are forbidden in the main event loop. CPU-intensive tasks are offloaded to worker threads or broken into chunks using `setImmediate` or `process.nextTick`.

### 10.2 Memory management

Explicit cleanup is performed for resources that require manual disposal (event listeners, timers, file handles). Memory leaks from retained references are avoided.

### 10.3 Bundle size

Code is written with tree-shaking in mind. Side effects in modules are avoided. Large dependencies are scrutinized for necessity.

## Conclusion

Adherence to this style guide ensures that the MineKot ecosystem remains consistent, readable, and maintainable. By following these standards, contributors help uphold the quality and performance expectations of the project.
