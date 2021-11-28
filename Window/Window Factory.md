# Overview
This document describes the Window Factory object type.

# Purpose
A Window Factory object is an object which is able to instantiate windows.

# Properties
## Singleton
This object is a singleton, identified by the UUID cf54a8e1-96aa-42e2-ac24-095824acdedf.

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