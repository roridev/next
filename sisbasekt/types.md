# sisbase-types

`Type: Shared Dependency`  
[Go Back to Project Outline](../sisbasekt.md)  


Discord data types.  

The types from this project will be used by all other sisbasekt projects.  

The objective of this project is to serve as a bridge between different backends, so all objects here will be high level and directly reflect the official API documentation.  

## Package Structure
`org.siscode.sisbase-types`  
Base Package.  

`org.siscode.sisbase-types.rest`  
Discord Rest API Types.  

`org.siscode.sisbase-type.gateway`  
Discord Gateway Types.  

`org.siscode.sisbase-type.annotation`  
Annotations used by the library  

## Annotations

### `@Since`
Describes which version of the API added that Type.  

| Field     | Type              | Description                              |
|-----------|-------------------|------------------------------------------|
| `version` | DiscordApiVersion | Version of the API which added that type |

## Other Types

### `DiscordApiVersion`
An Enum containing all Discord API Versions so that no invalid versions can be represented.

