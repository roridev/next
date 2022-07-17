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

### Repeating
Background procedures that repeat at a set interval given by the user.  

### Single-instance
Simple background procedures that are set once the bot loads and are used in other parts of the bot.  
