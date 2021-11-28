# Overview
This document describes the Singleton Checker object type.

# Purpose
The Singleton Checker object is an object which checks if a singleton exists and is recognized given an ID.

# Properties
## Singleton
This object is a singleton, identified by the UUID d04e215a-c521-43df-bc7d-866484a8098b.

# Methods
## Server-callable
### Check Singleton
Check Type is a query by the server to check if the client recognizes a given type.  The following parameters are passed:
- 128-bit UUID to check

The client should immediately make a Check Singleton Result call with the given type.

## Client-callable
### Check Singleton Result
Check Singleton Result is a response to the Check Singleton query.  The following parameters are passed:
- 128-bit UUID that was checked
- 1 byte: Response code as listed below:
-- 0: The singleton is recognized and exists
-- 1: The singleton is not recognized (generic error)
-- 2: The given UUID exists but is not a singleton
-- 3: The singleton is recognzied but is not implemented

The client MAY respond with 1 in any case where the singleton does not exist; it is not required to implement any other response codes.
The server MAY treat any response code other than 0 as an error; it is not required to implement any custom behaviour.