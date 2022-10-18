# sisbase-api
`Type: Subproject`  
Subproject of: [`SisbaseKT`](../sisbasekt.md)

A collection of interfaces representing the Discord API Spec.  
Implemented by `Backends`.  

## Dependencies
[types](types.md)  

## Project Structure
`org.sisbase.sisbase-api`  
Base package  

`org.sisbase.sisbase-api.rest`  
High level wrapper for Discord's Rest API  

`org.sisbase.sisbase-api.gateway`  
High level wrapper for Discord's Gateway  

## Usage

### Implementing a Backend
Backends are the actual libraries that connect to discord.  
An example of a valid backend would be Discord4J.  

To implement a gateway backend, extend `GatewayBackend`  
To implement a rest backend, extend `RestBackend`  
To implement a backend that supports both, extend `AbstractBackend`  

```kt
package org.siscode.sisbasekt-backends.discord4j
class Discord4JBackend() : AbstractBackend {
	public restBackend = Discord4JRest();
	public gatewaybackend = Discord4JGateway()
}
```

