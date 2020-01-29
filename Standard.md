# Overview
Polish is an RPC interface between a client and server.  It is designed for extensibility and interoperability.

Polish also contains a set of Standard Modules that are designed specifically to support desktop games.

# Connection
The method of communication is left up to a particular implementation.  Any binary-safe stream or datagram protocol which ensures that messages are recieved both reliably and in order is acceptable.  Examples of usable protocols include TCP, Websockets (in binary mode), enet, a local UNIX socket, or via bidirectional HTTP requests (with the client periodically requesting any new information).  When not operating locally, one may want to consider encrypting the protocol to ensure it is not tampered with.

Negotiation of the communication protocol, as well as the address and port to connect to, is also left up to the particular implementation.  For example, this may accomplished by launching the application with this information passed as an argument or by prompting the user for this information.  The rest of this document assumes that such a connection has been made and both sides comply with this document.

# Objects
During the course of operation, each side maintains an ordered list of objects.  The behaviour of each object is documented in an object type specification as referenced when it is created.  Each object may contain private data in order to fulfill the specification, but implementations may do this in any format.  The specification also lists zero or more methods which may be called on each object, either by the server (with the operation performed on the client) or the client (with the operation performed by the server), the behaviour of which is specified in the specification for the object.

If an object contains at most one server and at most one client method, it may be marked on its specification as a Method Object.  When the method is called, the ID of the method is omitted from the procedure call.

Object IDs must refer to the same object on both sides, and must not be re-used unless they are deleted by some well-defined means (that is, both the client and server are aware of its removal).

# Remote procedure call
At any time, the server or client may call a method on the other machine.  For the purposes of this section, the machine initiating the call is the caller, and the other is called the operator.  In order to call a method, a message with the following payload is sent:

- The ID of the object (as a 64-bit unsigned integer)
- If the object is not a method object, the ID of the method (as a 64-bit unsigned integer)
- Any parameters as specified by the method specification

Once the call is receieved, the method is performed in the order it is receieved.  If the operator needs to return some information about the call, it should do so in a separate remote procedure call.

# Initialization
At initialization, an object of the type Root Constructor is created and placed at object ID 0.