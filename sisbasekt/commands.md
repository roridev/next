# sisbase-commands
`Type: Subproject`  
Subproject of: [`SisbaseKT`](../sisbasekt.md)  

Command library.  

Implemented as an extension. Users can use `sisbase-commands` optionally for command handling, but can also use their own command handling solutions if they so desire.  

## Dependencies
[core](core.md)  

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


## Structure of the `sisbase-commands` extension

### Persistent Data
The extension must hold a `Registry` for commands during the bot lifetime.  

The `Registry` must safeguard against command conflicts, as **only one command** (including overloads) can be registered for a given `Identifier`.  

`Identifier`s are a way to uniquely identify a given command and they include the following fields:

| Field                | Type   | Description                                        | Observations                                             |
|----------------------|--------|----------------------------------------------------|----------------------------------------------------------|
| `source`             | String | The id of the extension that registers the command | Automatically set based on the extension's metadata file |
| `fullyQualifiedName` | String | The full command path for a given command          | Automatically set once the command is registered         |

An example of a `Identifier` is given below:

Command is called as `/help`  
```yml
source: "your-bot"
command: "help"
identifier: "your-bot::help"
```

Command is called as `/group subcommand`
```yml
source: "your-bot"
command: "group/subcommand"
identifier: "your-bot::group/subcommand"
```

Identifiers don't care about command overloads, this is handled internally by the library.  
Mismatches for commands with equal names registered by the same extension will cause the extension to be rejected. (DUPLICATE COMMAND)  
Mismatches for commands with equal names registered by different extensions are to be resolved manually on the `overrides.yml` file. (COMMAND MISMATCH)  

Example of a mismatch:

`ext-a` registers a `help` command. -> `ext-a::help`  
`ext-b` also registers a `help` command. -> `ext-b::help`  

Console Output:
```
[Sisbase-Commands] Command Mismatch Detected:

The following commands will become unavailable until their mismatches are resolved.

    for `help`:
	
	`ext-a@version` registered `help`
	`ext-b@version` registered `help`
	
Please resolve all command mismatches on `overrides.yaml.`

[Sisbase-Commands] Disabled `ext-a::help` due to: COMMAND MISMATCH
[Sisbase-Commands] Disabled `ext-b::help` due to: COMMAND MISMATCH
``` 

Example of a duplicate command:

`ext-a` registers a `help` command. -> `ext-a::help`  
`ext-a` later registers a `help` command. -> `ext-a::help`  

Console Output:
```
[Sisbase-Commands] Duplicate Command Detected:

The following extensions will become unavailable until their duplicate commands are removed.

	for `ext-a`:
	
	Duplicate `help` command registered.

Please contact the extension authors to remove the duplicate commands.

[Sisbase-Commands] Disabled `ext-a` due to: DUPLICATE COMMAND

```

On the case of a critical failure that leads to the extension being shutdown (or on the case of it being disabled)
all commands and hooks **must be unregistered** immediately.  

### Required hooks

For command parsing:

| Hook                     | Usage                                                                                                         |
|--------------------------|---------------------------------------------------------------------------------------------------------------|
| `onMessageReceived`      | Used for parsing the message contents, building the CommandContext, and forwarding the data to the call site. |
| `onGuildMessageReceived` | Same as `onMessageReceived`                                                                                   |
