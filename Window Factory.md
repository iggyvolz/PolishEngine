# Overview
This document describes the Window Factory object type.

# Purpose
A Window Factory object is an object which is able to instantiate windows.

# Properties
## Instantiability
The Window Factory is directly instantiable with an identifier calculated as specified in the Standard Identifier specification.

## Methods
The Window Factory is a method object.  The server may only call the Create Window method.

# Methods
## Server-callable
### Create Window
The Create Window method creates a Window object with the following parameters:
- 4-byte unsigned integer: Width of the window
- 4-byte unsigned integer: Height of the window
- 4-byte unsigned integer: X-position of the window (from left)
- 4-byte unsigned integer: Y-position of the window (from top)

# Errata
This section is non-normative.
## Future scope
This standard may be expanded to support fullscreen windows, or querying the client for its capabilities.