# Overview
Polish is an RPC interface between a client and server.  It is designed for extensibility and interoperability.

Polish also contains a set of Standard Modules that are designed specifically to support desktop games.

# Connection
The method of communication is left up to a particular implementation.  Any binary-safe stream or datagram protocol which ensures that messages are recieved both reliably and in order is acceptable.  Examples of usable protocols include TCP, Websockets (in binary mode), enet, a local UNIX socket, or via bidirectional HTTP requests (with the client periodically requesting any new information).  When not operating locally, one may want to consider encrypting the protocol to ensure it is not tampered with.

Negotiation of the communication protocol, as well as the address and port to connect to, is also left up to the particular implementation.  For example, this may accomplished by launching the application with this information passed as an argument or by prompting the user for this information.  The rest of this document assumes that such a connection has been made.

# Objects
During the course of operation, each side maintains a set of objects; each is indexed by a UUID.  The behaviour of each object is documented in an object type specification as referenced when it is created.  Each object may contain private data in order to fulfill the specification, but implementations may do this in any format.

The specification also lists and describes zero or more methods which may be called on each object, either by the server (with the operation performed on the client) or the client (with the operation performed by the server).  Each method contains a name, and the ID of the object-method is computed by taking the UUIDv5 using the object's UUID and the method name.

Some objects are designated as Singleton objects.  Singleton objects MUST exist at startup if they are supported by the client and the server.  Singleton objects MUST NOT alter behaviour if no methods are called on them.  A client or server MUST be able to operate if the other party supports a singleton that it does not.  A client or server SHOULD attempt to operate if the other party does not support a singleton that it does; unless that singleton is necessary for operation.  A client or server SHOULD ensure that the other party supports a particular singleton before attempting to use it when possible.

# Remote procedure call
At any time, the server or client may call a method on the other machine.  For the purposes of this section, the machine initiating the call is the caller, and the other is called the operator.  In order to call a method, a message with the following payload is sent:

- The ID of the object-method (128-bit UUID)
- Any parameters as specified by the method specification

Once the call is receieved, the method is performed in the order it is receieved.  If the operator needs to return some information about the call, it does so in a separate remote procedure call.

# Standard Data Types
## UUID
A UUID is sent as the 128 bits (16 bytes) comprising it.
## Fixed-size integer (int)
A fixed size integer is sent directly as the bits comprising it in Little Endian format.
## Variable-size integer (varint)
A integer-size integer is sent as:
- If the integer is one byte in size, and is not 0xfd, 0xfe, or 0xff:
-- 1 byte: the integer in Little Endian format
- If the integer is at most two bytes in size:
-- 1 byte: 0xfd
-- 2 bytes: the integer in Little Endian format
- If the integer is at most four bytes in size:
-- 1 byte: 0xfe
-- 4 bytes: the integer in Little Endian format
- Otherwise:
-- 1 byte: 0xff
-- 8 bytes: the integer in Little Endian format

A variable-size integer SHOULD be used when a integer MAY be up to 8 bytes, but is typically smaller.

The variable-size integer MAY be signed or unsigned; this must be specified when used.  When signed, the smallest size where all removed bytes are ff SHOULD be used.

## String
A string is is sent as:
- An unsigned varint comprising the number of bytes in the string
- The string encoded in UTF-8 format

Due to the fact that one ASCII character takes up one character, for string creation of purely ASCII strings, the following may be treated as:
- An unsigned varint comprising the number of characters in the string
- The string encoded in ASCII format
