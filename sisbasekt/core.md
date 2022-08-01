# sisbase-core
`Type: Subproject`  
Subproject of : [`SisbaseKT`](../sisbasekt.md)

Discord extension manager

## Dependencies
[PF4J](https://github.com/pf4j/pf4j) - Plugin Framework   
[koin](https://github.com/InsertKoinIO/koin) - Dependency Injection  
[api](api.md)  

## Features

- [Systems](core/systems.md) `META!`  
  User-provided lifelong processes. Uses range from repeating jobs bound to a timer or connectors between the bot and an external data source.  
  Systems can be manually unregistered during runtime via the `core.Manager::unregister` API.  
  
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

## Sisbase-Core Lifetime Description

### Initialization

1. Loads configuration
2. Start the Backend
3. Wait for the Backend to connect to Discord
4. Loads Extensions

### Lost Connection to Backend

5. Stops all Extensions
6. Try restarting the backend 3 times
7. Shuts down the backend
8. Shuts down

### Connection to Backend is Resumed

5. Loads Extensions

## Configuration / Metadata

See the [sisbasekt.mod.json Documentation](./mod_json.md) for information about the Metadata File Format.  

-! IDEA: Due to an change in [`types`](types.md), deriving the `@RequireApiVersion` automatically could be done.  
-: [Not Required] [FUTURE] 

## Developing extensions

### Metadata
In order for an extension to be loaded, the `sisbasekt.mod.json` file must be present at the root of the JAR file.  
For more information on the Metadata File Format, [see above](core.md#configuration--metadata)  

In order to make sure your extension is propperly supported, extension writers can annotate their systems with `@RequireApiVersion`.  

On the case of a backend not supporting said version that system will be disabled and a warning will be printed to the console.  

```
[Sisbase-Loader] Dependency Error:
	Extension `id@version` requires
		Discord API Version >= X
	Backend `id@version` provides
		Discord API Version == Z

The following features will be disabled:
- Feature Name [Description]
- Feature Name [Description]

Please update or change the backend to get these features back.
```

Extension writers can also add `require-api-version` to the `mod.json` metadata file.  

On the case of a backend not supporting said version the extension won't be loaded and an error will be printed to the console.  

```
[Sisbase-Loader] Critical Dependency Failure:
	Extension `id@version` requires
		Discord API Version >= X
	Backend `id@version` provides
		Discord API Version == Z

Extension `id@version` is now disabled.

Please update or change the backend to get `id` back.
```

