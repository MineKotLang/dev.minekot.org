---
title: "XML Style Guide"
params:
  menuPre: "<i class=\"fa-solid fa-code\"></i> "
weight: 6
---

## 1 Introduction

This document serves as the complete definition of MineKot's coding standards for source code in `Extensible Markup Language (XML)`. An XML source file is described as being in MineKot Style if and only if it adheres to the rules herein.

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

The global default indent size is four spaces, and the tab width is four spaces. Continuation indent size is four spaces.

### 3.2 Column limit

XML code has a column limit of `120 characters`. Except as noted, any line that would exceed this limit is line-wrapped.

### 3.3 Element and attribute case

All element and attribute names must be written in lowercase:

`````xml
<!-- Correct -->
<user id="123" name="John Doe">
    <email>john@example.com</email>
</user>

<!-- Incorrect -->
<User ID="123" NAME="John Doe">
    <EMAIL>john@example.com</EMAIL>
</User>
````

### 3.4 Self-closing tags

Self-closing tags must include a space before the closing slash:

`````xml
<!-- Correct -->
<item />
<br />
<input type="text" />

<!-- Incorrect -->
<item/>
<br/>
<input type="text"/>
````

### 3.5 Attribute quoting

All attribute values must be enclosed in double quotes. Single quotes are forbidden:

`````xml
<!-- Correct -->
<user id="123" name="John Doe" />

<!-- Incorrect -->
<user id='123' name='John Doe' />
````

### 3.6 Line breaks

The automatic preservation of arbitrary line breaks is disabled. Elements must rely on structural rules to break naturally.

### 3.7 Attribute alignment

The alignment of attributes in dense vertical columns is disabled. Attributes are placed on the same line or wrapped based on column limit:

`````xml
<!-- Correct -->
<user id="123" name="John Doe" email="john@example.com" />

<!-- Correct - Wrapped for column limit -->
<user
    id="123"
    name="John Doe"
    email="john@example.com"
/>

<!-- Incorrect - Vertical alignment -->
<user id="123"    name="John Doe"    email="john@example.com" />
````

## 4 Attribute arrangement

### 4.1 Ordering sequence

To ensure absolute structural predictability, XML attributes are arranged using an immutable hierarchical ordering scheme. Attributes within any block element are grouped and prioritized according to the following strict sequence:

1. Core namespace schema mapping definitions (`xmlns:android`)
2. All other namespace declarations matching `xmlns:.*` (alphabetical order)
3. Primary resource identifiers matching the pattern `.*:id`
4. Primary component or entity names matching the pattern `.*:name`
5. Standard non-namespaced literal identifier attributes named `name`
6. Layout or item appearance descriptors named `style`
7. Generic non-namespaced structural attributes (alphabetical order)
8. System namespaced attributes matching standard namespace scopes
9. Remaining namespaced properties (alphabetical order)

`````xml
<!-- Correct -->
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/container"
    android:name="com.example.Container"
    name="container"
    style="@style/ContainerStyle"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:customAttribute="value"
/>

<!-- Incorrect - Random order -->
<LinearLayout
    android:layout_width="match_parent"
    xmlns:android="http://schemas.android.com/apk/res/android"
    app:customAttribute="value"
    android:id="@+id/container"
    style="@style/ContainerStyle"
/>
````

## 5 Namespaces

### 5.1 Namespace declarations

Namespace declarations are placed on the root element whenever possible:

`````xml
<!-- Correct -->
<root xmlns="http://example.com/ns" xmlns:custom="http://example.com/custom">
    <child />
</root>

<!-- Incorrect -->
<root xmlns="http://example.com/ns">
    <child xmlns:custom="http://example.com/custom" />
</root>
````

### 5.2 Namespace prefixes

Namespace prefixes are descriptive and consistent across the document:

`````xml
<!-- Correct -->
<root xmlns:minekot="http://minekot.org/ns" xmlns:android="http://schemas.android.com/apk/res/android">
    <minekot:component />
</root>

<!-- Incorrect -->
<root xmlns:ns1="http://minekot.org/ns" xmlns:ns2="http://schemas.android.com/apk/res/android">
    <ns1:component />
</root>
````

### 5.3 Default namespace

A default namespace is used for the primary schema to reduce prefix usage:

`````xml
<!-- Correct -->
<root xmlns="http://example.com/ns">
    <child />
</root>

<!-- Acceptable - When multiple namespaces needed -->
<root xmlns="http://example.com/ns" xmlns:other="http://other.com/ns">
    <child />
    <other:element />
</root>
````

## 6 Schema validation

### 6.1 Schema references

XML documents include schema references (DTD, XSD, or RelaxNG) for validation:

`````xml
<!-- Correct - XSD -->
<root xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://example.com/ns schema.xsd">
    <!-- Content -->
</root>

<!-- Correct - DTD -->
<!DOCTYPE root SYSTEM "schema.dtd">
<root>
    <!-- Content -->
</root>
````

### 6.2 Validation

All XML documents must validate against their declared schemas. Invalid documents are not committed.

### 6.3 Schema versioning

Schema versions are included in namespace URIs or schema locations:

`````xml
<!-- Correct -->
<root xmlns="http://example.com/ns/2024/01">
    <!-- Content -->
</root>

<!-- Incorrect -->
<root xmlns="http://example.com/ns">
    <!-- Content -->
</root>
````

## 7 CDATA sections

### 7.1 CDATA usage

CDATA sections are used for text content that contains characters that would otherwise require escaping:

`````xml
<!-- Correct -->
<description>
    <![CDATA[
        This text contains <special> characters & symbols.
    ]]>
</description>

<!-- Incorrect -->
<description>
    This text contains &lt;special&gt; characters &amp; symbols.
</description>
````

### 7.2 CDATA limitations

CDATA sections are not used for attribute values. Attribute values must use entity references:

`````xml
<!-- Correct -->
<code text="if (x &lt; 10) { return true; }" />

<!-- Incorrect -->
<code text="<![CDATA[if (x < 10) { return true; }]]>" />
````

## 8 Comments

### 8.1 Comment formatting

Comments are placed on their own lines and use the standard XML comment syntax:

`````xml
<!-- Correct -->
<!-- Main configuration section -->
<config>
    <!-- Database settings -->
    <database>
        <host>localhost</host>
    </database>
</config>

<!-- Incorrect -->
<config><!-- Main configuration section -->
    <database><!-- Database settings -->
        <host>localhost</host>
    </database>
</config>
````

### 8.2 Comment content

Comments are descriptive and do not duplicate information evident from the element names:

`````xml
<!-- Correct -->
<!-- Maximum number of concurrent connections -->
<maxConnections>100</maxConnections>

<!-- Incorrect -->
<!-- The maxConnections element sets the maximum connections -->
<maxConnections>100</maxConnections>
````

## 9 Architecture and design

### 9.1 Data abstraction

XML configurations must strictly focus on structural layout representation and dependency definitions. Hardcoded interface strings, values, or environment properties are banned; externalize them completely into data providers or language bundles.

### 9.2 Hierarchical structure

XML documents maintain a clear hierarchical structure. Deep nesting is avoided when possible:

`````xml
<!-- Correct -->
<config>
    <database>
        <host>localhost</host>
        <port>5432</port>
    </database>
    <cache>
        <enabled>true</enabled>
    </cache>
</config>

<!-- Incorrect - Excessive nesting -->
<config>
    <settings>
        <database>
            <connection>
                <parameters>
                    <host>localhost</host>
                    <port>5432</port>
                </parameters>
            </connection>
        </database>
    </settings>
</config>
````

### 9.3 Element naming

Element names use `kebab-case` and are descriptive:

`````xml
<!-- Correct -->
<user-profile>
    <first-name>John</first-name>
    <last-name>Doe</last-name>
</user-profile>

<!-- Incorrect -->
<userProfile>
    <firstName>John</firstName>
    <lastName>Doe</lastName>
</userProfile>
````

### 9.4 Attribute vs element

Attributes are used for metadata and simple values. Elements are used for complex content:

`````xml
<!-- Correct -->
<user id="123" active="true">
    <name>John Doe</name>
    <email>john@example.com</email>
</user>

<!-- Incorrect -->
<user>
    <id>123</id>
    <active>true</active>
    <name>John Doe</name>
</user>
````

## 10 Documentation

### 10.1 XML documentation

Complex XML structures are documented with comments explaining purpose and relationships:

`````xml
<!-- ===================================
     Database Configuration
     Defines connection parameters for
     the primary database instance.
     =================================== -->
<database>
    <host>localhost</host>
    <port>5432</port>
</database>
````

### 10.2 Schema documentation

XSD schemas include documentation elements:

`````xml
<xs:element name="user">
    <xs:annotation>
        <xs:documentation>
            Represents a user in the system.
        </xs:documentation>
    </xs:annotation>
    <xs:complexType>
        <!-- Content -->
    </xs:complexType>
</xs:element>
````

## 11 Performance and optimization

### 11.1 File size

XML files are kept concise. Unnecessary whitespace and comments are removed in production.

### 11.2 Parsing efficiency

XML structures are designed for efficient parsing. Deep nesting and excessive attributes are avoided.

### 11.3 External entities

External entities are avoided for security reasons (XXE attacks). All content is included inline or referenced through secure mechanisms.

## Conclusion

Adherence to this style guide ensures that the MineKot ecosystem remains consistent, readable, and maintainable. By following these standards, contributors help uphold the quality and performance expectations of the project.
