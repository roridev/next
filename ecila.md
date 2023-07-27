# ecila
*spooky action at a distance*

general-purpose websocket library for inter-server communication for minecraft.

`ecila` aims to support the following projects

| Project  | Support       |
|----------|---------------|
| Purpur   | Not Supported |
| Quilt    | Not Supported |
| NeoForge | Not Supported |
| Velocity | Not Supported |

`ecila` is still on the earliest phases of development.

what differs `ecila` from other existing solutions is left as an exercise for the viewer.

## ideas

- simple protocol that can be reimplemented in any language
- send messages between servers
- broadcast messages to all servers
- dynamically adding nodes to a network
- authorization flow
- central orchestrating node

## future plans
- trust-less communication between any clients
- permanent channels with multiple listeners 

## pipe dreams
- p2p networking solution so that there isn't a single point of failure
- automatic delegation of the central node at a client's shutdown
- somehow working with docker


## proto
this is the ecila protocol, it's nothing novel.

a message:
```json5
{
  "proto": "ecila/0.1",
  "header" : {
    "from" : "<uuid>",
    "to" : "<uuid>",
    "timestamp" : "2023-07-26T19:35:51Z",
    "broadcast": true,
    "type" : "PlayerLoginEvent"
  },
  "message": {
    // The contents can be literally anything.
    // A "Keep Alive" message is a message sent from any client to the orchestrator
    // with no content.
  }
}
```

the `type` field is required and is used by the receiver to deserialize the message.  
the `from` and `to` fields determine the sender and receiver and are also required.  
the `message` field can be either a scalar, a vector or an object.
It is fully determined by the sender.  
the `broadcast` field determines if a message should be broadcasted to all listeners.
a message is only a broadcast if the receiver is the current orchestrator.

topology:
```json5
{
  "proto": "ecila/0.1",
  "topology": {
    "network": {
      "id": "<uuid>",
      "name" : "My silly little network",
      "created_at": "2023-07-26T19:35:51Z"
    },
    "orchestrator" : "<uuid>",
    "nodes": ["<uuid>", "<uuid>", "<uuid>"],
    "updated_at": "2023-07-26T19:35:51Z",
  }
}
```

the network topology *must* be stored by all members and updated constantly.

this topology is how each node discovers each other on the network, and topology updates can come from any other node, but they need to be validated by the orchestrator.

the `orchestrator` is the current node responsible for routing messages.

node:
```json5
{
  "proto": "ecila/0.1",
  "node" : {
    "id" : "<uuid>",
    "name" : "",
    "inet": {
      "type": "tcp",
      "ip": "::1",
    },
    "client": {
      "brand" : "quilt",
      "version" : "0.20.0-beta.4",
      "provides": {
        // fuck if i know, this whole provides thing is metadata.
        // i'm just adding stuff i think would be useful as all hell.
        "mods": [
          
        ],
        "minecraft": {
          "version": "1.20.1"
        }
      }
    },
    "expires_at": "<datetime>"
  }
}
```

the information about each node is only stored at the orchestrator, as that node is responsible for forwarding those messages.

a new node can only be added via a request for addition sent to the orchestrator.  
that same request, if accepted, returns a response with the node data for the orchestrator.  
this response is sent only once after the request for addition is received, and subsequent
requests for addition will be invalidated if the details for that node aren't the same as the already existing node.  
if the repeating request matches the existing node, a new response will be sent.  
otherwise, any subsequent requests to that node id will be queued until:

- the original node sends either a keep alive message or any message.
- the node expires.

## using

the idea is that all of that protocol bs is abstracted away so your code should look something like this:

```kotlin
import ecila.networking

suspend fun main() { 
    // Gets the currently available network, based on a config.
    // The default config will search on localhost::[port tbd] for a
    // HTTPS WebSocket server, attempt to ask its topology and save it.
    val network = Ecila()
    
    val self = EcilaClient(
        name="Silly Goobers' SMP",
        brand=ecila.ClientBrands.PURPUR,
        provides=Json.encodeToString(
            mapOf(
                    // This wouldn't be hardcoded, and likely abstracted away
                    // On the clients' implementation, since honestly you'd use
                    // `ecila-neoforged`, `ecila-quilt`, `ecila-purpur`.
                    // The example just shows that `ecila` doesn't care at all
                    // about what you put in `provides`.
                    Pair("plugins", arrayOf("essentials", "towny", "siegewar")),
                    Pair("minecraft", mapOf(Pair("version", "1.12.2")))
            ))
    )
    
    // Sends a request for addition 
    network.add(self)
    
    // Waits until a 1.20.1 quilt server is available 
    val quiltServer = network.waitForClient.with {
      it.client.brand == ecila.ClientBrands.QUILT
              && it.client.provides["minecraft"].version == "1.20.1"
    }
    
    // Idk, send anything.
    quiltServer.sendMessage(PlayerJoinedMessage(uuid))
    
    // Gets the receiving channel from a client as a flow
    quiltServer.getRecvChannelAsFlow { msg ->
        println("Just received a message from ${msg.header.from} with type ${msg.header.type}")
        // msg.header.from == quiltServer.id  
        println("Contents:")
        println(msg.contents)
    }
    
        
}
```