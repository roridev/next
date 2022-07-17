# sisbase-api
`Type: Subproject`  
Subproject of: [`SisbaseKT`](../sisbasekt.md)

Api for developing extensions, has two main abstractions: Commands and Systems.

## Commands
Simple interactions with the user caused by a direct call to them.

Has the following attributes:

| Field         | Type     | Description                                                                     |
|---------------|----------|---------------------------------------------------------------------------------|
| `name`        | String   | Name of the command                                                             |
| `description` | String?  | A short description of the command                                              |
| `user`        | User     | The user that called the command                                                |
| `channel`     | Channel  | The channel on which the command was called                                     |
| `guild`       | Guild?   | The guild on which the command was called                                       |

### Text-based
The third-party command system, based on detecting commands from messages sent on a given discord channel.  
Has an extensive permission system since the code is fully controlled by the library.  
Requires a system that checks every message for a valid command.  

Extends the base attributes with:

| Field    | Type           | Description                                                                                                 |
|----------|----------------|-------------------------------------------------------------------------------------------------------------|
| `checks` | [Precondition] | An array with all checks done to the text command by the permission engine, or an empty array if none exist |
| `group`  | Group?         | The group the command is a part of                                                                          |
| `alias`  | [String]       | An array containing all aliases to the command, or an empty array if none exist                             |

### Slash
Discord's native command system, supercedes text-based commands but has a limit of how many commands can be registered and require modifying the application.  
Does not require a system since discord dispatches the command to the bot via the gateway.  

**Currently, support for slash commands isn't planned, this could change in the future**  

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
| Field           | Type               | Description                                                          |
|-----------------|--------------------|----------------------------------------------------------------------|
| `applyToClient` | suspend fun (Kord) | A function that will be applied to the discord client after `OnInit` |

### Single-instance
Simple background procedures that are set once the bot loads and are used in other parts of the bot.  

