---
title: "YAML Style Guide"
params:
  menuPre: "<i class=\"devicons devicons-yaml\"></i> "
weight: 8
---

## 1 Introduction

This document serves as the complete definition of MineKot's coding standards for source code in `YAML Ain't Markup Language (YAML)`. A YAML source file is described as being in MineKot Style if and only if it adheres to the rules herein.

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

The global default indent size is four spaces, and the tab width is four spaces. The use of literal tab characters (`\t`) is structurally prohibited by the language standard and this guide; indentation must consist purely of spaces.

### 3.2 Column limit

YAML code has a column limit of `120 characters`. Except as noted, any line that would exceed this limit is line-wrapped.

### 3.3 Formatter control

Formatter control comments are enabled. Code regions can be excluded from formatting using YAML comments:

`````yaml
# Correct
# prettier-ignore
key1:value1 key2:value2
```

### 3.4 Key naming

Keys use `kebab-case` and are descriptive:

  `````yaml
# Correct
database:
  host: localhost
  port: 5432
  connection-timeout: 30

# Incorrect
database:
  host: localhost
  port: 5432
  connectionTimeout: 30
`````

### 3.5 Quoting

Double quotes are used for strings that contain special characters or require escaping. Single quotes are forbidden:

`````yaml
# Correct
message: "Hello, World!"
path: "/path/to/file"
special: "This contains \"quotes\""

# Incorrect
message: 'Hello, World!'
path: '/path/to/file'
`````

### 3.6 Comments

Comments are placed on their own lines and use the `#` character:

`````yaml
# Correct
# Database configuration
database:
  host: localhost
  port: 5432

# Incorrect
database: # Database configuration
  host: localhost
`````

## 4 Data types

### 4.1 Booleans

Boolean values use lowercase `true` and `false`:

`````yaml
# Correct
enabled: true
debug: false

# Incorrect
enabled: True
debug: False
enabled: yes
debug: no
`````

### 4.2 Null values

Null values are represented as `null` or `~`:

`````yaml
# Correct
timeout: null
retry-count: ~

# Incorrect
timeout: Null
retry-count: NULL
`````

### 4.3 Numbers

Numbers are written without quotes. Floating-point numbers use decimal notation:

`````yaml
# Correct
port: 5432
timeout: 30.5
percentage: 0.75

# Incorrect
port: "5432"
timeout: "30.5"
`````

### 4.4 Strings

Strings are unquoted unless they contain special characters or could be mistaken for other data types:

`````yaml
# Correct
name: John Doe
message: "Hello, World!"
path: "/path/to/file"

# Incorrect
name: "John Doe"
message: Hello, World!
`````

## 5 Multi-line strings

### 5.1 Literal block scalar

Literal block scalars (`|`) preserve newlines and are used for multi-line strings where formatting matters:

`````yaml
# Correct
description: |
  This is a multi-line
  string that preserves
  line breaks.

# Incorrect
description: "This is a multi-line\nstring that preserves\nline breaks."
`````

### 5.2 Folded block scalar

Folded block scalars (`>`) fold newlines into spaces and are used for long text:

`````yaml
# Correct
message: >
  This is a long message that
  will be folded into a single
  line when parsed.

# Incorrect
message: "This is a long message that will be folded into a single line when parsed."
`````

### 5.3 Chomping indicators

Chomping indicators control trailing newlines:

`````yaml
# Correct - Strip trailing newlines (default)
description: |-
  Single line

# Correct - Keep trailing newlines
description: |+
  Multiple lines


# Incorrect - Inconsistent use
description: |
  Multiple lines
`````

## 6 Anchors and aliases

### 6.1 Anchor definition

Anchors are defined using `&` and referenced using `*`:

`````yaml
# Correct
defaults: &defaults
  timeout: 30
  retry: 3

production:
  <<: *defaults
  host: prod.example.com

development:
  <<: *defaults
  host: dev.example.com
`````

### 6.2 Alias usage

Aliases are used to avoid duplication. Anchors are defined at the top level or within logical sections:

`````yaml
# Correct
database-config: &db-config
  host: localhost
  port: 5432

primary-database:
  <<: *db-config
  name: primary

secondary-database:
  <<: *db-config
  name: secondary

# Incorrect - Duplication
primary-database:
  host: localhost
  port: 5432
  name: primary

secondary-database:
  host: localhost
  port: 5432
  name: secondary
`````

### 6.3 Merge keys

Merge keys (`<<`) are used to include anchor contents:

`````yaml
# Correct
base: &base
  enabled: true
  timeout: 30

extended:
  <<: *base
  custom-field: value
`````

## 7 Lists and arrays

### 7.1 List formatting

Lists use hyphen notation with consistent indentation:

`````yaml
# Correct
servers:
  - host: server1.example.com
    port: 8080
  - host: server2.example.com
    port: 8080

# Incorrect
servers:
  - host: server1.example.com
    port: 8080
  - host: server2.example.com
    port: 8080
`````

### 7.2 Inline lists

Inline lists are used only for simple, short lists:

`````yaml
# Correct
tags: [ production, web, api ]

# Incorrect
servers:
  - [ server1, server2, server3 ]
`````

### 7.3 List ordering

Lists are ordered logically (alphabetical, priority, or sequence):

`````yaml
# Correct - Alphabetical
servers:
  - alpha.example.com
  - beta.example.com
  - gamma.example.com

# Correct - Priority
servers:
  - primary.example.com
  - secondary.example.com
  - tertiary.example.com

# Incorrect - Random order
servers:
  - gamma.example.com
  - alpha.example.com
  - beta.example.com
`````

## 8 Architecture and design

### 8.1 Data-driven engine integration

YAML configurations serve as data-driven control maps for the engine. Logic boundaries, plugin definitions, event triggers, and database targets must be stored inside clear keys rather than being evaluated inline.

### 8.2 Absolute zero hardcoding

Hardcoding user-facing messages, console responses, or system pathways within engine binaries is strictly forbidden. All custom text fields must be extracted into clean YAML mappings that utilize the **MiniMessage** framework for final presentation output. Hardcoded string concatenations or legacy styling codes are completely banned.

### 8.3 Hierarchical structure

YAML files maintain a clear hierarchical structure. Deep nesting is avoided when possible:

`````yaml
# Correct
database:
  host: localhost
  port: 5432
  credentials:
    username: admin
    password: secret

# Incorrect - Excessive nesting
config:
  database:
    connection:
      parameters:
        host: localhost
        port: 5432
`````

### 8.4 Grouping

Related configuration items are grouped together:

`````yaml
# Correct
database:
  host: localhost
  port: 5432
  name: mydb

cache:
  enabled: true
  ttl: 3600

# Incorrect - Mixed grouping
database:
  host: localhost
cache:
  enabled: true
database:
  port: 5432
`````

## 9 Documentation

### 9.1 File documentation

YAML files include a header comment describing their purpose:

`````yaml
# ===================================
# Application Configuration
# Defines runtime parameters for
# the application server.
# ===================================
`````

### 9.2 Section documentation

Major sections are documented with comments:

`````yaml
# Database connection settings
database:
  host: localhost
  port: 5432
`````

### 9.3 Value documentation

Complex or non-obvious values are documented:

`````yaml
# Timeout in seconds for database connections
timeout: 30
`````

## 10 Security

### 10.1 Sensitive data

Sensitive data (passwords, API keys) is not stored in plain text YAML files. Environment variables or secret management systems are used:

`````yaml
# Correct - Reference environment variable
database:
  password: ${DB_PASSWORD}

# Incorrect - Plain text password
database:
  password: mysecretpassword
`````

### 10.2 File permissions

YAML files containing sensitive information have restricted file permissions (600 or 640).

## 11 Performance and optimization

### 11.1 File size

YAML files are kept concise. Unnecessary comments and whitespace are minimized.

### 11.2 Parsing efficiency

YAML structures are designed for efficient parsing. Deep nesting and complex anchors are avoided when possible.

### 11.3 Lazy loading

Large YAML configurations are split into multiple files and loaded as needed.

## Conclusion

Adherence to this style guide ensures that the MineKot ecosystem remains consistent, readable, and maintainable. By following these standards, contributors help uphold the quality and performance expectations of the project.
