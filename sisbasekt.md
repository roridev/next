# SisbaseKT
`Type: Metaproject`   
Kotlin discord bot framework using the lessons learned by developing sisbase.net

## Uses
[`Kord`](https://github.com/kordlib/kord) - **Discord Library Wrapper** 
[`pf4j`](https://github.com/pf4j/pf4j) - **Plugin Framework**

## Subprojects
[`sisbase-core`](sisbasekt/core.md) - Fabric-like extension manager.  
[`sisbase-api`](sisbasekt/api.md) - Api for writing sisbase extensions.   
[`sisbase-commands`](sisbasekt/commands.md) - Command Parsing Library.  

## Rationale
Learning [`kotlin-coroutines`](https://github.com/Kotlin/kotlinx.coroutines).  

Simplifying discord bot creation by centralizing all the boilerplate in a single cohesive manager and abstracting feature creation under a singular api shared by all extensions.  

Future proofing code by having an easy-to-update path for extensions since all low-level bookkeeping is delegated to the manager.

