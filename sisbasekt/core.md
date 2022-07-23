# sisbase-core
`Type: Subproject`  
Subproject of : [`SisbaseKT`](../sisbasekt.md)

Discord extension manager

## Uses
[PF4J](https://github.com/pf4j/pf4j) - Plugin Framework   

## Features

- Systems `META!`  
  User-provided lifelong processes. Uses range from repeating jobs bound to a timer or connectors between the bot and an external data source.  
  Systems can be manually unregistered during runtime via the `core.Manager::unregister` API.  
  Systems can depend on other systems using the `@DependsOn` annotation.  
  Systems can add "soft dependencies" using the `@Extends` annotation.  
  
- System Handling `Important!`  
  Handles the lifetime of systems.  
  The system lifetime is as follows:  
  - `PreInit`
  - `Init` - **Required** 
  - `PostInit`
  - `Disable` - **Required**  

- Discord Connection `Important!`  
*Note: Actual discord connection will be delegated to a `Backend`*  
- Secret Storage
- Configuration API
- Prefix Handling `Important!`  
*Note: Prefix handling must be open to the user as easily allow configuring an external prefix handler.*  
