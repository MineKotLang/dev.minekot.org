---
title: "JSON Style Guide"
params:
  menuPre: "<i class=\"devicons devicons-json\"></i> "
weight: 5
---

## 1 Introduction

This document serves as the complete definition of MineKot's coding standards for source code in `JavaScript Object Notation (JSON)`. A JSON source file is described as being in MineKot Style if and only if it adheres to the rules herein.

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

JSON code has a column limit of `120 characters`. Except as noted, any line that would exceed this limit is line-wrapped.

### 3.3 Formatter control

Formatter control comments are enabled. Code regions can be excluded from formatting using JSONC comments:

`````jsonc
// Correct
// prettier-ignore
{"key":"value","key2":"value2"}
```

### 3.4 Key naming

Keys use `kebab-case` and are descriptive:

`````json
// Correct
{
  "database-host": "localhost",
  "connection-timeout": 30,
  "max-connections": 100
}

// Incorrect
{
  "databaseHost": "localhost",
  "connectionTimeout": 30,
  "maxConnections": 100
}
`````

### 3.5 Quoting

All keys and string values must be enclosed in double quotes. Single quotes are forbidden:

`````json
// Correct
{
  "name": "John Doe",
  "email": "john@example.com"
}

// Incorrect
{
  'name': 'John Doe',
  'email': 'john@example.com'
}
`````

### 3.6 Trailing commas

Trailing commas are forbidden in standard JSON. JSON5 or JSONC may allow trailing commas when explicitly supported:

`````json
// Correct - Standard JSON
{
  "name": "John Doe",
  "email": "john@example.com"
}

// Incorrect - Trailing comma in standard JSON
{
  "name": "John Doe",
  "email": "john@example.com"
}
`````

### 3.7 Whitespace

Spaces are used after colons in key-value pairs and after commas. No spaces are used before colons or commas:

`````json
// Correct
{
  "name": "John Doe",
  "email": "john@example.com"
}

// Incorrect
{
  "name": "John Doe",
  "email": "john@example.com"
}
`````

## 4 Data types

### 4.1 Strings

Strings are enclosed in double quotes. Special characters are escaped as needed:

`````json
// Correct
{
  "message": "Hello, World!",
  "path": "/path/to/file",
  "special": "This contains \"quotes\""
}

// Incorrect
{
  'message': 'Hello, World!',
  "path": '/path/to/file'
}
`````

### 4.2 Numbers

Numbers are written without quotes. Floating-point numbers use decimal notation:

`````json
// Correct
{
  "port": 5432,
  "timeout": 30.5,
  "percentage": 0.75
}

// Incorrect
{
  "port": "5432",
  "timeout": "30.5"
}
`````

### 4.3 Booleans

Boolean values use lowercase `true` and `false`:

`````json
// Correct
{
  "enabled": true,
  "debug": false
}

// Incorrect
{
  "enabled": True,
  "debug": False,
  "enabled": yes,
  "debug": no
}
`````

### 4.4 Null values

Null values are represented as `null`:

`````json
// Correct
{
  "timeout": null,
  "retry-count": null
}

// Incorrect
{
  "timeout": Null,
  "retry-count": NULL,
  "timeout": undefined
}
`````

### 4.5 Arrays

Arrays use square brackets with consistent indentation:

`````json
// Correct
{
  "servers": [
    {
      "host": "server1.example.com",
      "port": 8080
    },
    {
      "host": "server2.example.com",
      "port": 8080
    }
  ]
}

// Incorrect
{
  "servers": [
    {
      "host": "server1.example.com",
      "port": 8080
    },
    {
      "host": "server2.example.com",
      "port": 8080
    }
  ]
}
`````

### 4.6 Objects

Objects use curly braces with consistent indentation:

`````json
// Correct
{
  "database": {
    "host": "localhost",
    "port": 5432,
    "name": "mydb"
  }
}

// Incorrect
{
  "database": {
    "host": "localhost",
    "port": 5432,
    "name": "mydb"
  }
}
`````

## 5 Key ordering

### 5.1 Alphabetical ordering

Object keys are ordered alphabetically for consistency:

`````json
// Correct
{
  "database-host": "localhost",
  "database-name": "mydb",
  "database-port": 5432,
  "enabled": true
}

// Incorrect
{
  "enabled": true,
  "database-host": "localhost",
  "database-port": 5432,
  "database-name": "mydb"
}
`````

### 5.2 Logical grouping

Related keys are grouped together when alphabetical ordering would separate them:

`````json
// Correct
{
  "database-host": "localhost",
  "database-name": "mydb",
  "database-port": 5432,
  "cache-enabled": true,
  "cache-ttl": 3600
}

// Incorrect - Mixed grouping
{
  "cache-enabled": true,
  "database-host": "localhost",
  "cache-ttl": 3600,
  "database-name": "mydb"
}
`````

## 6 Number precision

### 6.1 Integer values

Integer values are used for whole numbers:

`````json
// Correct
{
  "port": 5432,
  "count": 100
}

// Incorrect
{
  "port": 5432.0,
  "count": 100.0
}
`````

### 6.2 Floating-point values

Floating-point values are used for decimal numbers. Precision is appropriate to the use case:

`````json
// Correct
{
  "percentage": 0.75,
  "ratio": 1.618
}

// Incorrect
{
  "percentage": 0.7500000000000000,
  "ratio": 1.618033988749895
}
`````

### 6.3 Scientific notation

Scientific notation is avoided unless necessary for very large or very small numbers:

`````json
// Correct
{
  "timeout": 3000000000
}

// Acceptable - When necessary
{
  "timeout": 3e9
}
`````

## 7 Date and time formats

### 7.1 ISO 8601 format

Dates and times use ISO 8601 format:

`````json
// Correct
{
  "created-at": "2024-01-15T10:30:00Z",
  "updated-at": "2024-01-15T10:30:00+00:00"
}

// Incorrect
{
  "created-at": "01/15/2024 10:30:00",
  "updated-at": "2024-01-15 10:30:00"
}
`````

### 7.2 Unix timestamps

Unix timestamps are used when numeric representation is preferred:

`````json
// Correct
{
  "created-at": 1705317000
}

// Incorrect
{
  "created-at": "1705317000000"
}
`````

### 7.3 Consistency

Date formats are consistent within a single JSON file or across related files:

`````json
// Correct - Consistent ISO 8601
{
  "created-at": "2024-01-15T10:30:00Z",
  "updated-at": "2024-01-16T11:45:00Z"
}

// Incorrect - Mixed formats
{
  "created-at": "2024-01-15T10:30:00Z",
  "updated-at": 1705403100
}
`````

## 8 Architecture and design

### 8.1 Data-driven foundation

JSON configuration templates represent the core blueprint of MineKot's data-driven architecture. Files must explicitly define configurations, options, and operational constants. Code logic must read from these files rather than evaluating inline conditions.

### 8.2 Schema integrity

All active JSON configuration sets must maintain a clear, self-describing layout. Missing parameters or empty data scopes should be avoided; use explicit empty types or fallback values where necessary to ensure parsing tools resolve contexts quickly.

### 8.3 Hierarchical structure

JSON files maintain a clear hierarchical structure. Deep nesting is avoided when possible:

`````json
// Correct
{
  "database": {
    "host": "localhost",
    "port": 5432,
    "credentials": {
      "username": "admin",
      "password": "secret"
    }
  }
}

// Incorrect - Excessive nesting
{
  "config": {
    "database": {
      "connection": {
        "parameters": {
          "host": "localhost",
          "port": 5432
        }
      }
    }
  }
}
`````

### 8.4 Grouping

Related configuration items are grouped together:

`````json
// Correct
{
  "database": {
    "host": "localhost",
    "port": 5432,
    "name": "mydb"
  },
  "cache": {
    "enabled": true,
    "ttl": 3600
  }
}

// Incorrect - Mixed grouping
{
  "database": {
    "host": "localhost"
  },
  "cache": {
    "enabled": true
  },
  "database": {
    "port": 5432
  }
}
`````

## 9 Documentation

### 9.1 JSONC comments

JSONC (JSON with Comments) files use `//` for single-line comments and `/* */` for multi-line comments:

`````jsonc
// Correct - JSONC
{
  // Database configuration
  "database": {
    "host": "localhost",
    "port": 5432
  }
}

/* Multi-line comment
   describing the configuration */
`````

### 9.2 Standard JSON

Standard JSON files do not include comments. Documentation is provided through external schema files or separate documentation files.

### 9.3 Schema validation

JSON schemas are used to document and validate JSON structure:

`````json
// Schema example
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "properties": {
    "name": {
      "type": "string",
      "description": "The user's name"
    },
    "email": {
      "type": "string",
      "format": "email",
      "description": "The user's email address"
    }
  },
  "required": [
    "name",
    "email"
  ]
}
`````

## 10 Security

### 10.1 Sensitive data

Sensitive data (passwords, API keys) is not stored in plain text JSON files. Environment variables or secret management systems are used:

`````json
// Correct - Reference environment variable (when supported by parser)
{
  "database-password": "${DB_PASSWORD}"
}

// Incorrect - Plain text password
{
  "database-password": "mysecretpassword"
}
`````

### 10.2 File permissions

JSON files containing sensitive information have restricted file permissions (600 or 640).

## 11 Performance and optimization

### 11.1 File size

JSON files are kept concise. Unnecessary whitespace is minimized in production.

### 11.2 Parsing efficiency

JSON structures are designed for efficient parsing. Deep nesting and complex nested objects are avoided when possible.

### 11.3 Minification

JSON files are minified in production to reduce file size:

`````json
// Minified
{
  "name": "John Doe",
  "email": "john@example.com"
}
`````

## 12 JSON5 and JSONC

### 12.1 JSON5

JSON5 extends JSON with features like trailing commas, single quotes, and unquoted keys. These features are used only when explicitly supported by the parser:

`````json5
// JSON5 example
{
  // Comments
  name: "John Doe",
  email: 'john@example.com',
}
`````

### 12.2 JSONC

JSONC adds comment support to JSON. Comments are used for documentation in configuration files:

`````jsonc
// JSONC example
{
  // User configuration
  "name": "John Doe",
  "email": "john@example.com"
}
`````

### 12.3 Standard JSON preference

Standard JSON is preferred for data exchange and APIs. JSON5 and JSONC are used only for configuration files where the parser explicitly supports them.

## Conclusion

Adherence to this style guide ensures that the MineKot ecosystem remains consistent, readable, and maintainable. By following these standards, contributors help uphold the quality and performance expectations of the project.
