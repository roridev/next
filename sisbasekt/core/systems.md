# Systems
`Type: Feature`  
Part of [`core`](../core.md)  

Long-term, usually lifelong background procedures.  

## Data Description
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

