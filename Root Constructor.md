# Overview
This document describes the Root Constructor object type.

# Purpose
A Root Constructor object is an object that is instantiated at the beginning of operation and is used to instantiate objects that are marked as Directly Instantiable.

# Properties
## Instantiability
The Root Constructor is not instantiable; it is created at the beginning of the connection.

## Methods
The Root Constructor is a method object.  The server may only call the Instantiate method.

# Methods
## Server-callable
### Instantiate
The Root Constructor may be called to construct an object which is marked as Directly Instantiable.  It is called with the following parameter:
- 256-byte identifier of the type to construct

An object of the type matching this identifier is constructed and placed at the first available object identifier, which may also be computed by the server to identify the new object.

If the identifier is unknown to the client, the behaviour is undefined.  Servers should use a method such as Type Checker to determine if non-standard object types are recognize and take appropriate behaviour if not.