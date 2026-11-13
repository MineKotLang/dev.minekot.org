---
title: "TypeScript Style Guide"
params:
  menuPre: "<i class=\"devicons devicons-typescript-icon\"></i> "
weight: 2
---

## 1 Introduction

This document serves as the complete definition of MineKot's coding standards for source code in the `TypeScript Programming Language`. A TypeScript source file is described as being in MineKot Style if and only if it adheres to the rules herein.

All projects under the MineKot umbrella are open-source and licensed under the `Apache License 2.0`. This guide enforces precise architectural boundaries and stylistic consistency to support a scalable, maintainable, and community-driven ecosystem.

TypeScript builds upon the JavaScript Style Guide with additional type system requirements and best practices.

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

1. **Module system:** ES6 modules (`import`/`export`) are mandatory.
2. **Import ordering:** Imports are ordered as follows: standard library imports, third-party library imports, local application imports, each group separated by a blank line.
3. **Named exports:** Named exports are preferred over default exports for clarity and tree-shaking.
4. **Import paths:** Relative imports must use explicit file extensions (`.ts` for source files, `.js` for declaration files).
5. **Type imports:** Type-only imports are used when only types are imported (`import type { Type } from 'module'`).

### 3.2 File organization

Source files are organized in the following sequence:

1. Copyright and license header (if applicable)
2. Import statements
3. Type definitions and interfaces
4. Constants
5. Functions and classes
6. Exports

## 4 Type system

### 4.1 Type annotations

1. **Explicit returns:** Function return types are explicitly declared. Implicit return types are forbidden.
2. **Parameters:** Function parameters are explicitly typed unless the type is immediately obvious from context.
3. **No `any`:** The `any` type is strictly forbidden. Use `unknown` for truly unknown types.
4. **Explicit generics:** Generic type parameters are explicitly specified when not inferred by the compiler.

[//]: # (@formatter:off)
`````typescript
// Correct
function getUserById(id: string): Promise<User> {
  // Implementation
}

const users: User[] = await fetchUsers();

// Incorrect
function getUserById(id: string) {
  // Implementation
}

const users = await fetchUsers();
`````
[//]: # (@formatter:on)

### 4.2 Interfaces vs. types

1. **Interfaces:** Interfaces are used for object shapes that can be extended or implemented.
2. **Type aliases:** Type aliases are used for unions, intersections, primitives, and utility types.
3. **Consistency:** Within a module, be consistent in using either interfaces or type aliases for similar constructs.

[//]: # (@formatter:off)
`````typescript
// Correct - Interface for extensible object shape
interface User {
  id: string;
  name: string;
}

// Correct - Type alias for union
type Status = 'active' | 'inactive' | 'pending';

// Correct - Type alias for utility type
type PartialUser = Partial<User>;
`````
[//]: # (@formatter:on)

### 4.3 Strict mode

All projects must enable TypeScript's strict mode in `tsconfig.json`:

[//]: # (@formatter:off)
```json
{
  "compilerOptions": {
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "strictFunctionTypes": true,
    "strictBindCallApply": true,
    "strictPropertyInitialization": true,
    "noImplicitThis": true,
    "alwaysStrict": true
  }
}
```
[//]: # (@formatter:on)

### 4.4 Null and undefined

1. **Strict null checks:** Strict null checks are enabled.
2. **Non-null assertions:** Non-null assertions (`!`) are forbidden except in specific, well-documented cases.
3. **Optional chaining:** Optional chaining (`?.`) is used for safe property access.
4. **Nullish coalescing:** Nullish coalescing (`??`) is used for null/undefined fallbacks.

### 4.5 Enums

1. **Const enums:** Const enums are preferred for performance.
2. **String enums:** String enums are preferred over numeric enums for readability.
3. **Enum usage:** Enums are used only when the set of values is fixed and known at compile time.

[//]: # (@formatter:off)
`````typescript
// Correct
const enum Status {
  Active = 'active',
  Inactive = 'inactive',
  Pending = 'pending'
}

// Incorrect
enum Status {
  Active = 0,
  Inactive = 1,
  Pending = 2
}
`````
[//]: # (@formatter:on)

## 5 Formatting

### 5.1 Column limit

TypeScript code has a column limit of `120 characters`. Except as noted, any line that would exceed this limit is line-wrapped.

### 5.2 Formatter control

Formatter control comments are enabled. Code regions can be excluded from formatting using `// prettier-ignore` for single lines or `/* prettier-ignore-start */` and `/* prettier-ignore-end */` for multi-line blocks:

[//]: # (@formatter:off)
`````typescript
// Correct
// prettier-ignore
const matrix: number[][] = [1, 2, 3, 4, 5, 6, 7, 8, 9];

/* prettier-ignore-start */
const complexObject: Config = {
  // Custom formatting
  key1: value1,
  key2: value2,
};
/* prettier-ignore-end */
`````
[//]: # (@formatter:on)

### 5.3 Indentation

The global default indent size is two spaces, and the tab width is two spaces. Continuation indent size is four spaces.

### 5.4 Spacing

Spaces are used around binary operators, after commas, after colons in type annotations, and before and after arrow function arrows. No spaces are used before commas, before colons in type annotations, or around unary operators. Spaces are used within object literal braces and within imports. No space is used before the function left parenthesis:

[//]: # (@formatter:off)
`````typescript
// Correct
const user = { name: "John", age: 30 };
import { foo, bar } from "module";

function foo() {
}

// Incorrect
const user={name:"John",age:30};
import{foo,bar}from"module";

function foo(){}
`````
[//]: # (@formatter:on)

### 5.5 Semicolons

Semicolons are avoided where possible. Automatic Semicolon Insertion (ASI) is relied upon for statement termination. Semicolons are only used when required by the language (e.g., to avoid ambiguous statements). The formatter enforces semicolon style.

### 5.6 Quotes

Single quotes are used for string literals unless the string contains single quotes, in which case double quotes are used. Template literals (backticks) are used for string interpolation or multi-line strings. Quote style is enforced by the formatter.

### 5.7 Blank lines

One blank line is kept between top-level declarations. No blank lines are kept at the beginning or end of blocks.

### 5.8 Trailing commas

Trailing commas are enforced in multiline arrays, objects, function calls, and type definitions to improve diff readability and match Kotlin style:

[//]: # (@formatter:off)
`````typescript
// Correct
const users: User[] = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
];

function createUser(
  name: string,
  email: string,
  role: string,
): User {
  return { name, email, role };
}

type UserConfig = {
  name: string;
  email: string;
  role: string;
};

// Incorrect
const users: User[] = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" }
];

function createUser(
  name: string,
  email: string,
  role: string
): User {
  return { name, email, role };
}

type UserConfig = {
  name: string;
  email: string;
  role: string
};
`````
[//]: # (@formatter:on)

### 5.9 Parameter wrapping

Function declarations and function call parameters are wrapped such that every item is on its own line when multiline:

1. A new line is required immediately after the opening left parenthesis `(`.
2. The closing right parenthesis `)` is placed on a new line following the final parameter.

[//]: # (@formatter:off)
`````typescript
// Correct
function createUser(
  name: string,
  email: string,
  role: string,
): User {
  return { name, email, role };
}

createUser(
  "John Doe",
  "john@example.com",
  "admin",
);

// Incorrect
function createUser(name: string, email: string, role: string): User {
  return { name, email, role };
}

createUser("John Doe", "john@example.com", "admin");
`````
[//]: # (@formatter:on)

### 5.10 Template literals

When embedding variables, properties, or methods within a template literal, curly braces (`${}`) are required. Omission of curly braces for simple variables is not allowed:

[//]: # (@formatter:off)
`````typescript
// Correct
const greeting = `Hello, ${name}!`;
const message = `User ${user.id} has ${user.points} points.`;

// Incorrect
const greeting = `Hello, $name!`;
const message = `User $user.id has $user.points points.`;
`````
[//]: # (@formatter:on)

## 6 Language features and ecosystem

### 6.1 Modern TypeScript

Code must use modern TypeScript features:

1. **Variable declarations:** `const` is used for variables that are not reassigned. `let` is used for variables that are reassigned. `var` is forbidden.
2. **Arrow functions:** Arrow functions are preferred for callbacks and short functions. Function declarations are used for named functions and hoisting requirements.
3. **Template literals:** Template literals are used for string interpolation and multi-line strings.
4. **Destructuring:** Destructuring is used for object and array property extraction.
5. **Spread/rest:** Spread and rest operators are used for array/object manipulation and function parameters.
6. **Optional chaining:** Optional chaining (`?.`) is used for safe property access.
7. **Nullish coalescing:** Nullish coalescing (`??`) is used for null/undefined fallbacks.

### 6.2 Utility types

Built-in utility types are preferred over manual type transformations:

1. **Partial:** `Partial<T>` for optional properties
2. **Required:** `Required<T>` for required properties
3. **Readonly:** `Readonly<T>` for immutable objects
4. **Record:** `Record<K, T>` for mapped types
5. **Pick/Omit:** `Pick<T, K>` and `Omit<T, K>` for property selection
6. **Exclude/Extract:** `Exclude<T, U>` and `Extract<T, U>` for union manipulation

### 6.3 Generics

1. **Naming:** Generic type parameters use descriptive single-letter names (`T`, `U`, `V`) or descriptive names (`TUser`, `TResponse`).
2. **Constraints:** Generic constraints are used when type parameters have requirements.
3. **Defaults:** Default type parameters are provided when appropriate.

[//]: # (@formatter:off)
`````typescript
// Correct
function merge<T extends object, U extends object>(obj1: T, obj2: U): T & U {
  return { ...obj1, ...obj2 };
}

interface Repository<T = User> {
  findById(id: string): Promise<T | null>;
}

// Incorrect
function merge(obj1: any, obj2: any): any {
  return { ...obj1, ...obj2 };
}
`````
[//]: # (@formatter:on)

## 7 Programming practices

### 7.1 Type guards

Type guards are used to narrow types at runtime:

[//]: # (@formatter:off)
`````typescript
// Correct
function isString(value: unknown): value is string {
  return typeof value === 'string';
}

if (isString(input)) {
  // input is string here
}

// Incorrect
if (typeof input === 'string') {
  // type not narrowed without explicit guard
}
`````
[//]: # (@formatter:on)

### 7.2 Discriminated unions

Discriminated unions are used for type-safe state management:

[//]: # (@formatter:off)
`````typescript
// Correct
interface LoadingState {
  status: 'loading';
}

interface SuccessState {
  status: 'success';
  data: User;
}

interface ErrorState {
  status: 'error';
  error: Error;
}

type State = LoadingState | SuccessState | ErrorState;

function handleState(state: State) {
  switch (state.status) {
    case 'loading':
      // state is LoadingState
      break;
    case 'success':
      // state is SuccessState, data is available
      console.log(state.data);
      break;
    case 'error':
      // state is ErrorState, error is available
      console.error(state.error);
      break;
  }
}
`````
[//]: # (@formatter:on)

### 7.3 Asynchronous operations

All non-trivial I/O operations, network tasks, and data queries **must** be executed asynchronously using `async/await` paradigms. Blocking execution loops on a shared execution thread is strictly forbidden.

### 7.4 Error handling

Uncaught exceptions or silent failures are completely banned. All asynchronous flows **must** safely wrap potential failure boundaries in `try-catch` blocks or robust promise chains (`.catch()`), ensuring errors are safely captured and routed to the logging system.

### 7.5 Equality checks

Strict equality (`===` and `!==`) is mandatory. Loose equality (`==` and `!=`) is forbidden.

## 8 Naming conventions

### 8.1 Variables and functions

Variables and functions use `camelCase` naming. Names are descriptive and indicate purpose or content.

[//]: # (@formatter:off)
`````typescript
// Correct
const userAccountCount = 42;

function getUserById(id: string): Promise<User> {
}

// Incorrect
const UserAccountCount = 42;

function get_user_by_id(id: string): Promise<User> {
}
`````
[//]: # (@formatter:on)

### 8.2 Constants

Constants that are never reassigned use `UPPER_SNAKE_CASE` naming.

[//]: # (@formatter:off)
`````typescript
// Correct
const MAX_CONNECTIONS = 100;
const API_BASE_URL = "https://api.example.com";

// Incorrect
const maxConnections = 100;
const apiBaseUrl = "https://api.example.com";
`````
[//]: # (@formatter:on)

### 8.3 Classes and interfaces

Classes and interfaces use `PascalCase` naming. Class instances use `camelCase`.

[//]: # (@formatter:off)
`````typescript
// Correct
interface UserService {
}

class UserServiceImpl implements UserService {
}

const userService = new UserServiceImpl();

// Incorrect
interface userService {
}

class userServiceImpl implements userService {
}

const UserService = new userServiceImpl();
`````
[//]: # (@formatter:on)

### 8.4 Type parameters

Generic type parameters use descriptive single-letter names (`T`, `U`, `V`) or descriptive names (`TUser`, `TResponse`).

[//]: # (@formatter:off)
`````typescript
// Correct
function identity<T>(value: T): T {
  return value;
}

interface Repository<TUser> {
  findById(id: string): Promise<TUser | null>;
}

// Incorrect
function identity<Value>(value: Value): Value {
  return value;
}
`````
[//]: # (@formatter:on)

### 8.5 Private members

Private members use the `private` keyword. Underscore prefix is not used for private members.

[//]: # (@formatter:off)
`````typescript
// Correct
class Database {
  private connection: Connection | null = null;

  private async connect(): Promise<void> {
    // Implementation
  }
}

// Incorrect
class Database {
  _connection: Connection | null = null;

  _connect(): Promise<void> {
    // Implementation
  }
}
`````
[//]: # (@formatter:on)

## 9 Architecture and design

### 9.1 Data-driven design

TypeScript scripts and utilities **must** be entirely data-driven. Logic routing, internal state thresholds, and functional options **must** be passed via typed objects or schemas instead of being hard-coded directly inside execution structures.

### 9.2 Zero hardcoding

Hardcoding structural keys, operational paths, or user-facing strings is strictly prohibited. Configuration parameters **must** be separated from functional source lines and pulled dynamically from designated management configurations.

### 9.3 Modular decoupling

Code is organized into small, focused modules with single responsibilities. Circular dependencies are forbidden. Modules export only what is necessary for public use.

### 9.4 Text and logging

Any user-facing text, console notification, or system logging **must** utilize the **MiniMessage** parsing paradigm, regardless of whether the specific repository operates within a Minecraft environment. The use of legacy formatting markers or direct concatenation for styled console text is banned.

### 9.5 Pure functions

Functions are preferred to be pure (no side effects) when possible. State mutations are explicit and localized. Immutable data structures are preferred.

### 9.6 Dependency injection

Dependency injection is used for testability and modularity. Classes receive dependencies through constructors rather than creating them internally.

[//]: # (@formatter:off)
`````typescript
// Correct
class UserService {
  constructor(private readonly database: Database) {
  }

  async getUser(id: string): Promise<User | null> {
    return this.database.findById(id);
  }
}

// Incorrect
class UserService {
  private database = new Database();

  async getUser(id: string): Promise<User | null> {
    return this.database.findById(id);
  }
}
`````
[//]: # (@formatter:on)

## 10 Documentation and comments

### 10.1 TSDoc documentation

Every public-facing function, utility module, and custom class constructor **must** feature a comprehensive TSDoc block outlining its core objective, expected parameters (`@param`), return types (`@returns`), and thrown exceptions (`@throws`).

[//]: # (@formatter:off)
`````typescript
/**
 * Retrieves a user by their unique identifier.
 * @param id - The unique identifier of the user.
 * @returns A promise that resolves to the user object, or null if not found.
 * @throws {Error} Throws an error if the database query fails.
 */
async function getUserById(id: string): Promise<User | null> {
  // Implementation
}
`````
[//]: # (@formatter:on)

### 10.2 Variable documentation

TSDoc is required for "complicated" variables. This includes objects with complex structures, configuration objects, and instances where the behavior is not immediately evident from the property name alone. Simple, self-evident primitives do not require TSDoc.

### 10.3 Inline comments

Code logic **must** prioritize self-documenting syntax via descriptive names and type annotations. Inline explanations (`//`) are restricted solely to areas containing inherently complex algorithms or mathematical boundaries that cannot be simplified through normal refactoring.

### 10.4 Comment formatting

Block comments (`/* */`) are used for multi-line explanations. Line comments (`//`) are used for single-line explanations. Comments are placed above the code they describe, not on the same line.

## 11 Performance and optimization

### 11.1 Type checking performance

1. **Avoid complex types:** Overly complex type definitions can slow down the compiler.
2. **Incremental compilation:** Incremental compilation is enabled in `tsconfig.json`.
3. **Project references:** Large projects use project references to improve build times.

### 11.2 Runtime performance

1. **Tree-shaking:** Code is written with tree-shaking in mind. Side effects in modules are avoided.
2. **Bundle size:** Large dependencies are scrutinized for necessity. Type-only imports are used when possible.
3. **No type erasure overhead:** Types do not exist at runtime, but type guards and assertions should be efficient.

### 11.3 Memory management

Explicit cleanup is performed for resources that require manual disposal (event listeners, timers, file handles). Memory leaks from retained references are avoided.

## Conclusion

Adherence to this style guide ensures that the MineKot ecosystem remains consistent, readable, and maintainable. By following these standards, contributors help uphold the quality and performance expectations of the project.
