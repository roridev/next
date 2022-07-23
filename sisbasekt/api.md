# sisbase-api
`Type: Subproject`  
Subproject of: [`SisbaseKT`](../sisbasekt.md)

A collection of interfaces representing the Discord API Spec.  
Implemented by `Backends`.  

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

### Developing extensions
Extension writers call the API provided by sisbase-api directly, and as such
can't know which backend will be chosen by the end user.  

In order to make sure your extension is propperly supported, extension writers can annotate their systems with `@RequireApiFeatures` and `@RequireApiVersion`.  

On the case of a backend not supporting said version or feature that system will be disabled and a warning will be printed to the console.  

```
[Sisbase-API] Dependency Error:
	Extension `id@version` requires
		Discord API Version >= X
	Backend `id@version` provides
		Discord API Version == Z

The following features will be disabled:
- Feature Name [Description]
- Feature Name [Description]

Please update or change the backend to get these features back.
```

Extension writers can also add `require-api-version` and `require-api-features` to the `mod.json` metadata file.  

On the case of a backend not supporting said version or features the extension won't be loaded and an error will be printed to the console.  

```
[Sisbase-API] Critical Dependency Failure:
	Extension `id@version` requires
		Discord API Version >= X
	Backend `id@version` provides
		Discord API Version == Z

Extension `id@version` is now disabled.

Please update or change the backend to get `id` back.
```

