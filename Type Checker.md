# Overview
This document describes the Type Checker object type.

# Purpose
A Type Checker object is an object which checks if a given object type is recognized and directly instantiable by the client.  This should be used before attempting to instantiate any non-standard object type.

# Properties
## Instantiability
The Type Checker is directly instantiable with an identifier calculated as specified in the Standard Identifier specification.

## Methods
The Type Checker is a method object.  The server may only call the Check Type method, and the client may only call the Check Type Result method.

# Methods
## Server-callable
### Check Type
Check Type is a query by the server to check if the client recognizes a given type.  The following parameters are passed:
- 256-byte identifier of the type to check

The client should immediately make a Check Type Result call with the given type.

## Client-callable
### Check Type Result
Check Type Result is a response to the Check Type query.  The following parameters are passed:
- 256-byte identifier of the type that was checked
- 1 byte: 0 if the type is recognized and directly instantiable (that is, it would not error under a call to the Root Constructor) and 1 otherwise (that is, a call to the Root Constructor would result in undefined behaviour)