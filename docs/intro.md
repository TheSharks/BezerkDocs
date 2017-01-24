On this page you can find a description of how Bezerk and WildBeast interact together. If you haven't yet read the [terminology page](terminology.md), then **do so now to understand this!**

##Installation
Clone the Bezerk repo the place of your choice from `https://github.com/TheSharks/Bezerk.git`. After this browse to the folder, do `npm install`. Then you need to configure WildBeast to use Bezerk, do this by changing `use` to true in the `bezerk` section in the WildBeast configuration file. Then start Bezerk.js with the system of your choice.

##General concept
All results will be returned in JSON format as with any API. The Bezerk server acts like a nerve center between a WildBeast shard and the Discord gateway. There is always this duality where different things happen to shards and listeners. This will also be highlighted in the documentation for the sake of clarity.

If WildBeast has been configured to use Bezerk the following will happen.

##Open websocket connection and identify shard
The below diagram illustrates the concept of Bezerk when it comes to opening a websocket connection and identifying a shard or listener.

![Open-Identify](https://i.dougley.com/pMU22jRy.png)

**Shard:** When the websocket connection between Bezerk and a shard is opened, OP code `HELLO` is emitted to the shard. When that OP code is received, the shard will then emit the `IDENTIFY_SHARD` code back to the server with an array that contains shard information handled by the WB. The payload will be in the format of `[shardid, shardcount]`. See [WildBeastDocs/sharding](https://docs.thesharks.xyz/sharding/) for more information. If the shard identification is successful, OP code `OK` is emitted to the shard which leads to a successful connection.

**Listener:** When the websocket connection between Bezerk and a listener is opened, OP code `HELLO` is emitted to the listener identically to the shard and it receiving `HELLO`. When the OP code is received, the listener will in turn emit `IDENTIFY_LISTENER` back. The payload array will contain all the events being listened from the shards by that specific listener. If the listener identification is successful, OP code `OK` is emitted to the listener which results in a successful connection.

##Emission and reception of events for a shard

The below diagram illustrates how Bezerk will work when the shard receives an event from a shard.

![Gateway-Event](https://i.dougley.com/kIv56TRX.png)

When the WildBeast shard receives an event from Discord (Includes library events) or Bezerk, an OP code containing the event name and further information in the payload will be emitted to the Bezerk server in a string format. **This will happen regardless of the event being listened for or not!** The Bezerk server will then check if any listeners are subscribed to the event in question. If not, the event will be discarded. If yes, the OP code and event payload will be sent to the listener.

##Emission and reception of events for a listener

The below diagram illustrates how a Bezerk listener will react to an incoming event from a shard.

![Event-Reception](https://i.dougley.com/T7h06GmY.png)

The diagram is a sort of continuation on the previous one. It starts with the Bezerk listener in the top-left corner.

When an event occurs, the listener will emit an OP code containing the event name, whatever else is required alongside it and a shard number if desired. The Bezerk server will then check if a shard is defined. If the shard is not defined, the event will be sent to all shards. If a shard is defined, the server will check if it is connected. If it's not, the event will be dropped. If the shard is connected, the shard will process the event and send the reply back to the listener via the Bezerk server.

###And that, in a nutshell, is how Bezerk operates!