# sisbase-core
`Type: Subproject`  
Subproject of : [`SisbaseKT`](../sisbasekt.md)

Discord extension manager

## Dependencies
[PF4J](https://github.com/pf4j/pf4j) - Plugin Framework   
[api](api.md)  

## Features

- [Systems](core/systems.md) `META!`  
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


## Systems
Long-term, usually lifelong background procedures.  

Has the following attributes:

| Field                | Type                | Description                                                                                                       |
|----------------------|---------------------|-------------------------------------------------------------------------------------------------------------------|
| `name`               | String              | Name of the system                                                                                                |
| `description`        | String?             | A short description of the system                                                                                 |
| `expansions`         | [Expansion]         | An array of system expansions, or an empty array if none exist                                                    |
| `OnInit`             | suspend fun         | A function that runs on the start of the system's lifetime                                                        |
| `OnDisable`          | suspend fun         | A function that runs on the end of the system's lifetime                                                          |
| `CheckPreconditions` | suspend fun -> Bool | A function that runs before `OnInit`, if it returns `true` the system is loaded, otherwise the system is skipped. |


### Repeating
Background procedures that repeat at a set interval given by the user.  

Implemented by a `Expansion` that has the following attributes:
| Field       | Type        | Description                                                     |
|-------------|-------------|-----------------------------------------------------------------|
| `timeout`   | Duration    | The period of time that will be used for repeating the function |
| `OnElapsed` | suspend fun | The function that will be repeated once `timeout` elapses       |

### Discord-linked
Background procedures that reads/writes data to Discord.  

Implemented by a `Expansion` that has the following attributes:
| Field           | Type                 | Description                                                          |
|-----------------|----------------------|----------------------------------------------------------------------|
| `applyToClient` | suspend fun (Client) | A function that will be applied to the discord client after `OnInit` |

### Single-instance
Simple background procedures that are set once the bot loads and are used in other parts of the bot.  


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

