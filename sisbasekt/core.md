# sisbase-core
`Type: Subproject`  
Subproject of : [`SisbaseKT`](../sisbasekt.md)

Discord extension manager

## Features

- Commands `META!`  
  User-provided commands following the contract:  
  `<PREFIX>Command [Arguments...]`  
  The prefix is usually ommited from the extension unless the `@RequiresPrefixes` annotation is used.  
  Commands are functions, be them anonymous or not, which can be inside of one or more Command Groups.  
  Command Groups are classes where a `@Group` annotation is used.  
  All preconditions can be checked simultaneous by calling `core.Commands::checkPreconditions`.  
  
- Systems `META!`  
  User-provided lifelong processes. Uses range from repeating jobs bound to a timer or connectors between the bot and an external data source.  
  Systems can be manually unregistered during runtime via the `core.Manager::unregister` API.  
  Systems can depend on other systems using the `@DependsOn` annotation.  
  Systems can add "soft dependencies" using the `@Extends` annotation.  
  
- Command Parsing `Important!`  
  Parses a user given string and serializes all inputs into commands/parameters with an extensible interface.  
  Likely to follow [`kotlinx.serialization`](https://github.com/Kotlin/kotlinx.serialization)'s `Serializable` format.  

- System Handling `Important!`  
  Handles the lifetime of systems.  
  The system lifetime is as follows:  
  - `PreInit`
  - `Init` - **Required** 
  - `PostInit`
  - `Disable` - **Required**  

- Discord Connection `Important!`
- Secret Storage
- Configuration API
- Prefix Handling `Important!`
