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

The global default indent size is two spaces. The use of literal tab characters (`\t`) is structurally prohibited by the language standard and this guide; indentation must consist purely of spaces.

### 3.2 Column limit

YAML code has a column limit of `120 characters`. Except as noted, any line that would exceed this limit is line-wrapped.

### 3.3 Formatter control

Formatter control comments are enabled. Code regions can be excluded from formatting using YAML comments:

[//]: # (@formatter:off)
`````yaml
# Correct
# prettier-ignore
key1:value1 key2:value2
`````
[//]: # (@formatter:on)

### 3.4 Key naming

Keys use `kebab-case` and are descriptive:

[//]: # (@formatter:off)
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
[//]: # (@formatter:on)

### 3.5 Quoting

Double quotes are used for strings that contain special characters or require escaping. Single quotes are forbidden:

[//]: # (@formatter:off)
`````yaml
# Correct
message: "Hello, World!"
path: "/path/to/file"
special: "This contains \"quotes\""

# Incorrect
message: 'Hello, World!'
path: '/path/to/file'
`````
[//]: # (@formatter:on)

### 3.6 Comments

Comments are placed on their own lines and use the `#` character:

[//]: # (@formatter:off)
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
[//]: # (@formatter:on)

## 4 Data types

### 4.1 Booleans

Boolean values use lowercase `true` and `false`:

[//]: # (@formatter:off)
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
[//]: # (@formatter:on)

### 4.2 Null values

Null values are represented as `null` or `~`:

[//]: # (@formatter:off)
`````yaml
# Correct
timeout: null
retry-count: ~

# Incorrect
timeout: Null
retry-count: NULL
`````
[//]: # (@formatter:on)

### 4.3 Numbers

Numbers are written without quotes. Floating-point numbers use decimal notation:

[//]: # (@formatter:off)
`````yaml
# Correct
port: 5432
timeout: 30.5
percentage: 0.75

# Incorrect
port: "5432"
timeout: "30.5"
`````
[//]: # (@formatter:on)

### 4.4 Strings

Strings are unquoted unless they contain special characters or could be mistaken for other data types:

[//]: # (@formatter:off)
`````yaml
# Correct
name: John Doe
message: "Hello, World!"
path: "/path/to/file"

# Incorrect
name: "John Doe"
message: Hello, World!
`````
[//]: # (@formatter:on)

## 5 Multi-line strings

### 5.1 Literal block scalar

Literal block scalars (`|`) preserve newlines and are used for multi-line strings where formatting matters:

[//]: # (@formatter:off)
`````yaml
# Correct
description: |
  This is a multi-line
  string that preserves
  line breaks.

# Incorrect
description: "This is a multi-line\nstring that preserves\nline breaks."
`````
[//]: # (@formatter:on)

### 5.2 Folded block scalar

Folded block scalars (`>`) fold newlines into spaces and are used for long text:

[//]: # (@formatter:off)
`````yaml
# Correct
message: >
  This is a long message that
  will be folded into a single
  line when parsed.

# Incorrect
message: "This is a long message that will be folded into a single line when parsed."
`````
[//]: # (@formatter:on)

### 5.3 Chomping indicators

Chomping indicators control trailing newlines:

[//]: # (@formatter:off)
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
[//]: # (@formatter:on)

## 6 Anchors and aliases

### 6.1 Anchor definition

Anchors are defined using `&` and referenced using `*`:

[//]: # (@formatter:off)
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
[//]: # (@formatter:on)

### 6.2 Alias usage

Aliases are used to avoid duplication. Anchors are defined at the top level or within logical sections:

[//]: # (@formatter:off)
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
[//]: # (@formatter:on)

### 6.3 Merge keys

Merge keys (`<<`) are used to include anchor contents:

[//]: # (@formatter:off)
`````yaml
# Correct
base: &base
  enabled: true
  timeout: 30

extended:
  <<: *base
  custom-field: value
`````
[//]: # (@formatter:on)

## 7 Lists and arrays

### 7.1 List formatting

Lists use hyphen notation with consistent indentation:

[//]: # (@formatter:off)
`````yaml
# Correct
servers:
  - host: server1.example.com
    port: 8080
  - host: server2.example.com
    port: 8080

# Incorrect
servers:
  - {host: server1.example.com, port: 8080}
  - {host: server2.example.com, port: 8080}
`````
[//]: # (@formatter:on)

### 7.2 Inline lists

Inline lists are used only for simple, short lists:

[//]: # (@formatter:off)
`````yaml
# Correct
tags: [ production, web, api ]

# Incorrect
servers:
  - [ server1, server2, server3 ]
`````
[//]: # (@formatter:on)

### 7.3 List ordering

Lists are ordered logically (alphabetical, priority, or sequence):

[//]: # (@formatter:off)
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
[//]: # (@formatter:on)

## 8 Architecture and design

### 8.1 Data-driven engine integration

YAML configurations serve as data-driven control maps for the engine. Logic boundaries, plugin definitions, event triggers, and database targets must be stored inside clear keys rather than being evaluated inline.

### 8.2 Absolute zero hardcoding

Hardcoding user-facing messages, console responses, or system pathways within engine binaries is strictly forbidden. All custom text fields must be extracted into clean YAML mappings that utilize the **MiniMessage** framework for final presentation output. Hardcoded string concatenations or legacy styling codes are completely banned.

### 8.3 Hierarchical structure

YAML files maintain a clear hierarchical structure. Deep nesting is avoided when possible:

[//]: # (@formatter:off)
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
[//]: # (@formatter:on)

### 8.4 Grouping

Related configuration items are grouped together:

[//]: # (@formatter:off)
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
[//]: # (@formatter:on)

## 9 Documentation

### 9.1 File documentation

YAML files include a header comment describing their purpose:

[//]: # (@formatter:off)
`````yaml
# ===================================
# Application Configuration
# Defines runtime parameters for
# the application server.
# ===================================
`````
[//]: # (@formatter:on)

### 9.2 Section documentation

Major sections are documented with comments:

[//]: # (@formatter:off)
`````yaml
# Database connection settings
database:
  host: localhost
  port: 5432
`````
[//]: # (@formatter:on)

### 9.3 Value documentation

Complex or non-obvious values are documented:

[//]: # (@formatter:off)
`````yaml
# Timeout in seconds for database connections
timeout: 30
`````
[//]: # (@formatter:on)

## 10 Security

### 10.1 Sensitive data

Sensitive data (passwords, API keys) is not stored in plain text YAML files. Environment variables or secret management systems are used:

[//]: # (@formatter:off)
`````yaml
# Correct - Reference environment variable
database:
  password: ${DB_PASSWORD}

# Incorrect - Plain text password
database:
  password: mysecretpassword
`````
[//]: # (@formatter:on)

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
