# SisbaseKT
`Type: Metaproject`   
Kotlin discord bot framework using the lessons learned by developing sisbase.net

## Subprojects
[`sisbase-core`](sisbasekt/core.md) - Fabric-like extension manager.  
[`sisbase-api`](sisbasekt/api.md) - Api for writing sisbase extensions.   
[`sisbase-commands`](sisbasekt/commands.md) - Command Parsing Library.  
[`sisbase-types`](sisbasekt/types.md) - Shared discord types.  

## Rationale
Learning [`kotlin-coroutines`](https://github.com/Kotlin/kotlinx.coroutines).  

Simplifying discord bot creation by centralizing all the boilerplate in a single cohesive manager and abstracting feature creation under a singular api shared by all extensions.  

Future proofing code by having an easy-to-update path for extensions since all low-level bookkeeping is delegated to the manager.

## Goals

- Extensibility

The library should be as extensible as possible as to allow advanced extension authors to have all tools necessary to implement their already existing code into an extension.  

- Easy upgrade path

If discord changes their API, upgrading a sisbasekt bot would be as easy as updating the `sisbase-core.jar` loader.  
If an extension author updates their extension, upgrading it would be as easy as replacing the `extensions/extension.jar` file.  
If someone decides to port their "fat bot"<sup>1</sup> into a sisbase extension the path should be as clear as possible.  

<sup>1</sup>- Bots which embed the discord library with them.   

- Modularity

Extension writers should only import what they need.  
E.g. If they already have their own solutions for command handling there would be no need to use `sisbase-commands`.  

- Backend Agnostic

In order to explain why this is a goal, a bit of history is required.  
Between 2019 and 2021 I had to swap between discord libraries constanly in order to keep my bots up-to-date with the latest API revision (at least 4 times [Discord4J -> discord.py -> DSharpPlus -> Discord.Net]), it was a complete pain and involved rewriting all of my bots' code (since in most cases the code is tied to the library).  
Some libraries had a very slow update cycle and had a "bleeding edge" fork where "experiments" (like keeping the Discord API version up-to-date, for some wierd reason) were done and you were forced to add a separate source and keep track of that, leading to "Update Framework Version" commits being way too frequent.  
Some libraries were propperly maintained but their design goals were incompatible with my use case and lacked the flexibility necessary to create personalized solutions to even prefix handling (which if you ever made anything more complex then a single-guild bot is a necessity).  
Some other libraries just were left in the dust with no one to take care of them.  
While some libraries never left the beta phase.  

**Developing bots shouldn't require you to pray that your flavour of library is updated.**  
If the library you're using dies, just *swap to a different one*, **no code changes required**.  
Make the choice of the backing a mere ~runtime detail~. Your code shall run the same on Discord4j and Kord.  

